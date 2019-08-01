---
title: "[스프링 MVC 게시판] 15. 파일 다중 업로드"
layout: post
category: PROJECT
tags: [Spring]
excerpt: "스프링 MVC 패턴을 이용해서 게시판을 만들어 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 파일 다중 업로드

  파일 다중 업로드를 구현하기 위해서는 컨트롤러와 View 파일만 수정하면 됩니다.
  단일 업로드에서 이미 Mapper.xml에는 다중 업로드에 관한 쿼리가 적용된 상태입니다.

### BoardController 작성

  ```java
  @Controller
public class BoardController {

	private static final Logger Logger = LoggerFactory.getLogger(BoardController.class);

	@Autowired
	private BoardService boardService;

	@Autowired
	private FilesService filesService;

	/* 객체의 이름이 일치하는 객체 자동주입
	* name 속성명에는 IOC 컨테이너에서 설정한 id 명으로 입력
	* 스프링 설정 파일을 불러올 때 유용하게 쓰임
	* name 속성으로 지정한 uploadPath를 사용 할 수 있음
	*/
	@Resource(name="uploadPath")
	private String uploadPath;

	// 게시판 페이징 + 검색
	@RequestMapping(value="/boardSearchList")
	public ModelAndView list(SearchCriteria cri) {
		Logger.info("!!!!!search!!!!!");

		ModelAndView mav = new ModelAndView();

		PageMaker pageMaker = new PageMaker();
		pageMaker.setCri(cri);
		pageMaker.setTotalCount(boardService.countArticle(cri.getSearchType(), cri.getKeyword()));

		List<BoardDTO> searchList = boardService.searchList(cri);
		int count = boardService.countArticle(cri.getSearchType(), cri.getKeyword());
		Logger.info("searchType     " + cri.getSearchType());
		Logger.info("keyword     " + cri.getKeyword());
		Logger.info("svf     " + String.valueOf(count));
		Map<String, Object> map = new HashMap<String, Object>();
		map.put("searchList", searchList);
		map.put("count", count);
		map.put("searchOption", cri.getSearchType());
		map.put("keyword", cri.getKeyword());
		mav.addObject("pageMaker", pageMaker);
		mav.addObject("map", map);
		return mav;
	}

	// 게시글 상세 내역 : 다중 업로드 적용
	@RequestMapping(value="/boardRead/{num}")
	public String boardRead(Model model, @PathVariable int num) {
		BoardDTO boardDTO = boardService.read(num);
		List<FileDetail> fileDetailList = filesService.findFileDetailList(boardDTO.getAtch_file_id());
		model.addAttribute("boardDTO", boardDTO);
		model.addAttribute("fileDetailList", fileDetailList);
		return "boardRead";
	}

	// a태그 혹은 주소창 입력시 들어오는 곳
	// 바인딩 에러처리를 위해 Model 객체와 model.addAttribute() 추가
	@RequestMapping(value="/boardWrite", method=RequestMethod.GET)
	public String boardWrite(Model model) {
		model.addAttribute("boardDTO", new BoardDTO());
		return "boardWrite";
	}

	// 게시글 작성 : 다중 업로드 버전
	// Hibernate-validator까지 처리한 코드
	// MultipartHttpServletRequest mtfRequest를 사용
	@RequestMapping(value="/boardWrite", method=RequestMethod.POST)
	public String boardWrite(@Valid BoardDTO boardDTO, BindingResult bindingResult, HttpSession session, MultipartHttpServletRequest mtfRequest, Model model) throws Exception {
		if(bindingResult.hasErrors()) {
			return "boardWrite"; // ViewResolver로 보냄
		} else {
			// file_sn 키 값 증가를 위한 변수
			int file_sn = 0;

			// uploadFile() : 내가 보낸 파일 명이 아닌, 저장된 시스템 파일명
			FileMaster fileMaster = new FileMaster();
			FileDetail fileDetail = new FileDetail();

			// getFiles안의 키값은 input multiple을 적용한 name값을 적으면 됩니다.
			List<MultipartFile> fileList = mtfRequest.getFiles("file");

			// 다중 파일이더라도 키값은 하나로 동일하고 file_sn만 따로 주면됨
			String atch_file_id = checkAtchFileId();

			// FileMaster 키값 설정
			fileMaster.setAtch_file_id(atch_file_id);
			filesService.insertFileMaster(fileMaster);

			// 게시판
			boardDTO.setAtch_file_id(atch_file_id);
			boardDTO.setId((String)session.getAttribute("id"));

			for (MultipartFile mf : fileList) {
				String save_name = uploadFile(mf);
				model.addAttribute("save_name", save_name);

				// 파일 세부 사항
				fileDetail.setAtch_file_id(atch_file_id);
				fileDetail.setFile_sn(file_sn);
				fileDetail.setOri_name(mf.getOriginalFilename());
				fileDetail.setSave_name(save_name);
				fileDetail.setSave_path(uploadPath);
				fileDetail.setFile_size((int)mf.getSize());

				filesService.insertFileDetail(fileDetail);
				Logger.info("file_sn ..... " + String.valueOf(file_sn));
				file_sn++; // 다중 파일이므로 file_sn을 증가 (PK : atch_file_id + file_sn)
			}				

			boardService.write(boardDTO);
			return "redirect:/boardSearchList";
		}
	}

	// 저장 파일명 생성
	private String uploadFile(MultipartFile file) throws Exception {
		/* UUID는 유니크한 ID값을 random으로 toString()화 하고 뒤에 오리지널 파일 명과 합침
		* 장점 : 성능도 괜찮고, 유니크한 ID값을 가질 수있다.
		* 단점 : 파일 명이 길어진다.
		* 서버에 저장된 파일명을 가져와서 uploadPath 경로에 만들고 FileCopyUtils.copy()를 이용해서 쓴다.
		* MultipartFile의 getBytes() 메서드를 이용하여 target에 데이터를 쓴다.
		*/
		String save_name = UUID.randomUUID().toString() + "_" + file.getOriginalFilename();
		File target = new File(uploadPath, save_name);
		FileCopyUtils.copy(file.getBytes(), target);
		return save_name;
	}

	// atch_file_id 생성 : char(25)
	private String makeAtchFileId() {
		Random random = new Random();
		StringBuilder temp = new StringBuilder();
		for (int i=0; i<8; i++) {
		    int rIndex = random.nextInt(3);
		    switch (rIndex) {
		    case 0:
		        // a-z
		        temp.append((char) ((int) (random.nextInt(26)) + 97));
		        break;
		    case 1:
		        // A-Z
		        temp.append((char) ((int) (random.nextInt(26)) + 65));
		        break;
		    case 2:
		        // 0-9
		        temp.append((random.nextInt(10)));
		        break;
		    }
		}
		String atch_file_id = "FILE_" + "000000000000" + temp;
		return atch_file_id;
	}

	// atch_file_id 생성 검증(중복 제거)
	private String checkAtchFileId() {
		List<BoardDTO> list = new ArrayList<>();
		list = boardService.list();
		boolean flag = true;
		String atch_file_id = null;
		if(!list.isEmpty()) {
			while(flag) {
				atch_file_id = makeAtchFileId();
				for(BoardDTO board : list) {
					if(!atch_file_id.equals(board.getAtch_file_id())) {
						flag = false;
					} else flag = true;
				}
			}
			return atch_file_id;
		} else return atch_file_id = makeAtchFileId();
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
			return "redirect:/boardSearchList";
		} else {
			rttr.addFlashAttribute("msg", "삭제 권한이 없습니다.");
			return "redirect:/boardSearchList";
		}
	}
}
  ```

  주의 깊게 봐야 하는 부분은 다음과 같습니다.

  ```java
  // getFiles안의 키값은 input multiple을 적용한 name 값을 적으면 됩니다.
  List<MultipartFile> fileList = mtfRequest.getFiles("file");
  ```

  즉 저 file 문자열은 boardWrite.jsp의 다음 코드와 같습니다.

  ```html
  <!-- name 값은 컨트롤러에서 getFiles("name값") 처럼 사용할 수 있습니다. -->
  <td><input multiple="multiple" type="file" name="file" /></td>
  ```

### boardRead.jsp

  boardWrite.jsp는 단일 업로드에서 차이를 알려줬으므로 한 줄만 바꿔서 쓰시면 됩니다.

  ```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>MayEye BAEKJH Board</title>
</head>
<body>
	<table border="1">
		<tr>
			<th>제목</th>
			<td>${boardDTO.title}</td>
		</tr>
		<tr>
			<th>내용</th>
			<td>${boardDTO.contents}</td>
		</tr>
		<tr>
			<th>작성일</th>
			<td>${boardDTO.date}</td>
		</tr>
		<tr>
			<th>조회수</th>
			<td>${boardDTO.count}</td>
		</tr>
		<c:forEach var="file" items="${fileDetailList}" varStatus="loop">
		<tr>
			<th>첨부파일${loop.count}</th>
			<td><a href="<c:url value="/fileDownload/${boardDTO.num}/${file.atch_file_id}/${file.file_sn}" />">${file.ori_name}</a></td>
		</tr>
		</c:forEach>
	</table>
	<div>
		<a href="<c:url value="/boardEdit/${boardDTO.num}" />">수정</a>
		<a href="<c:url value="/boardDelete/${boardDTO.num}" />">삭제</a>
		<a href="<c:url value="/boardSearchList" />">목록</a>
	</div>
</body>
</html>
  ```

  GET 방식으로 키값을 넘겨줘야 합니다. 따라서 컨트롤러에서는 키값을 이용하여 FileDetail에 있는 다른 상세 정보를 가져와서 사용할 수 있습니다.
