---
title: "[스프링 MVC 게시판] 5. 로그인 구현"
layout: post
category: PROJECT
tags: [Spring]
excerpt: "스프링 MVC 패턴을 이용해서 게시판을 만들어 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 로그인 구현

### VO(DTO) 생성

  UsersDTO(UsersVO)를 생성하겠습니다.

  ```java
package com.mayeye.board.dto;

public class UsersDTO {

	private String id;
	private String name;
	private String pwd;

	public String getId() {
		return id;
	}

	public void setId(String id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getPwd() {
		return pwd;
	}

	public void setPwd(String pwd) {
		this.pwd = pwd;
	}

	@Override
	public String toString() {
		return "UsersDTO [id=" + id + ", name=" + name + ", pwd=" + pwd + "]";
	}
}
  ```

### Mapper.xml 작성

  ```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- 다른 mapper와 중복되지 않도록 네임스페이스 기재 -->
<mapper namespace="usersDAO">
	<!-- 사용자 정보 조회 -->
	<select id="select" parameterType="String" resultType="usersDto">
		select * from b_users
		where id = #{id}
	</select>

	<!-- 비밀번호 체크 -->
	<select id="checkPw" resultType="int">
		select count(*) from b_users
		where id = #{id} and pwd = #{pwd}
	</select>
</mapper>
  ```

### DAO Interace 작성

  ```java
  package com.mayeye.board.dao;

  import com.mayeye.board.dto.UsersDTO;

  public interface UsersDAO {

  	public UsersDTO select(UsersDTO usersDTO);
  	public boolean checkPw(String id, String pwd);

  }
  ```

### DAOImpl 작성

  ```java
package com.mayeye.board.dao;

import java.util.HashMap;
import java.util.Map;

import org.mybatis.spring.SqlSessionTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

import com.mayeye.board.dto.UsersDTO;

@Repository
public class UsersDAOImpl implements UsersDAO {

	@Autowired
	private SqlSessionTemplate sqlSessionTemplate;

	@Override
	public UsersDTO select(UsersDTO usersDTO) {
		UsersDTO dto = (UsersDTO)sqlSessionTemplate.selectOne("usersDAO.select", usersDTO.getId());
		return dto;
	}

	@Override
	public boolean checkPw(String id, String pwd) {
		boolean flag = false;
		Map<String, String> map = new HashMap<String, String>();
		map.put("id", id);
		map.put("pwd", pwd);
		int count = sqlSessionTemplate.selectOne("usersDAO.checkPw", map);
		if(count == 1) return true;
		else return flag;
	}

}
  ```

### Service Interface 작성

  ```java
package com.mayeye.board.service;

import com.mayeye.board.dto.UsersDTO;

public interface UsersService {

	public UsersDTO select(UsersDTO usersDTO);
	public boolean checkPw(String id, String pwd);

}
  ```

### ServiceImpl 작성

  ```java
package com.mayeye.board.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.mayeye.board.dao.UsersDAO;
import com.mayeye.board.dto.UsersDTO;

@Service
public class UsersServiceImpl implements UsersService{

	@Autowired
	private UsersDAO usersDAO;

	public UsersDAO getUsersDAO() {
		return usersDAO;
	}

	public void setUsersDAO(UsersDAO usersDAO) {
		this.usersDAO = usersDAO;
	}

	@Override
	public UsersDTO select(UsersDTO usersDTO) {
		return usersDAO.select(usersDTO);
	}

	@Override
	public boolean checkPw(String id, String pwd) {
		return usersDAO.checkPw(id, pwd);
	}

}
  ```

### Controller 작성

  ```java
@Controller
public class UsersController {
	private static final Logger Logger = LoggerFactory.getLogger(UsersController.class);

	@Autowired
	private UsersService usersService;

	// 로그인 페이지로 이동
	@RequestMapping(value="/login", method = RequestMethod.GET)
	public String login(UsersDTO usersDTO) {
		return "login";
	}

	/* 로그인 처리 : HttpServletRequest를 사용
	@RequestMapping(value="/login", method = RequestMethod.POST)
	public String login(UsersDTO usersDTO, HttpServletRequest request) {
		HttpSession session = request.getSession();
		session.setAttribute("usersDTO", usersService.select(usersDTO.getId()));
		return "boardList";
	}
	*/

	// 로그인 처리 : HttpSession을 사용
	@RequestMapping(value="/login", method = RequestMethod.POST)
	public String login(UsersDTO usersDTO, HttpSession session, RedirectAttributes rttr, Model model) {
		// 비밀번호 처리
		boolean flag = usersService.checkPw(usersDTO.getId(), usersDTO.getPwd());

		if(flag) {
			UsersDTO user = usersService.select(usersDTO);
			session.setAttribute("user", user);
			session.setAttribute("id", user.getId());
			return "redirect:/boardList";
		} else {
			rttr.addFlashAttribute("msg", "아이디 또는 비밀번호가 틀렸습니다");
			return "redirect:/login";
		}

	}

	// 로그아웃 처리 : invalidate() 사용
	@RequestMapping(value="/logout")
	public String logout(UsersDTO usersDTO, HttpSession session) throws Exception {
		session.invalidate();
		return "redirect:/login";
	}
}
  ```

### View 작성

  ```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	<style>
		input[type=text] { /* text 창에만 적용 */
			color : red;
		}
		input:hover, textarea:hover { /* 마우스 올라 올 때 */
			background : aliceblue;
		}
		input[type=text]:focus, input[type=password]:focus { /* 포커스 받을 때 */
			font-size : 120%;
		}
		label {
			display : block; /* 새 라인에서 시작 */
			padding : 5px;
		}
		label span {
			display : inline-block;
			width : 90px;
			text-align : right;
			padding : 10px;
		}
	</style>
<title>MayEye BAEKJH Board</title>
</head>
<body>
	<center><br>
	<h3>Users Login</h3><br>
	<hr>
	<form:form commandName="usersDTO" mehtod="post">
		<form:label path="id"><span>ID:</span>
			<form:input path="id" />
			<form:errors path="id" />
		</form:label>
		<form:label path="pwd"><span>Password:</span>
			<form:input path="pwd" />
			<form:errors path="pwd" />
			</form:label><br>
		<input type="submit" value="로그인">&nbsp;&nbsp;
		<input type="reset" value="취소">
	</form:form>

	<c:if test="${msg ne null}">
		<p style="color:#f00;">${msg}</p>
	</c:if>
</body>
</html>
  ```
