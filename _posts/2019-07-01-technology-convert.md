---
title: "OFFICE TO PDF"
layout: post
category: Technology
tags: [Maven, Java]
excerpt: "OFFICE TO PDF"
author: BAEKJungHo
---

* content
{:toc}

## OFFICE TO PDF

  2019.07.01(월) 첫 회사에 취직하여 출근하고나서 개발 팀장님께 받은 과제가 Java로 OFFICE 파일 및 한글(hwp)파일을
  PDF로 변환하고 PDF를 HTML로 변환하라는 과제를 받았다.

  팀장님은 아무런 참고사항 없이 인터넷을 뒤져가면서 어느정도 까지 할 수 있나를 보고싶으셨다고 한다.

  사실 `국비지원`으로 학원 수업을 듣다가 `조기취업`한 상태여서 메이븐과 스프링 사용에 대해 미숙해서, 이번 과제를
  메이븐을 사용해서 해보기로했다.

  > 이 프로젝트의 핵심은 `Apache POI 와 JODConverter`이다.

## TXT TO PDF

  `.txt` 파일을 `.pdf`로 변화하는 것도 쉬웠다. 이 또한 인터넷에 소스가 널렸다.

## PPT TO PDF

### Apache POI만 사용

  `.ppt` 파일을 `.pdf`로 변환하는건 쉬웠다. 인터넷에 소스가 널리고 널렸다. 처음에는
  Apache POI만을 사용하여 변환을 하였는데 텍스트, 워드아트, 스마트차트, 표, 차트는 정상적으로 나오지만
  마지막에 도형에 있어서 변환이 제대로 안되었다.

  그리고, `.pptx` 파일을 `.pdf`로 변환할 때에는 도형은 정상적으로 나오지만 표와 차트가 나오지 않는 현상이 발생하였다.

  인터넷을 뒤져보니 확장자 마지막에 x가 붙은 파일들은 OFFICE 버전이 2007 이후라서 파일 변환에 있어서 개발이 진행중이거나
  거의 불가능하다고 나와있다.

  Apache POI만을 사용하여 ppt와 pptx를 pdf로 파일변환할 때, 핵심은 다음과 같다.

  ```java
  XMLSlideShow ppt = new XMLSlideShow(is); // pptx 방식
  SlideShow ppt = new SlideShow(is); // ppt 방식

  XSLFSlide[] slide = ppt.getSlides(); //pptx 방식
  Slide[] slide = ppt.getSlides(); // ppt 방식
  ```

  그리고 다음과 같은 문제 사항이 있었다.

  - 문제사항 및 해결

    Apache POI를 사용할 때 HSSF 쓸 경우, poi-3.9....jar만 있으면된다. XSSF를 사용해야 하는데 많은 의존성이 필요하다.
    (poi, poi-scratchpad, poi-ooxml), 버전을 3.12로 사용했더니 HSSF를 사용한다는 오류가 나서 3.9 버전으로 교체

### JODConverter와 Apache POI를 같이 사용

  `JODConverter`를 사용하기 위해서 메이븐에 의존성을 추가해야 한다. 또한 `오픈오피스(OPENOFFICE)`를 설치해야 하는데
  다음 버전을 추천한다. JODConverter는 오피스 파일만 변환할 수 있도록 지원하고, HWP 파일은 지원하지 않는다.

  > [오픈오피스 설치(OPENOFFICE)](http://www.asrgo.com/?l=jp&mid=PDS&document_srl=164398)
  >
  > 구글 검색 키워드 : OOo_3.3.0_Win_x86_install-wJRE_ko.exe

  ```xml
  <dependency>
    <groupId>org.jodconverter</groupId>
      <artifactId>jodconverter-core</artifactId>
      <version>4.0.0-RELEASE</version>
  </dependency>
  ```

  JODConverter 4.2.2 버전이 최신이지만, 이 버전으로 하게 될 경우 소스가 먹통이 됬다.

  JODConverter를 사용하니 ppt 파일은 정상적으로 출력이되며, pptx 파일은 그래도 몇가지 변환에 있어서 문제가 있었다.
  하지만 코드도 간결하고 사용하기 편했다.

  - ppt를 pdf로 변환하는 것은 성공
  - pptx를 pdf로 변환하는 것은 몇가지 변환이 제대로 안됨
  - ppt를 바로 html로 변환하는 것은 성공

  ```java
  import java.io.File;
  import java.io.PrintWriter;
  import java.io.Writer;

  import org.apache.pdfbox.pdmodel.PDDocument;
  import org.fit.pdfdom.PDFDomTree;
  import org.jodconverter.OfficeDocumentConverter;
  import org.jodconverter.office.DefaultOfficeManagerBuilder;
  import org.jodconverter.office.OfficeException;
  import org.jodconverter.office.OfficeManager;

  public class JODConverterTest {

	public static void main(String[] args) {
		JODConverterTest jc = new JODConverterTest();

		// JODConverter를 사용 : OFFICE TO PDF
		OfficeManager officeManager = new DefaultOfficeManagerBuilder().build();

		try {
			officeManager.start();
			OfficeDocumentConverter converter = new OfficeDocumentConverter(officeManager);
			converter.convert(new File("C:\\Users\\MAYEYE\\Desktop\\workspace\\file\\pptA.ppt"),
      new File("C:\\Users\\MAYEYE\\Desktop\\workspace\\file\\pptA.pdf"));
			officeManager.stop();
		} catch (OfficeException e) {
			e.printStackTrace();
		}
	 }
  }
  ```

  ```java
  converter.convert(new File("변환할 파일 경로 및 파일명"), new File("변환된 파일 경로 및 파일명"));
  ```

## PDF TO HTML

  pdf 파일을 html로 변환하기 위해서 팀장님이 보라고 알려준 사이트가 있다.

  > PDF TO HTML : [https://www.baeldung.com/pdf-conversions-java](https://www.baeldung.com/pdf-conversions-java)

  다음 사이트에서 정보를 얻어서 위 JODConverter를 사용한 코드에 덧 붙여서 pdf로 변환후 html로 바로 변환하려고 했는데
  `ClassNotFoundException`이 발생했다.

  - SOURCE

  ```java
  package com.argo.parser;

  import java.io.File;
  import java.io.PrintWriter;
  import java.io.Writer;

  import org.apache.pdfbox.pdmodel.PDDocument;
  import org.fit.pdfdom.PDFDomTree;
  import org.jodconverter.OfficeDocumentConverter;
  import org.jodconverter.office.DefaultOfficeManagerBuilder;
  import org.jodconverter.office.OfficeException;
  import org.jodconverter.office.OfficeManager;

  public class JODConverterTest {
	private static void generateHTMLFromPDF(String filename) {
	  try {
		  PDDocument pdf = PDDocument.load(new File(filename));
		  Writer output = new PrintWriter("C:\\Users\\MAYEYE\\Desktop\\workspace\\file\\pptAHTML.html", "utf-8");
		  new PDFDomTree().writeText(pdf, output);

		  output.close();
	  } catch(Exception e) { e.printStackTrace(); }

	}

	public static void main(String[] args) {
		// JODConverter를 사용 : OFFICE TO PDF
		OfficeManager officeManager = new DefaultOfficeManagerBuilder().build();

		try {
			officeManager.start();
			OfficeDocumentConverter converter = new OfficeDocumentConverter(officeManager);
			converter.convert(new File("C:\\Users\\MAYEYE\\Desktop\\workspace\\file\\pptA.ppt"), new File("C:\\Users\\MAYEYE\\Desktop\\workspace\\file\\pptA.pdf"));
			officeManager.stop();
		} catch (OfficeException e) {
			e.printStackTrace();
		}

		// PDF TO HTML
		generateHTMLFromPDF("C:\\Users\\MAYEYE\\Desktop\\workspace\\file\\pptA.pdf");
	 }
  }
  ```

  - ERROR

  ```
  Exception in thread "main" java.lang.NoClassDefFoundError: org/apache/pdfbox/text/PDFTextStripper
	at java.lang.ClassLoader.defineClass1(Native Method)
	at java.lang.ClassLoader.defineClass(ClassLoader.java:763)
	at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:142)
	at java.net.URLClassLoader.defineClass(URLClassLoader.java:468)
	at java.net.URLClassLoader.access$100(URLClassLoader.java:74)
	at java.net.URLClassLoader$1.run(URLClassLoader.java:369)
	at java.net.URLClassLoader$1.run(URLClassLoader.java:363)
	at java.security.AccessController.doPrivileged(Native Method)
	at java.net.URLClassLoader.findClass(URLClassLoader.java:362)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:349)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
	at java.lang.ClassLoader.defineClass1(Native Method)
	at java.lang.ClassLoader.defineClass(ClassLoader.java:763)
	at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:142)
	at java.net.URLClassLoader.defineClass(URLClassLoader.java:468)
	at java.net.URLClassLoader.access$100(URLClassLoader.java:74)
	at java.net.URLClassLoader$1.run(URLClassLoader.java:369)
	at java.net.URLClassLoader$1.run(URLClassLoader.java:363)
	at java.security.AccessController.doPrivileged(Native Method)
	at java.net.URLClassLoader.findClass(URLClassLoader.java:362)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:349)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
	at com.argo.parser.JODConverterTest.generateHTMLFromPDF(JODConverterTest.java:20)
	at com.argo.parser.JODConverterTest.main(JODConverterTest.java:43)
  Caused by: java.lang.ClassNotFoundException: org.apache.pdfbox.text.PDFTextStripper
	at java.net.URLClassLoader.findClass(URLClassLoader.java:382)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:349)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
	... 26 more
  ```

  아직 이 부분은 해결을 하지 못했다.

## XLSX TO PDF

  엑셀 파일을 PDF로 변환하기 위해서 JODConverter를 사용했는데, 엑셀 또한 제대로 변환이 안된부분이 있었다.

  PPT를 바로 HTML로 변환하는것은 됬지만, XLSX을 바로 HTML로 변환하는것은 안됬다.

## HWP TO PDF

  HWP를 PDF로 변환하는게 가장 어려운 것 같다. 물론 해결을 못했다.

  아무리 뒤져도 소스파일이 나오지 않는다.

  그나마 참고할 만한 사이트는 다음과 같다.

  > [https://elfinlas.github.io/2019/01/22/hwp2img-java/](https://elfinlas.github.io/2019/01/22/hwp2img-java/)

  HWP를 PDF로 변환하기 위해서는 HWP를 html로 변환하고나서 해야 할 거 같은데, 해결하기 조금 빡센거 같다.

## 결과

  HWP를 HTML로 변환하고 PDF로 변환하면 될 거같은 느낌이 오는데, HWP를 HTML로 변환하는 방법을
  찾기가 상당히 어렵다..

  HWP를 텍스트로 추출하고 TXT를 PDF로 변환하는 것은 성공했지만, 영어와 특수문자는 정상적으로 PDF에 출력이 되는 반면
  한글은 출력이 되지않는다.

  따라서 오피스파일(.x가 붙지않은)을 PDF로 변환하고, PDF를 HTML로 변환하는 코드와 HWP를 텍스트로 변환하고
  PDF로 변환하는 코드와 메이븐 의존성 라이브러리는 다음과 같다.

  - 메이븐 라이브러리

  ```xml
  <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.argo</groupId>
  <artifactId>java-hwp</artifactId>
  <version>0.0.1-SNAPSHOT</version>

  	<properties>
		<project.source.encoding>UTF-8</project.source.encoding>
		<project.source.version>1.7</project.source.version>
	</properties>

	<repositories>
	        <repository>
	            <id>com.e-iceblue</id>
	            <name>e-iceblue</name>
	            <url>http://repo.e-iceblue.com/nexus/content/groups/public/</url>
	        </repository>
	</repositories>


  <dependencies>
  	   	<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>1.7.5</version>
			<scope>provided</scope>
		</dependency>

		<!-- free spire.pdf for java -->

	    <dependency>
	        <groupId> e-iceblue </groupId>
	        <artifactId>spire.pdf</artifactId>
	        <version>2.2.0</version>
	    </dependency>

		<!-- Apache POI 추가 -->

		<dependency>
		    <groupId>org.apache.poi</groupId>
		    <artifactId>poi-ooxml</artifactId>
		    <version>3.15</version>
		</dependency>

		<dependency>
		    <groupId>org.apache.poi</groupId>
		    <artifactId>poi-scratchpad</artifactId>
		    <version>3.15</version>
		</dependency>

	    <!-- ITEXT 추가 -->

		<dependency>
		    <groupId>com.itextpdf</groupId>
		    <artifactId>itextpdf</artifactId>
		    <version>5.5.10</version>
		</dependency>

		<dependency>
		    <groupId>com.itextpdf.tool</groupId>
		    <artifactId>xmlworker</artifactId>
		    <version>5.5.10</version>
		</dependency>

	   <!-- PDFBOX 추가 -->
		<dependency>
		    <groupId>org.apache.pdfbox</groupId>
		    <artifactId>pdfbox-tools</artifactId>
		    <version>2.0.3</version>
		</dependency>

		<dependency>
		    <groupId>net.sf.cssbox</groupId>
		    <artifactId>pdf2dom</artifactId>
		    <version>1.6</version>
		</dependency>

	   <!-- https://mvnrepository.com/artifact/org.jodconverter/jodconverter-core -->
		<dependency>
    		<groupId>org.jodconverter</groupId>
   		    <artifactId>jodconverter-core</artifactId>
   		    <version>4.0.0-RELEASE</version>
		</dependency>
   </dependencies>

   	<build>
		<finalName>${project.artifactId}</finalName>
		<plugins>
			<plugin>
				<artifactId>maven-compiler-plugin</artifactId>
				<configuration>
					<source>${project.source.version}</source>
					<target>${project.source.version}</target>
					<encoding>${project.source.encoding}</encoding>
				</configuration>
			</plugin>
			<plugin>
				<artifactId>maven-resources-plugin</artifactId>
				<configuration>
					<encoding>${project.source.encoding}</encoding>
				</configuration>
			</plugin>
		</plugins>
	</build>
  </project>
  ```

  - 자바 소스

  ```java
  package com.argo.parser;

 import java.io.BufferedReader;
 import java.io.File;
 import java.io.FileNotFoundException;
 import java.io.FileOutputStream;
 import java.io.FileReader;
 import java.io.FileWriter;
 import java.io.IOException;
 import java.io.PrintWriter;
 import java.io.StringWriter;
 import java.io.Writer;

 import org.apache.pdfbox.pdmodel.PDDocument;
 import org.fit.pdfdom.PDFDomTree;
 import org.jodconverter.OfficeDocumentConverter;
 import org.jodconverter.office.DefaultOfficeManagerBuilder;
 import org.jodconverter.office.OfficeException;
 import org.jodconverter.office.OfficeManager;

 import com.argo.hwp.HwpTextExtractor;
 import com.itextpdf.text.Document;
 import com.itextpdf.text.DocumentException;
 import com.itextpdf.text.Element;
 import com.itextpdf.text.Font;
 import com.itextpdf.text.PageSize;
 import com.itextpdf.text.Paragraph;
 import com.itextpdf.text.pdf.PdfWriter;
 import com.spire.pdf.FileFormat;
 import com.spire.pdf.PdfDocument;

 public class JODConverterTest {

	private static void generatePDFFromOFFICE(String fileName) {
		// JODConverter를 사용 : OFFICE TO PDF
		OfficeManager officeManager = new DefaultOfficeManagerBuilder().build();

		try {
			officeManager.start();
			OfficeDocumentConverter converter = new OfficeDocumentConverter(officeManager);
			converter.convert(new File(fileName), new File("C:\\Users\\MAYEYE\\Desktop\\workspace\\file\\test1XLSX.pdf")); // pdf
			officeManager.stop();
		} catch (OfficeException e) {
			e.printStackTrace();
		}
	}

	private static void generateHTMLFromPDFUsingPDFBox(String fileName) {
		  try {
			  PDDocument pdf = PDDocument.load(new File(fileName));
			  Writer output = new PrintWriter("C:\\Users\\MAYEYE\\Desktop\\workspace\\file\\test1XLSXHTML.html", "utf-8"); // html
			  new PDFDomTree().writeText(pdf, output);
			  output.close();
		} catch(Exception e) { e.printStackTrace(); }
	}

	private static void hwpConvertText(String path) {
		File hwp = new File(path); // 텍스트를 추출할 HWP 파일
		Writer writer = new StringWriter(); // 추출된 텍스트를 출력할 버퍼
		try {
			HwpTextExtractor.extract(hwp, writer);
		} catch (FileNotFoundException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		} // 파일로부터 텍스트 추출
		String text = writer.toString(); // 추출된 텍스트
		try {
			File file = new File("C:\\Users\\MAYEYE\\Desktop\\workspace\\file\\hwpTxt.txt");
			if(!file.exists()) { // 해당 경로에 파일이 존재하지 않으면
			  file.createNewFile(); // 파일생성
			}
			FileWriter fw = new FileWriter("C:\\Users\\MAYEYE\\Desktop\\workspace\\file\\hwpTxt.txt");
			char[] data = text.toCharArray();

			for(int i=0; i<data.length; i++) {
				fw.write(data[i]);
			}
			fw.flush();
			fw.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	private static void generatePDFFromText(String fileName) {
		// PDF 파일, 버전 및 출력 파일의 크기를 정의
		Document pdfDoc = new Document(PageSize.A4);
		try {
			PdfWriter.getInstance(pdfDoc, new FileOutputStream("C:\\Users\\MAYEYE\\Desktop\\workspace\\file\\hwptestPDF.pdf"))
			  .setPdfVersion(PdfWriter.PDF_VERSION_1_7);
		} catch (FileNotFoundException e) {
			e.printStackTrace();
		} catch (DocumentException e) {
			e.printStackTrace();
		}
		pdfDoc.open();

		// 글꼴과 새 단락을 생성하는 데 사용되는 명령을 정의
		Font myfont = new Font();
		myfont.setStyle(Font.NORMAL);
		myfont.setSize(11);
		try {
			pdfDoc.add(new Paragraph("\n"));
		} catch (DocumentException e) {
			e.printStackTrace();
		}

		// 마지막으로 새로 만든 PDF 파일에 단락을 추가
		BufferedReader br;
		String strLine;

		try {
			br = new BufferedReader(new FileReader(fileName));
			while ((strLine = br.readLine()) != null) {
				Paragraph para = new Paragraph(strLine + "\n", myfont);
				para.setAlignment(Element.ALIGN_JUSTIFIED);
				pdfDoc.add(para);
			}
			pdfDoc.close();
			br.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}

	public static void main(String[] args) {
		// OFFICE TO PDF
		generatePDFFromOFFICE("C:\\Users\\MAYEYE\\Desktop\\workspace\\file\\test1.xlsx"); // office

		// PDF TO HTML USING PDFBOX
		generateHTMLFromPDFUsingPDFBox("C:\\Users\\MAYEYE\\Desktop\\workspace\\file\\test1XLSX.pdf"); // pdf

		// HWP TO TEXT
		hwpConvertText("C:\\Users\\MAYEYE\\Desktop\\workspace\\file\\hwptest.hwp"); // hwp

		// TEXT TO PDF
		generatePDFFromText("C:\\Users\\MAYEYE\\Desktop\\workspace\\file\\hwpTxt.txt"); // txt
	 }
  }
  ```
