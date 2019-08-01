---
title: "[스프링 MVC 게시판] 10. 게시글 삭제"
layout: post
category: PROJECT
tags: [Spring]
excerpt: "스프링 MVC 패턴을 이용해서 게시판을 만들어 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 게시글 삭제

  게시글 삭제는 CRUD의 UPDATE에 해당합니다. 따라서 Mapper.xml에서는 delete 쿼리가 필요합니다.
  게시글 삭제의 핵심은 상태값(del_chk)을 줘서, 게시글을 실제로 삭제하는게 아닌 상태값을 변경하는게 핵심입니다.

### VO(DTO) 생성

  boardDTO이므로 6장의 게시판 목록 부분의 DTO랑 같습니다.

### Mapper.xml 작성

  ```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- 다른 mapper와 중복되지 않도록 네임스페이스 기재 -->
<mapper namespace="boardDAO">
	<!-- 게시판 목록 -->
	<select id="list" resultType="boardDTO">
	<![CDATA[
		SELECT b.num, b.title, u.name, b.date, b.count, b.id
		FROM b_board as b
		INNER join b_users as u
		ON (b.id = u.id)
		WHERE del_chk = 'N'
		ORDER BY num DESC
	]]>
	</select>

	<!-- 게시물 전체 수 가져오기 -->
	<select id="boardCount" resultType="int">
		SELECT count(*)
		FROM b_board;
	</select>

	<!-- 해당 게시글 보기 -->
	<select id="select" parameterType="int" resultType="boardDTO">
		SELECT *
		FROM b_board
		WHERE num = #{num}
	</select>

	<!-- 삽입 -->
	<insert id="insert" parameterType="boardDTO">
		INSERET INTO b_board (title, contents, id) VALUES(#{title},#{contents},#{id})
	</insert>

	<!-- 수정 -->
	<update id="update" parameterType="boardDTO">
		UPDATE b_board SET title = #{title},
		contents = #{contents}
		WHERE num = #{num}
	</update>

	<!-- 삭제 : 상태값 -->
	<delete id="delete" parameterType="boardDTO">
	<![CDATA[
		UPDATE b_board SET del_chk = 'Y'
		WHERE num = #{num}
	]]>
	</delete>

</mapper>
  ```

  삭제 할 때에는 del_chk라는 컬럼 값을 Y로 바꿔서, 나중에 조회할 때 컬럼 값이 N인 애들만 조회하면 됩니다.
  이 방식은 실무에서 자주 사용하는 방식입니다.

### DAO Interface 작성

  ```java
public interface BoardDAO {

	public List<BoardDTO> list();
	public void delete(BoardDTO boardDTO);
	public int update(BoardDTO boardDTO);
	public void insert(BoardDTO boardDTO);
	public BoardDTO select(int num);

}
  ```

### DAOImpl 작성

  ```java
@Repository
public class BoardDAOImpl implements BoardDAO {

	@Autowired
	private SqlSessionTemplate sqlSessionTemplate;

	@Override
	public List<BoardDTO> list() {
		return sqlSessionTemplate.selectList("boardDAO.list");
	}

	@Override
	public void delete(BoardDTO boardDTO) {
		sqlSessionTemplate.delete("boardDAO.delete", boardDTO);
	}

	@Override
	public int update(BoardDTO boardDTO) {
		return sqlSessionTemplate.update("boardDAO.update", boardDTO);
	}

	@Override
	public void insert(BoardDTO boardDTO) {
		sqlSessionTemplate.insert("boardDAO.insert", boardDTO);
	}

	@Override
	public BoardDTO select(int num) {
		BoardDTO dto = (BoardDTO)sqlSessionTemplate.selectOne("boardDAO.select", num);
		return dto;
	}

}
  ```

### Service Interface 작성

  ```java
public interface BoardService {

	public List<BoardDTO> list();
	public void delete(BoardDTO boardDTO);
	public int edit(BoardDTO boardDTO);
	public void write(BoardDTO boardDTO);
	public BoardDTO read(int num);

}
  ```

### ServiceImpl 작성

  ```java
@Service
public class BoardServiceImpl implements BoardService {

	@Autowired
	private BoardDAO boardDAO;

	@Override
	public List<BoardDTO> list() {
		return boardDAO.list();
	}

	@Override
	public void delete(BoardDTO boardDTO) {
		boardDAO.delete(boardDTO);
	}

	@Override
	public int edit(BoardDTO boardDTO) {
		return boardDAO.update(boardDTO);
	}

	@Override
	public void write(BoardDTO boardDTO) {
		boardDAO.insert(boardDTO);
	}

	@Override
	public BoardDTO read(int num) {
		// boardDAO.updateReadCount(num);
		return boardDAO.select(num);
	}

}
  ```

### Controller 작성

  ```java
@Controller
public class BoardController {

	private static final Logger Logger = LoggerFactory.getLogger(BoardController.class);

	@Autowired
	private BoardService boardService;

	// 게시판 목록
	@RequestMapping(value="/boardList")
	public String boardList(Model model, HttpSession session) {
		if(session.getAttribute("id") == null) {
			model.addAttribute("msg", "잘못된 접근입니다!!");
			model.addAttribute("url", "login");
			return "redirect";
		} else {
			model.addAttribute("boardList", boardService.list());
			model.addAttribute("user", session.getAttribute("user"));
			return "boardList";
		}
	}

	// 게시글 상세 내역
	@RequestMapping(value="/boardRead/{num}")
	public String boardRead(Model model, @PathVariable int num) {
		model.addAttribute("boardDTO", boardService.read(num));
		return "boardRead";
	}

	// a태그 혹은 주소창 입력시 들어오는 곳
	// 바인딩 에러처리를 위해 Model 객체와 model.addAttribute() 추가
	@RequestMapping(value="/boardWrite", method=RequestMethod.GET)
	public String boardWrite(Model model) {
		model.addAttribute("boardDTO", new BoardDTO());
		return "boardWrite";
	}

	// 게시글 작성
	// Hibernate-validator까지 처리한 코드
	@RequestMapping(value="/boardWrite", method=RequestMethod.POST)
	public String boardWrite(@Valid BoardDTO boardDTO, BindingResult bindingResult, HttpSession session) {
		if(bindingResult.hasErrors()) {
			return "boardWrite"; // ViewResolver로 보냄
		} else {
			boardDTO.setId((String)session.getAttribute("id"));
			boardService.write(boardDTO);
			return "redirect:/boardSearchList"; // 새글을 반영하기 위해 컨트롤러로 보냄
		}
	}

	// 게시글 수정권한 판단
	@RequestMapping(value="/boardEdit/{num}", method=RequestMethod.GET)
	public String boardEdit(@PathVariable int num, Model model, HttpSession session, RedirectAttributes rttr) {
		if(session.getAttribute("id").equals(boardService.read(num).getId())) {
			 model.addAttribute("boardDTO", boardService.read(num));
			return "boardEdit";
		} else {
			rttr.addFlashAttribute("msg", "수정 권한이 없습니다");
			return "redirect:/boardSearchList";
		}
	}

	// 수정 검증 : BindingResut + Validator
	@RequestMapping(value="/boardEdit/{num}", method=RequestMethod.POST)
	public String boardEdit(@Valid BoardDTO boardDTO, BindingResult bindingResult) {
		if(bindingResult.hasErrors()) return "boardEdit";
		else {
			boardService.edit(boardDTO);
			return "redirect:/boardSearchList";
		}
	}

	// 게시글 삭제
	@RequestMapping(value="/boardDelete/{num}", method=RequestMethod.GET)
	public String boardDelete(@PathVariable int num, Model model, HttpSession session, RedirectAttributes rttr) {
		if(session.getAttribute("id").equals(boardService.read(num).getId())) {
			boardService.delete(boardService.read(num));
			return "redirect:/boardList";
		} else {
			rttr.addFlashAttribute("msg", "삭제 권한이 없습니다.");
			return "redirect:/boardList";
		}
	}
}
  ```

  게시글 삭제도 수정과 마찬가지로 HttpSession 객체를 이용해서 getAttribute로 ID값을 가져와서 권한이 있는지 판단하고
  RedirectAttributes를 사용한다는 점이 비슷합니다.
