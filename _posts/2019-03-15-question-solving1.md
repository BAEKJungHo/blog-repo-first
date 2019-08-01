---
title: "Question Solving1"
layout: post
category: Java
tags: [Java]
excerpt: "자바에 관한 문제들을 풀어봅시다."
author: BAEKJungHo
---

* content
{:toc}

자바공부를 위하여 여러 문제들을 풀어봅시다.

# 반복문을 사용한 문제1.

  ref) 이것이 자바다 127p 예제 응용

  키 값을 입력받고 증속은 속도 1씩 증가 감속은 1씩감소, 급가속은 10증가, 급감속은 10감소를 나타내는 프로그램

  ```java
  public class WhileKeyControlExample {
	public static void main(String[] args) throws Exception {
		int speed = 0;
		int keyCode = 0;

		while(true) {
			if(keyCode!=13 && keyCode!=10) {
				System.out.println("----------------------------------------------------");
				System.out.println("1.증속 | 2. 감속 | 3. 급가속 | 4. 급감속 | 5. 중지 ");
				System.out.println("-----------------------------------------------------");
				System.out.println("선택 : ");
			}

			keyCode = System.in.read();

			if(keyCode == '1') {
				speed++;
			}
			else if(keyCode == '2') {
				if(speed > 0)
					speed--;
				else
					continue;
			}
			else if(keyCode == '3') {
				speed += 10;
			}
			else if(keyCode == '4') {
				if(speed >= 10) {
					speed -= 10;
				}
				else
					continue;
			}
			else if(keyCode == '5') {
				break;
			}
			System.out.println("현재속도 = " + speed);
		 }
		System.out.println("프로그램 종료");
  	}
  }
  ```

# 반복문을 사용한 문제2.

  ref) 이것이 자바다 4장 확인문제 7번

  while문과 Scanner를 이용해서 키보드로부터 입력된 데이터로 예금, 출금, 조회, 종료 기능을 제공하는 코드를 작성해보시오.

  ```java
  import java.util.Scanner;
  public class Exercise07 {
	public static void main(String[] args) throws Exception{
		boolean run = true;

		int balance = 0;
		int num = 0;
		String str = ""; // 번호문자열을 저장할 String 객체

		Scanner sc = new Scanner(System.in);

		while(run) {
			System.out.println("----------------------------------");
			System.out.println("1.예금 | 2.출금 | 3.잔고 | 4.종료");
			System.out.println("----------------------------------");
			System.out.print("선택>");

			str = sc.nextLine();

      // 문자열 비교는 equals() 클래스 메소드 사용
			if(str.equals("1")) {
				System.out.print("예금액>");
				num = Integer.parseInt(sc.nextLine());
				balance += num;
				System.out.println("현재 잔고 : " + balance);
			}
			else if(str.equals("2")) {
				System.out.print("출금액>");
				num = Integer.parseInt(sc.nextLine());
				if(num > balance) {
					System.out.println("잔고보다 출금액이 많습니다.");
					continue;
				}
				else balance -= num;

				System.out.println("현재 잔고 : " + balance);
			}
			else if(str.equals("3")) {
				System.out.println("잔고>" + balance);
			}
			else if(str.equals("4")) {
				run = false;
			}

		}
		System.out.println("exit");
		sc.close();
	 }
  }
  ```

  > 이 문제를 풀면서 알게 된 점은 입력값을 nextInt()로 받게 되면, 지속적으로 값을 입력받게 될 경우
  > 버벅거리는 현상이 발생합니다(렉의 일종).
  >
  > 이러한 현상을 해결하기 위해서 num = Integer.parseInt(sc.nextLine()); 사용
  >
  > nextLine()을 이용하여 값을 입력 받게 될 경우 버벅거리던 문제가 해결 되었습니다.
  >
  > 문자열을 정수로 변환하기 위해 Integer.parseInt()를 사용했습니다.
  >
  > 한가지 더 문제가 있었는데 if문이나 else if문의 조건식안에 sc.nextLine().equals("숫자")로
  > 비교를 하게 될 경우 버벅거리는 문제가 또 발생하였습니다.
  >
  > 이 문제를 해결하기 위하여 String 객체 str을 만들어 문자열을 입력받고
  >
  > str.euqals("숫자")로 비교 해줬더니 해결되었습니다.

# 배열을 사용한 문제1.

  ref) 이것이 자바다 5장 확인문제 8번 응용

  주어진 배열의 전체 항목의 합과, 평균, 최대값, 최소값, 분산을 구하여라.

  ```java
  public class Exercise08 {
	public static void main(String[] args) {
		int[][] array = {
				{95, 86},
				{83, 92, 96},
				{78, 83, 93, 87, 88}
		};

		int sum = 0;
		int max = array[0][0], min = array[0][0];
		double varSub = 0.0;
		double avg = 0.0, variance = 0.0;

		for(int i=0; i<array[1].length; i++) {
			for(int k=0; k<array[i].length; k++) {
				// 합 구하기
				sum += array[i][k];

			}
		}

		// 평균 구하기
		avg = sum/(array[0].length + array[1].length + array[2].length);

		for(int i=0; i<array[1].length; i++) {
			for(int k=0; k<array[i].length; k++) {
				// 분산의 분모 구하기
        // 표준편차의 제곱
        // 표준 편차 평균과 각 값의 차이
				varSub += (array[i][k] - avg) * (array[i][k] - avg);

			}
		}

    // 분산 구하기
		variance = varSub/(array[0].length + array[1].length + array[2].length);


		for(int i=0; i<array[1].length; i++) {
			for(int k=0; k<array[i].length-1; k++) {
				// 최댓값, 최솟값 구하기
				if(array[i][k+1]> max) {
					max = array[i][k+1];
				}
				else if(array[i][k] < min) {
					min = array[i][k];
				}

			}
		}

		System.out.println("SUM : " + sum);
		System.out.println("AVG : " + avg);
		System.out.println("MAX : " + max);
		System.out.println("MIN : " + min);
		System.out.println("VAR : " + variance);
	 }
  }
  ```

# Matrix Multiplication

  배열을 사용하여 ex) {2, 3} x {3, 2} = {2, 2}의 배열 만들기.

  VERSON1은 제가 한 것이고, VERSION2는 강사님께서 작성하신 방법 입니다.

  - VERSION 1.

  ```java
  import java.util.Scanner;
  public class OpenChallenge {
	public static void main(String[] args) {
		int row, col; // 행, 열
		int leftCount = 0, rightCount = 0; // 카운트

		Scanner scan = new Scanner(System.in);
		System.out.print("행을 입력하세요");
		row = Integer.parseInt(scan.nextLine());

		System.out.print("열을 입력하세요");
		col = Integer.parseInt(scan.nextLine());

    // Matrix 생성
		int[][] left = new int[row][col];
		int[][] right = new int[col][row];
		int[][] merge = null;

		// left Array Input value
		for(int i=0; i<row; i++) {
			for(int k=0; k<col; k++) {
				System.out.println(++leftCount +"번째 left array값을 입력하세요");
				left[i][k] = Integer.parseInt(scan.nextLine());
			}
		}

		// right Array Input value
		for(int i=0; i<col; i++) {
			for(int k=0; k<row; k++) {
				System.out.println(++rightCount +"번째 right array값을 입력하세요");
				right[i][k] = Integer.parseInt(scan.nextLine());
			}
		}

		merge = new int[row][col]; // merge 크기

		// Matrix Multiplication
		for(int i=0; i<row; i++) {
			for(int k=0; k<row; k++) {
				int sum = 0;
				for(int p=0; p<col; p++) {
					sum += left[i][p]*right[p][k];
				}
				merge[i][k] = sum;
			}
		}

		for(int i=0; i<row; i++) {
			for(int k=0; k<row; k++) {
				System.out.print(merge[i][k]);
				System.out.println();
			}
		 }
	  }
  }
  ```

  - VERSION 2.

  ```java
  import java.util.Arrays;

  public class MatrixMultiplication {
	public static void main(String[] args) {
		int[][] x = { {1, 2, 3, 4}, {5, 6, 7, 8} };
		int[][] y = { {1, 2}, {3, 4}, {5, 6}, {7, 8} };
		matPrint(x); matPrint(y);
		int[][] result = matMul(x, y);
		if (result != null)
			matPrint(result);
		else
			System.out.println("x 행렬의 열수와 y 행렬의 행수가 같아야 함");
	}

	public static int[][] matMul(int[][] x, int[][] y) {
		if (x[0].length != y.length)
			return null;
		int row = x.length;
		int col = y[0].length;
		int size = y.length;
		int[][] result = new int[row][col];

		for (int r=0; r < row; r++) {
			for (int c=0; c < col; c++) {
				int sum = 0;
				for (int i=0; i < size; i++) {
					sum += x[r][i] * y[i][c];
				}
				result[r][c] = sum;
			}
		}

		return result;
	}

	public static void matPrint(int[][] mat) {
		for(int[] matRow: mat)
			System.out.println(Arrays.toString(matRow));
	 }
 }
 ```

# 클래스와 객체 문제1.

  1. 다음의 필드를 갖는 StudentScore Class를 작성하시오 /
     성명, 수학, 영어, 과학, 평균
  2. 성명과 수학, 영어, 과학 점수를 매개변수로 하는 생성자를 작성하시오.
  3. toString() 메소드를 만드시오.
  4. 평균을 구하는 메소드(double average())를 만드시오.
     단, 이 메소드는 평균값을 리턴도 하여야 하고, 필드의 평균값도 고쳐야 함.
  5. 이름과 수학, 영어, 과학 성적을 입력으로 받아서 이름과 수학, 영어, 과학, 평균을 출력하는 프로그램을 작성하시오.

  ```java
  public class StudentScore {

	String name;
	int math;
	int english;
	int science;
	double avg;

	public StudentScore(String name, int math, int english, int science){
		this.name = name;
		this.math = math;
		this.english = english;
		this.science = science;
	}

	double average() {
		this.avg = (math+english+science) / 3.0;
		return avg;
	}

	@Override
	public String toString() {
		return "StudentScore [name=" + name + ", math=" + math + ", english=" + english + ", science=" + science
				+ ", avg=" + avg + "]";
	 }
  }
  ```

  ```java
  import java.util.Scanner;
  public class StudentScoreExample {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.println("이름 | 수학 | 영어 | 과학");

		String name = scan.nextLine();
		int math = scan.nextInt();
		int english = scan.nextInt();
		int science = scan.nextInt();
		double avg;

		StudentScore student1 = new StudentScore(name, math, english, science);
		avg = student1.average();
		System.out.println(student1.toString());

	 }
  }
  ```

# 클래스와 객체 문제2.

ref) 이것이 자바다 5장 확인문제 20번

Getter()와 Setter()를 이용하고 객체필드는 private로 외부에서 변경 못하게 선언했습니다.

계좌 생성(초기화)을 위한 생성자를 만들었습니다.

- 생성자 있는 클래스

```java
package bank;

public class Account {

	private String ano;
	private String owner;
	private int balance;

	public Account(String ano, String owner, int balance) {
		this.ano = ano;
		this.owner = owner;
		this.balance = balance;
	}

	public String getAno() {
		return ano;
	}

	public void setAno(String ano) {
		this.ano = ano;
	}

	public String getOwner() {
		return owner;
	}

	public void setOwner(String owner) {
		this.owner = owner;
	}

	public int getBalance() {
		return balance;
	}

	public void setBalance(int balance) {
		this.balance = balance;
	 }
  }
  ```

  - 메인 메소드가 있는 클래스

  ```java
  package bank;

  import java.util.Scanner;

  public class BankApplication {
	private static Account[] accountArray = new Account[100];
	private static Scanner scan = new Scanner(System.in);
  // 계좌 생성 카운트 변수
	private static int createCount = 0;

	// Create Account
	private static void createAccount() {
		String ano;
		String owner;

		System.out.print("계좌번호를 입력하세요");
		ano = scan.nextLine();
		System.out.print("이름을 입력하세요 : ");
		owner = scan.nextLine();

		// Initialize balance ZERO
		accountArray[createCount] = new Account(ano, owner, 0);
		createCount++;
	}

	// Show list
  // 계좌를 생성한 만큼만 반복문을 돌리게 설정
	private static void accountList() {
		for(int i=0; i<createCount; i++) {
			System.out.println(i + "번째 계좌 : " + accountArray[i].getAno());
		}
	}

	// Deposit
	private static void deposit() {
		System.out.print("입금할 계좌 번호를 입력하세요 : ");
		String account = scan.nextLine();

		System.out.print("입금 금액을 입력하세요 : ");
		findAccount(account).setBalance(Integer.parseInt(scan.nextLine()));

		System.out.println("입금된 금액 : " + findAccount(account).getBalance());
	}

	// Withdraw
	private static void withdraw() {
		int withdrawMoney = 0; // 출금액
		System.out.print("출금할 계좌 번호를 입력하세요 : ");
		String account = scan.nextLine();

		System.out.println("출금 금액을 입력하세요");
		withdrawMoney = Integer.parseInt(scan.nextLine());

		findAccount(account).setBalance(findAccount(account).getBalance()-withdrawMoney);

		System.out.println("출금한 금액 : " + withdrawMoney);
		System.out.println("남은 잔고 : " + findAccount(account).getBalance());
	}

	// Account 배열에서 ano와 동일한 Account 객체 찾기
  // 매개변수 ano에 넘긴 계좌번호와 일치한 인덱스 찾기
	@SuppressWarnings("unlikely-arg-type")
	private static Account findAccount(String ano) {
		int checkNumber = 0;
		for(int i=0; i<createCount; i++) {
			if(ano.equals(accountArray[i].getAno()))
				checkNumber = i;
		}

		return accountArray[checkNumber];
	}

	public static void main(String[] args) {
		boolean run = true;

		while(run) {
			System.out.println("-------------------------------------------------------");
			System.out.println("1.계좌생성 | 2. 계좌목록 | 3. 예금 | 4. 출금 | 5. 종료 ");
			System.out.println("-------------------------------------------------------");
			System.out.print("선택>");

			int selectNo = Integer.parseInt(scan.nextLine());

			if(selectNo == 1) {
				createAccount();
			} else if(selectNo == 2) {
				accountList();
			} else if(selectNo == 3) {
				deposit();
			} else if(selectNo == 4) {
				withdraw();
			} else if(selectNo == 5) {
				run = false;
			} else if(selectNo == 6) {
				accountList();
			}
		}
		System.out.println("프로그램 종료");
	 }
  }
  ```

# 클래스와 객체 문제3.

  ref) 이것이 자바다 5장 확인문제 19번

  - 생성자가 있는 클래스

  ```java
  package exercise;

  public class Account {

	private int balance;

	static final int MIN_BALANCE = 0;
	static final int MAX_BALANCE = 1000000;

	public int getBalance() {
		return balance;
	}

	public void setBalance(int balance) {
		if((balance < MIN_BALANCE) || (balance > MAX_BALANCE)) {
			this.balance = this.balance;
		}
		else {
			this.balance = balance;
		}
	 }
  }
  ```

  - 메인 메소드가 있는 클래스

  ```java
  package exercise;

  public class AccountExample {
	public static void main(String[] args) {
		Account account = new Account();

		account.setBalance(10000);
		System.out.println("현재잔고 : " + account.getBalance());

		account.setBalance(-100);
		System.out.println("현재잔고 : " + account.getBalance());

		account.setBalance(2000000);
		System.out.println("현재잔고 : " + account.getBalance());

		account.setBalance(300000);
		System.out.println("현재잔고 : " + account.getBalance());

	 }
  }
  ```

# 클래스와 객체 문제4.

  노래를 나타내는 song이라는 클래스를 설계하라. song 클래스는 다음과 같은 필드를 갖는다.

  - 노래의 제목을 나타내는 title

  - 가수를 나타내는 artist

  - 노래가 속한 앨범 제목을 나타내는 album

  - 노래의 작곡가를 나타내는 composer, 작곡가는 여러명일 수 있다.

  - 노래가 발표된 연도를 나타내는 year

  - 노래를 속한 앨범에서의 트랙 번호를 나타내는 track

  생성자는 기본 생성자와 모든 필드를 초기화하는 생성자를 작성하고, 노래의 정보를 화면에 출력하는 show() 메소드를 작성하시오.
  Queen의 “Love of My Life" 노래를 Song 객체로 생성하고, show()를 이용하여 이 노래의 정보를 출력하는 프로그램을 작성하라.

  -----------------------------------------------------------------------------

  - 생성자가 있는 클래스

  ```java
  package openchallenge;

  public class Song {

	private String title;
	private String artist;
	private String album;
	private String composer;

	private int year;
	private int track;

	public Song() {

	}

	public Song(String title, String artist, String album, String composer, int year, int track) {
		this.title = title;
		this.artist = artist;
		this.album = album;
		this.composer = composer;
		this.year = year;
		this.track = track;
	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public String getArtist() {
		return artist;
	}

	public void setArtist(String artist) {
		this.artist = artist;
	}

	public String getAlbum() {
		return album;
	}

	public void setAlbum(String album) {
		this.album = album;
	}

	public String getComposer() {
		return composer;
	}

	public void setComposer(String composer) {
		this.composer = composer;
	}

	public int getYear() {
		return year;
	}

	public void setYear(int year) {
		this.year = year;
	}

	public int getTrack() {
		return track;
	}

	public void setTrack(int track) {
		this.track = track;
	}

	public void show() {
		System.out.println("Song [title=" + title + ", artist=" + artist + ", album=" + album + ", composer=" + composer + ", year="
				+ year + ", track=" + track + "]");
	 }

  }
  ```

  - 메인 메소드가 있는 클래스

  ```java
  package openchallenge;

  public class SongTest {

	public static void main(String[] args) {
		Song song = new Song("Love of My Life", "Queen", "Bohemian Rhapsody", "Freddie Mercury", 2018, 10);

		song.show();

	 }
  }
  ```

# 클래스와 객체를 문제5.

  다수의 클래스를 정의하고 활용하는 연습을 해보자.

  더하기+, 빼기-, 곱하기* 나누기/를 수행하는 각 클래스 Add, Sub, Mul, Div를 만들어라.

  이들은 모두 다음 필드와 메소드를 가진다.

  - int 타입의 a, b 필드 : 연산하고자 하는 피연산자

  - void setValue(int a, int b) : 피연산자를 객체 내에 설정한다.

  - int calculate() : 해당 클래스의 목적에 맞는 연산을 실행하고 그 결과를 리턴한다.

   main() 메소드에서는 키보드로부터 두 정수와 계산하고자 하는 연산자를 입력받은 후
   Add, Sub, Mul, Div 중에서 이 연산을 실행할 수 있는 객체를 생성하고 setValue()와 calculate()를
   호출하여 그 결과 값을 화면에 출력한다

   ----------------------------------------------------------------------------

   - Add, Sub, Mul, Div 클래스

   ```java
   package openchallenge;

   public class Add {

	 private int a;
	 private int b;

   public void setValue(int a, int b) {
		this.a = a;
		this.b = b;
	 }

	 public int calculate() {
		return a+b;
	 }
  }
  ///////////////////////////////////////////////////////////////////////////
  package openchallenge;

  public class Sub {

	private int a;
	private int b;

	public void setValue(int a, int b) {
		this.a = a;
		this.b = b;
	}

	public int calculate() {
		if(a>b)
			return a-b;
		else
			return b-a;
	 }
  }
  ///////////////////////////////////////////////////////////////////////////
  package openchallenge;

  public class Mul {

	private int a;
	private int b;

	public void setValue(int a, int b) {
		this.a = a;
		this.b = b;
	}

	public int calculate() {
		return a*b;
	 }
  }
  ///////////////////////////////////////////////////////////////////////////
  package openchallenge;

  public class Div {

	private int a;
	private int b;

	public void setValue(int a, int b) {
		this.a = a;
		this.b = b;
	}

	public int calculate() {
		int ifDiv = 0;
		int elseDiv = 0;

    // 나눗셈은 0으로 나누게 되면 ArithmeticException 발생
    // try-catch 사용
		try {
			if(a>b)
				ifDiv = a/b;
			else
				elseDiv = b/a;
		}catch(ArithmeticException e) {
			System.out.println("0으로 나누면 안됨");
		}

		return ifDiv+elseDiv;
  	}
  }
  ```

  - 메인 메소드가 있는 클래스

  ```java
  package openchallenge;

  import java.util.Scanner;
  public class ArithmeticTest {
	public static void main(String[] args) {
		int num1 = 0;
		int num2 = 0;
		int arithmetic = 0;

		Add add = new Add();
		Sub sub = new Sub();
		Mul mul = new Mul();
		Div div = new Div();

		Scanner scan = new Scanner(System.in);

		while(true) {
			System.out.println("수행할 사칙연산을 고르세요(1. 덧셈 / 2. 뺄셈 / 3. 곱셈 / 4.나눗셈 / 5. 종료)");
			arithmetic = Integer.parseInt(scan.nextLine());

			if(arithmetic == 5) {
				break;
			}

			System.out.print("사칙연산을 할 숫자 2개를 입력하세요 :");

			num1 = Integer.parseInt(scan.nextLine());
			num2 = Integer.parseInt(scan.nextLine());

			switch(arithmetic) {
			case 1:
				add.setValue(num1, num2);
				System.out.println("덧셈 : " + add.calculate());
				break;
			case 2:
				sub.setValue(num1, num2);
				System.out.println("뺄셈 : " + sub.calculate());
				break;
			case 3:
				mul.setValue(num1, num2);
				System.out.println("곱셈 : " + mul.calculate());
				break;
			case 4:
				div.setValue(num1, num2);
				System.out.println("나눗셈 : " + div.calculate());
				break;
			}
		}
		System.out.println("프로그램 종료");
	 }
  }
  ```

# 상속을 사용한 문제1.

  SongLyrics 클래스를 만들어 [클래스와 객체를 이용한 문제4.]를 상속받아서
  메소드 오버라이딩을 통해 가사까지 출력하게 만드는 프로그램.

  메인 메소드가 있는 클래스의 이름은 SongLyricsExample

  ```java
  public class Song {

  	String title;
  	String artist;
  	String album;
  	String[] composer;

  	int year;
  	int track;

  	public Song(String title, String artist, String album, String[] composer, int year, int track) {
  		this.title = title;
  		this.artist = artist;
  		this.album = album;
  		this.composer = composer;
  		this.year = year;
  		this.track = track;
  	}

  	public void show() {
  		System.out.println("Song [title=" + title + ", artist=" + artist + ", album=" + album + ", composer=" + composer + ", year="
  				+ year + ", track=" + track + "]");
  	}
  }
  /////////////////////////////////////////////////////////////////////////////
  public class SongLyrics extends Song {

  	private String lyrics;

  	public SongLyrics(String title, String artist, String album, String[] composer, int year, int track, String lyrics) {
  		super(title, artist, album, composer, year, track);
  		this.lyrics = lyrics;
  	}

  	@Override
  	public void show() {
  		System.out.println("Song [title=" + title + ", artist=" + artist + ", album=" + album + ", composer=" + composer + ", year="
  				+ year + ", track=" + track + ", lyrics=" + lyrics + "]");
  	}
  }
  /////////////////////////////////////////////////////////////////////////////
  public class SongLyricsExample {
  	public static void main(String[] args) {
  		String[] composer = new String[] { "Freddie Mercury" };
  		SongLyrics songLyrics = new SongLyrics("Love of My Life", "Queen", "Bohemian Rhapsody", composer , 2018, 10,
  				"Love of my life,\r\n" +
  						"You hurt me,\r\n" +
  						"You broken my heart,\r\n" +
  						"Now you leave me");

  		songLyrics.show();

  	}
  }
  ```

# 상속을 사용한 문제2.

  Customer 클래스를 만든다. 가지고 있는 필드명은 이름, 전화번호, 주소가 있고
  Customer 클래스를 상속받는 자식클래스 이름은 Member이다.

  Mebember 클래스의 필드명에는 고객번호와 마일리지가 추가되고

  전체 정보를 출력하는 메소드를 오버라이딩해서 작성한다.

  메인메소드의 이름은 MemberExample로 만든다.

  ```java
  public class Customer {

	String name;
	String phoneNumber;
	String address;

	public Customer(String name, String phoneNumber, String address) {
		this.name = name;
		this.phoneNumber = phoneNumber;
		this.address = address;
	}

	public void show() {
		System.out.println("Customer [name=" + name + ", phoneNumber=" + phoneNumber + ", address=" + address + "]");
	 }
  }
  /////////////////////////////////////////////////////////////////////////////
  public class Member extends Customer{
	private int memNumber;
	private int mileage;

	public Member(String name, String phoneNumber, String address, int memNumber, int mileage) {
		super(name, phoneNumber, address);
		this.memNumber = memNumber;
		this.mileage = mileage;
	}

	@Override
	public void show() {
		System.out.println("Customer [name=" + name + ", phoneNumber=" + phoneNumber
				+ ", address=" + address + ", memNumber=" + memNumber + ", mileage=" + mileage + "]");
	 }
  }
  /////////////////////////////////////////////////////////////////////////////
  public class MemberExample {
	public static void main(String[] args) {
		Member member = new Member("백정호", "010-8924-1810", "대전 갈마동", 8924, 1000);

		member.show();
	 }
  }
  ```

# 인터페이스를 사용한 문제1.

  정수형 계산기를 인터페이스로 정의하고 이를 구현하고자 한다.

  두 개의 정수형 값을 인자로 받아 정수값을 리턴하는 add, subtract, multiply 추상메소드를 갖는 인터페이스 파일(Calculator.java)을 작성하시오.

  위에서 정의한 인터페이스를 구현하고 실행하는 프로그램(MyCalc.java, MyCalcApp.java)을 작성하시오.

  ```java
  public interface Calculator {
  	public void add(int a, int b);
  	public void subtract(int a, int b);
  	public void multiply(int a, int b);
  }

  public class MyCalc implements Calculator {
	private int sum = 0;
	private int sub = 0;
	private int mul = 0;

  @Override
	public void add(int a, int b) {
		this.sum = a + b;
		System.out.println(this.sum);
	}

  @Override
	public void subtract(int a, int b) {
		if(a >= b) {
			this.sub = a - b;
			System.out.println(this.sub);
		}
		else {
			this.sub = b - a;
			System.out.println(this.sub);
		}
	}

  @Override
	public void multiply(int a, int b) {
		this.mul = a * b;
		System.out.println(this.mul);
   }
  }

  import java.util.Scanner;
  public class MyCalcApp {
	public static void main(String[] args) {
		Calculator cal = null;
		cal = new MyCalc();

		Scanner scan = new Scanner(System.in);

		System.out.println("숫자 2개를 입력하세요 : ");

		int a = Integer.parseInt(scan.nextLine());
		int b = Integer.parseInt(scan.nextLine());

		cal.add(a, b);
		cal.subtract(a, b);
		cal.multiply(a, b);

		scan.close();
	 }
  }
  ```

# 인터페이스를 사용한 문제2.

  정수형 Array를 매개변수로 받아 여러가지 결과를 처리하는 프로그램을 인터페이스로 정의하고 이를
  구현하고자 한다.

  다음을 추상메소드로 갖는 인터페이스 파일(MyMulti.java)을 작성하시오.

  1. 최대값을 찾아 최대값을 리턴하는 max 메소드
  2. 최소값을 찾아 최소값을 리턴하는 min 메소드
  3. 정수형 Array의 합을 구해서 결과를 리턴하는 sum 메소드
  4. 정수형 Array의 평균값을 더블 타입으로 구해서 결과를 리턴하는 avg 메소드

  위에서 정의한 인터페이스를 구현하고 실행하는 프로그램(MyMultiImpl.java, MyMultiApp.java)을 작성하시오.

  ```java
  public interface MyMulti {

	public int max(int[] array);
	public int min(int[] array);
	public int sum(int[] array);
	public double avg(int[] array);

  }

  public class MyMultiImpl implements MyMulti {
	private int maxNumber = 0;
	private int minNumber = 0;
	private int sum = 0;

  @Override
	public int max(int[] array) {
		for(int i=0; i<array.length; i++) {
			if(array[i] > this.maxNumber) {
				this.maxNumber = array[i];
			}
		}
		return this.maxNumber;
	}

  @Override
	public int min(int[] array) {
		this.minNumber = array[0];
		for(int i=0; i<array.length; i++) {
			if(array[i] < this.minNumber) {
				this.minNumber = array[i];
			}
		}
		return this.minNumber;
	}

  @Override
	public int sum(int[] array) {
		for(int arrays : array) {
			this.sum += arrays;
		}
		return this.sum;
	}

  @Override
	public double avg(int[] array) {
		return this.sum / array.length;
	 }
  }

  public class MyMultiApp {
	public static void main(String[] args) {
		MyMulti mymulti = null;

		mymulti = new MyMultiImpl();

		int[] array = new int[] {10, 20, 30, 40, 50};

		System.out.println(mymulti.max(array));
		System.out.println(mymulti.min(array));
		System.out.println(mymulti.sum(array));
		System.out.println(mymulti.avg(array));
	 }
  }
  ```

# 인터페이스를 사용한 문제3.

  문자열(String) Array를 입력으로 받아 정렬하는 프로그램을 작성하고자 한다.

  오름차순으로 정렬해서 문자열 Array를 리턴하는 sort 메소드와

  내림차순으로 정렬해서 문자열 Array를 리턴하는 sort 메소드를 갖는 MySort 인터페이스 파일을 작성.

  > public abstract String[] sort(String[] strArray);

  > public abstract String[] sort(String[] strArray, boolean descOrder);

  위에서 정의한 인터페이스를 구현하고 실행하는 프로그램(MySortImpl.java, MySortApp.java)을 작성하시오.

  ```java
  public interface MySort {
	public abstract String[] sort(String[] strArray);
	public abstract String[] sort(String[] strArray, boolean descOrder);
  }

  public class MySortImpl implements MySort {

	private String[] sort = null; // 옮겨 담을 배열
	private String temp; // 임시 저장소

  @Override
	public String[] sort(String[] strArray) {
		// 배열 크기 설정
		sort = new String[strArray.length];

		// 배열 복사
		for(int i=0; i<strArray.length; i++) {
			sort[i] = strArray[i];
		}

		// 오름차순 정렬
        for (int i=0; i<sort.length-1; i++) {
            for (int k=0; k<sort.length-i-1; k++) {
                if ((sort[k].compareTo(sort[k+1])) > 0) {
                    temp = this.sort[k];
                    sort[k] = sort[k+1];
                    sort[k+1] = temp;
                }
            }
        }   
        return this.sort;

	}

  @Override
	public String[] sort(String[] strArray, boolean descOrder) {
		// 배열 크기 설정
		sort = new String[strArray.length];

		// 배열 복사
		for(int i=0; i<strArray.length; i++) {
			sort[i] = strArray[i];
		}

		// 내림차순 정렬
		if(descOrder) {
	        for (int i=0; i<sort.length-1; i++) {
	            for (int k=0; k<sort.length-i-1; k++) {
	                if ((sort[k].compareTo(sort[k+1])) < 0) {
	                    temp = this.sort[k];
	                    sort[k] = sort[k+1];
	                    sort[k+1] = temp;
	                }
	            }
	        }   
	        return this.sort;
	    // 오름차순 정렬
		} else if(!descOrder) {
	        for (int i=0; i<strArray.length-1; i++) {
	            for (int k=0; k<strArray.length-i-1; k++) {
	                if ((strArray[k].compareTo(strArray[k+1])) > 0) {
	                    strArray[k] = this.sort[i];
	                    strArray[k] = strArray[k+1];
	                    this.sort[i] = strArray[k+1];
	                }
	            }
	        }   
	        return this.sort;
		}
		return null;
	 }
  }

  public class MySortApp {
	public static void main(String[] args) {
		MySort mysort = null;
		mysort = new MySortImpl();

		String[] array = new String[] {"김길동", "박길동", "이길동", "홍길동", "정길동"};

		for(String osc : mysort.sort(array)) {
			System.out.print(osc + " ");
		}

		System.out.println();

		for(String desc : mysort.sort(array, true)) {
			System.out.print(desc + " ");
		}
	 }
  }
  ```

  여기서 중요한 점이 배열을 복사하지 않고 매개변수 자체로 버블정렬을 실행하면
  원본 배열도 바뀐 상태가 되버립니다.

  array[i] = array[i+1];은 array[i+1]의 스택에 있는 메모리 번지를 array[i]로 옮긴다라는
  의미기때문에 원본 배열도 같이 바뀌게 됩니다.

  따라서 배열을 복사하여 정렬을 해야 원본 배열은 그대로 유지됩니다.

# 예외를 사용한 문제1.

  회원관리를 하는데 Exception을 활용하려고 한다.

  - loginIds 배열에는 “abcde”, “fghij”, “klmno”, “pqrst”, “uvwxyz” 가 있다.

  - passwords 배열에는 “abcde12”, “fghij12”, “klmno12”, “pqrst12”, “uvwxyz12” 가 있다.

  - loginIds 배열과 passwords 배열은 각각 login ID와 그 ID에 해당하는 패스워드이다.

  사용자가 login ID와 패스워드를 입력하는 프로그램을 작성한다.

  사용자가 입력한 login ID와 password 정보가 맞는지 확인하는 프로그램을 작성한다.

  - 입력한 login ID와 password 정보가 loginIds/passwords 배열에 있는 정보와 같은 경우에는
     “로그인 성공” 라는 메시지를 출력한다.

  - 입력한 login ID가 배열에 없으면 InvalidLoginIdException을 발생시키고 이를 처리한다.
     예외처리는 “로그인 아이디가 없습니다.” 라는 메시지를 출력한다.

  - 패스워드를 잘못 입력하면 IncorrectPasswordException을 발생시키고 이를 처리한다.
     예외처리는 “패스워드를 잘못 입력하였습니다.” 라는 메시지를 출력한다.

## 풀이1.

  ```java

  public class InvalidLoginIdException extends Exception {
  	public InvalidLoginIdException() { }
  	public InvalidLoginIdException(String message) {
  		super(message);
  	 }
  }

  public class IncorrectPasswordException extends Exception {
  	public IncorrectPasswordException() { }
  	public IncorrectPasswordException(String message) {
  		super(message);
  	}
  }

  import java.util.Scanner;
  public class MemberExample {
	public static void main(String[] args) {

		try {
			loginMember();
		} catch(InvalidLoginIdException e) {
			String message = e.getMessage();
			System.out.println(message);
		} catch(IncorrectPasswordException e) {
			String message = e.getMessage();
			System.out.println(message);
		}

	}

	public static void loginMember() throws InvalidLoginIdException, IncorrectPasswordException {
		Scanner scan = new Scanner(System.in);

		String[] loginIds = new String[] { "abcde", "fghij", "klmno", "pqrst", "uvwxyz" };
		String[] passwords = new String[] { "abcde12", "fghij12", "klmno12", "pqrst12", "uvwxyz12" };

		String inputID;
		String inputPW;
		String successMessage = null;

		System.out.print("아이디를 입력하세요 : ");
		inputID = scan.nextLine();
		System.out.println("패스워드를 입력하세요 : ");
		inputPW = scan.nextLine();

		for(int i=0; i<loginIds.length; i++) {
			if(inputID.equals(loginIds[i]) == inputPW.equals(passwords[i])) {
				if(inputID.equals(loginIds[i]) != false) {
					successMessage = "Success to Login";
					break;
				}
				else if(inputID.equals(loginIds[i]) == false) {
					successMessage = "패스워드와 아이디가 올바르지 않습니다";
					break;
				}
			} else if(!inputID.equals(loginIds[i])) {
				throw new InvalidLoginIdException("Not exist ID");
			} else if(!inputPW.equals(passwords[i])) {
				throw new IncorrectPasswordException("Not Correct PW");
			}
		}

		if(successMessage.equals("Success to Login")) {
			System.out.println(successMessage);
		}
		else {
			System.out.println(successMessage);
		}
	 }
  }
  ```

## 풀이2.

  ```java
  import java.util.Scanner;

  public class LoginApp {
	static String[] loginIds = {"abcde", "fghij", "klmno", "pqrst", "uvwxyz"};
	static String[] passwords = {"abcde12", "fghij12", "klmno12", "pqrst12", "uvwxyz12"};
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.print("Login ID: ");
		String loginId = scan.nextLine();
		System.out.print("Password: ");
		String password = scan.nextLine();
		scan.close();

		try {
			if (checkLoginPassword(loginId, password))
				System.out.println("로그인 성공");
		} catch (InvalidLoginIdException | IncorrectPasswordException e) {
			System.out.println(e.getMessage());
		}
	}

	static boolean checkLoginPassword(String loginId, String password)
			throws InvalidLoginIdException, IncorrectPasswordException {
		int index = -1;
		for (int i=0; i<loginIds.length; i++) {
			if (loginIds[i].equals(loginId)) {
				index = i;
				break;
			}
		}
		if (index < 0)
			throw new InvalidLoginIdException("로그인 아이디가 없습니다.");
		if (passwords[index].equals(password))
			return true;
		else
			throw new IncorrectPasswordException("패스워드를 잘못 입력하였습니다.");
	 }
  }
  ```

# 코딩 연습문제1.

  일전에 뭐 게임 회사에서 본 간단한 퀴즈 테스트임.

  - 0~9까지의 문자로 된 숫자를 입력 받았을 때, 이 입력 값이 중복된 것이 있는지 확인하는 함수를
    구하시오.(최대 10회 입력을 받는다고 가정)

  - sample inputs: 0123456789 01234 0123456789 6789012345 012322456789

  - sample outputs: true true false true false

## List 사용

  ```java
  import java.util.ArrayList;
  import java.util.List;
  import java.util.Scanner;
  public class NumberOverlapCheckExercise {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);

		List<String> str = new ArrayList<>();

		boolean[] result = new boolean[10];

		for(int i=0; i<10; i++) {
			System.out.print(i + "번째 숫자를 입력하세요(최대 9자리까지 가능) : ");
			str.add(scan.nextLine()); // 문자열을 List에 넣기
			String array = str.get(i); // 문자열을 array에 저장
			int length = array.length();

			for(int k=0; k<length-1; k++) {
				for(int p=k+1; p<length; p++) {
					if(str.get(i).substring(k, k+1).equals(str.get(i).substring(p, p+1))) {
						result[i] = true;
					}
				}
			}
			System.out.println(result[i]);
		}
	 }
  }
  ```

## Arrays 사용

  ```java
  import java.util.Arrays;
  import java.util.Scanner;

  public class Ex03CheckDuplicate {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		String[] numbers = new String[10];
		boolean[] results = new boolean[10];

		for (int i=0; i<10; i++) {
			System.out.print("숫자 입력: ");
			String str = scan.nextLine();
			numbers[i] = str;
			if (i != 0) {
				boolean found = false;
				for (int k=0; k<i; k++) {
					if (str.equals(numbers[k])) {
						found = true;
						break;
					}
				}
				results[i] = found ? false : true;
			} else {
				results[0] = true;
			}
		}
		System.out.println(Arrays.toString(numbers));
		System.out.println(Arrays.toString(results));
	 }
  }
  ```

## StringTokenizer 사용

  ```java
  import java.util.Arrays;
  import java.util.Scanner;
  import java.util.StringTokenizer;

  public class Ex03CheckDuplicateAdvanced {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		String[] numbers = new String[10];
		boolean[] results = new boolean[10];

		System.out.print("숫자 입력: ");
		String line = scan.nextLine();
		StringTokenizer st = new StringTokenizer(line, " ");
		int count = st.countTokens();
		if (count > 10)
			count = 10;
		for (int i=0; i<count; i++) {
			String str = st.nextToken();
			numbers[i] = str;
			if (i != 0) {
				boolean found = false;
				for (int k=0; k<i; k++) {
					if (str.equals(numbers[k])) {
						found = true;
						break;
					}
				}
				results[i] = found ? false : true;
			} else {
				results[0] = true;
			}
		}
		String[] numbersCopy = Arrays.copyOf(numbers, count);
		boolean[] resultsCopy = Arrays.copyOf(results, count);

		System.out.println(Arrays.toString(numbersCopy));
		System.out.println(Arrays.toString(resultsCopy));
	 }
  }
  ```

# 코딩 연습문제2.

  디지털 시계에 하루동안(00:00~23:59) 3이 표시되는 시간을 초로 환산하면 총 몇 초(second) 일까요?

  - 디지털 시계는 하루동안 다음과 같이 시:분(00:00~23:59)으로 표시됩니다.

  00:00 (60초간 표시됨) 00:01 00:02 ... 23:59

  ```java
  public class DigitalWatchExercise {
	public static void main(String[] args) {
		int seconds = 0;

		for (int hour=0; hour<24; hour++) {
			for (int minute=0; minute<60; minute++) {
				if (hour%10 == 3 || minute/10 == 3 || minute%10 == 3)
					seconds += 60;
				/*String time = String.format("%2d%2d", hour, minute);
				if (time.indexOf("3") >= 0)
					seconds += 60;*/
			}
		}
		System.out.println("디지털 시계에 3이 표시되는 시간 = " + seconds + "초");
	 }
  }
  ```

# 코딩 연습문제3.

  1~1000에서 각 숫자의 개수를 구하시오.

  - 예로 10 ~ 15 까지의 각 숫자의 개수를 구해보자.

  10 = 1, 0

  11 = 1, 1

  12 = 1, 2

  13 = 1, 3

  14 = 1, 4

  15 = 1, 5

  그러므로 이 경우의 답은 0:1개, 1:7개, 2:1개, 3:1개, 4:1개, 5:1개

## 풀이1.

  ```java
  public class Ex04CountNumber {
	public static void main(String[] args) {
		int[] counts = {0, 0, 0, 0, 0, 0, 0, 0, 0, 0};

		for (int i=1; i<=9; i++) {
			counts[i]++;
		}
		for (int i=10; i<=99; i++) {
			int firstDigit = i / 10;
			int secondDigit = i % 10;
			counts[firstDigit]++; counts[secondDigit]++;
		}
		for (int i=100; i<=999; i++) {
			int firstDigit = i / 100;
			int secondDigit = (i / 10) % 10;
			int thirdDigit = i % 10;
			counts[firstDigit]++; counts[secondDigit]++; counts[thirdDigit]++;
		}
		counts[1]++; counts[0] += 3; 	// 1000

		System.out.println(Arrays.toString(counts));
	 }
  }
  ```

## 풀이2.

  ```java
  public class Ex04CountNumberRecursion {
	public static void main(String[] args) {
		System.out.println("8! = " + factorial(8));
		for (int i=1; i<21; i++)
			System.out.println(i + ": " + fibonacci(i));
	}

	static int factorial(int n) {
		if (n == 0)
			return 1;
		return n*factorial(n-1);
	}

	static int fibonacci(int n) {
		if (n == 1 | n == 2)
			return 1;
		return fibonacci(n-1) + fibonacci(n-2);
	 }
  }
  ```

# 코딩 연습문제4.

  10~1000까지 각 숫자를 분해하여 곱하기한 값의 전체 합을 구하시오.

  - 예로, 10~15까지의 각 숫자를 분해하여 곱하기한 값의 전체 합은 다음과 같다.

  10 = 1 * 0 = 0  11 = 1 * 1 = 1

  12 = 1 * 2 = 2  13 = 1 * 3 = 3

  14 = 1 * 4 = 4  15 = 1 * 5 = 5

  이 경우의 답은 0+1+2+3+4+5 = 15

  ```java
  public class NumberSplitExample {
	public static void main(String[] args) {
		int sum = 0;
		for (int i=10; i<=99; i++) {
			int firstDigit = i / 10;
			int secondDigit = i % 10;
			sum += firstDigit * secondDigit;
		}
		for (int i=100; i<=999; i++) {
			int firstDigit = i / 100;
			int secondDigit = (i / 10) % 10;
			int thirdDigit = i % 10;
			sum += firstDigit * secondDigit * thirdDigit;
		}
		System.out.println(sum);
	 }
  }
  ```

# 코딩 연습문제5.

  dashInsert() 메소드는 숫자로 구성된 문자열을 매개변수로 받은 뒤, 문자열 내에서 홀수가 연속되면 두 수 사이에 - 를 추가하고,
  짝수가 연속되면 * 를 추가하여 문자열을 리턴하는 기능을 갖고 있다.

  (예, 454 => 454, 4546793 => 454*67-9-3)

  dashInsert 메소드를 작성하고 이를 이용한 프로그램을 작성하시오.

  - 입력 : 화면에서 숫자로 된 문자열을 입력받는다. (예: 4546793)

  - 출력 : `*, -` 가 적절히 추가된 문자열을 화면에 출력한다. (예: 454*67-9-3)

## 풀이1.

  ```java
  public class CeaserPasswordExercise {
	public static void main(String[] args) {
		dashInsert("4546793");
	}

	public static void dashInsert(String str) {
		StringBuilder sb = new StringBuilder();
		sb.append(str);

		for(int i=0; i<sb.length()-1; i++) {
			switch((int)sb.charAt(i)) {
			case 56:
			case 54:
			case 52:
			case 50:
				switch((int)sb.charAt(i+1)) {
				case 56:
				case 54:
				case 52:
				case 50:
					sb.insert(i+1, "*");
					break;
				default:
					break;
				}
				break;
			case 57:
			case 55:
			case 53:
			case 51:
			case 49:
				switch((int)sb.charAt(i+1)) {
				case 57:
				case 55:
				case 53:
				case 51:
				case 49:
					sb.insert(i+1, "-");
					break;
				default:
					break;
				}
				break;
			default:
				break;
			}
		}
		System.out.println(sb.toString());
	 }
  }
  ```

## 풀이2.

  ```java
  import java.util.Scanner;
  public class DashInsertExample {
  	public static void main(String[] args) {
  		Scanner scan = new Scanner(System.in);
  		System.out.print("숫자를 입력하세요");
  		String str = scan.nextLine();
  		dashInsert(str);
  		scan.close();
  	}

  	public static void dashInsert(String str) {

  		StringBuilder sb = new StringBuilder();
  		sb.append(str);
  		for(int i=0; i<sb.length()-1; i++) {
  			if(((int)sb.charAt(i) % 2 == 0) && ((int)sb.charAt(i+1) % 2 == 0) && ((int)sb.charAt(i+1) != 42) && ((int)sb.charAt(i) != 42)) {
  				sb.insert(i+1, "*");
  			}
  			else if(((int)sb.charAt(i) % 2 != 0) && ((int)sb.charAt(i+1) % 2 != 0) && ((int)sb.charAt(i+1) != 45) && ((int)sb.charAt(i) != 45)) {
  				sb.insert(i+1, "-");
  			}
  		}
  		System.out.println(sb.toString());
  	}
  }
  ```

## 풀이3.

  ```java
  public class Ex02DashInsert {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.print("숫자 입력: ");
		String str = scan.nextLine();

		System.out.println("전: " + str);
		System.out.println("후: " + dashInsert(str));
		scan.close();
	}

	static String dashInsert(String str) {
		StringBuilder sb = new StringBuilder();
		boolean odd = false;	// previous number가 홀수인지 여부
		boolean even = false;	// previous number가 짝수인지 여부
		for(int i=0; i<str.length(); i++) {
			if (Integer.valueOf(str.charAt(i)) % 2 == 0) {
				if (even)
					sb.append('*');
				even = true; odd = false;
			} else {
				if (odd)
					sb.append('-');
				odd = true; even = false;
			}
			sb.append(str.charAt(i));
		}
		return sb.toString();
	 }
  }
  ```

# 코딩 연습문제6.

  자기 자신을 제외한 모든 양의 약수들의 합이 자기 자신이 되는 자연수를 완전수라고 한다.

  예를 들면, 6과 28은 완전수이다. 6=1+2+3 // 1,2,3은 각각 6의 약수 28=1+2+4+7+14 // 1,2,4,7,14는 각각 28의 약수.

  입력으로 자연수 N을 받고, 출력으로 N 이하의 모든 완전수를 출력하는 코드를 작성하시오.

## 풀이1.

  ```java
  import java.util.Scanner;
  public class PerfectNumberExample {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);

		System.out.print("숫자를 입력하세요 : ");
		int number = Integer.parseInt(scan.nextLine());

		for(int i=6; i<=number; i++) {
			int sum = 0;
			for(int k=1; k<i; k++) {
				if(i%k == 0) {
					sum += k;
				}
			}
			if(sum == i) {
				System.out.println(i);
			}
		}
    scan.close();
	 }
  }
  ```

## 풀이2.

  ```java
  public class Ex03PerfectNumber {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		int number = 0;
		while(true) {
			System.out.print("양의 정수 입력: ");
			number = Integer.parseInt(scan.nextLine());
			if (number > 0)
				break;
		}
		scan.close();

		for (int i=2; i<=number; i++) {
			if (isPerfectNumber(i))
				System.out.print(i + " ");
		}
	}

	static boolean isPerfectNumber(int n) {
		int sum = 0;
		for (int i=1; i<n; i++) {
			if (n % i == 0)
				sum += i;
		}
		if (sum == n)
			return true;
		return false;
	 }
  }
  ```

# 코딩 연습문제7.

  1부터 10까지 자연수를 각각 제곱해 더하면 다음과 같다 (제곱의 합).

  1^2 + 2^2 + ... + 10^2 = 385

  1부터 10을 먼저 더한 다음에 그 결과를 제곱하면 다음과 같습니다 (합의 제곱).  

  (1 + 2 + ... + 10)^2 = 55^2 = 3025   

  따라서 1부터 10까지 자연수에 대해 "합의 제곱"과 "제곱의 합" 의 차이는 3025 - 385 = 2640 이 된다.

  입력으로 자연수 N을 받아, 1부터 N까지 자연수에 대해 "합의 제곱"과 "제곱의 합"의 차이는 얼마인가?

## 풀이1.

  ```java
  import java.util.Scanner;
  public class SqrtSumExample {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);

		System.out.print("숫자를 입력하세요 : ");
		int N = Integer.parseInt(scan.nextLine());

		int sum = 0;
		int sqrtSum = 0;
		int sumSqrt = 0;

		// 제곱합, 합제곱
		for(int i=1; i<=N; i++) {
			sqrtSum += (i*i);
      sum += i;
		}

		sumSqrt = sum*sum;

		System.out.println(sumSqrt - sqrtSum);
		scan.close();
	 }
  }
  ```

## 풀이2.

  ```java
  public class Ex04SumOfSquare {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		int number = 0;
		while(true) {
			System.out.print("양의 정수 입력: ");
			number = Integer.parseInt(scan.nextLine());
			if (number > 0)
				break;
		}
		scan.close();

		int sumOfSquare = 0, squareOfSum = 0;
		for (int i=1; i<=number; i++) {
			sumOfSquare += i * i;
			squareOfSum += i;
		}
		squareOfSum *= squareOfSum;
		System.out.println("1에서 " + number + "까지");
		System.out.println("합의 제곱 = " + squareOfSum);
		System.out.println("제곱의 합 = " + sumOfSquare);
		System.out.println("차이 = " + (squareOfSum - sumOfSquare));
	 }
  }
  ```

# 코딩 연습문제8.

  시저 암호는, 고대 로마의 황제 줄리어스 시저가 만들어 낸 암호인데, 예를 들어 알파벳 A를 입력했을 때, 그 알파벳의 n개 뒤에 오는 알파벳이 출력되는 것이다.

  예를 들어 바꾸려는 단어가 'CAT"고, n을 5로 지정하였을 때 "HFY"가 되는 것이다.

  어떠한 암호를 만들 문장과 n을 입력했을 때 암호를 만들어 출력하는 프로그램을 작성하시오.

  입력 : 화면에서 문자열과 n값을 입력받는다. (예: ROSE  5) / 출력 : 암호화된 문자열을 출력

## 풀이1.

  ```java
  import java.util.Scanner;
  import java.util.StringTokenizer;
  public class AlphabetPassword {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);

		System.out.print("문자열과 숫자를 입력하세요 : ");
		String str = scan.nextLine();

		StringTokenizer st = new StringTokenizer(str, " ");
		StringBuilder sb = new StringBuilder();
		StringBuilder numSb = new StringBuilder();

		sb.append(st.nextToken()); // 알파벳
		numSb.append(st.nextToken()); // 숫자

		String alphabet = sb.toString();
		int number = Integer.parseInt(numSb.toString());

		for(int i=0; i<alphabet.length(); i++) {
			sb.setCharAt(i, (char) (alphabet.charAt(i)+number));
		}

		System.out.println(sb.toString());
		scan.close();
	 }
  }
  ```

## 풀이2.

  ```java
  import java.util.Scanner;
  public class Ex05CaesarEncryption {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.print("문자열과 숫자 입력: ");
		String str = scan.nextLine();
		String[] tmp = str.split(" ");
		int number = Integer.parseInt(tmp[1]);
		str = makeCaesarEncryption(tmp[0], number);
		System.out.println(str);
		scan.close();
	}

	static String makeCaesarEncryption(String str, int number) {
		byte[] plain = str.getBytes();
		byte[] crypt = new byte[str.length()];
		for (int i=0; i<str.length(); i++) {
			if (plain[i] + number <= 90)
				crypt[i] = (byte)(plain[i] + number);
			else
				crypt[i] = (byte)(plain[i] + number - 26);
		}
		return new String(crypt);
	 }
  }
  ```

# 코딩 연습문제9.

  앞에서부터 읽을 때나 뒤에서부터 읽을 때나 모양이 같은 수를 대칭수(palindrome)라고 부른다.

  두 자리 수를 곱해 만들 수 있는 대칭수 중 가장 큰 수는 9009 (= 91 × 99) 이다.

  세 자리 수를 곱해 만들 수 있는 가장 큰 대칭수는 얼마인가?

## 풀이1.

  ```java
  public class PalindromeExample {
	public static void main(String[] args) {

		int[] buffer = new int[1000];
		int palindrome = 0;
		int mul = 0;
		int length = 0;

		buffer[0] = 100;
		for(int m=0; m<buffer.length; m++) {
			for(int i=100; i<1000; i++) {
				for(int k=100; k<1000; k++) {
					mul = i * k;
					String strNum = String.valueOf(mul);
					length = strNum.length();

					if(length == 5) {
						boolean result1 = strNum.charAt(0) == strNum.charAt(4);
						boolean result2 = strNum.charAt(1) == strNum.charAt(3);

						if((result1 == true) && (result2 == true)) {
							buffer[m] = mul;
							if(buffer[m] > palindrome) {
								palindrome = buffer[m];
							}
						}
					}
					else if(length == 6) {
						boolean result1 = strNum.charAt(0) == strNum.charAt(5);
						boolean result2 = strNum.charAt(1) == strNum.charAt(4);
						boolean result3 = strNum.charAt(2) == strNum.charAt(3);

						if((result1 == true) && (result2 == true) && (result3 == true)) {
							buffer[m] = mul;
							if(buffer[m] > palindrome) {
								palindrome = buffer[m];
							}
						}
					}
				}
			}
		}
		System.out.println("가장 큰 대칭수 : " + palindrome);
	 }
  }
  ```

## 풀이2.

  ```java
  public class Ex06Palindrome {
	public static void main(String[] args) {
		int max = 0;
		int a = 0, b = 0;
		for (int i=100; i<=999; i++) {
			for (int k=100; k<=999; k++) {
				if (isPalindrome(i*k)) {
					if (max < i*k) {
						a = i; b = k; max = i*k;
					}
				}
			}
		}
		System.out.println(a + ", " + b + ", " + max);
	}

	static boolean isPalindrome(int number) {
		String str = String.valueOf(number);
		int len = str.length();
		for (int i=0; i<len/2; i++) {
			if (str.charAt(i) != str.charAt(len-1-i))
				return false;
		}
		return true;
	 }
  }
  ```

# 코딩 연습문제10.

  세 자연수 a, b, c 가 피타고라스 정리 a2 + b2 = c2 를 만족하면 피타고라스 수라고 부른다.

  (여기서 a < b < c 이고 a + b > c)

  예를 들면, 32 + 42 = 9 + 16 = 25 = 52 이므로 3, 4, 5는 피타고라스 수입니다.

  a + b + c = 1000 인 피타고라스 수를 구하시오. (답은 한가지 뿐이다)

## 풀이1.

  ```java
  public class PythagorasExample {
  	public static void main(String[] args) {

  		int pythagorasA = 0;
  		int pythagorasB = 0;
  		int pythagorasC = 0;

  		for(int a=1; a<500; a++) {
  			for(int b=a+1; b<500; b++) {
  				pythagorasC = 1000 - a - b;
  				if((b<pythagorasC) && (a<pythagorasC)) {
  					if((a+b) > pythagorasC) {
  						if(((a*a+b*b) == pythagorasC*pythagorasC) && ((a+b+pythagorasC) == 1000)){
  							pythagorasA = a;
  							pythagorasB = b;
  						}
  					}
  				}
  			}
  		}
  		pythagorasC = 1000 - pythagorasA - pythagorasB;
  		System.out.println(pythagorasA + ", " + pythagorasB + ", " + pythagorasC);
  	}
  }
  ```

## 풀이2.

  ```java
  public class Ex07Pythagoras {
	public static void main(String[] args) {
		for (int a = 1; a <= 332; a++) {
			for (int b = a+1; b < 500; b++) {
				int c = 1000 - a - b;
				if (c < b)
					break;
				if (a*a + b*b == c*c) {
					System.out.println(a + ", " + b + ", " + c);
					System.exit(0);
				}
			}
		}
	 }
  }
  ```
