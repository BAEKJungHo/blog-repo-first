---
title: "암호화(Encryption)와 해시(Hash)"
layout: post
category: Technology
tags: [Technology]
excerpt: ""
author: BAEKJungHo
---

* content
{:toc}

## 암호화(Encryption)

  - 암호화에서 사용하는 표현
    - `평문(PlainText)` - 암호화 하기전의 메시지
    - `암호문(CipherText)` - 암호화된 메시지(해독 불가능)
    - `암호화(Encryption)` - 평문을 암호문으로 변환하는 과정
    - `복호화(Decryption)` - 암호문을 평문으로 변환하는 과정

  암호화(Encryption)는 평문(암호화 하기전의 메시지)을 암호문(암호화된 메시지)로 변환하는 과정을 뜻합니다.

  암호화는 크게 3가지로 나눌 수 있습니다.

  - `대칭키 암호화 방식(Symmetric Encryption)`
    - 암호화할 때 사용한 키와 복호화할 때 사용한 키가 같은 암호 방식
    - 대표적인 알고리즘
      - DES
      - 3DES
      - AES
    - 가장 보편적으로 사용하는 알고리즘은 `AES 알고리즘이며, 128 ~ 256bit의 키를 사용`
    - 전에는 DES알고리즘을 사용하였으나 취약점이 발견되어 AES암호화 방식을 사용
    - 양방향 암호화(복호화가 가능)
    - ![p1](/images/posts/201907/p1.jpg)
    - 대칭키 암호화 방식은 2가지로 나뉨
      - 블록(Block) 암호화 방식
        - DES (64bit, 56bit)
        - 3-DES
        - AES (128bit, 192bit, 256bit)
        - SEED (128bit) - KISA에서 만든 암호화알고리즘
        - ARIA
      - 스트림(Stream) 암호화 방식
        - RC4(동기식)
        - A5/1, A5/2, A5/3 (자기 동기식)
    - 속도가 빠르지만, 송신자와 수신자가 같은 키를 사용해야하며, 기기마다 다른 키를 가지고 있으며, 자주 키를 바꾸어 줘야 하기 때문에 관리가 어려움
    - Email, SRTP, HTTPS 등 어플리케이션에서 많은 양의 데이터를 처리하기 위해서 사용

  - `비대칭키 암호화 방식(Asymmetric Encryption)`
    - 암호화할 때의 키와 복호화할 때 키가 서로 다른 암호 방식
    - 대표적인 알고리즘
      - RSA(Rivest, Shamir, Adleman)
      - ECC
      - DSS
    - 양방향 암호화(복호화가 가능)
    - ![p2](/images/posts/201907/p2.jpg)

  - `해싱(hashing)`
    - 해싱을 이용한 암호화 방식으로 암호화(encryption)/복호화(decryption)과는 다른 개념
    - 해싱을 이용해 평문을 암호문으로 암호화하는 것은 가능하지만, 암호문을 평문으로 복호화 하는 것은 불가능
    - 고정된 길이의 문자열을 생성, 복호화가 불가능 하므로 원문을 알수 없음
    - `패스워드 암호화 할때 사용(SHA-2)`
    - `인증에 사용`
    - 대표적인 알고리즘
      - MD5
      - SHA-0
      - SHA-1
      - SHA-2(SHA-224, SHA-256, SHA-384, SHA-512)
    - 단방향 암호화(복호화가 불가능)

  이제 각 암호화 방식을 자세하게 배워 보겠습니다.

## 암호화(Encryption)와 해시(Hash)의 차이

  암호화는 `암호화 알고리즘`을 이용하고, 인증은 `해시 함수`를 이용하여 `Verification Data`를 만들어 원문에 태그(Tag)를 붙여서 전송합니다. 암호화 알고리즘과 해쉬 함수의 동작 방식을 이해하면, 암호화와 인증의 차이를 이해할 수 있습니다.

  암호화는 기본적으로 양방향 통신을 전제로 하므로 암호와와 복호화가 가능해야 합니다. 복호화 되지 않는 암호화는 의미가 없는 것입니다.

  해시는 메세지를 고정된 길이의 문자열로 변환합니다. 해싱을 통해 생성된 `Message Digest`는 복호화 될 필요가 없습니다. 해시는 메세지마다 다른 내용의 Message Digest를 만들기 때문에 메세지의 지문(fingerprint)으로 볼 수 있습니다.

__즉, 암호화는 복호화를 전제로한 양방향 통신을 위한 것이며, 해시는 고정된 길이의 문자열을 생성하고 복호화 할 수 없습니다.__

## 해싱(Hasing)

  해시는 가변길이의 데이터를 고정된 길이의 문자열로 변환하는 역할을 하며, 복호화가 되지 않으므로 원문을 알 수 없습니다. 대표적인 해시 알고리즘은 `MD5(Message Digest)`와 `SHA(Secure Hash Algorithm)`가 있습니다. 아래 왼쪽 그림은 일반적인 해시 과정입니다.

  ![p3](/images/posts/201907/p3.jpg)

  일반적으로 해시는 보안상의 문제가 있습니다. 원문을 해싱하여 Vertification Data를 붙여서 전송하는 것이 일반적인데 전송 과정에서 누군가가 원문을 변환한 후에 Verification Data를 붙여서 수신자에게 보낸다면
  수신자는 메세지의 변경 여부를 알지 못합니다.

  그래서 위의 오른쪽 그림과 같이 `Secret Key`를 추가하여 `Verification Data`를 생성하는 것입니다. 이를 `MAC(Message Authentication Code)`라고 합니다.
  MAC은 인증과 무결성을 동시에 제공할 수 있으며, 암호화에 비해 연산이 빠르다는 장점이 있습니다. MAC은 생성 방식에 따라 다양하게 나눌 수 있는데 IP Telephony에서는 `HMAC(Hash-based MAC)`을 주로 사용합니다.

  > 추가 설명
  >
  > 해시 함수는 가변 입력 메시지를 취하여 각 메시지에 대해 고정 길이 메시지 다이제스트를 계산하는 함수입니다. 이 다이제스트는 디지털 지문으로 사용되어 수신기가 원본 메시지가 전송 중에 변경되지 않았는지 확인할 수 있습니다. 해시 함수는 진위성을 보장하지 않는 암호화되지 않은 암호화되지 않은 프리미티브입니다. 비밀 키와 결합하여 메시지 인증 코드를 생성 할 수 있습니다. 또한 디지털 서명 체계에 사용되는 암호 원시 (cryptographic primitive) 역할을합니다.

### MD5(Message Digest)

  > Wikipedia
  >
  > `MD5(Message-Digest algorithm 5)는 128비트 암호화 해시 함수`이다. RFC 1321로 지정되어 있으며, 주로 프로그램이나 파일이 원본 그대로인지를 확인하는 무결성 검사 등에 사용된다. 1991년에 로널드 라이베스트가 예전에 쓰이던 MD4를 대체하기 위해 고안했다. 1996년에 MD5의 설계상 결함이 발견되었다. 이것은 매우 치명적인 결함은 아니었지만, 암호학자들은 해시 용도로 SHA-1과 같이 다른 안전한 알고리즘을 사용할 것을 권장하기 시작했다. 2004년에는 더욱 심한 암호화 결함이 발견되었고. 2006년에는 노트북 컴퓨터 한 대의 계산 능력으로 1분 내에 해시 충돌을 찾을 정도로 빠른 알고리즘이 발표 되기도 하였다. 현재는 MD5 알고리즘을 보안 관련 용도로 쓰는 것은 권장하지 않으며, 심각한 보안 문제를 야기할 수도 있다. 2008년 12월에는 MD5의 결함을 이용해 SSL 인증서를 변조하는 것이 가능하다는 것이 발표되었다.

### SHA(Secure Hash Algorithm)

  > SHA(Secure Hash Algorithm, 안전한 해시 알고리즘)
  >
  > Digest(다이제스트) : 해시 함수가 출력하는 압축된 문장

  SHA(Secure Hash Algorithm, 안전한 해시 알고리즘) 함수들은 서로 관련된 암호학적 해시 함수들의 모음이다. 이들 함수는 미국 국가안보국(NSA)이 1993년에 처음으로 설계했으며 미국 국가 표준으로 지정되었다. SHA 함수군에 속하는 최초의 함수는 공식적으로 SHA라고 불리지만, 나중에 설계된 함수들과 구별하기 위하여 `SHA-0`이라고도 불린다. 2년 후 SHA-0의 변형인 `SHA-1`이 발표되었으며, 그 후에 4종류의 변형, 즉 `SHA-224, SHA-256, SHA-384, SHA-512`가 더 발표되었다. 이들을 통칭해서 `SHA-2`라고 하기도 한다.

  SHA-1은 SHA 함수들 중 가장 많이 쓰이며, `TLS, SSL, PGP, SSH, IPSec` 등 많은 보안 프로토콜과 프로그램에서 사용되고 있다. SHA-1은 이전에 널리 사용되던 MD5를 대신해서 쓰이기도 한다. __좀 더 중요한 기술에는 SHA-256이나 그 이상의 알고리즘을 사용할 것을 권장한다.__

  > SHA-0과 SHA-1에 대한 공격은 이미 발견되었다. SHA-2에 대한 공격은 아직 발견되지 않았으나, 전문가들은 SHA-2 함수들이 SHA-1과 비슷한 방법을 사용하기 때문에 공격이 발견될 가능성이 있다고 지적한다.

  ![p6](/images/posts/201907/p6.jpg)

  SHA-256, SHA-384, SHA-512는 2001년에 초안으로 처음으로 발표되었으며, 2002년에 SHA-1과 함께 정식 표준(FIPS PUB 180-2)으로 지정되었다. 2004년 2월에 삼중 DES의 키 길이에 맞춰 해시값 길이를 조정한 SHA-224가 표준에 추가되었다. SHA-256과 SHA-512는 각각 32바이트 및 64바이트 워드를 사용하는 해시 함수이며, 몇몇 상수들이 다르긴 하지만 그 구조는 라운드의 수를 빼고는 완전히 같다. SHA-224와 SHA-384는 서로 다른 초기값을 가지고 계산한 SHA-256과 SHA-512 해시값을 최종 해시값 길이에 맞춰 잘라낸 것이다.

  - SHA-1 해시값(MessageDigest)

  ```
  SHA1("The quick brown fox jumps over the lazy dog")
  = 2fd4e1c67a2d28fced849ee1bb76e7391b93eb12
  ```

  해시값은 눈사태 효과 때문에 메시지가 조금만 바뀌어도 완전히 바뀔 수 있다. 다음 예시는 위의 예제 끝에 마침표(.)를 찍은 것이다.

  ```
  SHA1("The quick brown fox jumps over the lazy dog.")
  = 408d94384216f890ff7a0c3528e8bed1e0b01621
  ```

  빈 문자열의 해시값은 다음과 같다.

  ```
  SHA1("") = da39a3ee5e6b4b0d3255bfef95601890afd80709
  ```

  즉 위 값들을 `MessageDigest`라고 합니다.

#### SHA-256을 사용한 비밀번호 암호화

  혼자 코딩을 하거나, 간단한 테스트를 할 때에는 패스워드를 평문으로 비교하는데, 이것은 간단히 하기 위한 거고 테스트가 아닌 일반 사이트에서 이렇게 하면 개인정보보호법 29조 위반으로 과태료 폭탄을 맞을 수 있습니다.

  __패스워드는 반드시 PBKDF2 이상의 보안성을 가지는 해시 함수로 해싱해야 하며 대부분 SHA를 사용합니다.__ 보통은 SHA-1이 많이 사용되며, 좀 더 중요한 기술에는 SHA-256 이상을 권장합니다.

  > 로그인 처리와 같이, 사용자 암호를 입력받는 곳에는 SHA를 사용한 해싱과정이 들어가야 합니다.

  SHA-2가 생성하는 다이제스트(Digest) 출력 길이는 `224, 256, 384, 512 bit`로 다양합니다.

  - SHA-256을 사용한 암호화 샘플 코드

  ```java
  import java.security.*;

  public class SecurityUtil {
    public String encryptSHA256(String str) {
      String sha = "";
      try {
        MessageDigest md = MessageDigest("SHA-256");
        md.update(str.getBytes());
        byte[] byteDatas = md.digest();
        StringBuffer sb = new StringBuffer();
        for(byte byteData : byteDatas) {
          sb.append(Integer.toString(byteData&0xff) + 0x100, 16).substring(1));
        }
        sha = sb.toString();
      } catch(NoSuchAlgotrithmException e) {
        log.info("Encrypt Error : NoSuchAlgotrithmException");
        sha = null;
      }
      return sha;
    }
  }
  ```

  - SecurityUtil 클래스를 이용해서 패스워드를 암호화하는 클래스

  ```java
  public class SecurityExample {
    public static void main(String[] args) {
      SecurityUtil securityUtil = new SecurityUtil();
      String rtn1 = securityUtil.encryptSHA256("ABADAF!!");
      String rtn2 = securityUtil.encryptSHA256("ABADAF!!");

      if(rtn1.equals(rtn2)) { System.out.println("Equals"); }
    }
  }
  ```

  위 결과는 `Equals`가 출력됩니다. 따라서 로그인 처리 과정에서 사용자가 입력한 패스워드와 DB에 저장된 패스워드(SHA-256으로 암호화된 패스워드)를 비교해서 처리해야 할 때는,
  사용자가 입력한 패스워드도 SHA-256으로 암호화해서 암호화된 값 끼리 비교해야 합니다.

  > 위 코드를 보면 알 수 있듯이 SHA를 통한 해싱은 평문 값이 동일할 경우 해싱을 통해 나오는 `MessageDigest` 해시값도 동일합니다.

## 대칭키 암호화(Symmetric Encryption)

### DES(Data Encryption Standard)

  1975년에 IBM에서 개발하고 1979년에 미국 NBS(National Bureau of Standards, 현 NIST)가 국가 표준 암호 알고리즘으로 지정한 대칭키 암호 알고리즘입니다.

  블록 암호 기법을 사용하며 16단계의 파이스텔 네트워크(Feistel Network)를 거쳐 암호화를 수행합니다. 블록의 단위는 64비트로 평문을 64비트 단위로 암호화를 수행하여 64비트의 암호화 문서를 생성하며 키 길이는 64비트지만 실제로는 패리티 비트가 8비트 붙어있어서 실제 키 길이(=암호화 강도)는 56비트입니다. `문제는 56비트 키가 오늘날 컴퓨팅 환경에서는 너무 짧다는 것입니다.`

  컴퓨팅 환경이 훨씬 발전한 2015년을 기준으로 하면 암호화의 의미가 없습니다. 평범한 개인용 PC로도 최적화된 방법(SIMD, GPGPU 등등)을 사용하면 수 시간 만에 뚫을 수 있습니다. 따라서 현대에는 기존에 암호화된 문서를 복호화하는 용도로만 사용하고 신규 암호화 문서를 생성하는데는 `절대로 사용하지 말 것을 권장하는 암호화 알고리즘`입니다.

### AES(Advanced Encryption Standard)

  > Wikipedia. [https://en.wikipedia.org/wiki/Advanced_Encryption_Standard](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)

  AES는 `대체-치환 네트워크 (substitution-permutation network)` 로 알려진 설계 원리를 기반으로하며 소프트웨어와 하드웨어 모두에서 효율적입니다. AES는 이전의 DES와 달리 Feistel 네트워크를 사용하지 않습니다 . AES는 128 비트 의 고정 블록 크기 와 128, 192 또는 256 비트 의 키 크기 를 가진 Rijndael의 변형입니다 . 대조적으로, Rijndael 그 자체 는 최소 128 개 및 최대 256 개 비트가있는 32 비트의 배수가 될 수있는 블록 및 키 크기로 지정됩니다.

  ![p4](/images/posts/201907/p4.jpg)

  > AES 알고리즘의 모든 주요 길이 (즉, 128, 192 및 256)의 설계와 강도는 분류 된 정보를 SECRET 수준까지 보호하기에 충분합니다. TOP SECRET 정보는 192 또는 256 키 길이 중 하나를 사용해야합니다. 국가 안보 시스템 및, 또는 정보를 보호하기위한 제품의 AES 구현은 NSA가 획득 및 사용하기 전에 검토하고 인증해야합니다.

## 비 대칭키 암호화(Asymmetric Encryption)

### RSA(Rivest, Shamir, Adleman)

  RSA는 공개키 암호시스템의 하나로, 암호화뿐만 아니라 전자서명이 가능한 최초의 알고리즘으로 알려져 있습니다. RSA가 갖는 전자서명 기능은 인증을 요구하는 전자 상거래 등에 RSA의 광범위한 활용을 가능하게 하였습니다.

  1978년 로널드 라이베스트(Ron Rivest), 아디 샤미르(Adi Shamir), 레너드 애들먼(Leonard Adleman)의 연구에 의해 체계화되었으며, RSA라는 이름은 이들 3명의 이름 앞글자를 딴 것입니다. RSA 암호체계의 안정성은 큰 숫자를 소인수 분해하는 것이 어렵다는 것에 기반을 두고 있습니다. 그러므로 큰 수의 소인수 분해를 획기적으로 빠르게 할 수 있는 알고리즘이 발견된다면 이 암호 체계는 가치가 떨어질 것입니다.

  ![p5](/images/posts/201907/p5.jpg)

  RSA는 두 개의 키를 사용한다. 여기서 키란 메시지를 열고 잠그는 `상수(constant)`를 의미합니다. 일반적으로 많은 공개키 알고리즘의 공개키(public key)는 모두에게 알려져 있으며 메시지를 암호화(encrypt)하는데 쓰이며, `암호화된 메시지는 개인키(private key)를 가진 자만이 복호화(decrypt)하여 열어볼 수 있습니다.` 하지만 RSA 공개키 알고리즘은 이러한 제약조건이 없습니다. 즉 개인키로 암호화하여 공개키로 복호화할 수도 있습니다.

  __공개키 알고리즘은 누구나 어떤 메시지를 암호화할 수 있지만, 그것을 해독하여 열람할 수 있는 사람은 개인키를 지닌 단 한 사람만이 존재한다는 점에서 대칭키 알고리즘과 차이를 가집니다.__

  RSA는 소인수 분해의 난해함에 기반하여, 공개키만을 가지고는 개인키를 쉽게 짐작할 수 없도록 디자인되어 있습니다.

  보다 이해하기 쉬운 예를 들자면, A라는 사람에게 B라는 사람이 메시지를 전하고자 할 때 B는 A의 열린 자물쇠를 들고 와 그의 메시지를 봉인(공개키 암호화 과정에 해당)하고, 그런 다음 A에게 전해 주면, 자물쇠의 열쇠(개인키에 해당)를 가지고 있는 A가 그 메시지를 열어보는(개인키 복호화 과정에 해당) 식이 됩니다. 중간에 그 메시지를 가로채는 사람은 그 열쇠를 가지고 있지 않으므로 메시지를 열람할 수 없습니다.

  메시지와 공개키 모두를 알 수 있다면 변조된 메시지를 보낼 수 있기 때문에, 실제로는 수신측의 공개키만을 사용하여 암호화하는 경우는 드물다. 송수신 양측의 키쌍을 사용하는 방법으로는 A의 개인키로 암호화 -> B의 공개키로 암호화 한 메시지를 전달하고 복호화 과정은 B의 개인키로 복호화 -> A의 공개키로 복호화로 구성된 방식이 일반적이다. RSA의 디자인 상, 그 열쇠(개인키에 해당)는 자물쇠의 형태(공개키에 해당)만 보고서는 쉽게 제작할 수가 없게 되어 있습니다.

## 상호간 암호화 스팩 공유에 관하여

  암호화에 관해서 협의를 할 때, 일반적으로 아래와 같이 의견 교환을 할 것입니다.

  - `암호화 스팩`
    - 알고리즘 : `AES-256`
    - 암호화키 : `abcdefghijklmnopqrstuvwxyz123457890`
    - 인코딩 : `UTF-8`

  알고리즘, 암호화키 만 보내는 경우가 일반적이며, 인코딩을 보내지 않는 경우도 심심찮게 확인할 수 있습니다. 그리고 샘플코드 라도 첨부파일로 보내주면 다행인데, 그렇지 않은 경우도 허다합니다. _일부 경우에는 검증되지 않은 블로그 포스트나 링크를 보내주고 그대로 해달라고 하기도 합니다._

  많은 개발자 들이 암호화에 대한 정확한 이해가 없이 의사소통을 하고 이런 방식이 어느 순간부터 대중화(?) 된 것 같습니다. 여기에 관련해서 해당 부분에 대한 간략한 이론적인 내용을 알아보고, 이제부터는 어떻게 암호화 스팩을 상호 간 공유해야 하는지 알아보겠습니다.

  > 짚고 넘어가야할 부분은 여기서는 대중적으로 상호 간 통신 시 가장 많이 쓰이는 대칭키 암호화 방식으로 이야기 하겠습니다. 코드는 Java 기준으로 설명하겠습니다.

### 암호화 인터페이스

  ```java
public interface Crypto {
  // 암호화
  String encrypt(String plain);
  // 복호화
  String decrypt(String cipher);
}
  ```

  일반적으로 대부분의 개발자들은 개발 시 암/복호화를 위해서 위와 같은 인터페이스가 필요할 것입니다.

  반론으로 왜 byte[] 가 지원되지 않는가?, 왜 암호화 키를 입력 받지 않는가도 있지만, 그것은 생각하지 않겠습니다.

  위와 같은 간단한 인터페이스를 통해서 간단하게 암호화가 가능하게 할려면 최소 아래와 같은 내용을 확인해야합니다.

#### 대칭키 암호화 시 스팩

  - 암호화 알고리즘 : `key size`, 필요에 따라서 `block size`
  - 암호화 모드
  - Padding 방식
  - 암호화 키
    - 일부 암호화에서는 초기화 벡터도 필요함
  - 암호문자열 바이트인코딩 + 문자인코딩
  - 키문자열(암호화키, 초기화벡터) - 바이트 간 인코딩 방식 (문자인코딩, 바이트인코딩 둘 다 가능)
    - 암호 문자열과 인코딩 방식과 키문자열 인코딩 방식이 다르면 필요

  즉, 위 내용을 풀어 쓰면 아래와 같습니다.

  - 암호화 알고리즘 : `AES`
    - key size : 256 bit
    - block size : 128 bit (AES의 블록사이즈는 128로 고정입니다. 고로 block size는 skip해도 됩니다.)
  - 암호화 모드 : `CBC`
  - Padding 방식 : `PKCS5`
  - 암호화키 : `12345678901234567890123456789012 (32자)`
    - 초기화벡터 : `1234567890123456 (16자)`
  - 암호문 인코딩 방식 : `Base64 + UTF-8`
  - 키 인코딩 방식 : `ASCII (문자인코딩)`

  이렇게 보아도 이해가 잘 안되는 거 같습니다. 그래서 위에서 선언한 인터페이스를 구현하면서 어떻게 해당 내용이 구현되는지 보면서 알아보겠습니다.

#### 암호화 인터페이스 구현

  - `암호화 알고리즘, 모드, 패딩`

  ```java
public class AES256Crypto implements Crypto {

  // 알고리즘/모드/패딩
  private static final String algorithm = "AES/CBC/PKCS5Padding";

  // 암호화
  public String encrypt(String plain) {
    Cipher c = Cipher.getInstance(algorithm);
    // TODO
    return null;
  }
  ...
}
  ```

  기본적으로 알고리즘, 모드, 패딩이 필요합니다.

  - 알고리즘은 암호화 알고리즘이며 예제의 경우 AES 방식을 사용합니다. 블록암호화 방식이기 때문에 Byte Padding이 필요합니다.
  - 모드는 암호화 방식입니다. 예제의 경우 CBC를 사용하는데 이럴 경우 초기화 벡터가 필요합니다.
  - 블록 암호화 방식이기 때문에 padding 방식이 필요합니다.

  - `암호화 키, key size`

  ```java
public class AES256Crypto implements Crypto {
  // 알고리즘/모드/패딩
  private static final String algorithm = "AES/CBC/PKCS5Padding";
  // 암호화 키
  private final String secretKey;

  public AESCrypto(String secretKey) {
    if (secretKey.length != 256 / 8) {
      throw new IllegalArgumentException("'secretKey' must be 256 bit");
    }
    this.secretKey = secretKey;
  }

  // 암호화
  public String encrypt(String plain) {
    Cipher c = Cipher.getInstance(algorithm);
    // TODO
    return null;
  }
  ...
}
  ```

  256 bit 암호화 방식이기 때문에 키 입력에 대한 유효성을 추가하였습니다. - 아직은 논란이 있는 코드입니다. key size가 암호화 알고리즘의 bit 수를 가르키는 것과 동일하게 됩니다. __즉 AES256 이라는 것은 암호화 키 사이즈가 256 bit 라는 말과 동일합니다.__

  - `초기화 벡터, block size`

  ```java
public class AES256Crypto implements Crypto {
  // 알고리즘/모드/패딩
  private static final String algorithm = "AES/CBC/PKCS5Padding";
  // 암호화 키
  private final String secretKey;
  // 초기화 벡터
  private final String iv;

  public AESCrypto(String secretKey, String iv) {
    if (secretKey.length != 256 / 8) {
      throw new IllegalArgumentException("'secretKey' must be 256 bit");
    }
    if (iv.length != 128 / 8) {
      throw new IllegalArgumentException("'iv' must be 128 bit");
    }
    this.secretKey = secretKey;
    this.iv = iv;
  }

  // 암호화
  public String encrypt(String plain) {
    Cipher c = Cipher.getInstance(algorithm);
    // TODO
    return null;
  }
  ...
}
  ```

  `AES`의 block size는 128 bit 고정이기 때문에 별 다른 변화가 없습니다.

  AES의 모태가 되는 `Rijendael` 알고리즘 경우에는 128 192 256 bit이기 때문에 달라질 수 있습니다.

  단, `초기화벡터(iv)`의 경우 block size와 같아야 하기 때문에 128 bit 여야합니다. iv의 경우 더 강력한 암호화를 위해서는 암호화 요청 시 마다 달라지는 것이 보안에 더 좋으나, 예제이므로 우선은 초기 세팅으로 표현하겠습니다.

  - `암호문 인코딩 인터페이스`

  ```java
public interface Encoder {
  // string -> bytes
  byte[] encode(String str);
  // bytes -> string
  String decode(byte[] bytes);
}
  ```

  갑자기 새로운 인터페이스가 추가되었습니다. 암호문을 인코딩 하기 위해서는 위와 같은 인터페이스가 필요하기 때문입니다.

  간단하게 설명하면 `string -> bytes 가 encode이며, bytes -> string 는 decode` 입니다. 기본적으로 인코딩은 charset와 연관관계가 깊은 문자인코딩으로 생각하기 쉬운데, 암호화의 경우에는 암호화로 인해 문자인코딩과는 별개의 규칙이 없는 bytes가 반환되므로 문자로 표현할 수 없게 됩니다.

  고로 암호문을 위한 인코딩 은 1차적으로 문자(charset)와 연관없는 인코딩이 가능해야합니다. __즉 문자열을 기준으로 해서 byte화 시키는 문자인코딩과는 다르게 bytes를 기준으로 문자열화 시키는 인코딩 방식이 필요 하게 됩니다.__

  가장 간단하게는 bytes를 16진수 기반으로 표현하는 Hex 방식이나 Base64 방식을 사용하는 것이 좋습니다.

  예제는 Base64 방식으로 진행하겠습니다.

  - `암호문 Base 64 인코딩 구현`

  ```java
public class Base64Encoder implements Encoder {
  // base64는 아스키 코드 내로 표현가능한 인코딩 방식
  private static final String ASCII = "US-ASCII";

  // string -> bytes
  public byte[] encode(String str) {
    return Base64.encodeBase64(str.getBytes(ASCII));
  }
  // bytes -> string
  public String decode(byte[] bytes) {
    return new String(Base64.decodeBase64(bytes), ASCII);
  }
}
  ```

  편의상 Exception 핸들링은 제외하였습니다.

  위 구현체를 바탕으로 `AES256Crypto` 를 계속 구현해 보겠습니다. 아까 예시에서 정의한 `“암호문 인코딩 방식 : Base64 + UTF-8”` 에서 UTF-8 은 이후에 나옵니다. Base64Encoder에 정의된 것과 혼동하면 안됩니다. : _base64 스팩 자체가 ASCII에 의존하는 방식_ 입니다.

  > 참고 : [Wikipedia. BASE64](https://ko.wikipedia.org/wiki/%EB%B2%A0%EC%9D%B4%EC%8A%A464)

  - `암호문 구현체에 암호문 인코더 주입`

  ```java
public class AES256Crypto implements Crypto {
  // 알고리즘/모드/패딩
  private static final String algorithm = "AES/CBC/PKCS5Padding";
  // 암호화 키
  private final String secretKey;
  // 초기화 벡터
  private final String iv;
  // 문자인코딩 방식
  private final String charset = "UTF-8";
  // 암호문 바이트 인코더
  private Encoder encoder = new Base64Encoder();
  ...

  // 암호화
  public String encrypt(String plain) {
    // 암호화 키 생성
    byte[] keyData = secretKey.getBytes("US-ASCII");
    SecretKey secureKey = new SecretKeySpec(keyData, "AES");

    Cipher c = Cipher.getInstance(algorithm);
    // 암호화 키 주입, iv 생성 주입, 초기화 - 이상하지만 우선 무시
    c.init(Cipher.ENCRYPT_MODE, secureKey, new IvParameterSpec(iv.getBytes("US-ASCII")));
    // 문자인코딩 방식을 통한 string -> byte 변환 후 암호화
    byte[] encrypted = c.doFinal(plain.getBytes(charset));
    // encoder#encode를 통한 byte -> string 변환
    return encoder.encode(encrypted);
  }
  ...
}
  ```

  객체 변수로 charset 항목이 UTF-8로 추가되었습니다. 그리고 encoder 객체도 default로 생성된 상태입니다.

  __입력받은 plain은 문자열 이기 때문에 문자인코딩 방식을 통해서 byte로 변환하겠습니다.__

  ```java
  // 문자인코딩 방식을 통한 string -> byte 변환
  plain.getBytes(charset)
  ```

  Java에서는 기본적으로 `String#getBytes(String charset)` 메서드가 있기 때문에 이것을 이용해서 bytes로 변환하였습니다.

  암호화 한 후 bytes 로 반환된 값을 문자열로 출력해야하는데 이럴 경우에는 바이트 인코딩 방식이 필요합니다. 위에서 미리 선언해 둔 `Encoder 인터페이스`를 통해서 변환하겠습니다.

  ```java
  // encoder.encode를 통한 byte -> string 변환
  encoder.decode(encrypted);
  ```

  이렇게 해서 1차적으로 암호화 부분 구현이 완료되었습니다.

  - `키 인코더 구현`

  ```java
public class StringEncoder implements Encoder {
  // 문자인코딩
  private final String charset;

  public StringEncoder(String charset) {
    this.charset = charset;
  }
  // string -> bytes
  public byte[] encode(String str) {
    return str.getBytes(charset);
  }
  // bytes -> string
  public String decode(byte[] bytes) {
    return new String(bytes, charset);
  }
}
  ```

  갑자기 매우 단순한 키 인코더 구현체가 나와서 당황스러울 듯 합니다. 하지만 키 인코딩의 경우 문자인코딩, 바이트인코딩 방식 둘 다 가능하기 때문에 위와 같이 Encoder 인터페이스를 통한 구현체를 제공하는 것이 다형성이 도움이 되기 때문에 이렇게 구성해보았습니다.

  지금의 예제는 ASCII 방식으로 암호화 키와 초기화 벡터를 제공하지만 이런 경우에는 키 bytes 구성이 아스키 기반으로 인해서 단순해 집니다. 고로 가능하면 __다양한 바이트 구성이 가능한 base64나 hex를 이용해서 키 bytes를 제공할 수 있게 하는 것이 보안에 유리합니다.__

  우선 예제를 위해서 위 StringEncoder를 사용하는 것으로 진행하겠습니다.

  _중요한 점은 위 인코더는 문자인코딩 방식이기 때문에 암호문 인코더로 사용하면 정상적인 암/복호화가 불가능 하게 됩니다._

  - `암호문 구현체에 키 인코더 주입 후 변경`

  ```java
  public class AES256Crypto implements Crypto {
    // 알고리즘/모드/패딩
    private static final String algorithm = "AES/CBC/PKCS5Padding";
    // 암호화 키
    private final String secretKey;
    // 초기화 벡터
    private final String iv;
    // 문자인코딩 방식
    private final String charset = "UTF-8";
    // 암호문 바이트 인코더
    private Encoder encoder = new Base64Encoder();
    // 키 인코더
    private Encoder keyEncoder = new StringEncoder("US-ASCII");
    ...

    // 암호화
    public String encrypt(String plain) {
      // 암호화 키 생성 - keyEncoder를 이용
      byte[] keyData = keyEncoer.encode(secretKey);
      SecretKey secureKey = new SecretKeySpec(keyData, "AES");

      Cipher c = Cipher.getInstance(algorithm);
      // 암호화 키 주입, iv 생성 주입, 초기화 - iv의 경우 keyEncodr를 이용
      c.init(Cipher.ENCRYPT_MODE, secureKey, new IvParameterSpec(keyEncoer(iv)));
      byte[] encrypted = c.doFinal(plain.getBytes(charset));
      return encoder.encode(encrypted);
    }
    ...
  }
  ```

  keyEncoder를 이용해서 암호화 키와 초기화 벡터를 bytes로 성공적으로 변환하였습니다. 여기에서 keyEncoder의 경우 ASCII charset을 기반으로 생성하였는데, 대부분의 경우 암호화 키를 예제(12345678901234567890123456789012)와 같이 영어와 숫자 또는 특수문자 기반으로 문자열을 제공하기 때문에 ASCII 만으로도 충분합니다.

  만약 한국어 등을 이용한다면 해당 문자를 표현할 수 있는 `charset(ex:UTF-8, EUC-KR)`로 변환해야 합니다만 그런 케이스는 거의 없을 것입니다.

  위에도 설명했지만 가능하면 __문자열 키도 base64 과 같은 바이트 인코딩 방식으로 제공하는 것이 보안에 좋습니다.__

  - `복호화 메소드 구현`

  ```java
  public class AES256Crypto implements Crypto {
  // 알고리즘/모드/패딩
  private static final String algorithm = "AES/CBC/PKCS5Padding";
  // 암호화 키
  private final String secretKey;
  // 초기화 벡터
  private final String iv;
  // 문자인코딩 방식
  private final String charset = "UTF-8";
  // 암호문 인코더
  private Encoder encoder = new Base64Encoder();
  // 키 인코더
  private Encoder keyEncoder = new StringEncoder("US-ASCII");
  ...

  // 복호화
  public String decrypt(String cipher) {
    // 암호화 키 생성 - keyEncoder를 이용
    byte[] keyData = keyEncoer(secretKey);
    SecretKey secureKey = new SecretKeySpec(keyData, "AES");

    Cipher c = Cipher.getInstance(algorithm);
    c.init(Cipher.DECRYPT_MODE, secureKey, new IvParameterSpec(keyEncoer(iv)));
    // encoder.decode 를 통해서 string -> bytes 변환
    byte[] encrypted = encoder.decode(cipher);
    // 복호화 후 문자인코딩 방식을 통한 byte -> string 변환 후 반환
    return new String(c.doFinal(encrypted), charset);
  }
}
  ```

  암호화와는 반대로 진행하는 것을 확인할 수 있습니다.

  - `암호화`
    1. String#getBytes(String)(문자 인코딩)를 이용해서 bytes로 변환
    2. 암호화
    3. encoder.encode(바이트 인코딩)를 이용해서 String으로 변환

  - `복호화`
    1. encoder.decode(바이트 디코딩)를 통해서 bytes로 변환
    2. 복호화
    3. new String(byte[], String)(문자 디코딩)을 이용해서 String으로 변환

  ![p7](/images/posts/201907/p7.jpg)

  _구현체에 중복코드가 많아서 약간의 리펙토링을 진행하고 전체코드를 보겠습니다._

  - `AES256Crypt 리펙토링`

  ```java
  public class AES256Crypto implements Crypto {
  public static final int KEY_SIZE = 256;
  public static final int BLOCK_SIZE = 128;
  private static final String AES = "AES";
  // 알고리즘/모드/패딩
  private static final String algorithm = AES + "/CBC/PKCS5Padding";
  // 암호화 키
  private final SecretKey secretKey;
  // 초기화 벡터
  private final IvParameterSpec iv;
  // 문자인코딩 방식
  private final String charset = "UTF-8";
  // 암호문 인코더
  private Encoder encoder = new Base64Encoder();
  // 키 인코더
  private Encoder keyEncoder = new StringEncoder("US-ASCII");

  public AESCrypto(String secretKey, String iv) {
    this.setSecretKey(secretKey);
    this.setIv(iv);
  }

  private void setSecretKey(String secretKey) {
    byte[] keyBytes = keyEncoder.encode(secretKey);
    if (keyBytes.length != KEY_SIZE / 8) {
      throw new IllegalArgumentException("'secretKey' must be "+ KEY_SIZE +" bit");
    }
    this.secretKey = new SecretKeySpec(keyBytes, AES);
  }

  private void setIv(String iv) {
    byte[] ivBytes = keyEncoder.encode(iv);
    if (ivBytes.length != BLOCK_SIZE / 8) {
      throw new IllegalArgumentException("'iv' must be "+ BLOCK_SIZE +" bit");
    }
    this.iv = new IvParameterSpec(keyEncoer(iv));
  }
  // 암호화
  public String encrypt(String plain) {
    Cipher c = Cipher.getInstance(algorithm);
    c.init(Cipher.ENCRYPT_MODE, this.secretKey, this.iv);
    byte[] encrypted = c.doFinal(plain.getBytes(charset));
    return encoder.encode(encrypted);
  }
  // 복호화
  public String decrypt(String cipher) {
    Cipher c = Cipher.getInstance(algorithm);
    c.init(Cipher.DECRYPT_MODE, this.secureKey, this.iv);
    byte[] encrypted = encoder.decode(cipher);
    return new String(c.doFinal(encrypted), charset);
  }
}
  ```

  - `사용 예시`

  ```java
public static Example {
  // 암호화 키
  static final String key = "12345678901234567890123456789012";
  // 초기화 벡터
  static final String iv = "1234567890123456";

  public static void main(String[] args) {
    // 암호화 객체 생성
    Crypto aes256 = new AES256Crypto(key, iv);

    String plain = "1234 가나다라 !@#$"
    // 암호화
    String cipher = aes256.encode(plain);

    // 출력
    System.out.println("plain = " + plain);
    System.out.println("cipher = " + cipher);

    // 복호화
    String plain2 = aes256.decode(cipher);
    System.out.println("plain2 = " + plain2);

    // 검증
    assert plain.equals(plain2);
  }
}
  ```

### 결론

  __다음과 같은 스팩을 바탕으로 암호화 방식을 공유하자.__

  - 암호화 알고리즘 : `AES`
    - key size : 256 bit
    - block size : 128 bit (AES의 블록사이즈는 128로 고정입니다. 고로 block size는 skip해도 됩니다.)
  - 암호화 모드 : `CBC`
  - Padding 방식 : `PKCS5`
  - 암호화키 : `12345678901234567890123456789012 (32자)`
    - 초기화벡터 - CBC모드일 경우 : `1234567890123456 (16자)`
  - 암호문 인코딩 방식 : `Base64(바이트인코딩) + UTF-8(문자인코딩)`
  - 키 인코딩 방식 - 암호문 인코딩과 다른경우 : `ASCII (문자인코딩, 바이트인코딩 둘 다 가능)`

## 참조

  > [http://redutan.github.io/2015/11/20/about-crypto](http://redutan.github.io/2015/11/20/about-crypto)
  >
  > [https://brownbears.tistory.com/73](https://brownbears.tistory.com/73)
  >
  > [https://cornswrold.tistory.com/102](https://cornswrold.tistory.com/102)
  >
  > [https://ko.wikipedia.org/wiki/SHA](https://ko.wikipedia.org/wiki/SHA)
