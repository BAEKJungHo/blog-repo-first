---
title: "[스프링 MVC 게시판] 11. 조회수 증가"
layout: post
category: PROJECT
tags: [Spring]
excerpt: "스프링 MVC 패턴을 이용해서 게시판을 만들어 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 조회수 증가

  조회수 증가는 Mapper.xml과 DAO Interface, DAOImlp, Service Interface, ServiceImpl만 수정하면 됩니다.

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

	<!-- 조회수 증가 -->
	<update id="updateCount" parameterType="int">
		UPDATE b_board SET
		count = count+1
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

  조회수 증가는 해당 게시글을 클릭하거나, 수정하기 위해 들어가게 되면 조회수가 증가하는 방식입니다.

### DAO Interface 작성

  ```java
public interface BoardDAO {

	public List<BoardDTO> list();
	public void delete(BoardDTO boardDTO);
	public int update(BoardDTO boardDTO);
	public void insert(BoardDTO boardDTO);
	public BoardDTO select(int num);
	public int updateReadCount(int num);

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

	@Override
	public int updateReadCount(int num) {
		return sqlSessionTemplate.update("boardDAO.updateCount", num);
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

  Service Interface는 동일합니다.

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
		boardDAO.updateReadCount(num); // 조회수 증가
		return boardDAO.select(num);
	}

}
  ```
