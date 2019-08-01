---
title: "Apache Commons IO"
layout: post
category: Apache
tags: [Apache, Java]
excerpt: "File Class와 Apache Commons IO에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > File Class와 Apache FileUtils에 대해서 배워 봅시다.

## File Class

  ```java
  File directoryOrFile = new File(path);
  ```

  위 코드에서 변수명을 `DirectoryOrFile`로 주는 이유는 File 객체 생성 및 초기화시, 경로(파일 혹은 디렉터리가 존재하는 경로)를 넣어 객체 초기화를 시켜주면, DirectoryOrFile은 해당 경로에 따른 디렉터리나, 파일을 가리킵니다.

  __즉, 클래스 이름은 비록 File이지만 Directory와 File을 관리하는 클래스 입니다.__

 위 코드에서 path가 Directory를 가리키게 될 경우, 디렉터리안에 있는 파일 혹은 디렉터리를 조회하기 위해서는 파일을 배열로 선언해서 받아야 합니다.
 만약, path가 file명까지 정확하게 가리킨다면, DirectoryOrFile변수명은 해당 파일을 가리키게 됩니다.

 배열로 받아서 파일과 디렉터리를 구분하는 코드는 아래와 같습니다.

 ```java
 File[] directoryOrFiles = directoryOrFile.listFiles();

 for(int i=0; i<directoryOrFiles.length; i++) {
   File fileOrDir = directoryOrFiles[i];

   if(file.isFile()) { // fileOrDir이 파일일 경우

   } else if(file.isDirectory()) { // fileOrDir이 디렉터리 일 경우

   }
 }
 ```

 - 디렉토리 생성 : File dir = new File("디렉터리의 경로");
 - 부모 디렉토리를 파라미터로 인스턴스 생성 : File newFile = new File(dir,"파일명");

 파일 클래스를 사용할 때 주로 파일을 버퍼를 이용해서 읽는 이유는 문자를 효율적으로 입출력하여 CPU부하를 줄일 수 있기 때문입니다.

### 파일 전체 경로를 얻는 메서드

  ```java
    // String으로 받고싶은경우 반환형 File을 String으로 바꾸고
    // return문도 주석처리한 코드로 변경하면 됩니다.
    public File getFileByPathToFile(String path) {
        Assert.notNull(path, "path 은 null이 올 수 없습니다.");
        try {
            Resource resource = resourceLoader.getResource(path);
            File file = resource.getFile();
            return file;
      // return file.getCanonicalPath();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
  ```

### 확장자 얻는 방법(String 클래스 이용)

  String 클래스의 lastIndexOf를 이용하는 방법입니다. lastIndexOf는 오른쪽부터 문자열을 세기 때문에, lastIndexOf는 파일 확장자를 구할때 사용됩니다.(lastIndexOf는 맨오른쪽에 적용된 규칙을 찾을 때 사용)

  ```
  File file = new File("c:\\logs\\test.txt");
  String fileName = file.getName();
  int find = fileName.lastIndexOf(".");
  System.out.println("확장자 : " + fileName.substring(find + 1));
  System.out.println("확장자 : " + fileName.substring(find, fileName.length());
  ```

### 확장자 얻는 방법(Apache 이용)

  Apache Commons IO에 보시면 `FilenameUtils`라는 클래스가 제공됩니다. `getExtension()` 메서드를 쓰시면 확장자를 얻을 수 있습니다.

  ```java
  destinationFileNameExtention = FilenameUtils.getExtension(origin).toLowerCase();
  ```

## 생성자 및 메서드

### 파일 클래스 생성자

  - File(File parent, String child) : parent 객체 폴더의 child라는 파일의 File 객체를 생성
  - File(String pathName) : pathName에 해당하는 File 객체 생성
  - File(String parent, String, child) :  parent 폴더 경로의 child라는 파일에 대한 File 객체를 생성
  - File(URI uri) : file uri 경로에 대한 파일의 File 객체를 생성

### 파일 클래스 메서드

  - File getAbsoluteFile() : 파일의 절대 경로를 넘겨준다.
  - String getAbsolutePath() : 파일의 절대 경로를 문자열로 넘겨준다.
  - File getCanonicalFile() : 파일의 Canonical 경로를 넘겨준다.
  - String getCanonicalPath() :  파일의 Canonical 경로를 문자열로 넘겨준다.
  - String getName() : 파일이나 폴더의 이름을 넘겨준다.
  - String getParent() : 부모 경로에 대한 경로명을 문자열로 넘겨준다.
  - File getParentFile() : 부모 폴더를 File의 형태로 리턴한다.
  - String getPath() : 파일의 경로를 문자열의 형태로 리턴한다.
  - long getTotalSpace() : 하드디스크의 총 용량을 리턴한다.
  - long getUsableSpace() : 하드디스크의 사용 가능한 용량을 리턴한다.
  - long getFreeSpace() : 하드디스크의 남은 공간을 리턴한다.
  - int hashCode() : hash code를 반환한다.
  - long lastModified() : 해당 경로 파일의 최종 수정 일자를 반환한다.
  - long length() : 해당 경로 파일의 길이를 반환한다.
  - Path toPath() : java.nio.file.Path 객체로 반환한다.
  - URI toURI() : URI 형태로 파일 경로를 반환한다.
  - File[] listRoots() : 하드디스크의 루트 경로를 반환한다.
  - String[] list() : 경로의 파일들과 폴더를 문자열 배열로 반환한다.
  - String[] list(FilenameFilter filter) : filter에 만족되는 파일들과 폴더 이름을 문자열 배열로 반환한다.
  - File[] listFiles() : 해당 경로의 파일들과 폴더의 파일을 배열로 반환한다.
  - File[] listFiles(FileFilter filter) : filter에 만족되는 파일들과 폴더를 File 배열로 반환한다.
  - File[] listFiles(FilenameFilter filter) : filter에 만족되는 파일들과 폴더를 File 배열로 반환한다.

### 생성, 수정, 삭제 메서드

  - boolean createNewFile() : 주어진 이름의 파일이 없으면 새로 생성한다.
  - static File createTempFile(String prefix, String suffix) : default temporary-file 디렉토리에 파일 이름에 prefix와 suffix를 붙여 임시파일을 생성한다.
  - static File createTempFile(String prefix, String suffix, File directory) : 새로운 임시파일을 파일 이름에 prefix와 suffix를 붙여 directory 폴더에 생성한다.
  - boolean delete() : 파일이나 폴더를 삭제한다. 단, 폴더가 비어있지 않으면 삭제할 수 없다.
  - void deleteOnExit() : 자바가상머신이 끝날 때 파일을 삭제한다.
  - boolean mkdir() : 해당 경로에 폴더를 만든다.
  - boolean mkdirs() : 존재하지 않는 부모 폴더까지 포함하여 해당 경로에 폴더를 만든다.
  - boolean renameTo(File dest) : dest로 File 이름을 변경한다.

### 체크 메서드

  - boolean exists() : 파일의 존재 여부를 리턴한다.
  - boolean isAbsolute() : 해당 경로가 절대경로인지 여부를 리턴한다.
  - boolean isDirectory() : 해당 경로가 폴더인지 여부를 리턴한다.
  - boolean isFile() : 해당 경로가 일반 file 인지 여부를 리턴한다.
  - boolean isHidden() : 해당 경로가 숨김 file 인지 여부를 리턴한다.

### 권한 메서드

  - boolean canExecute(): 파일을 실행할 수 있는지 여부를 리턴한다.
  - boolean canRead() : 파일을 읽을 수 있는지 여부를 리턴한다.
  - boolean canWrite() : 파일을 쓸 수 있는지 여부를 리턴한다.
  - boolean setExecutable(boolean executable) : 파일 소유자의 실행 권한을 설정한다.
  - boolean setExecutable(boolean executable, boolean ownerOnly) : 파일의 실행 권한을 소유자 또는 모두에 대해 설정한다.
  - boolean setReadable(boolean readable) : 파일의 소유자의 읽기 권한을 설정한다.
  - boolean setReadable(boolean readable, boolean ownerOnly) : 파일의 읽기 권한을 소유자 또는 모두에 대해 설정한다.
  - boolean setReadOnly() : 파일을 읽기 전용으로 변경한다.
  - boolean setWritable(boolean writable) : 파일의 소유자의 쓰기 권한을 설정한다.
  - boolean setWritable(boolean writable boolean ownerOnly) : 파일의 쓰기 권한을 소유자 또는 모두에 대해 설정한다.

### 파일 덮어쓰기와 이어쓰기

  ```java
  public class MainClass {
  	public static void main(String[] args) {
  		FileWriter fw, fw_append; // FileWriter 선언

  		try {
  			fw = new FileWriter(".\\java_Text.txt", false); // 파일이 있을경우 덮어쓰기
  			fw.write("Writer 1 : Hello world \r\nWrite Test\r\n");
  			fw.close();
  		} catch (IOException e1) {
  			e1.printStackTrace();
  		}

  		try {
  			fw_append = new FileWriter(".\\java_Text.txt", true); // 파일이 있을경우 이어쓰기
  			fw_append.write("Writer 2 : Append Test\r\nGoodbye~");
  			fw_append.close();
  		} catch (IOException e1) {
  			e1.printStackTrace();
  		}
  		return;
  	}
  }
  ```

### 파일 이동

```java
public vpid copyOriginalFile(String path) {
  File originalFile = new File(path);
  InputStream is = null;
  OutputStream os = null;

  try {
    is = new FileInputStream(originalFile); // 원본파일
    os = new FileOutputStream(newFolderPath); // 이동시킬 위치

    byte[] buffer = new byte[1024];
    int length;

    while((length = is.read(buffer)) > 0) {
      os.write(buffer, 0, length);
    }
  } catch (IOException e) {
    throw new RuntimeException();
  } finally {
    if(is != null) {
      try {
        os.close();
      } catch (IOException e) {
        throw new RuntimeException();
      }
    }
  }
}
```

## getCanonicalPath(), getPath(), getAbsolutePath()

  `getPath()`는 File 객체를 생성할 때 넣어준 경로를 그대로 반환합니다. 그리고 `getAbsolutePath()`와 `getCanonicalPath()`는 getPath()와는 달리 프로그램을 실행시킨 위치 정보도 함께 반환해 줍니다.

  ```java
File file = new File("src", "test");
System.out.println("getPath: " + file.getPath());
System.out.println("getAbsolutePath: " + file.getAbsolutePath());
System.out.println("getCanonicalPath: " + file.getCanonicalPath());
   ```

  위의 코드를 실행해보면 getPath()는 `src/test` 그대로 반환한 반면, getAbsolutePath()와 getCanonicalPath()는 현재 프로그램을 실행한 경로`D:\workspace\Test`를 포함하고 있습니다.

  ```
getPath: src\test
getAbsolutePath: D:\workspace\Test\src\test
getCanonicalPath: D:\workspace\Test\src\test
  ```

  absolute path와 canonical path는 결과가 같은데 두 메소드의 차이는 `현재 경로(".")`나 `상위 경로("..")`를 함께 사용하면 알 수 있습니다.

  ```java
File file = new File("../../workspace");
System.out.println("getPath: " + file.getPath());
System.out.println("getAbsolutePath: " + file.getAbsolutePath());
System.out.println("getCanonicalPath: " + file.getCanonicalPath());
  ```

  현재 경로(D:\workspace\Test)의 상위의 상위에 있는 workspace의 File은 D:\workspace가 될 것입니다. 이를 표현하는 방법에 따라 absolute path와 canonical path는 달라집니다. __absolute path는 현재 경로의 뒤에 내가 넣은 경로가 붙은 형태가 되고, canonical path는 실제로 그 상위 경로 기호("..")가 없어진 경로가 반환됩니다.__ 이렇게 표현되는 것 외에 큰 차이는 없습니다. ".."이 있어도 똑같을테니.. canonical path는 `symbolic link` 등도 참조가 가능하다고 합니다.

  ```
getPath: ..\..\workspace
getAbsolutePath: D:\workspace\Test\..\..\workspace
getCanonicalPath: D:\workspace
  ```

## try-with-resources

  자바를 이용해 외부 자원에 접근하는 경우 한가지 주의해야할 점은 외부자원을 사용한 뒤 제대로 자원을 닫아줘야 합니다.
  즉, InputStream혹은 OutputStream Writer Reader를 사용할 경우 close()로 자원을 닫아줘야 합니다.

  ```java
public static void main(String[] args) {
    FileInputStream fis = null;
    try{
         fis = new FileInputStream("");
    }catch(IOException e){
      throw new RuntimeException(e);
    }finally {
        fis.close();
    }
}
  ```

  이정도로 끝나면 좋겠지만 문제는 저 close() 메서드도 Checked Exception인 IOException 을 던지므로 이런 코드가 나타납니다.

  ```java
public static void main(String[] args) {
    FileInputStream fis = null;
    try{
         fis = new FileInputStream("");
    }catch(IOException e){
      throw new RuntimeException(e);
    }finally {
        try {
            fis.close();
        }catch (IOException ie){

        }
    }
}
  ```

  지금 코드에서 fis가 null일 확률은 없지만 try절에 로직이 더 추가되고 앞단에서 예외가 발생한다면 fis가 null일수도있으므로 그런 코드에는 분기문이 하나 더 추가됩니다.

  ```java
public static void main(String[] args) {
  FileInputStream fis = null;
  try{
    fis = new FileInputStream("");
  } catch(IOException e){
    throw new RuntimeException(e);
  } finally {
    try {
        if(fis != null) {
            fis.close();
        }
    }catch (IOException ie){
      throw new RuntimeException(e);
    }
  }
}
  ```

  이런 코드는 장황하고 보기힘들며, 이해하기 어렵고 중첩적입니다. 그리고 무엇보다 개발자가 실수하기 쉽습니다. 그래서 __JDK1.7에는 AutoCloseable 인터페이스가 추가됐습니다.__

  ```java
/**
 * @author Josh Bloch
 * @since 1.7
 */
public interface AutoCloseable {
    void close() throws Exception;
}
  ```

  인터페이스 추가와 함께 소소한 문법적 추가도 이뤄졌는데 try 절에 `()` 가 들어갈 수 있습니다.

  ```java
public static void main(String[] args) {
    try(FileInputStream fis = new FileInputStream("")){

    }catch(IOException e){
      throw new RuntimeException(e);
    }
}
  ```

  이런식으로 try(){} 형태로 사용이 가능하며 () 안에 들어올 수 있는건 `AutoCloseable` 구현체뿐입니다. 이런 문법을 `try with resources` 라고 부릅니다. 저렇게하면 무슨 이점이 있을까? 매우 직관적인 인터페이스 명칭이나 위 예제를 보고 눈치챌수도있겠지만 __try catch 절이 종료되면서 자동으로 close() 메서드를 호출해줍니다.__ 이 기능은 JDK 1.7 부터 지원합니다.

### 파일 덮어쓰기

  try-with-resources 구문을 사용하면, 위에서 Writer를 사용한 코드보다 훨씬 간결해 집니다.

  ```java
  // 파일이 존재할 경우 덮어쓰기 : false
  // 파일이 존재할 경우 이어쓰기 : true
  try (FileWriter fileWriter = new FileWriter(getFileFullPath(searchVo), false)) {
        fileWriter.write(searchVo.getContent());
    } catch (IOException e) {
        throw new RuntimeException();
  }
  ```

## Apache Commons IO

  > [Wikepedia. Apache Commons IO](https://ko.wikipedia.org/wiki/%EC%95%84%ED%8C%8C%EC%B9%98_%EC%BB%A4%EB%A8%BC%EC%A6%88)
  >
  > 아파치 커먼즈(Apache Commons)는 재사용 가능한 자바 기반의 컴포넌트를 모아놓은 통합 프로젝트이다.

### 텍스트 파일을 라인별로 읽어 List 컨테이너로 파싱하는 예제

  ```java
File file = new File("c:/text.txt");
try {
    List lines = FileUtils.readLines(file, "utf-8");
    Iterator it = lines.iterator();
    while(it.hasNext()) {
        System.out.println(it.next());
    }
} catch (IOException e) {
    e.printStackTrace();
}
  ```

  매우 직관적이고 심플합니다. 하지만 대용량의 텍스트 파일의 경우 모든 데이터를 읽어 하나의 리스트 컨테이너에 집어 넣으니 메모리 문제가 발생할 수 있다는 점을 염두해 둬야 합니다.

### 텍스트 파일을 구성하는 라인을 하나 하나 필요할때마다 읽어 처리할 수 있는 예제

  ```java
File file = new File("c:/text.txt");
LineIterator it = null;
try {
    it = FileUtils.lineIterator(file);
    while(it.hasNext()) {
        System.out.println(it.next());
    }
} catch (IOException e) {
    e.printStackTrace();
} finally {
    if(it != null) {
        LineIterator.closeQuietly(it);
    }
}
  ```

### 파일 경로를 정규화

  ```java
  String filename = "c:/commons/io/../../lang/../project.txt";
  String normailzed = FilenameUtils.normalize(filename);
  System.out.println(normailzed);

  // 결과 : c:/project.txt
  ```

### 해당 경로에 대한 저장 장치의 가용 용량을 얻는 코드

  ```java
try {
  long freeSpace = FileSystemUtils.freeSpaceKb("c:/");
  System.out.println(freeSpace + "kb");
} catch (IOException e) {
  e.printStackTrace();
}
  ```

### 파일 덮어쓰기

#### FileUtils

  Apache의 FileUtils를 사용하면 Writer만을 사용한 코드나, try-with-resources를 사용한 코드보다 훨신 간결해 집니다.

  ```java
  try {
    FileUtils.write(new File(getFileFullPath(searchVo)), searchVo.getContent(), "UTF-8");
  } catch (IOException e) {
      throw new RuntimeException();
  }
  ```

  __FileUtils.write(new File(파일경로), 파일에 추가할 내용, 인코딩)__ write() 메서드의 첫번째 파라미터로 File 객체가 들어가야 합니다.

#### FileCopyUtils

  > FileCopyUtils(내용(바이트배열), 원본파일객체)

  원본파일이 존재할 경우 덮어쓰고, 없는경우 새로 생성합니다.

  - MultipartFile을 사용하여, 업로드할 파일을 가져와서, 원본에 쓰는 코드

  ```java
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
  ```

### 파일 읽기

  - Line으로 읽기

  ```java
File file = new File("test.txt");
List<String> ls = FileUtils.readLines(file, "UTF-8");
  ```

  - byte array로 읽기

  ```java
File file = new File("test.exe");
byte[] contents = FileUtils.readFileToByteArray(file);
  ```

  - readFileToString

  ```java
File file = new File("test.txt");
String content= FileUtils.readFileToString(file);
  ```

### 파일 복사

  ```java
  File copy = new File(dir, "testFile1Copy.txt");
  File original = new File(dir, "testFile1.txt");
  // original을 copy로 복사
  FileUtils.copyFile(original, copy);
  FileUtils.forceDelete(testFile2);
  ```

### 파일 내용 긁어오기

  FileReader로 사용하는 방법이랑 FileUtils를 사용한 방법을 비교하겠습니다.

#### FileReader

```java
public List<String> getFileContents(FileManageHistoryVo historyVo) {
  List<String> fileContents = new ArrayList<>();
  FileReader fileReader = null;
  BufferedReader bufferedReader = null;

  try {
      fileReader = new FileReader(getFileFullPath(historyVo));
      bufferedReader = new BufferedReader(fileReader);
      String line = "";
      while ((line = bufferedReader.readLine()) != null) {            
        fileContents.add(line);
      }
  } catch (FileNotFoundException e) {
      throw new RuntimeException();
  } catch (IOException e1) {
      throw new RuntimeException();
  } finally {
      if (bufferedReader != null) {
          try {
              bufferedReader.close();
          } catch (IOException e) {
              throw new RuntimeException();
          }
      }
      if (fileReader != null) {
          try {
              fileReader.close();
          } catch (IOException e) {
              throw new RuntimeException();
          }
      }
  }
  return fileContents;
}
```

#### FileUtils

  ```java
  public String getFileContents(FileManageHistoryVo historyVo) {
      try {
          return FileUtils.readFileToString(getFileByPathToFile(historyVo.getPath()), Charset.forName("UTF-8"));
      } catch (IOException e) {
          throw new RuntimeException(e);
      }
  }
  ```
