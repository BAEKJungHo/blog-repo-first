---
title: "[스프링 MVC 게시판] 16. 파일 다운로드"
layout: post
category: PROJECT
tags: [Spring]
excerpt: "스프링 MVC 패턴을 이용해서 게시판을 만들어 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 파일 다운로드

  파일 다운로드는 FileController를 따로 만들어서 그 곳에 구현하면 됩니다.
  boardRead.jsp은 이미 수정한 상태 이므로, 다운로드 로직만 구현하면 됩니다.

### FileController 작성

  ```java
@Controller
public class FileController {

	private static final Logger Logger = LoggerFactory.getLogger(FileController.class);

	@Autowired
	private FilesService filesService;

	@RequestMapping(value="/fileDownload/{num}/{atch_file_id}/{file_sn}")
	public String fileDownload(@PathVariable int num, @PathVariable String atch_file_id, @PathVariable int file_sn,
			HttpServletRequest request, HttpServletResponse response) {

		FileDetail fileDetail = new FileDetail();
		fileDetail.setAtch_file_id(atch_file_id);
		fileDetail.setFile_sn(file_sn);
		fileDetail = filesService.findFileDetail(fileDetail);

		String path = fileDetail.getSave_path(); // 저장된 경로
		String fileName = fileDetail.getOri_name(); // 원본 파일명
		String savedName = fileDetail.getSave_name(); // 저장된 파일명
		String downName = null;
		String browser = request.getHeader("User-Agent"); // 브라우저 종류를 가져오는것

    // 저장된 경로에 저장된 파일명으로 객체를 만듭니다.
		File file = new File(path, savedName);

	  FileInputStream fileInputStream = null;
	  ServletOutputStream servletOutputStream = null;

		try {
		// 파일 인코딩 : 브라우저 확인 파일명 encode  
    // 원본 파일명을 인코딩 하여, 다운로드하는 상대방도 원본파일명으로 받을 수 있다.
	     if(browser.contains("MSIE") || browser.contains("Trident") || browser.contains("Chrome")){
	         downName = URLEncoder.encode(fileName,"UTF-8").replaceAll("\\+", "%20");
	     } else{
				   downName = new String(fileName.getBytes("UTF-8"), "ISO-8859-1");
	     }

	     response.setHeader("Content-Disposition","attachment;filename=\"" + downName +"\"");
       // 디폴트 미디어 타입은 운영체제 종종 실행파일, 다운로드를 의미    
	     response.setContentType("application/octer-stream");
	     response.setHeader("Content-Transfer-Encoding", "binary;");

       // 서버에 저장된 파일을, 저장된 파일명으로 위에서 객체를 만들었으므로
       // 아래 코드는 서버에 저장된 파일을 읽는 역할 입니다.
	     fileInputStream = new FileInputStream(file);
	     // servletInputStream : 서버의 파일을 자바로 읽는다.
       // 파일을 읽어올 때에는 FileInputStream으로 읽어온 뒤
       // 브라우저에 출력할 때에는 ServletOutputStream을 사용합니다.
       // 즉, 사용자가 해당 파일을 다운로드 받을 수 있게 출력을 해줘야 합니다.
	     servletOutputStream = response.getOutputStream(); // 서버의 파일을 바깥쪽으로 보낼때
       byte b [] = new byte[1024];
       int data = 0;

       // 바이트를 더이상 읽을 수 없으면 -1을 리턴
	     while((data=(fileInputStream.read(b, 0, b.length))) != -1) {
	         servletOutputStream.write(b, 0, data);
	     }

	     servletOutputStream.flush(); // 출력

		} catch (UnsupportedEncodingException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
	     if(servletOutputStream != null){
	         try{
	             servletOutputStream.close();
	         }catch (IOException e){
	             e.printStackTrace();
	         }
	     }
	     if(fileInputStream != null){
	        try{
	           fileInputStream.close();
	        }catch (IOException e){
	           e.printStackTrace();
          }
	      }
	   }
		return null;
	}
}
  ```

  ServletOutputStream 은 추상클래스이기 때문에 인스턴스를 생성할 수 없습니다.
  ServletResponse 클래스에 `getOutputStream()` 이라는 함수를 통해 servletOutputStream 인스턴스를 받아서 사용해야한다.

  위 코드를 보면 servletOutputStream을 먼저 닫아주고, fileInputStream을 닫아주는데, 이것은 DB에서 쿼리 사용을 위한 객체를 닫아주는 순서와 같습니다. DB에서 Statement객체 ResultSet객체 등을 사용할 때 먼저 열어준 반대의 순서로 닫아줘야 합니다. 즉, A, B, C 순서로 생성했으면 C, B, A 순서로 닫아줘야 합니다.

  > 예외 처리 할 때, 예를들어 IOException만 처리하면 되는 것을 Exception으로 처리하면, 나중에 보안 취약점 점검에 걸리기 때문에, 귀찮더라도 필요한 예외처리만 해주는 것이 좋다.

  content-Type에서 `application/octer-stream`의 의미는 디폴트 미디어 타입을 말하며, 운영체제 종종 실행파일, 다운로드를 의미합니다.
