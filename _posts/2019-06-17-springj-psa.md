---
title: "PSA"
layout: post
category: Spring
tags: [Spring]
excerpt: "Portable Service Abstraction(PSA)에 대해서 배워 봅시다."
author: BAEKJungHo
---

* content
{:toc}

## 목표

  > Portable Service Abstraction(PSA)에 대해서 배워 봅시다.

## PSA

  > PSA(Portable Service Abstraction)는 매우 잘 만든 인터페이스를 뜻합니다. 일관성 있는 서비스 추상화라고도 합니다.

  스프링에서 제공하는 대부분의 API는 PSA입니다.

  서비스 추상화의 예로 JDBC를 들 수 있습니다. JDBC라고 하는 표준 스펙이 있기에 오라클을 사용하든, MySQL을 사용하든, `Connection, Statement, ResultSet`을 이용해서 공통된 방식으로 코드를 작성할 수 있습니다.
  데이터베이스 종류에 관계없이 같은 방식으로 제어할 수 있는 이유는 `어댑터 패턴(Adapter Pattern)`을 활용했기 때문입니다.

  __이처럼 어댑터 패턴(Apdater Pattern)을 적용해 같은 일을 하는 다수의 기술을 곧통의 인터페이스로 제어할 수 있게 한 것을 서비스 추상화(PSA)라고 합니다.__

## 트랜잭션

  > [Platform TransactionManager](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/PlatformTransactionManager.html)

  - @Transactional

  다음 소스는 [Spring-PetClinic](https://baekjungho.github.io/spring-petclinic/)에 있는 코드 입니다.

  ```java
  public interface OwnerRepository extends Repository<Owner, Integer> {

    /**
     * Retrieve {@link Owner}s from the data store by last name, returning all owners
     * whose last name <i>starts</i> with the given name.
     * @param lastName Value to search for
     * @return a Collection of matching {@link Owner}s (or an empty Collection if none
     * found)
     */
    @Query("SELECT DISTINCT owner FROM Owner owner left join fetch owner.pets WHERE owner.lastName LIKE :lastName%")
    @Transactional(readOnly = true)
    Collection<Owner> findByLastName(@Param("lastName") String lastName);

    /**
     * Retrieve an {@link Owner} from the data store by id.
     * @param id the id to search for
     * @return the {@link Owner} if found
     */
    @Query("SELECT owner FROM Owner owner left join fetch owner.pets WHERE owner.id =:id")
    @Transactional(readOnly = true)
    Owner findById(@Param("id") Integer id);

    /**
     * Save an {@link Owner} to the data store, either inserting or updating it.
     * @param owner the {@link Owner} to save
     */
    void save(Owner owner);
  }
  ```

## 캐시

  > [CacheManager](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/cache/CacheManager.html)

  캐시도 트랜잭션과 마찬가지로 `Manager`를 사용합니다. @Cacheable, @CacheEvict ..... 등 어노테이션을 처리하는 `Aspect`가 있고
  그리고 Aspect에서는 Manager를 사용합니다.

  ```java
  @Configuration
  @EnableCaching
  class CacheConfiguration {

    @Bean
    public JCacheManagerCustomizer petclinicCacheConfigurationCustomizer() {
        return cm -> {
            cm.createCache("vets", cacheConfiguration());
        };
    }

    /**
     * Create a simple configuration that enable statistics via the JCache programmatic configuration API.
     * <p>
     * Within the configuration object that is provided by the JCache API standard, there is only a very limited set of
     * configuration options. The really relevant configuration options (like the size limit) must be set via a
     * configuration mechanism that is provided by the selected JCache implementation.
     */
    private javax.cache.configuration.Configuration<Object, Object> cacheConfiguration() {
        return new MutableConfiguration<>().setStatisticsEnabled(true);
    }
  }
  ```

## 참조

  > [인프런 백기선님의 스프링 프레임워크 입문](https://www.inflearn.com/course/spring/dashboard)
