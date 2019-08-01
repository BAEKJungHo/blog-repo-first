---
title: "커넥션 풀"
layout: post
category: JDBC
tags: [DataBase, MySQL]
excerpt: "커넥션 풀(Connection Pool)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

JSP, Servlet 공부를 하면서 참고한 서적입니다.

_프로젝트로 배우는 자바 웹 프로그래밍_

## 커넥션 풀

  이클립스에서 JDBC를 연동하여 사용할때에는 커넥션(Connection) 설정을 위해서
  DAO 생성자에서 DB에서 연결할 때마다 Connection을 생성했습니다.

  ```java
  // Constructor : JDBC 드라이버를 로딩 & DB Connection 구하기
  public XXXDAO() {
    try {
      Class.forName("com.mysql.jdbc.Driver");
      conn = DriverManager.getConnection(URL, USERNAME, PASSWORD);
    } catch(Exception e) {
      e.printStackTrace();
    }
  }
  ```

  `커넥션 풀(Connection Pool)`을 사용하지 않을 때에는 클라이언트와 JDBC(데이터베이스)가
  연결할 때 마다 쿼리를 날리고, 결과를 클라이언트로 반환하는 과정을 거쳤습니다.

  하지만, 커넥션 풀을 사용할 경우 어플리케이션에서 필요로 하는 시점에 커넥션을 만드는 것이
  아니라 __미리 일정한 수의 커넥션을 만들어 놓고 필요한 시점에 어플리케이션에 제공하는
  서비스 및 관리 체계__ 를 말합니다.

  - __커넥션 풀 동작 과정__

  1. 웹 어플리케이션 서버가 시작될 때 일정 수의 커넥션을 미리 생성.
  2. 웹 어플리케이션 요청에 따라 생성된 커넥션 객체를 전달.
  3. 일정 수 이상의 커넥션이 사용되면 새로운 커넥션 생성.
  4. 사용하지 않는 커넥션은 종료하고 최소한의 기본 커넥션을 유지.

  > Tomcat에서 커넥션 풀을 관리해줍니다.

## DBCP 설정

  - __Tomcat Server 생성__

  Eclipse 하단 콘솔(Console)창에 있는 Servers를 클릭하고 오른쪽 마우스를 눌러 New-Server를
  클릭합니다. 그리고 ServerName을 알맞게 고쳐 생성합니다. 생성한 서버를 오른쪽 마우스를 클릭하여
  `Add and Remove..`를 클릭하여 해당 서버만 Configured쪽으로 옮깁니다.

  만약 어떤 프로젝트를 `Open projects from File System`으로 가져온 경우 해당 프로젝트 우클릭 -
  Build Path - Configure Build Path - add Library - Server Runtime을 추가합니다.

  > Tomcat Settings : [https://baekjungho.github.io/apache-tomcat/](https://baekjungho.github.io/apache-tomcat/)

  - __Connector 다운로드__

  jdbc연동을 위해 http://dev.mysql.com/downloads/connector/j/ 에서 Connector를 다운로드합니다.
  다운로드한 mysql-connector-java-5.1.47.jar 파일을 톰캣이 설치된 폴더 /lib 안에 넣습니다.

  > C:\Program Files\Apache Software Foundation\Tomcat 8.5\lib

  - __DBCP 설정__

  Eclipse에서 Java EE Perspective를 누르고 왼쪽 `Servers` 폴더를 누르면 자신이 생성한
  서버 폴더가 나옵니다. 해당 서버 폴더를 클릭하면 `context.xml`파일이 나오는데
  클릭하여 열고, 다음 소스코드를 < /Context > 위에 붙여넣습니다.

  ```java
  <Resource name="jdbc/ezen"
          auth="Container"
          type="javax.sql.DataSource"
          factory="org.apache.tomcat.jdbc.pool.DataSourceFactory"
          testWhileIdle="true"
          testOnBorrow="true"
          testOnReturn="false"
          validationQuery="SELECT 1"
          validationInterval="30000"
          timeBetweenEvictionRunsMillis="30000"
          maxActive="100"
          minIdle="10"
          maxWait="10000"
          initialSize="10"
          removeAbandonedTimeout="60"
          removeAbandoned="true"
          logAbandoned="true"
          minEvictableIdleTimeMillis="30000"
          jmxEnabled="true"
          jdbcInterceptors=
          "org.apache.tomcat.jdbc.pool.interceptor.ConnectionState;org.apache.tomcat.jdbc.pool.interceptor.StatementFinalizer"
          username="javauser"
          password="javapass"
          driverClassName="com.mysql.jdbc.Driver"
          poolPreparedStatements="true"
          url="jdbc:mysql://localhost:3306/ezen"/>
  ```

  여기서 name, username, password, url을 자신의 DB에 맞게 수정합니다.
  그리고 Eclipse에서 Java Resources의 src 밑에 패키지(util)를 하나 생성하여 `DBManager.java` 파일을
  추가합니다.

  - __DBManager.java__

  ```java
  import java.sql.Connection;
  import java.sql.PreparedStatement;
  import java.sql.Statement;

  import javax.naming.Context;
  import javax.naming.InitialContext;
  import javax.sql.DataSource;

  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;

  /**
   * File : DBManager.java
   * Desc : 데이터베이스 연결 처리 클래스
   *
   */
  public class DBManager {
	private static final Logger LOG = LoggerFactory.getLogger(DBManager.class);
	// 데이터베이스 관련 객체 선언
	Statement stmt = null;
	PreparedStatement pstmt = null;

	/**
	 * JNDI를 이용해 Connection 객체 리턴
	 * @return
	 */
	public static Connection getConnection() {
		Connection conn;
		try {
			Context initContext = new InitialContext();
			DataSource ds = (DataSource) initContext.lookup("java:comp/env/jdbc/ezen");
			conn = ds.getConnection();
		}
		catch (Exception e) {
			System.out.println(e);
			e.printStackTrace();
			return null;
		}
		return conn;
	 }
  }
  ```

  그리고 `XXXDAO` 클래스에서 각각의 쿼리역할을 하는 메소드마다 다음 소스 코드를 추가하여 사용합니다.

  ```java
  public class XXXDAO {
  	private static final Logger LOG = LoggerFactory.getLogger(AdminDAO.class);
  	Connection conn; // Connection 참조변수 생성
  	PreparedStatement pstmt;
  	ResultSet rs;

    public returnType XXXMethod() {
      conn = DBManager.getConnection(); // DB Connection 구하기
    }
  }
  ```
