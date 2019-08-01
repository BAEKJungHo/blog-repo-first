---
title: "JDBC Question "
layout: post
category: JDBC
tags: [Java, MySQL, DataBase]
excerpt: "JDBC에 관한 문제를 풀어봅시다."
author: BAEKJungHo
---

* content
{:toc}

JDBC에 관한 문제를 풀어봅시다.

# JDBC 연습문제1.

  - 조건1

  ![sol1](/images/posts/201904/sol1.jpg)

  - 조건2

  ![sol2](/images/posts/201904/sol2.jpg)

  - 릴레이션(테이블)

  ```sql
  CREATE TABLE member (
  	id int(6) unsigned NOT NULL AUTO_INCREMENT,
 	  password varchar(10) NOT NULL,
  	name varchar(10) NOT NULL,
  	birthday date NOT NULL,
  	address varchar(50),
  	PRIMARY KEY (id)
  ) AUTO_INCREMENT=100001 DEFAULT CHARSET=utf8;
  ```

  - DTO(Data Transfer Object)

  ```java
  public class MemberDTO {
	private int id;
	private String password;
	private String name;
	private String birthday;
	private String address;

	public MemberDTO(int id, String password, String name, String birthday, String address) {
		super();
		this.id = id;
		this.password = password;
		this.name = name;
		this.birthday = birthday;
		this.address = address;
	}

	// id는 auto increment라서 제외한 생성자
	public MemberDTO(String password, String name, String birthday, String address) {
		super();
		this.password = password;
		this.name = name;
		this.birthday = birthday;
		this.address = address;
	}

	public MemberDTO() { }

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getBirthday() {
		return birthday;
	}

	public void setBirthday(String birthday) {
		this.birthday = birthday;
	}

	public String getAddress() {
		return address;
	}

	public void setAddress(String address) {
		this.address = address;
	}

	@Override
	public String toString() {
		return "MemberDTO [id=" + id + ", password=" + password + ", name=" + name + ", birthday=" + birthday
				+ ", address=" + address + "]";
	 }
  }
  ```

  - DAO(Data Access Object)

  ```java
  import java.sql.Connection;
  import java.sql.DriverManager;
  import java.sql.PreparedStatement;
  import java.sql.ResultSet;
  import java.sql.SQLException;
  import java.util.ArrayList;
  import java.util.List;

  import basic03.EaglesDTO;

  public class MemberDAO {

	private Connection conn;
	private static final String USERNAME = "javauser";
	private static final String PASSWORD = "javapass";
	private static final String URL = "jdbc:mysql://localhost:3306/world?verifyServerCertificate=false&useSSL=false";


	// Constructor : JDBC 드라이버를 로딩 & DB Connection 구하기
	public MemberDAO() {
		try {
			Class.forName("com.mysql.jdbc.Driver");
			conn = DriverManager.getConnection(URL, USERNAME, PASSWORD);
		} catch(Exception e) {
			e.printStackTrace();
		}
	}

	// INSERT QUERY - 회원가입
	public void insertMember(MemberDTO member) {
		String query = "insert into member values (?, ?, ?, ?, ?);";
		PreparedStatement pStmt = null;
		try {
			pStmt = conn.prepareStatement(query);
			pStmt.setInt(1, member.getId());
			pStmt.setString(2, member.getPassword());
			pStmt.setString(3, member.getName());
			pStmt.setString(4, member.getBirthday());
			pStmt.setString(5, member.getAddress());

			pStmt.executeUpdate();
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			try {
				if(pStmt != null && !pStmt.isClosed())
					pStmt.close();
			} catch(SQLException se) {
				se.printStackTrace();
			}
		}
	}

	// UPDATE QUERY - 변경
	public void updateMember(MemberDTO member) {
		String query = "update member set password=?, name=?, birthday=?, address=? where id=?;";
		PreparedStatement pStmt = null;
		try {
			pStmt = conn.prepareStatement(query);
			pStmt.setString(1, member.getPassword());
			pStmt.setString(2, member.getName());
			pStmt.setString(3, member.getBirthday());
			pStmt.setString(4, member.getAddress());
			pStmt.setInt(5, member.getId());

			pStmt.executeUpdate();
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			try {
				if(pStmt != null && !pStmt.isClosed())
					pStmt.close();
			} catch(SQLException se) {
				se.printStackTrace();
			}
		}
	}

	// DELETE QUERY - 삭제
	public void deleteMember(int id) {
		String query = "delete from member where id=?;";
		PreparedStatement pStmt = null;
		try {
			pStmt = conn.prepareStatement(query);
			pStmt.setInt(1, id);

			pStmt.executeUpdate();
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			try {
				if(pStmt != null && !pStmt.isClosed())
					pStmt.close();
			} catch(SQLException se) {
				se.printStackTrace();
			}
		}
	}

	// selectOne
	public MemberDTO selectOne(int id) {
		String query = "select * from member where id=?;";
		PreparedStatement pStmt = null;
		MemberDTO member = new MemberDTO();
		try {
			pStmt = conn.prepareStatement(query);
			pStmt.setInt(1,  id);
			ResultSet rs = pStmt.executeQuery();

			while(rs.next()) {
				member.setId(rs.getInt(1));
				member.setPassword(rs.getString(2));
				member.setName(rs.getString(3));
				member.setBirthday(rs.getString(4));
				member.setAddress(rs.getString(5));
			}
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			try {
				if(pStmt != null && !pStmt.isClosed())
					pStmt.close();
			} catch(SQLException se) {
				se.printStackTrace();
			}
		}
		return member;
	}

	// selectCondition
	public List<MemberDTO> selectCondition(String query) {
		PreparedStatement pStmt = null;
		List<MemberDTO> memberList = new ArrayList<MemberDTO>();
		try {
			pStmt = conn.prepareStatement(query);
			ResultSet rs = pStmt.executeQuery();

			while(rs.next()) {
				MemberDTO member = new MemberDTO();
				member.setId(rs.getInt(1));
				member.setPassword(rs.getString(2));
				member.setName(rs.getString(3));
				member.setBirthday(rs.getString(4));
				member.setAddress(rs.getString(5));

				memberList.add(member);
			}
		} catch(Exception e) {
			e.printStackTrace();
		} finally {
			try {
				if(pStmt != null && !pStmt.isClosed())
					pStmt.close();
			} catch(SQLException se) {
				se.printStackTrace();
			}
		}
		return memberList;
	}

	// 이름으로 검색 - 동명이인 존재 가능성
	public List<MemberDTO> selectName(String name) {
		String sql ="select * from member where name like '" + name + "';";
		List<MemberDTO> memberList = selectCondition(sql);
		return memberList;
	}

	// select - 조회 - 최근순서(내림차순)
	public List<MemberDTO> selectAll() {
		String sql ="select * from member order by id desc;";
		List<MemberDTO> memberList = selectCondition(sql);
		return memberList;
	}

	// 로그인 검증(ID, PASSWORD)
	public void checkLogin(int id, String password) {
		MemberDTO member = new MemberDTO();
		MemberDAO mDao = new MemberDAO();
		member = mDao.selectOne(id); // 해당 id에 속하는 컬럼값 들을 member객체에 저장

		// member객체로 Getter()를 사용하여 ID와 PASSWORD값 얻어와서 비교하기
		if((member.getId() == id) && (member.getPassword().equals(password))) {
			System.out.println("로그인 성공!");
		} else {
			System.out.println("로그인 실패!");
		}
	}
	// Connection Close
 	public void close() {
		try {
			if(conn != null && !conn.isClosed())
				conn.close();
		} catch(SQLException e) {
			e.printStackTrace();
		}
	 }
  }
  ```

  - MemberApplication

  ```java
  import java.util.List;
  import java.util.Scanner;

  import basic03.EaglesDTO;
  public class MemberApplication {
	public static void main(String[] args) {
		boolean run = true;

		int id;
		String password;
		String name;
		String birthday;
		String address;

		MemberDTO member;
		List<MemberDTO> memberList;
		MemberDAO mDao = new MemberDAO();

		while(run) {
			System.out.println("회원메뉴 1-가입, 2-조회, 3-변경, 4-삭제, 5-검색, 6-로그인, 7-종료");
			Scanner scan = new Scanner(System.in);
			int number = Integer.parseInt(scan.nextLine());
			switch(number) {
			case 1: // Insert
				System.out.print("패스워드>"); password = scan.nextLine();
				System.out.print("이름>"); name = scan.nextLine();
				System.out.print("생년월일>"); birthday = scan.nextLine();
				System.out.print("주소>"); address = scan.nextLine();

				mDao.insertMember(new MemberDTO(password, name, birthday, address));
				System.out.println("입력 완료 !!");
				break;
			case 2: // SelectAll
				memberList = mDao.selectAll();
				for (MemberDTO members : memberList) {
					System.out.println(members.toString());
				}
				break;
			case 3: // Update
				System.out.print("ID 번호를 입력해 주세요 : ");
				id = Integer.parseInt(scan.nextLine());

				System.out.print("패스워드>"); password = scan.nextLine();
				System.out.print("이름>"); name = scan.nextLine();
				System.out.print("생년월일>"); birthday = scan.nextLine();
				System.out.print("주소>"); address = scan.nextLine();

				// Update 전
				System.out.println("변경 전");
				member = mDao.selectOne(id);
				System.out.println(member.toString());

				member.setPassword(password);
				member.setName(name);
				member.setBirthday(birthday);
				member.setAddress(address);
				mDao.updateMember(member);

				// Update 후
				System.out.println("변경 후");
				member = mDao.selectOne(id);
				System.out.println(member.toString());
				break;
			case 4: // Delete
				System.out.print("삭제할 ID 번호를 입력해 주세요 : ");
				id = Integer.parseInt(scan.nextLine());

				member = mDao.selectOne(id);
				if(member.getId() == 0) {
					System.out.println("ID번호가 잘 못 입력 되었습니다.");
					continue;
				} else {
					System.out.println(member.toString() + "\n");
					mDao.deleteMember(id);
					System.out.println("삭제 후 릴레이션");
					memberList = mDao.selectAll();
					for (MemberDTO members : memberList) {
						System.out.println(members.toString());
					}
				}
				break;
			case 5: // Search by Name
				System.out.print("검색할 이름을 입력하세요 : ");
				name = scan.nextLine();
				memberList = mDao.selectName(name);
				for (MemberDTO members : memberList) {
					System.out.println(members.toString());
				}
				break;
			case 6: // Login
				System.out.print("로그인 아이디 : ");
				id = Integer.parseInt(scan.nextLine());
				System.out.print("로그인 패스워드 : ");
				password = scan.nextLine();
				mDao.checkLogin(id, password);
				run = false;
				break;
			case 7:
				System.out.println("종료되었습니다!");
				run = false;
				break;
			} // End of switch

		} // End of while

		// Connection close
		mDao.close();
	 }
  }
  ```

# JDBC 연습문제2.

  - 조건1

  ![bbs1](/images/posts/201904/sol1.jpg)

  - 조건2

  ![bbs2](/images/posts/201904/sol2.jpg)

  - 릴레이션(테이블)

  ```sql
  create table bbs (
  id int unsigned not null auto_increment,
  memberId int unsigned not null,
  title varchar(50) not null,
  date datetime not null default current_timestamp,
  content varchar(400),
  primary key(id),
  foreign key(memberId) references member(id)
  ) default charset=utf8;
  ```

  - DTO(Data Transfer Object)

  ```java
  public class BbsDTO {

	private int id;
	private int memberId;
	private String title;
	private String date;
	private String content;
	private String name;

	public BbsDTO(int id, int memberId, String title, String date, String content) {
		super();
		this.id = id;
		this.memberId = memberId;
		this.title = title;
		this.date = date;
		this.content = content;
	}

	public BbsDTO(int memberId, String title, String date, String content) {
		super();
		this.memberId = memberId;
		this.title = title;
		this.date = date;
		this.content = content;
	}

	public BbsDTO( ) { }

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public int getMemberId() {
		return memberId;
	}

	public void setMemberId(int memberId) {
		this.memberId = memberId;
	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public String getDate() {
		return date;
	}

	public void setDate(String date) {
		this.date = date;
	}

	public String getContent() {
		return content;
	}

	public void setContent(String content) {
		this.content = content;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	@Override
	public String toString() {
		return "BbsDTO [id=" + id + ", 글쓴이=" + name + ", 제목=" + title + ", 수정일=" + date.substring(0, 16) + ", 내용="
				+ content + "]";
	}

	// DetailSearch
	public String toStrDetail() {
		return "BbsDTO [글쓴이=" + name + ", 제목=" + title + ", 수정일=" + date.substring(0, 16) + ", 내용="
				+ content + "]";
	}
	// Update
	public String toStrOne() {
		return "BbsDTO [id=" + id + ", memberId=" + memberId + ", 제목=" + title + ", 수정일=" + date.substring(0, 16) + ", 내용="
				+ content + "]";
	 }
  }
  ```

  date.substring(0, 16)를 사용한 이유는 자바에서 시간을 표현할때 뒤에 소수점 0(.0)이
  붙은 형태로 출력되기 때문에 문자열로 잘라야합니다.

  - DAO(Data Access Object)

  ```java
  import java.sql.Connection;
  import java.sql.DriverManager;
  import java.sql.PreparedStatement;
  import java.sql.ResultSet;
  import java.sql.SQLException;
  import java.util.ArrayList;
  import java.util.List;

  import practice1.MemberDAO;
  import practice1.MemberDTO;

  public class BbsDAO {

  	private Connection conn;
  	private static final String USERNAME = "javauser";
  	private static final String PASSWORD = "javapass";
  	private static final String URL = "jdbc:mysql://localhost:3306/world?verifyServerCertificate=false&useSSL=false";


  	// Constructor : JDBC 드라이버를 로딩 & DB Connection 구하기
  	public BbsDAO() {
  		try {
  			Class.forName("com.mysql.jdbc.Driver");
  			conn = DriverManager.getConnection(URL, USERNAME, PASSWORD);
  		} catch(Exception e) {
  			e.printStackTrace();
  		}
  	}

  	// INSERT QUERY - 글쓰기
  	public void insertBbs(BbsDTO bbs) {
  		String query = "insert into bbs(memberId, title, date, content) values (?, ?, ?, ?);";
  		PreparedStatement pStmt = null;
  		try {
  			pStmt = conn.prepareStatement(query);
  			pStmt.setInt(1, bbs.getMemberId());
  			pStmt.setString(2, bbs.getTitle());
  			pStmt.setString(3, bbs.getDate());
  			pStmt.setString(4, bbs.getContent());

  			pStmt.executeUpdate();
  		} catch(Exception e) {
  			e.printStackTrace();
  		} finally {
  			try {
  				if(pStmt != null && !pStmt.isClosed())
  					pStmt.close();
  			} catch(SQLException se) {
  				se.printStackTrace();
  			}
  		}
  	}

  	// DELETE QUERY - 삭제
  	public void deleteBbs(int memberId) {
  		String query = "delete from bbs where memberid=?;";
  		PreparedStatement pStmt = null;
  		try {
  			pStmt = conn.prepareStatement(query);
  			pStmt.setInt(1, memberId);

  			pStmt.executeUpdate();
  		} catch(Exception e) {
  			e.printStackTrace();
  		} finally {
  			try {
  				if(pStmt != null && !pStmt.isClosed())
  					pStmt.close();
  			} catch(SQLException se) {
  				se.printStackTrace();
  			}
  		}
  	}

  	// UPDATE QUERY - 수정
  	public void updateBbs(BbsDTO bbs) {
  		String query = "update bbs set title=?, date=?, content=? where memberid=?;";
  		PreparedStatement pStmt = null;
  		try {
  			pStmt = conn.prepareStatement(query);
  			pStmt.setString(1, bbs.getTitle());
  			pStmt.setString(2, bbs.getDate());
  			pStmt.setString(3, bbs.getContent());
  			pStmt.setInt(4, bbs.getMemberId());

  			pStmt.executeUpdate();
  		} catch(Exception e) {
  			e.printStackTrace();
  		} finally {
  			try {
  				if(pStmt != null && !pStmt.isClosed())
  					pStmt.close();
  			} catch(SQLException se) {
  				se.printStackTrace();
  			}
  		}
  	}

  	// selectCondition
  	public List<BbsDTO> selectCondition(String query) {
  		PreparedStatement pStmt = null;
  		List<BbsDTO> bbsList = new ArrayList<BbsDTO>();
  		try {
  			pStmt = conn.prepareStatement(query);
  			ResultSet rs = pStmt.executeQuery();

  			while(rs.next()) {
  				BbsDTO bbs = new BbsDTO();
  				bbs.setId(rs.getInt(1));
  				bbs.setName(rs.getString(2));
  				bbs.setTitle(rs.getString(3));
  				bbs.setDate(rs.getString(4));
  				bbs.setContent(rs.getString(5));

  				bbsList.add(bbs);
  			}
  		} catch(Exception e) {
  			e.printStackTrace();
  		} finally {
  			try {
  				if(pStmt != null && !pStmt.isClosed())
  					pStmt.close();
  			} catch(SQLException se) {
  				se.printStackTrace();
  			}
  		}
  		return bbsList;
  	}

  	// selectOne
  	// selectOne - id로 상세조회
  	public BbsDTO selectOne(int memberId) {
  		String query = "select * from bbs where memberid=?;";
  		PreparedStatement pStmt = null;
  		BbsDTO bbs = new BbsDTO();
  		try {
  			pStmt = conn.prepareStatement(query);
  			pStmt.setInt(1,  memberId);
  			ResultSet rs = pStmt.executeQuery();

  			while(rs.next()) {
  				bbs.setId(rs.getInt(1));
  				bbs.setMemberId(rs.getInt(2));
  				bbs.setTitle(rs.getString(3));
  				bbs.setDate(rs.getString(4));
  				bbs.setContent(rs.getString(5));
  			}
  		} catch(Exception e) {
  			e.printStackTrace();
  		} finally {
  			try {
  				if(pStmt != null && !pStmt.isClosed())
  					pStmt.close();
  			} catch(SQLException se) {
  				se.printStackTrace();
  			}
  		}
  		return bbs;
  	}

  	// 전체조회
  	// select - 조회
  	public List<BbsDTO> selectAll() {
  		String sql ="select b.id as 아이디, m.name as 이름, b.title as 제목, b.date as 수정일, b.content as 내용"
  				+ " from bbs as b " +
  				"inner join member as m " +
  				"on b.memberid = m.id " +
  				"order by date desc";
  		List<BbsDTO> bbsList = selectCondition(sql);
  		return bbsList;
  	}

  	// ID로 상세조회
  	// 제목으로 상세조회
  	public BbsDTO selectId(int id) {
  		String sql ="select m.name as 이름, b.title as 제목, b.date as 수정일, b.content as 내용"
  				+ " from bbs as b " +
  				"inner join member as m " +
  				"on b.memberid = m.id " +
  				"where b.id=? " +
  				"order by date desc";
  		PreparedStatement pStmt = null;
  		BbsDTO bbs = new BbsDTO();
  		try {
  			pStmt = conn.prepareStatement(sql);
  			pStmt.setInt(1,  id);
  			ResultSet rs = pStmt.executeQuery();

  			while(rs.next()) {
  				bbs.setName(rs.getString(1));
  				bbs.setTitle(rs.getString(2));
  				bbs.setDate(rs.getString(3));
  				bbs.setContent(rs.getString(4));
  			}
  		} catch(Exception e) {
  			e.printStackTrace();
  		} finally {
  			try {
  				if(pStmt != null && !pStmt.isClosed())
  					pStmt.close();
  			} catch(SQLException se) {
  				se.printStackTrace();
  			}
  		}
  		return bbs;
  	}

  	// Connection Close
   	public void close() {
  		try {
  			if(conn != null && !conn.isClosed())
  				conn.close();
  		} catch(SQLException e) {
  			e.printStackTrace();
  		}
  	}
  }
  ```

  - BbsApplication

  ```java
  import java.text.SimpleDateFormat;
  import java.util.Date;
  import java.util.List;
  import java.util.Scanner;

  import practice1.MemberDAO;
  import practice1.MemberDTO;
  public class BbsApplication{

  	public static void main(String[] args) {
  		boolean run = true;
  		int number;

  		while(run) {
  			System.out.println("=====회원메뉴 1-쓰기, 2-조회, 3-수정, 4-삭제, 5-상세조회, 6-종료=====");
  			Scanner scan = new Scanner(System.in);
  			number = Integer.parseInt(scan.nextLine());
  			switch(number) {
  			case 1:
  				write(); break;
  			case 2:
  				search(); break;
  			case 3:
  				update(); break;
  			case 4:
  				delete(); break;
  			case 5:
  				DetailSearch(); break;
  			case 6:
  				System.out.println("종료되었습니다.");
  				run = false;
  				break;
  			}
  		} // End of while
  	} // End of main

  	public static void write() {
  		MemberDAO mDao = new MemberDAO();
  		MemberDTO member = new MemberDTO();

  		int memberId;
  		String memberPassword;
  		String title;
  		String content;

  		// 로그인
  		Scanner scan = new Scanner(System.in);
  		System.out.print("로그인 아이디 : ");
  		memberId = Integer.parseInt(scan.nextLine());
  		System.out.print("로그인 패스워드 : ");
  		memberPassword = scan.nextLine();
  		if(mDao.checkLogin(memberId, memberPassword) == true) {
  			System.out.println("로그인 성공!");

  			member.setId(memberId);

  			// 날짜 변환
  			SimpleDateFormat format1 = new SimpleDateFormat ("yy-MM-dd HH:mm");
  			Date time = new Date();
  			String time1 = format1.format(time);

  			// 글쓰기
  			System.out.print("제목>"); title = scan.nextLine();
  			System.out.print("내용>"); content = scan.nextLine();
  			BbsDAO bDao = new BbsDAO();
  			bDao.insertBbs(new BbsDTO(member.getId(), title, time1, content));
  			System.out.println("입력 완료 !!");
  		} else {
  			System.out.println("로그인 실패!");
  		}
  	}

  	public static void search() {
  		BbsDAO bDao = new BbsDAO();
  		List<BbsDTO> bbsList;

  		bbsList = bDao.selectAll();
  		for (BbsDTO bbs : bbsList) {
  			System.out.println(bbs.toString());
  		}
  	}

  	public static void update() {
  		MemberDAO mDao = new MemberDAO();
  		MemberDTO member = new MemberDTO();
  		BbsDAO bDao = new BbsDAO();
  		BbsDTO bbs = new BbsDTO();

  		int memberId;
  		String title;
  		String content;
  		String memberPassword;

  		// 로그인
  		Scanner scan = new Scanner(System.in);
  		System.out.print("Member 아이디를 입력해주세요 : ");
  		memberId = Integer.parseInt(scan.nextLine());
  		System.out.print("로그인 패스워드 : ");
  		memberPassword = scan.nextLine();
  		if(mDao.checkLogin(memberId, memberPassword) == true) {
  			System.out.println("로그인 성공!");
  			member.setId(memberId);

  			// 날짜 변환
  			SimpleDateFormat format1 = new SimpleDateFormat ("yy-MM-dd HH:mm");
  			Date time = new Date();
  			String time1 = format1.format(time);

  			System.out.print("제목>"); title = scan.nextLine();
  			System.out.print("내용>"); content = scan.nextLine();

  			// Update 전
  			System.out.println("변경 전");
  			bbs = bDao.selectOne(memberId);
  			System.out.println(bbs.toStrOne());

  			bbs.setTitle(title);
  			bbs.setContent(content);
  			bbs.setDate(time1);
  			bDao.updateBbs(bbs);

  			// Update 후
  			System.out.println("변경 후");
  			bbs = bDao.selectOne(memberId);
  			System.out.println(bbs.toStrOne());
  		} else {
  			System.out.println("로그인 실패!");
  		}
  	}

  	public static void delete() {
  		int memberId;
  		BbsDAO bDao = new BbsDAO();
  		BbsDTO bbs = new BbsDTO();
  		List<BbsDTO> bbsList;

  		System.out.print("삭제할 Member 아이디를 입력해 주세요 : ");
  		Scanner scan = new Scanner(System.in);
  		memberId = Integer.parseInt(scan.nextLine());

  		bbs = bDao.selectOne(memberId);
  		if(bbs.getMemberId() == 0) {
  			System.out.println("ID번호가 잘 못 입력 되었습니다.");
  		} else {
  			System.out.println(bbs.toString() + "\n");
  			bDao.deleteBbs(memberId);
  			System.out.println("삭제 후 릴레이션");
  			bbsList = bDao.selectAll();
  			for (BbsDTO bb : bbsList) {
  				System.out.println(bb.toString());
  			}
  		}
  	}

  	public static void DetailSearch() {
  		int id;
  		BbsDAO bDao = new BbsDAO();
  		BbsDTO bbs = new BbsDTO();

  		System.out.print("ID를 입력하세요 : ");
  		Scanner scan = new Scanner(System.in);
  		id = Integer.parseInt(scan.nextLine());

  		System.out.println("검색결과");
  		bbs = bDao.selectId(id);
  		System.out.println(bbs.toStrDetail());
  	}
  }
  ```
