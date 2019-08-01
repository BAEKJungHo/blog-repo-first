---
title: "Question Solving2"
layout: post
category: Java
tags: [Java]
excerpt: "자바에 관한 문제들을 풀어봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바공부를 위하여 여러 문제들을 풀어봅시다.

# 코딩 연습문제1.

  입력 : 190327-4

  출력 : 2019년 3월 27일 (수), 여자

  단, 입력값을 숫자로 바꿀 수 없다거나 이상한 문자열이 오면 값을 포맷에 맞게 올바르게
  입력할때까지 만든다. 메소드는 메인 메소드를 제외하고 최소 2개이상 더 만들어서 코딩한다.

  ```java
  import java.time.LocalDate;
  import java.time.format.DateTimeFormatter;
  import java.util.Scanner;
  import java.util.StringTokenizer;
  public class SsnExample {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);

		// While문 빠져나오기 위한 변수
		boolean run = true;

		StringBuilder sbDate = new StringBuilder();
		StringBuilder sbSex = new StringBuilder();

		// StringBuilder 안에 있는 문자열을 옮겨 담을 변수
		String date = null;
		String sex = null;

		// 성별판단을 위한 변수
		boolean isSex;

		while(run) {
			System.out.print("입력 : ");

			String ssn = scan.nextLine();
			StringTokenizer st = new StringTokenizer(ssn, "-");

			sbDate.append(st.nextToken());
			sbSex.append(st.nextToken());

			date = sbDate.toString();
			sex = sbSex.toString();

			// boolean
			run = inputError(date, sex);
			isSex = genderCheck(sex);

			if((run == true) || (isSex == true)) {
				System.out.println("다시 입력하세요 !!");

				// StringBuilder 비우기
				sbDate.delete(0, date.length());
				sbSex.delete(0, sex.length());
				continue;
			}
		}

		// 맨앞 인덱스에 "20"추가해서 4자리 연도수 만들기
		sbDate.insert(0, "20");
		date = sbDate.toString();

		// 연도, 월, 일
		String year = date.substring(0, 4);
		String month = date.substring(4, 6);
		String day = date.substring(6, 8);

		// Java.time 패키지 사용
		LocalDate targetDate = LocalDate.of(Integer.valueOf(year), Integer.valueOf(month), Integer.valueOf(day));
		DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy년 M월 d일 (E)");
		String calender = targetDate.format(dateTimeFormatter);

		// 성별
		String gender = "";

		if((Integer.valueOf(sex) == 2) || (Integer.valueOf(sex) == 4)) {
			gender = "여자";
		}
		else if((Integer.valueOf(sex) == 1) || (Integer.valueOf(sex) == 3)) {
			gender = "남자";
		}

		System.out.println("출력 : " + calender + ", " + gender);

		scan.close();
	}

	// 숫자로 변환 가능한지 판별하는 메소드
	public static boolean isStringInt(String s) {
	    try {
	        Integer.parseInt(s);
	        return true;
	    } catch (NumberFormatException e) {
	        return false;
	    }
	  }

	// 길이와 숫자변환이 안되는 경우
	public static boolean inputError(String d, String s) {
		// 입력된 길이 판별
		if((d.length() != 6) || (s.length() != 1)) {
			return true;
		}
		// 숫자변환가능여부 메소드 호출
		else if((isStringInt(d) != true) || (isStringInt(s) != true)) {
				return true;
		}
		// 조건성립
		else if((d.length() == 6) || (s.length() == 1)){
			if((isStringInt(d) == true) || (isStringInt(s) == true)) {
				return false;
			 }
		}
		return false;
	}

	// 숫자는 맞지만 성별(1~4)이 다른경우
	public static boolean genderCheck(String g) {
		if(isStringInt(g) == true) {
			if((Integer.valueOf(g) >= 1) && Integer.valueOf(g) < 5) {
				return false;
			}
		} else {
			return true;
		}
		return false;
	 }
  }
  ```

# 코딩 연습문제2.

  다음 코딩 문제는 카카오 입사 문제로 나왔던 문제입니다.

  - 조건

    ![dart1](/images/posts/201904/dart1.jpg)

  - 예시

    ![dart2](/images/posts/201904/dart2.jpg)

  ```java
  public class Ex01Dart {
	public int number;
	public char area;
	public boolean optionStar;
	public boolean optionAcha;
	public int score;

	@Override
	public String toString() {
		return "Dart [number=" + number + ", area=" + area + ", optionStar=" + optionStar + ", optionAcha="
				+ optionAcha + ", score=" + score + "]";
	 }
  }

  import java.util.Scanner;
  import java.util.StringTokenizer;

  public class Ex01DartApp {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.print("Dart 게임 입력> ");
		String game = scan.nextLine();
		System.out.println("획득한 점수 = " + dartResult(parseDart(game)));
		scan.close();
	}

	static int dartResult(Ex01Dart[] dart) {
		int score = 0;
		for (int i=0; i<3; i++) {
			switch (dart[i].area) {
			case 'S':
				dart[i].score = dart[i].number;
				break;
			case 'D':
				dart[i].score = dart[i].number * dart[i].number;
				break;
			case 'T':
				dart[i].score = dart[i].number * dart[i].number * dart[i].number;
			}
		}
		for (int i=0; i<3; i++) {
			if (dart[i].optionStar) {
				dart[i].score *= 2;
				if (i != 0) {
					dart[i-1].score *= 2;
				}
			}
			if (dart[i].optionAcha)
				dart[i].score *= -1;
		}
		score = dart[0].score + dart[1].score + dart[2].score;
		return score;
	}

	static Ex01Dart[] parseDart(String str) {
		Ex01Dart[] dart = new Ex01Dart[3];
		StringTokenizer st = null;
		String s; String number = null;
		int index = 0;
		int len = str.length();

		for (int i=0; i<3; i++) {
			dart[i] = new Ex01Dart();
			dart[i].optionStar = false;
			dart[i].optionAcha = false;
			s = str.substring(index);
			//System.out.println(s);

			st = new StringTokenizer(s, "SDT");
			dart[i].number = Integer.parseInt(st.nextToken());
			index += (dart[i].number == 10) ? 2 : 1;
			dart[i].area = str.charAt(index);
			index++;
			if (index < len) {
				if (str.charAt(index) == '*')
					dart[i].optionStar = true;
				if (str.charAt(index) == '#')
					dart[i].optionAcha = true;
				if (dart[i].optionStar || dart[i].optionAcha)
					index++;
			}
			//System.out.println(i + dart[i].toString());
		}
		return dart;
	 }
  }
  ```

  본 풀이는 강의 중 교수님께서 푼 방식입니다.

# 코딩 연습문제2.

  - 조건

    ![prac1](/images/posts/201904/prac1.jpg)

  ```java
  public class Member implements Comparable<Member> {

	private String name;
	private String id;
	private String password;
	private int age;

	public Member(String name, String id, String password, int age) {
		super();
		this.name = name;
		this.id = id;
		this.password = password;
		this.age = age;
	}

	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}

    @Override
    public int compareTo(Member o){
      // 문자열 크기비교 String클래스의 메소드 a.compareTo(b) > 0 사용
      if(this.age > o.age) return 1;
      else if(this.age == o.age) return 0;
      else return -1;
    }
  }

  public class MemberExample {
	public static void main(String[] args) {
		Member john = new Member("john", "jh", "jh123", 26);
		Member mike = new Member("mike", "mk", "mk123", 25);

		System.out.println(john.compareTo(mike));
	 }
  }
  ```

# 코딩 연습문제3.

  - 조건

    남은 퇴근 시간을 계산하는 프로그램을 작성하시오.

    입력: 현재 시간을 hh:mm:ss 형식으로 입력.

    출력: 퇴근 시간을 18:30:00 으로 가정하고 퇴근 시간까지 남은 시간을 출력.

  ```java
  package Openchallenge;

  import java.text.ParseException;
  import java.text.SimpleDateFormat;
  import java.util.Date;
  import java.util.Scanner;
  public class GetLiveWorkTimeExample {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.print("시간을 입력하세요(HH:mm:ss) : ");
		String currentTime = scan.nextLine();

		try {
			SimpleDateFormat format1 = new SimpleDateFormat("HH:mm:ss");

			Date date1 = format1.parse("18:30:00");
			Date date2 = format1.parse(currentTime);
			long diff = date1.getTime() - date2.getTime();

			String time1 = format1.format(diff-3600*1000*9);
			System.out.println("퇴근까지 남은 시간 : " + time1);
		} catch (ParseException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	 }
  }
  ```

# 코딩 연습문제4.

  - 조건

    ![sol3](/images/posts/201904/sol3.jpg)

  - Collection FrameWork 사용 X

  ```java
  import java.util.Arrays;
  import java.util.StringTokenizer;

  public class NotUsedCollectionExample {

	public static void main(String[] args) {
		String name = "이유덕,이재영,권종표,이재영,박민호,강상희,이재영,김지완,최승혁,이성연,박영서,"
				+ "박민호,전경헌,송정환,김재성,이유덕,이재영,전경헌";

		eliDuplicatedString(name);
	}

	public static void eliDuplicatedString(String argName) {
		int countLee = 0;
		int countKim = 0;
		int countLeeJaeYoung = 0;
		int duplicatedCount = 0;

		StringTokenizer st = new StringTokenizer(argName, ",");
		int countToken = st.countTokens();

		String[] names = new String[countToken];
		String[] notDuplicatedName;

		System.out.print("초기 : ");
		for(int i=0; i<countToken; i++) {
			names[i] = st.nextToken();
			System.out.print(" "+ names[i]);
		}
		System.out.println();

		for(int i=0; i<names.length; i++) {
			// 이씨 김씨 구하기
			if(names[i].substring(0, 1).equals("이")) {
				countLee++;
			} else if(names[i].substring(0, 1).equals("김")) {
				countKim++;
			}

			// 이재영 찾기
			if(names[i].equals("이재영")) {
				countLeeJaeYoung++;
			}
		}

		System.out.println("이씨 : " + countLee + "명");
		System.out.println("김씨 : " + countKim + "명");
		System.out.println("이재영 중복 횟수 : " + countLeeJaeYoung);

		// 중복된 이름 제거
		for(int i=0; i<names.length-1; i++) {
			for(int k=i+1; k<names.length; k++) {
				if(names[i].equals(names[k]) && !names[i].equals("Duplicated") && !names[k].equals("Duplicated")) {
					names[k] = "Duplicated";
					duplicatedCount++;
				}
			}
		}

		int p = 0;
		notDuplicatedName = new String[names.length-duplicatedCount];
		// 중복안된 이름 배열에 저장
		for(int i=0; i<names.length; i++) {
			if(!names[i].equals("Duplicated")) {
				notDuplicatedName[p] = names[i];
				p++;
			}
		}

		System.out.println("중복된 이름 제거 : " + Arrays.toString(notDuplicatedName));

		// 중복 제거한 이름 오름 차순 정렬
		for(int i=0; i<notDuplicatedName.length; i++) {
			String temp;
			for(int k=0; k<notDuplicatedName.length-1; k++) {
				if(notDuplicatedName[k].compareTo(notDuplicatedName[k+1]) > 0) {
					temp = notDuplicatedName[k];
					notDuplicatedName[k] = notDuplicatedName[k+1];
					notDuplicatedName[k+1] = temp;
				}
			}
		}
		System.out.println("오름차순 정렬 : " + Arrays.toString(notDuplicatedName));
	 }
  }
  ```

  - Collection FrameWork 사용 O

  ```java
  import java.util.ArrayList;
  import java.util.Iterator;
  import java.util.List;
  import java.util.StringTokenizer;
  import java.util.TreeSet;

  public class UsedCollectionExample {

	public static void main(String[] args) {

		String name = "이유덕,이재영,권종표,이재영,박민호,강상희,이재영,김지완,최승혁,이성연,박영서,"
				+ "박민호,전경헌,송정환,김재성,이유덕,이재영,전경헌";

		List<String> list = new ArrayList<>();
		TreeSet treeSet = new TreeSet();

		int countLee = 0;
		int countKim = 0;
		int countLeeJaeYoung = 0;

		StringTokenizer st = new StringTokenizer(name, ",");
		int countToken = st.countTokens();

		// 리스트에 문자열 넣고, treeSet에 List넣기
		for(int i=0; i<countToken; i++) {
			list.add(st.nextToken());
			treeSet.addAll(list);
		}

    Iterator iterator = treeSet.iterator();
    System.out.print("중복 제거 및 오름차순정렬 : ");
    while (iterator.hasNext()) {
      System.out.print(iterator.next() + " ");
    }
    System.out.println();

    for(int i=0; i<list.size(); i++) {
		if(list.get(i).substring(0, 1).equals("이")) {
			countLee++;
		} else if(list.get(i).substring(0, 1).equals("김")) {
			countKim++;
		}
		if(list.get(i).equals("이재영")) {
			countLeeJaeYoung++;
		}
    }

		System.out.println("이씨 : " + countLee + "명");
		System.out.println("김씨 : " + countKim + "명");
		System.out.println("이재영 중복 횟수 : " + countLeeJaeYoung);
	 }
  }
  ```

# 코딩 연습문제5.

  - 조건

    ![sol4](/images/posts/201904/sol4.jpg)

  - Enum

  ```java
  public enum Position {
	부장, 차장, 과장, 대리, 사원
	// 내부적으로 인덱스를 가지는데 선언한 순서대로 0, 1, 2, 3, 4를 가진다
  // 뒤에 나열된 값이 더 크다고 판단된다.
  }
  ```

  - Employee

  ```java
  import java.time.LocalDate;

  public class Employee {

	public int empNo;
	public String name;
	public Position position;
	public LocalDate date;

	public Employee(int empNo, String name, Position position, LocalDate date) {
		super();
		this.empNo = empNo;
		this.name = name;
		this.position = position;
		this.date = date;
	 }
  }
  ```

  - Apllication

  ```java
  import java.time.LocalDate;
  import java.util.*;

  public class EmployeeManageMentApplication {

	public static void main(String[] args) {
    // 익명구현객체로 Comparator 구현
		TreeSet<Employee> treeSet = new TreeSet<Employee>(new Comparator<Employee>() {
			@Override
			public int compare(Employee e1, Employee e2) {
				if(e1.position.compareTo(e2.position) > 0) return 1;
				else if(e1.position.compareTo(e2.position) < 0) return -1;
				else {
					return e1.date.compareTo(e2.date);
				}
			}
		});

		treeSet.add(new Employee(1, "백부장", Position.부장, LocalDate.of(2018, 01, 10)));
		treeSet.add(new Employee(2, "최부장", Position.부장, LocalDate.of(2018, 05, 10)));
		treeSet.add(new Employee(3, "박차장", Position.차장, LocalDate.of(2018, 07, 10)));
		treeSet.add(new Employee(4, "김차장", Position.차장, LocalDate.of(2018, 11, 10)));
		treeSet.add(new Employee(5, "황과장", Position.과장, LocalDate.of(2018, 12, 10)));
		treeSet.add(new Employee(6, "주과장", Position.과장, LocalDate.of(2019, 01, 10)));
		treeSet.add(new Employee(7, "이대리", Position.대리, LocalDate.of(2019, 02, 10)));
		treeSet.add(new Employee(8, "차대리", Position.대리, LocalDate.of(2019, 03, 10)));
		treeSet.add(new Employee(9, "유사원", Position.사원, LocalDate.of(2019, 04, 10)));
		treeSet.add(new Employee(10, "정사원", Position.사원, LocalDate.of(2019, 04, 11)));

		for(Employee employee : treeSet) {
			System.out.println(employee.name + " - " + employee.position + " - " + employee.date);
		}
	 }
  }
  ```

  LocalDate 사용은 다음과 같이 할 수 있습니다.

  1. LocalDate currentDate = LocalDate.now();
  2. LocalDate myDate = LocalDate.of(int year, int month, int datOfMonth);

# 코딩 연습문제6.

  - 조건

    ![grep](/images/posts/201904/grep.jpg)

  - GrepApplication

  ```java
  import java.io.*;
  import java.util.Scanner;

  public class GrepApplication {
	public static void main(String[] args) throws Exception {

		Scanner scan = new Scanner(System.in);
		System.out.print("찾을 문자열 : ");
		String findStr = scan.nextLine();
		System.out.println("찾을 파일명 :");
		String filePath = scan.nextLine();

		FileReader fr = new FileReader(filePath);
		BufferedReader br = new BufferedReader(fr);


		String data;
		int lineNo = 0;
		while((data=br.readLine()) != null) {
			if(data.indexOf(findStr) > -1)
				System.out.println(++lineNo + ": " + data);
			else
				continue;
		}
		br.close();
	 }
  }
  ```

# 코딩 연습문제7.

  - 조건

    ![p1](/images/posts/201904/p1.jpg)

  - 풀이

  ```java
  import java.util.*;

  public class MarathonApplication {
	public static void main(String[] args) {
		List<String> participants = new ArrayList<String>(); // 참가자

		participants.add("홍길동"); participants.add("홍길동"); participants.add("홍길동");
		participants.add("손오공"); participants.add("저팔계"); participants.add("사오정");
		participants.add("손오공"); participants.add("신용재"); participants.add("신용재");
		participants.add("윤민수"); participants.add("류재현"); participants.add("양다일");

		System.out.print("참가자 : ");
		for(String str : participants) {
			System.out.print(" " + str);
		}
		System.out.println();

		complete(participants);

	}

	public static void complete(List<String> list) {
		Random random = new Random();
		int fail = random.nextInt(12)+1;
		List<String> completioners = new ArrayList<String>();
		// List 복사를 위해 addAll()사용
		completioners.addAll(list);
		System.out.println("미완주 : " + completioners.get(fail));
		completioners.remove(fail);

		System.out.print("완주 : ");
		for(String str : completioners) {
			System.out.print(" " + str);
		}
	 }
  }
  ```

# 코딩 연습문제8.

  - 조건

    ![p2](/images/posts/201904/p2.jpg)

  - 풀이1.

  ```java
  import java.io.*;
  import java.util.*;
  import java.util.regex.Pattern;

  public class CsvApplication {
	public static void main(String[] args) {
		String filePath = "D:\\Workspace\\Egovernment\\Java\\practice\\src\\addrInput.txt";

		List<String> name = new ArrayList<String>();
		List<String> email = new ArrayList<String>();
		List<String> phone = new ArrayList<String>();

		StringTokenizer st = null;

		Scanner scan = new Scanner(System.in);
		String yn;

		int i = 0;

		try {
			System.out.print("정보를 입력 하시겠습니까?(Y/N) : ");
			yn = scan.nextLine();
			if(yn.equals("Y")) { inputMember(); }

			BufferedReader br = new BufferedReader(new FileReader(filePath));
            String line = "";

            while((line = br.readLine()) != null) { // 한 줄씩 읽기
                System.out.println(line);
                st = new StringTokenizer(line, ",");
                while(st.hasMoreTokens()) {
                	try {
                   	name.add(i, st.nextToken());
                   	email.add(i, st.nextToken());
                	phone.add(i, st.nextToken());
                	} catch(NoSuchElementException e) {
                       	name.add(i, " ");
                    	email.add(i, " ");
                    	phone.add(i, " ");
                	}
                	i++;
                }
            }
            eliDuplicated(name, phone, email);
            br.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
		try {
		BufferedReader br = new BufferedReader(new FileReader(filePath));
        String line = "";
        System.out.println("=====================중복 제거 및 정렬 후======================");
        while((line = br.readLine()) != null) { // 한 줄씩 읽기
            System.out.println(line);
        }
		} catch(Exception e) { }
	}

	public static void eliDuplicated(List<String> name, List<String> phone, List<String> email) throws Exception {
		System.out.println("==================중복제거 함수 실행===================");
		String filePath = "D:\\Workspace\\Egovernment\\Java\\practice\\src\\addrInput.txt";

		for(int i=0; i<name.size()-1; i++) {
			for(int k=i+1; k<name.size(); k++) {
				if(name.get(i).equals(name.get(k)) && phone.get(i).equals(phone.get(k))) { // 이름과 전화번호가 같으면 중복
					System.out.println("중복이름 : " + name.get(i));

					System.out.println("중복이름에 해당되는 Email : " + email.get(i) + " & " + email.get(k));
				    String input = "Please delete email or id : ";
				    System.out.println(input);

				    BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
				    String word = reader.readLine();
				    reader.close();

				    System.out.println("Remove: " + word);
				    String wordToRemove = word;


				    StringBuffer newContent = new StringBuffer();

					TreeSet treeSet = new TreeSet();

				    // 한 줄씩 읽어서 입력한 값이 그 줄에 포함되어있는지 확인
				    BufferedReader br = new BufferedReader(new FileReader(filePath));
			      	while ((word = br.readLine()) != null) {
			      		if (!word.trim().contains(wordToRemove)) { // 포함되어 있지 않으면 treeSet에 넣어 정렬
			      			treeSet.add(word);
			      		}
			      	}
			      	// 트리셋이 비어있는지 확인하면서 버퍼에 추가
			      	while(!treeSet.isEmpty()) {
		      			newContent.append(treeSet.pollFirst());
		      			newContent.append("\r\n"); // new line
			      	}
				    br.close();

			      	FileWriter removeLine = new FileWriter(filePath);
			      	BufferedWriter change = new BufferedWriter(removeLine);
			      	PrintWriter replace = new PrintWriter(change);
			      	replace.write(newContent.toString());
			      	replace.close();
				}
			}
		}
	}

	public static void inputMember() throws Exception {
		String filePath = "D:\\Workspace\\Egovernment\\Java\\practice\\src\\addrInput.txt";
		boolean run = true;
		Scanner scan = new Scanner(System.in);
		List<String> newName = new ArrayList<String>();
		List<String> newEmail = new ArrayList<String>();
		List<String> newPhone = new ArrayList<String>();

		while(run) {
			System.out.println("==================입력 함수 실행===================");
			System.out.println("=================1. 입력 2. 종료===================");
			int number = Integer.parseInt(scan.nextLine());
			switch(number) {
			case 1:
				System.out.print("이름을 입력하세요 : ");
				newName.add(scan.nextLine());
				System.out.print("이메일을 입력하세요 : ");
				newEmail.add(scan.nextLine());
				System.out.print("전화번호을 입력하세요 : ");
				newPhone.add(scan.nextLine());
				break;
			case 2:
				run = false;
				break;
			}
		}

		FileWriter fw = new FileWriter(filePath ,true); // 기존에 추가하여 작성(true)

		for(int i=0; i<newName.size(); i++) {
			String str = newName.get(i)+","+newEmail.get(i)+","+newPhone.get(i);
			fw.write("\r\n");
			fw.write(str);
		}
		fw.close();
	 }
  }
  ```

  - 풀이2.

  ```java
  import java.util.Objects;

  public class Member implements Comparable<Member> {
	private String name;
	private String mail;
	private String tel;

	public String getName() { return name; }
	public String getMail() { return mail; }
	public String getTel() { return tel; }
	public void setName(String name) { this.name = name; }
	public void setMail(String mail) {
		String mailRegExp = "^([a-z0-9_\\.-]+)@([\\da-z\\.-]+)\\.([a-z\\.]{2,6})$";
		if (mail.matches(mailRegExp))
			this.mail = mail;
		else
			this.mail = "";
	}
	public void setTel(String tel) {
		String telRegExp = "^01(?:0|1|[6-9])-(?:\\d{3}|\\d{4})-\\d{4}$";
		if (tel.matches(telRegExp))
			this.tel = tel;
		else
			this.tel = "";
	}

	@Override
	public int compareTo(Member m) {
		if (name.compareTo(m.getName()) > 0) return 1;
		if (name.compareTo(m.getName()) < 0) return -1;
		return tel.compareTo(m.getTel());
	}
	@Override
	public String toString() {
		return "Member [name=" + name + ", mail=" + mail + ", tel=" + tel + "]";
	 }
  }

  import java.io.*;
  import java.util.*;
  import java.util.regex.Pattern;

  import java.io.BufferedReader;
  import java.io.FileReader;
  import java.util.StringTokenizer;
  import java.util.TreeSet;

  public class CsvApplication {
	public static void main(String[] args) throws Exception {
		FileReader fr = new FileReader("c:/Temp/addrInput.txt");
		BufferedReader br = new BufferedReader(fr);
		TreeSet<Member> memberSet = new TreeSet<Member>();
		String line = null;

		while ((line = br.readLine()) != null) {
			System.out.println(line);
			Member member = new Member();
			StringTokenizer st = new StringTokenizer(line, ",");
			member.setName(st.nextToken().trim());
			member.setMail(st.nextToken().trim());
			member.setTel(st.nextToken().trim());
			memberSet.add(member);
		}
		System.out.println();
		for (Member member: memberSet)
			System.out.println(member);
		br.close();
		fr.close();
	 }
  }
  ```

# 코딩 연습문제9.

  - 조건

    ![p3](/images/posts/201904/p3.jpg)

  - 풀이

  ```java
  public class Word implements Comparable<Word> {
	private String word;
	private int count;

	public Word() { }
	public Word(String word, int count) {
		this.word = word;
		this.count = count;
	}
	public String getWord() {
		return word;
	}
	public void setWord(String word) {
		this.word = word;
	}
	public int getCount() {
		return count;
	}
	public void setCount(int count) {
		this.count = count;
	}
	@Override
	public int compareTo(Word w) {
		if (count < w.getCount()) return 1;
		if (count > w.getCount()) return -1;
		return word.compareTo(w.getWord());
	}
	@Override
	public String toString() {
		return "[word=" + word + ", count=" + count + "]";
	 }
  }

  import java.io.BufferedReader;
  import java.io.FileReader;
  import java.util.HashMap;
  import java.util.Map;
  import java.util.Scanner;
  import java.util.Set;
  import java.util.StringTokenizer;
  import java.util.TreeSet;

  public class Ex03WordCount {
	public static void main(String[] args) throws Exception {
		Scanner scan = new Scanner(System.in);
		System.out.print("조회할 파일명: ");
		String filePath = scan.nextLine();
		scan.close();

		FileReader fr = new FileReader(filePath);
		BufferedReader br = new BufferedReader(fr);
		String line = null;
		HashMap<String, Integer> wordMap = new HashMap<String, Integer>();

		// 파일에서 읽어서 wordMap에 저장
		int count = 0;
		while ((line = br.readLine()) != null) {
			StringTokenizer st = new StringTokenizer(line, ",. ?!()");
			while (st.hasMoreTokens()) {
				count++;
				String word = st.nextToken();
				if (wordMap.containsKey(word)) {
					int val = wordMap.get(word);
					wordMap.put(word, val+1);
				} else {
					wordMap.put(word, 1);
				}
			}
		}
		System.out.println("총 단어수 = " + count);
		// wordMap에 저장되어 있는 데이터를 Top 10을 뽑기 위해 wordSet(TreeSet)에 저장
		count = 0;
		TreeSet<Ex03Word> wordSet = new TreeSet<Ex03Word>();
		Set<Map.Entry<String,Integer>> wordMapSet = wordMap.entrySet();
		for (Map.Entry<String,Integer> entry : wordMapSet) {
			wordSet.add(new Ex03Word(entry.getKey(), entry.getValue()));
			count++;
		}
		System.out.println("총 단어 종류 = " + count);

		// wordSet 내용중 10개만 출력
		count = 1;
		for (Ex03Word word: wordSet) {
			System.out.println(word.toString());
			if (count++ == 10)
				break;
		}
		br.close();
		fr.close();
	}
  }
  ```

# 코딩 연습문제10.

  - 조건

    ![p4](/images/posts/201904/p4.jpg)

  - 풀이

  ```java
  import java.io.File;
  import java.io.FileOutputStream;

  public class Ex04RandomFile {
	public static void main(String[] args) throws Exception {
		mkdirs();
		String fileName = null;
		for (int i=0; i<100; i++) {
			int numFile = (int)(Math.random() * 9000) + 1000;
			int numContent = (int)(Math.random() * 3) + 1;
			if (numFile <= 3333) {
				fileName = "c:/Temp/Ex04/low/" + numContent + "/" + numFile + ".txt";
			} else if (numFile <= 6666) {
				fileName = "c:/Temp/Ex04/mid/" + numContent + "/" + numFile + ".txt";
			} else {
				fileName = "c:/Temp/Ex04/high/" + numContent + "/" + numFile + ".txt";
			}
			FileOutputStream fos = new FileOutputStream(fileName);
			int content = (int)'0' + numContent;
			fos.write(content);
			fos.flush();
			fos.close();
		}
	}
	static void mkdirs() {
		File file = new File("c:/Temp/Ex04/low/1");
		file.mkdirs();
		file = new File("c:/Temp/Ex04/low/2");
		file.mkdirs();
		file = new File("c:/Temp/Ex04/low/3");
		file.mkdirs();
		file = new File("c:/Temp/Ex04/mid/1");
		file.mkdirs();
		file = new File("c:/Temp/Ex04/mid/2");
		file.mkdirs();
		file = new File("c:/Temp/Ex04/mid/3");
		file.mkdirs();
		file = new File("c:/Temp/Ex04/high/1");
		file.mkdirs();
		file = new File("c:/Temp/Ex04/high/2");
		file.mkdirs();
		file = new File("c:/Temp/Ex04/high/3");
		file.mkdirs();
	 }
  }
  ```
