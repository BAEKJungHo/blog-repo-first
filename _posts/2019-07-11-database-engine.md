---
title: "MySQL Engine(Server, Storage)"
layout: post
category: DataBase
tags: [MySQL]
excerpt: "MySQL DDL 생성시 지정하는 Engine에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > MySQL DDL 생성시 지정하는 Engine에 대해서 배워 봅시다.

## MySQL Engine

  > MySQL Engine Document : [https://dev.mysql.com/doc/refman/5.7/en/storage-engine-setting.html](https://dev.mysql.com/doc/refman/5.7/en/storage-engine-setting.html)

  MySQL은 크게 아래의 2가지 구조로 되어 있습니다.

  - __서버 엔진(Server Engine)__ : 클라이언트(또는 사용자)가 Query를 요청했을때, Query Parsing과, 스토리지 엔진에 데이터를 요청하는 작업을 수행합니다.

  - __스토리지 엔진(Storage Engine)__ : 물리적 저장장치에서 데이터를 읽어오는 역할을 담당합니다.

  여기서 중점적으로 봐야할 것은 `스토리지 엔진` 입니다. 그 이유는 __데이터를 직접적으로 다루는 역할을 하므로 엔진 종류 마다 동작원리가 다르고, 따라서 트랜잭션, 성능과 같은 주요 이슈에도 밀접하게 연관__ 되어 있기 때문이다.

  MySQL의 스토리지 엔진은 `Plug in` 방식이며, 기본적으로 8가지의 스토리지 엔진이 탑재 되어 있습니다. 아래의 명령으로도 탑재된 스토리지 엔진을 확인 할 수 있습니다.

  ```sql
  SHOW ENGINES;
  ```

  위에서 말한 것과 같이 Plug in 방식 이기때문에 스토리지 엔진 교체 작업이 비교적 간단합니다. 또한 기본 탑재 플러그인 외에 서드파티에서 제공하는 다양한 스토리지 엔진을 손쉽게 적용할 수 있다는 것이 장점입니다.

  각 플러그인의 `'.so'(Shared Object)` 파일을 얻었다면 아래와 같이 설치/삭제 할 수 있습니다.

  ```sql
  INSTALL PLUGIN ha_example SONAME 'ha_example.so'
  ```

  ```sql
  UNINSTALL PLUGIN ha_example;
  ```

  그리고 `CREATE TABLE` 문을 사용하여 테이블을 생성할 때, 맨 마지막 구문에 스토리지 엔진의 이름을 추가함으로 아주 간단하게 설정 할 수 있습니다. 아래와 같습니다.

  ```sql
-- ENGINE=INNODB not needed unless you have set a different
-- default storage engine.
CREATE TABLE t1 (i INT) ENGINE = INNODB;
-- Simple table definitions can be switched from one to another.
CREATE TABLE t2 (i INT) ENGINE = CSV;
CREATE TABLE t3 (i INT) ENGINE = MEMORY;
  ```

## Storage Engine

  > Storage Engine 대표적인 3가지 : InnoDB, MyISAM, Archive

  MySQL 기본엔진의 종류는 버전에 따라 다릅니다. MySQL 버전이 5.5미만이면 `MyISAM`이 MySQL의 기본 엔진이고, MySQL 5.5부터는 `InnoDB`가 기본 엔진입니다.

### ENGINE=InnoDB

  InnoDB는 따로 스토리지 엔진을 명시하지 않으면 default 로 설정되는 스토리지 엔진입니다. InnoDB는 transaction-safe 하며, 커밋과 롤백, 그리고 데이터 복구 기능을 제공하므로 데이터를 효과적으로 보호 할 수 있습니다.

  - 트랜젝션 지원
  - 빈번한 쓰기, 수정, 삭제시 처리 능력
  - 디스크, 전원 등의 장애 발생시 복구 성능
  - 동시처리가 많은 환경에 적합
  - Row 단위 락킹

  > InnoDB는 기본적으로 row-level locking 제공하며, 또한 데이터를 clustered index에 저장하여 PK 기반의 query의 I/O 비용을 줄입니다. 또한 FK 제약을 제공하여 데이터 무결성을 보장합니다.

### ENGINE=MyISAM

  트랜잭션을 지원하지 않고 table-level locking을 제공합니다. 따라서 multi-thread 환경에서 성능이 저하 될 수 있습니다. 특정 세션이 테이블을 변경하는 동안 테이블 단위로 lock이 잡히기 때문입니다.

  - 상대적으로 높은 성능
  - 읽기 위주의 요청에 유리
  - 테이블 단위 락킹

  > 텍스트 전문 검색(Fulltext Searching)과 지리정보 처리 기능도 지원되는데, 이를 사용할 시에는 파티셔닝을 사용할 수 없다는 단점이 있습니다.

### ENGINE=Archive

  '로그 수집'에 적합한 엔진입니. 데이터가 메모리상에서 압축되고 압축된 상태로 디스크에 저장이 되기 때문에 row-level locking이 가능합니다.

  다만, 한번 INSERT된 데이터는 UPDATE, DELETE를 사용할 수 없으며 인덱스를 지원하지 않습니다. 따라서 거의 가공하지 않을 원시 로그 데이터를 관리하는데에 효율적일 수 있고, 테이블 파티셔닝도 지원합니다. 다만 트랜잭션은 지원하지 않습니다.
