---
title: "@Transactional"
layout: post
category: Spring
tags: [Spring]
excerpt: "@Transactional 어노테이션에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > @Transactional 어노테이션에 대해서 배워 봅시다.

## @Transactional

  > @Transactional
  >
  > 자바 코드 내에서 Transaction 처리를 지원하는 어노테이션

  @Transactional의 동작 순서는 다음과 같습니다.

  1. Interface ClassLevel
  2. Interface MethodLevel
  3. 구현체 ClassLevel
  4. 구현체 MethodLevel

  1번부터 4번까지 모두 @Transactional 어노테이션이 처리 되었다면, 가장먼저 Interface의 클래스 위에 붙어있는 경우를 가장 먼저 찾고,그리고 Interface의 Method에 붙어있는 어노테이션을 찾습니다. Interface Method를 찾게 되면,
  1번에 적용된 @Transactional은 덮어씌워집니다. 그리고 3번을 찾고, 4번을 찾으면서 최종적으로 적용되는 @Transactional 어노테이션은 4번에 적용된 어노테이션 입니다.

  Interface에 @Transactional 적용이 되어있으면, 구현체에 적용하지 않아도, 구현체에서도 적용이 된 효과를 볼 수 있습니다.

### 읽기전용, CRUD 모든권한

  - @Transactional(readOnly=true) : 읽기전용(READ) : 조회(검색)전용
  - @Transactional(readOnly=false) = @Transactional: CRUD

  Interface나 Repository 구현체 클래스 위에 `@Transactional(readOnly=true)`를 선언하고, CRUD의 기능을 가져야하는 메서드에 따로 `@Transactional` 어노테이션을 붙입니다.
  메서드에 붙은 @Transactional 어노테이션이, 클래스에 붙은 @Transactional 어노테이션을 덮어씌워서, 메서드에 붙은 @Transactional 어노테이션을 실행 시킵니다.

### 역할

  예를들어서 `FileManageHistoryRepository.findFileHistoryByPath();`를 호출한다고 했을 때 다음과 같은 방식으로 동작합니다.

  1. MecFileManageHistoryRepository.findFileHistoryByPath(); 호출
  2. Spring Container가 중간에 가로채서 `Proxy 객체`를 호출
  3. Proxy 객체가 findFileHistoryByPath를 호출
  4. 스프링이 알아서 findFileHistoryByPath에 대한 `전, 후 트랜잭션 처리`를 함

  @Tramsactional은 `PlatformTransactionManager` 역할을 알아서 해주며, @Transactional을 달면 아래 코드와 같은 동작을 알아서 해줍니다.

  ```java
  DefaultTransactionDefinition def = new DefaultTransactionDefinition();

  // explicitly setting the transaction name is something that can only be done programmatically
  def.setName("SomeTxName");
  def.setPropagationBehavior(TransactionDefinition.PROPAGATION_REQUIRED);

  TransactionStatus status = txManager.getTransaction(def);
  try {
      // execute your business logic here
  	// Proxy 객체가 해당 메서드를 호출
  } catch (MyException ex) {
      txManager.rollback(status);
      throw ex;
  }

  txManager.commit(status);
  ```

  즉, 커밋, 롤백에 관한 모든 처리등을 하기 위해서는 원래 위와 같은 코드를 작성해야하는데, 스프링에서는 `@Transactional` 어노테이션을 작성하기만 하면
  알아서 트랜잭션에 관한 처리를 해줍니다.

### ServiceImple에서 @Transactional 사용하기

  예를들어 XXXServiceImpl의 메서드 한 곳에서, 여러개의 메서드를(@Transactional 어노테이션이 각각 다르게 적용된) 호출하여 DB작업을 하는경우(각각의 메서드는 Repository에서 @Transactional가 되어있습니다.)

  각각의 메서드는 서로 연관되어있기 때문에, 하나라도 실패시 커밋 롤백이 제대로 이루어지기 위해서 해당 메서드 위에 @Transactional를 다시 선언해서 묶어줍니다.

  특별한 상황이 아니라면 Repository의 추상 메서드에서만 `@Transactional` 어노테이션을 달아주면 되지만, 위 처럼 한 메서드 내에서 @Transactional이 적용된 여러 메서드를 호출해서 DB 작업을 하는 경우에는
  해당 메서드 위에도 @Transactional 처리를 해야 합니다.
