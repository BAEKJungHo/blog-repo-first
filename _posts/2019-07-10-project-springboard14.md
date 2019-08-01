---
title: "[스프링 MVC 게시판] 14. 파일 단일 업로드"
layout: post
category: PROJECT
tags: [Spring]
excerpt: "스프링 MVC 패턴을 이용해서 게시판을 만들어 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 파일 단일 업로드

  파일 업로드를 사용하기 위해서는 Multipart를 사용해야 합니다. Multipart를 사용하기 위해서는
  의존성 설정, 스프링 설정파일 설정, View에서 form태그 설정을 해줘야 합니다.

  > Mulipart
  >
  > Multipart는 HTTP를 통해 File을 SERVER로 전송하기 위해 사용되는 `Content-Type` 입니다.

  실무에서는 파일 업로드를 위해서 DB 테이블을 2개 가지고 있습니다. Master와 Detail을 가지고 있는데
  File(Master) 테이블은 키값, 등록일, 파일사용상태(State)를 컬럼으로 가지고 있습니다.

  File(Detail) 테이블은 파일에 대한 상세 정보가 담겨있습니다. 이 테이블에서는 키값(중복가능)과 나중에 다중 파일 업로드 구현을 위해
  (예를들어 파일을 3개 업로드하면 시리얼 넘버 형식으로 1, 2, 3 순서로 붙게 하기 위함) 시리얼 넘버를 컬럼으로 가지고 있어야 하며,
  이 2개를 조합해서 기본키로 지정합니다.

  만약 시리얼 넘버를 유니크로 지정할 경우, 중복이 일어나게 되서 유니크로는 지정하면 안됩니다.

  또한 파일 키값은 고정적인 문자열(char)로 지정하는 편이 좋습니다. char로 지정하게 되면 검색에 있어서 성능이 향상됩니다.

### 폴더 생성

  업로드할 파일을 가지고 있는 폴더를 생성해야 합니다.

  업로드할 파일은 `webapp/WEB-INF/폴더생성`과 같은 방식으로 하는게 좋습니다. 만약 `webapp/폴더생성`과 같은 방식으로 하게되면
  접근성이 쉬워져서 파일에 대한 보안이 약하게 됩니다.

  > webapp은 일반적으로 경로만 입력해주면 접근이 가능하지만, WEB-INF안에 들어있으면 컨트롤러를 타지 않는이상 접근이 불가능 합니다.

### DB 테이블 생성

  파일 업로드, 다운로드 구현을 위해 File 테이블을 2개 생성함에 따라 Board 테이블에도 파일 키값을 가지고 있어야 합니다.

  - Board

  ```sql
CREATE TABLE b_board (
  num int(6) unsigned NOT NULL AUTO_INCREMENT,
  title varchar(50) NOT NULL,
  count int(5) unsigned default 0,
  date datetime NOT NULL DEFAULT current_timestamp,
  contents varchar(250),
  id varchar(20) not null default '',
  atch_file_id char(25) not null default '0',
  del_chk varchar(1) not null default 'N',
  PRIMARY KEY (num),
  foreign key (id) references b_users(id),
  foreign key (atch_file_id) references b_filemaster(atch_file_id)
) AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
  ```

  - FileMaster

  ```sql
create table b_filemaster (
  atch_file_id char(25) not null default '0',
  create_dt datetime not null default current_timestamp,
  use_at char(1) not null default 'Y',
  primary key (atch_file_id)
) DEFAULT CHARSET=utf8;
  ```

  - FileDetail

  ```sql
create table b_filedetail (
  atch_file_id char(25) not null default '0',
  file_sn int(10) not null,
  ori_name varchar(150) not null,
  save_name varchar(150) not null,
  save_path varchar(2000) not null,
  file_size int(10) not null,
  primary key (atch_file_id, file_sn),
  foreign key (atch_file_id) references b_filemaster(atch_file_id)
) DEFAULT CHARSET=utf8;
  ```

### Dependency 설정

  ```xml
<!-- 파일 업로드 -->
<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
<dependency>
   <groupId>commons-fileupload</groupId>
   <artifactId>commons-fileupload</artifactId>
   <version>1.3.3</version>
</dependency>
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.4</version>
</dependency>
  ```

  commons-fileupload를 의존성 설정할 때에 버전을 1.3.3이상으로 하는게 좋습니다.
  1.3.3미만은 GitHub에 소스를 올릴때 Vulnerable(취약)하다고 나옵니다.

### servlet-context.xml 설정

  스프링 설정 파일(IOC 컨테이너)에도 파일 업로드, 다운로드 구현을 위해 `MultiPartResolver`와 업로드 파일 경로가 담긴 `uploadPath`라는 이름으로 빈을 하나 등록해줘야 합니다.

  > Multipart 지원 기능을 사용하려면 먼저 MultipartResolver를 스프링 설정 파일에 등록. 스프링에서 기본으로 제공하는 MultipartResolver는 `CommonsMultipartResolver`입니다. CommonsMultipartResolver를 MultipartResolver로 사용하려면 다음과 같이 빈 이름으로 `multipartResolver`를 사용해서 등록하면 됩니다.

  ```xml
  <!-- 파일업로드에 필요한 Bean -->
  <beans:bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
      <!-- 파일 업로드 용량 -->
      <beans:property name="maxUploadSize" value="10485760" />
  </beans:bean>

  <beans:bean id="uploadPath" class="java.lang.String">
    <beans:constructor-arg value="서버에 업로드된 파일을 저장할 경로"></beans:constructor-arg>
  </beans:bean>
  ```

### View 설정

  MultipartFile이나 MultipartHttpServletRequest를 이용하여 업로드를 구현할 때에, View에서는 파일 전송을 위해 form에서 다음과 같이 형식을 지정해줘야 합니다.

  ```html
  <!-- form 커스텀 태그 사용 -->
  <form:form commandName="boardDTO" mehtod="post" enctype="multipart/form-data">
  ```

  즉, `enctype="multipart/form-data"`를 작성해야 File 업로드를 구현할 수 있습니다.
  그리고 Input 태그는 아래와 같이 설정해주면 됩니다.

  ```html
  <!-- 단일 업로드 방식 -->
  <td><input type="file" name="file" /></td>

  <!-- 다중 업로드 방식 : multiple 속성 추가-->
  <input multiple="multiple" type="file" name="file" />
  ```

### BoardDTO

  추가된 속성들이 있습니다. 해당 컬럼을 추가하고 Getter, Setter를 생성하면 됩니다.

  ```java
  private String ori_name; // 원본 파일명
  private String atch_file_id;
  ```

### FileMaster(VO)

  ```java
public class FileMaster {

	private String atch_file_id;
	private String create_dt;
	private String use_at;

	public String getAtch_file_id() {
		return atch_file_id;
	}

	public void setAtch_file_id(String atch_file_id) {
		this.atch_file_id = atch_file_id;
	}

	public String getCreate_dt() {
		return create_dt;
	}

	public void setCreate_dt(String create_dt) {
		this.create_dt = create_dt;
	}

	public String getUse_at() {
		return use_at;
	}

	public void setUse_at(String use_at) {
		this.use_at = use_at;
	}

	@Override
	public String toString() {
		return "FileMaster [atch_file_id=" + atch_file_id + ", create_dt=" + create_dt + ", use_at=" + use_at + "]";
	}

}
  ```

### FileDetail(VO)

  ```java
public class FileDetail {

	private String atch_file_id;
	private int file_sn;
	private String ori_name;
	private String save_name;
	private String save_path;
	private int file_size;

	public String getAtch_file_id() {
		return atch_file_id;
	}

	public void setAtch_file_id(String atch_file_id) {
		this.atch_file_id = atch_file_id;
	}

	public int getFile_sn() {
		return file_sn;
	}

	public void setFile_sn(int file_sn) {
		this.file_sn = file_sn;
	}

	public String getOri_name() {
		return ori_name;
	}

	public void setOri_name(String ori_name) {
		this.ori_name = ori_name;
	}

	public String getSave_name() {
		return save_name;
	}

	public void setSave_name(String save_name) {
		this.save_name = save_name;
	}

	public String getSave_path() {
		return save_path;
	}

	public void setSave_path(String save_path) {
		this.save_path = save_path;
	}

	public int getFile_size() {
		return file_size;
	}

	public void setFile_size(int file_size) {
		this.file_size = file_size;
	}

	@Override
	public String toString() {
		return "FileDetail [atch_file_id=" + atch_file_id + ", file_sn=" + file_sn + ", ori_name=" + ori_name
				+ ", save_name=" + save_name + ", save_path=" + save_path + ", file_size=" + file_size + "]";
	}

}
  ```

### BoardMapper.xml 수정

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
		SELECT b.num, b.title, u.name, b.date, b.count, b.contents, b.id, b.atch_file_id, f.ori_name
		FROM b_board as b
		INNER join b_users as u
		ON (b.id = u.id)
		inner join b_filedetail as f
		on (b.atch_file_id = f.atch_file_id)
		WHERE del_chk = 'N'
		ORDER BY num DESC
	]]>
	</select>

	<!-- 총 게시글 수 -->
	<select id="countBoardList" resultType="Integer">
    <![CDATA[
        SELECT count(*)
        FROM b_board
        WHERE del_chk = 'N'
    ]]>
	</select>

	<!-- 해당 게시글 보기 -->
	<select id="select" parameterType="int" resultType="boardDTO">
	<![CDATA[
		SELECT DISTINCT b.num, b.title, u.name, b.date, b.count, b.contents, b.id, b.atch_file_id
		FROM b_board as b
		INNER join b_users as u
		ON (b.id = u.id)
		inner join b_filemaster as m
		on (b.atch_file_id = m.atch_file_id)
		inner join b_filedetail as f
		on (m.atch_file_id = f.atch_file_id)
		WHERE del_chk = 'N' AND num = #{num}
		ORDER BY num DESC
	]]>
	</select>

	<!-- 삽입(첨부파일 추가)-->
	<insert id="insert" parameterType="boardDTO">
		INSERT INTO b_board (title, contents, id, atch_file_id)
		VALUES (#{title},#{contents},#{id},#{atch_file_id})
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

	<!-- 검색 대한 게시글 수 -->
	<select id="countArticle" resultType="int">
		SELECT count(*)
		FROM b_board as b
		INNER JOIN b_users as u
		ON (b.id = u.id)
		WHERE del_chk = 'N'
		<include refid="search" />
	</select>

	<!-- 검색 -->
	<select id="searchList" resultType="boardDTO">
		SELECT b.num, b.title, u.name, date_format(b.date, '%Y-%m-%d') as date, b.count, b.id, b.atch_file_id
		FROM b_board as b
		INNER JOIN b_users as u
		ON (b.id = u.id)
		WHERE del_chk = 'N'
		<include refid="search" />
		ORDER BY num DESC
		LIMIT #{pageStart}, #{perPageNum}
	</select>

	<!-- MyBatis 동적 sql -->
	<sql id="search">
		<choose>
			<when test="searchType == 'all'">
				AND (u.name LIKE CONCAT('%', #{keyword}, '%')
				OR b.contents LIKE CONCAT('%', #{keyword}, '%')
				OR b.title LIKE CONCAT('%', #{keyword}, '%'))
			</when>
			<when test="searchType == 'writer'">
				AND u.name LIKE CONCAT('%', #{keyword}, '%')
			</when>
			<when test="searchType == 'content'">
				AND b.contents LIKE CONCAT('%', #{keyword}, '%')
			</when>
			<when test="searchType == 'title'">
				AND b.title LIKE CONCAT('%', #{keyword}, '%')
			</when>
		</choose>
	</sql>
</mapper>
  ```

  처음 게시판 목록때 만들었던 list를 살리고, 위와 같이 수정하였습니다. 또한 각 쿼리마다 조인연산과, 추가된 컬럼명들이 있습니다.

### FilesMapper.xml 작성

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

  <!-- 다른 mapper와 중복되지 않도록 네임스페이스 기재 -->
  <mapper namespace="filesDAO">
  	<!-- FileDetail Insert -->
  	<insert id="insertFileDetail" parameterType="fileDetail">
  		INSERT INTO b_filedetail (atch_file_id, file_sn, ori_name, save_name, save_path, file_size)
  		VALUES (#{atch_file_id},#{file_sn},#{ori_name},#{save_name},#{save_path},#{file_size})
  	</insert>

  	<!-- FileMaster Insert -->
  	<insert id="insertFileMaster" parameterType="fileMaster">
  		INSERT INTO b_filemaster (atch_file_id)
  		VALUES (#{atch_file_id})
  	</insert>

  	<select id="findFileDetailList" parameterType="String" resultType="fileDetail">
  		<![CDATA[
  			SELECT m.atch_file_id, f.file_sn, f.ori_name
  			FROM b_filedetail as f
  			INNER JOIN b_filemaster as m
  			ON (f.atch_file_id = m.atch_file_id)
  			WHERE m.atch_file_id = #{atch_file_id}
  		]]>
  	</select>

  	<select id="findFileDetail" parameterType="fileDetail" resultType="fileDetail">
  		<![CDATA[
  			SELECT *
  			FROM b_filedetail
  			WHERE
  				atch_file_id = #{atch_file_id}
  			AND
  				file_sn = #{file_sn}
  		]]>
  	</select>
  </mapper>
  ```

### FilesDAO Interface 작성

  ```java
public interface FilesDAO {

	public List<Map<String, Object>> selectFileDetailList(Map<String, Object> map);
	public void insertFileMaster(FileMaster fileMaster);
	public void insertFileDetail(FileDetail fileDetail);
	public List<FileDetail> findFileDetailList(String atch_file_no);
	public FileDetail findFileDetail(FileDetail fileDetail);

}
  ```

### FilesDAOImpl 작성

  ```java
@Repository
public class FilesDAOImpl implements FilesDAO {

	@Autowired
	SqlSessionTemplate sqlSessionTemplate;

	@Override
	public List<Map<String, Object>> selectFileDetailList(Map<String, Object> map) {
		return sqlSessionTemplate.selectList("filesDAO.selectFileList", map);
	}

	@Override
	public void insertFileMaster(FileMaster fileMaster) {
		sqlSessionTemplate.insert("filesDAO.insertFileMaster", fileMaster);
	}

	@Override
	public void insertFileDetail(FileDetail fileDetail) {
		sqlSessionTemplate.insert("filesDAO.insertFileDetail", fileDetail);
	}

	@Override
	public List<FileDetail> findFileDetailList(String atch_file_no) {
		return sqlSessionTemplate.selectList("filesDAO.findFileDetailList", atch_file_no);
	}

	@Override
	public FileDetail findFileDetail(FileDetail fileDetail) {
		return sqlSessionTemplate.selectOne("filesDAO.findFileDetail", fileDetail);
	}

}
  ```

### FilesService Interface 작성

  ```java
public interface FilesService {

	public List<Map<String, Object>> selectFileDetailList(Map<String, Object> map);
	public void insertFileMaster(FileMaster fileMaster);
	public void insertFileDetail(FileDetail fileDetail);
	public List<FileDetail> findFileDetailList(String atch_file_no);
	public FileDetail findFileDetail(FileDetail fileDetail);

}
  ```

### FilesServiceImpl 작성

  ```java
@Service
public class FilesServiceImpl implements FilesService {

	@Autowired
	FilesDAO filesDAO;

	public List<Map<String, Object>> selectFileDetailList(Map<String, Object> map) {
		return filesDAO.selectFileDetailList(map);
	}

	@Override
	public void insertFileMaster(FileMaster fileMaster) {
		filesDAO.insertFileMaster(fileMaster);
	}

	@Override
	public void insertFileDetail(FileDetail fileDetail) {
		filesDAO.insertFileDetail(fileDetail);
	}

	@Override
	public List<FileDetail> findFileDetailList(String atch_file_no) {
		return filesDAO.findFileDetailList(atch_file_no);
	}

	@Override
	public FileDetail findFileDetail(FileDetail fileDetail) {
		return filesDAO.findFileDetail(fileDetail);
	}

}
  ```

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

	// 게시글 작성 : 단일 업로드 버전
	// MultipartFile을 이용하면 View에서 enctype="multipart/form-data" 처리한 폼의 파일 정보를 받을 수 있다.
	// MultipartFile의 각종 메서드 사용
	@RequestMapping(value="/boardWrite", method=RequestMethod.POST)
	public String boardWrite(@Valid BoardDTO boardDTO, BindingResult bindingResult, HttpSession session, MultipartFile file, Model model) throws Exception {
		Logger.info("upload POST ..... OriginalName={}, size={}", file.getOriginalFilename(), file.getSize());
		if(bindingResult.hasErrors()) {
			return "boardWrite"; // ViewResolver로 보냄
		} else {
			// uploadFile() : 내가 보낸 파일 명이 아닌, 저장된 시스템 파일명
			FileMaster fileMaster = new FileMaster();
			FileDetail fileDetail = new FileDetail();

			String atch_file_id = checkAtchFileId();
			String save_name = uploadFile(file);
			model.addAttribute("save_name", save_name);

			// 파일 마스터
			fileMaster.setAtch_file_id(atch_file_id);

			// 파일 세부 사항
			fileDetail.setAtch_file_id(atch_file_id);
			fileDetail.setFile_sn(file_sn);
			fileDetail.setOri_name(file.getOriginalFilename());
			fileDetail.setSave_name(save_name);
			fileDetail.setSave_path(uploadPath);
			fileDetail.setFile_size((int)file.getSize());

			// 게시판
			boardDTO.setAtch_file_id(atch_file_id);
			boardDTO.setId((String)session.getAttribute("id"));

			filesService.insertFileMaster(fileMaster);
			filesService.insertFileDetail(fileDetail);
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

  서버에 저장되는 파일명은 유니크하기 때문에, 파일을 생성하는 메서드를 만들고, 다시 한번 검증하는 메서드를 만들었습니다.
  그 이유는 첫 게시글을 작성하거나, 파일 생성기가 극악의 확률로 똑같은 키를 만들경우, FileMaster에서 중복이 되므로 방지하기 위함입니다.

### boardWrite.jsp 수정

  ```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!-- form 태그라이브러리 추가 -->
<!-- BindingResult를 사용하기 위해 추가하는 것임  -->
<!-- form 태그 라이브러리를 사용하면 action 속성이 없는데, 브라우저 주소창을 참고해서 스프링이 자동으로 action 정보를 설정해준다. -->
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>MayEye BAEKJH Board</title>
</head>
<body>
	<c:if test="${msg eq null}" >
	<form:form commandName="boardDTO" mehtod="post" enctype="multipart/form-data">
		<table border="1">
			<tr>
				<th><form:label path="title">제목</form:label></th>
				<td>
					<form:input path="title" />
					<form:errors path="title" />
				</td>
			</tr>
			<tr>
				<th><form:label path="contents">내용</form:label></th>
				<td>
					<form:input path="contents" />
					<form:errors path="contents" />
				</td>
			</tr>
			<tr>
				<!-- 단일 업로드 버전 -->
				<td><input type="file" name="file" /></td>

				<!-- 다중 업로드 버전 -->
				<!-- <td><input multiple="multiple" type="file" name="file" /></td> -->
			</tr>
		</table>
		<div>
			<input type="submit" value="등록">
			<a href="<c:url value="/boardSearchList" />"> 목록</a>
		</div>
	</form:form>
	</c:if>
</body>
</html>
  ```

  단일 업로드와 다중 업로드의 차이는 아래와 같습니다. 그리고 파일 업로드를 하기 위해 `enctype="multipart/form-data"`를 꼭 적어줘야 합니다.

  ```html
  <!-- 단일 업로드 버전 -->
  <td><input type="file" name="file" /></td>

  <!-- 다중 업로드 버전 -->
  <td><input multiple="multiple" type="file" name="file" /></td>
  ```

### boardRead.jsp 수정

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
		<tr>
			<th>첨부파일</th>
			<td><a href="<c:url value="/fileDownload/${boardDTO.num}/" />">${file.ori_name}</a></td>
		</tr>
	</table>
	<div>
		<a href="<c:url value="/boardEdit/${boardDTO.num}" />">수정</a>
		<a href="<c:url value="/boardDelete/${boardDTO.num}" />">삭제</a>
		<a href="<c:url value="/boardSearchList" />">목록</a>
	</div>
</body>
</html>
  ```

  첨부파일명을 클릭할 경우 다운로드가 가능하게 하기 위하여 나중을 위한 a태그를 걸었습니다.
