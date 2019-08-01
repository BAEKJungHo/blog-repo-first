---
title: "YAML"
layout: post
category: YAML
tags: [YAML]
excerpt: "야믈(YAML)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > YAML에 대해서 배워 봅시다.

## YAML

  > YAML(YAML Ain't Markup Language) : YAML is a human-readable data serialization language.

  YAML이란 스프링이랑 스프링 부트에서 XML과 JSON을 장점을 가지고 환경설정파일에서 XML의 대체자로 떠오르는 언어입니다.

  YAML의 원시 데이터 구조에는 노드와 같은 간단한 표현이 포함됩니다. Representation Node Graph라고도합니다. 여기에는 직렬화 트리를 만들기 위해 직렬화되는 매핑, 시퀀스 및 스칼라 수량이 포함됩니다. 직렬화를 통해 객체는 바이트 스트림으로 변환됩니다. 직렬화 이벤트 트리를 사용하면 다음 다이어그램과 같이 문자 스트림을 표현할 수 있습니다.

  역순은 바이트 스트림을 직렬화 된 이벤트 트리로 구문 분석합니다. 나중에 노드는 노드 그래프로 변환됩니다. 이 값은 나중에 YAML 기본 데이터 구조로 변환됩니다. 아래 그림은 이것을 설명합니다.

  ![a1](/images/posts/201908/a1.jpg)

  YAML의 정보는 `기계 처리` 및 `사람의 소비` 라는 두 가지 방식으로 사용됩니다 . YAML의 프로세서는 위에 주어진 다이어그램의 보완 뷰 사이에서 정보를 변환하는 절차를위한 도구로 사용됩니다.

  YAML은 데이터 객체를 직렬 형식으로 표현하기위한 직렬화 절차를 포함합니다. YAML 정보 처리에는 `표현, 직렬화, 표현 및 파싱`의 세 단계가 포함됩니다.

## 개요

  - 데이터 교환 형식의 하나
  - 사람이 읽을 수 있도록 정한 데이터 직렬화 형식
  - C, Perl, PHP, Python, 루비, 자바스크립트 등에서 사용가능
  - 모든 데이터를 리스트, 해쉬, 스칼라 데이터의 조합으로 표현
  - 확장자: .yaml, .yml
  - 첫 릴리즈: 2001년

## 특징

  JSON이 YAML의 부분집합입니다. 즉, YAML 파서는 JSON을 읽을 수 있습니다. 또한, YAML 파일에 JSON 형식을 섞어 쓸 수 있습니다.

  - 한 줄 주석 : `#`
  - python처럼 들여쓰기를 사용해 구조체를 만듬

### list

  list를 표현하는것은 아래와 같이 사용

```
- foo
- bar
```

  한 줄로 나타내는 방법도 있는데, JSON이랑 똑같이 하면 됩니다. (따옴표는 생략가능 but 특수문자 등을 이용할 때는 따옴표 사용)

```
- [foo, bar]
```

  - EXAMPLE

```
--- # Favorite movies, block format
- Casablanca
- Spellbound
- Notorious
--- # Shopping list, inline format
[milk, bread, eggs]
```

### map

  map은 아래와 같이 사용합니다.

```
person:
  name: BAEKJH
  age:26
  hobby:
    - basketball
    - workout
```

  위 내용을 JSON 형식으로 바꾸면 아래와 같습니다.

```
{
  "person": {
    "name": "BAEKJH",
    "age": 26,
    "hobby" : ["basketball", "workout"]
  }
}
```

### literal block

  - `|`를 사용하면 개행을 유지하는 리터럴 블록을 사용할 수 있습니다.

```
text_multi_line: |
이렇게 쓰면 개행이 유지된다.
즉, 이 라인은 두 번째 줄이 된다.
리터럴 블록은 인덴트로 구별한다.
인덴트가 앞으로 돌아가면 블록이 끝나는 것이다.
리터럴 블록은 여기까지.
  ```

  - `>`를 사용하면 개행을 무시하는 리터럴 블록을 사용할 수 있습니다.

```
text_single_line: >
이렇게 쓰면 개행이 무시된다.
즉, 이 라인이 윗줄과 합쳐져 한 줄이 되는 것이다.
각 라인의 개행 문자는 삭제되며, 스페이스로 교체된다.
```

### inherit

  상속은 아래와 같이 표현할 수 있습니다.

```
bird: &default_bird
cry: 짹짹
fly: true
color: white

참새:
<<: *default_bird
color: brown

오리:
<<: *default_bird
cry: 꽉꽉

닭:
<<: *default_bird
cry: 꼬끼오
fly: false
```

## RePresentation

  YAML은 `시퀀스, 매핑 및 스칼라` 의 세 가지 노드를 사용하는 데이터 구조를 나타냅니다.

### 시퀀스

  `시퀀스`는 키 값 쌍의 정렬되지 않은 연결을 매핑하는 정렬 된 항목 수를 나타냅니다. Perl 또는 Python 배열 목록에 해당합니다.

  아래에 표시된 코드는 시퀀스 표현의 예입니다.

  ```yaml
  product:
   - sku         : BL394D
     quantity    : 4
     description : Football
     price       : 450.00
   - sku         : BL4438H
     quantity    : 1
     description : Super Hoop
     price       : 2392.00
  ```

### 매핑

  반면에 `매핑`은 사전 데이터 구조 또는 해시 테이블을 나타냅니다. 같은 예가 아래에 언급되어 있습니다.

  ```
batchLimit: 1000
threadCountLimit: 2
key: value
keyMapping: <What goes here?>
  ```

### 스칼라

  스칼라는 문자열, 정수, 날짜 및 원자 데이터 유형의 표준 값을 나타냅니다. YAML에는 데이터 유형 구조를 지정하는 노드도 포함됩니다.

## 직렬화(Serialization)

  인간 친화적인 키 순서 및 앵커 이름을 쉽게 만드는 YAML의 직렬화 프로세스가 필요합니다. 직렬화의 결과는 `YAML 직렬화 트리`입니다. YAML 데이터의 일련의 이벤트 호출을 생성하기 위해 탐색 될 수 있습니다.

  직렬화에 대한 예제가 아래에 나와 있습니다.

  ```yaml
  consumer:
   class: 'AppBundle\Entity\consumer'
   attributes:
      filters: ['customer.search', 'customer.order', 'customer.boolean']
   collectionOperations:
      get:
         method: 'GET'
         normalization_context:
       groups: ['customer_list']
   itemOperations:
      get:
         method: 'GET'
         normalization_context:
            groups: ['customer_get']
  ```

## Presentation

  YAML 직렬화의 최종 출력을 `프리젠테이션` 이라고합니다. 이는 인간 친화적 인 방식으로 문자 스트림을 나타냅니다. YAML 프로세서는 스트림 생성, 들여 쓰기 및 포맷팅을 위한 다양한 프리젠테이션 세부 정보를 포함합니다. 이 과정은 사용자의 선호도에 따라 진행됩니다.

  YAML 프리젠테이션 프로세스의 예는 JSON 값을 만든 결과입니다.

  ```json
  {
   "consumer": {
      "class": "AppBundle\\Entity\\consumer",
      "attributes": {
         "filters": [
            "customer.search",
            "customer.order",
            "customer.boolean"
         ]
      },
      "collectionOperations": {
         "get": {
            "method": "GET",
            "normalization_context": {
               "groups": [
                  "customer_list"
               ]
            }
         }
      },
      "itemOperations": {
         "get": {
            "method": "GET",
            "normalization_context": {
               "groups": [
                  "customer_get"
               ]
            }
         }
      }
   }
}
  ```

## 파싱

  구문 분석은 표현의 역 과정입니다. 문자 스트림을 포함하고 일련의 이벤트를 생성합니다. 직렬화 이벤트를 발생시키는 프리젠 테이션 프로세스에서 소개 된 세부 사항을 버립니다. 구문이 잘못된 입력으로 인해 구문 분석 절차가 실패 할 수 있습니다. 기본적으로 YAML의 형식이 올바른지 여부를 확인하는 절차입니다.

  ```
  ---
   environment: production
   classes:
      nfs::server:
         exports:
            - /srv/share1
            - /srv/share3
   parameters:
      paramter1
  ```

  세 개의 하이픈을 사용하여 나중에 문서에서 정의 할 다양한 속성이있는 문서의 시작을 나타냅니다.

  YAML lint는 YAML의 온라인 구문 분석기이며 YAML 구조를 구문 분석하여 유효한지 여부를 확인하는 데 도움을줍니다.

  > YAML 린트의 공식 링크 : [http://www.yamllint.com/](http://www.yamllint.com/)

## 참조

  > [https://johngrib.github.io/wiki/YAML/](https://johngrib.github.io/wiki/YAML/)
  >
  > [https://zetawiki.com/wiki/%EC%96%98%EB%A9%80_YAML](https://zetawiki.com/wiki/%EC%96%98%EB%A9%80_YAML)
  >
  > [https://www.tutorialspoint.com/yaml/yaml_processes.htm](https://www.tutorialspoint.com/yaml/yaml_processes.htm)
