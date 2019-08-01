---
title: "MyBatis selectKey의 바인딩 효과"
layout: post
category: DataBase
tags: [MyBatis]
excerpt: "MyBatis에서 지원하는 selectKey의 역할에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > MyBatis에서 지원하는 selectKey의 역할에 대해서 배워 봅시다.

## MyBatis에서 지원하는 selectKey

  selectKey 쿼리태그는 `update와 insert문`에서 지원합니다.

### SEQ를 AUTOINCREMENT를 사용하지 않고 Mapper.xml에서 증가설정 하는 방법

```xml
  <insert id="createFile" parameterType="fileManageHistory">
    <selectKey keyProperty="seq" resultType="Integer" order="BEFORE">
      SELECT
      IFNULL(MAX(SEQ),0) + 1 AS SEQ
      FROM
      FILE_MANAGE_HISTORY
    </selectKey>
    INSERT INTO FILE_MANAGE_HISTORY (SEQ, PATH, CONTENT, TYPE, REG_ID, MOD_DATE)
    VALUES(#{seq},#{path},#{content},#{type},#{reg_id})
  </insert>
```

  `IFNULL(MAX(SEQ), 0) + 1`의 의미는 MAX(SEQ), SEQ의 MAX값이 NULL일 경우 0으로 세팅하며, 만약 0이 아닐경우
  해당 SEQ값을 가져와서 1을 증가시키라는 의미입니다.

  keyProperty의 키값은 `<insert id="createFile" parameterType="XXXVo">` 파라미터 타입의 VO 클래스에 있는
  속성명과 동일해야 합니다.

  order는 순서를 정할 수 있습니다. `before`는 insert쿼리 전에 실행, `after`는 이후에 실행

## MyBatis selectKey 태그를 사용한 SEQ 바인딩

  실무의경우 테이블에서 `SEQ(Sequence Number, AutoIncrement Number)` 컬럼을 가지고 있는 경우,
  (이 경우의 SEQ는 외래키가 아닌 기본키입니다. 즉, DB 테이블에 다른 값들을 넣어서 Insert 시키면, SEQ가 순차적으로 1부터 증가하는 경우)
  MySQL을 사용하는 경우에는 AutoIncrement 속성으로 SEQ를 자동증가 시킬 수 있지만, 실무에서는 AutoIncrement를 좋아하지 않습니다.

  왜냐하면, Oracle은 AutoIncrement 속성이 없기 때문입니다. 실무에서는 한가지 DB만 사용하는 것이아니라 여러가지를 사용할 수 있기 때문에,
  Mapper.xml도 사용하는 DB에 따라 여러개를 만들어 두기도 합니다.

  다시 본론으로 넘어가서 [SEQ를 AutoIncrement 속성없이 자동증가 시키는 방법](https://baekjungho.github.io/database-mybatisselectkey/#seq%EB%A5%BC-autoincrement%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%A7%80-%EC%95%8A%EA%B3%A0-mapperxml%EC%97%90%EC%84%9C-%EC%A6%9D%EA%B0%80%EC%84%A4%EC%A0%95-%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95)은 위에 설명한 내용과 같습니다.

  개발을 하다보면 __컨트롤러에서 A 테이블에 속성들을 Insert시키고, A테이블의 SEQ(PrimaryKey)값을 가져와서, B 테이블에 외래키(FK)로 삽입해야하는 경우가 있습니다.__
  이때 일반적인 상식으로는, A 테이블에 Insert를 했으니, Insert를 시켰던 속성들을가지고 A 테이블의 SEQ를 가져오는 쿼리문과, 코드를 짤 것이며, SEQ를 가져와서
  B 테이블에 삽입하는 방식을 취할 것입니다.

  하지만, MyBatis의 `selectKey 쿼리태그`를 사용한다면, A 테이블에 Insert를 시킴과 동시에, DB 테이블에 생성된 `SEQ값을 VO에 자동 바인딩` 시켜줍니다.

  - EXAMPLE

  ```java
  public String createXXX(@ModelAttribute("xxxVo") XXXVo xxxVo, @ModelAttribute("zzzVo") ZZZVo zzzVo) {
    .....
    XXXService.createXXX(xxxVo); // create하는 순간 xxxVo의 seq값이 DB에 생성되고, VO에 바인딩이됨
    zzzVo.setSeq(xxxVo.getSeq()); // 바인딩된 seq값을 가져와서 zzzVo의 속성에 세팅
    ZZZService.createZZZ(zzzVo); // zzzVo DB에 삽입
  }
  ```

  개인적으로 저는 이 부분에 대해서 배울때 너무 신기했습니다.

  자동 바인딩이 되기 위해서는 selectKey의 `keyProperty="seq"`가 insert 쿼리태그에 있는 parameterType의 VO 클래스에 있는 `seq 속성명`과 동일해야합니다.
  위 XML코드를 보면 사실상 쿼리가 2번 실행되는 것입니다. selectKey가 먼저 실행되고, 생성된 SEQ값을 VO 속성에 반환하여 바인딩을 시켜주고, insert 쿼리문을 실행합니다.

  이렇게 되면, 불필요하게 컨트롤러에서 A 테이블의 SEQ값을 구하는 코드를 작성하지 않아도 됩니다.
