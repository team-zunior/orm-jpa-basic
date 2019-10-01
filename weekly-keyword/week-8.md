## 8주차 점검 키워드 

### hee
- named query 
  - [spring data jpa](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.query-methods.at-query)의 interface method의 `@Query` 에서도 named query 와 같이 로딩 시점에 미리 파싱되어 문법을 점검해준다.
  - 이름없는 named query 라고 부른다.
  ```java
  public interface UserRepository extends JpaRepository<User, Long> {
    @Query("select u from User u where u.emailAddress = ?1")
    User findByEmailAddress(String emailAddress);
  }
  ```
- 벌크 연산 주의 
  - 해결: **벌크 연산 수행 후 영속성 컨텍스트 초기화** `em.clear()`
  - 영속성 컨텍스트의 내용과 DB에 반영된 내용이 일치하지 않는 문제가 발생할 수도 있기 때문
  - e.g. [Modifying Queries](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.modifying-queries)


### namjunemy
- 경로 표현식

  ```java
  select m.username -> 상태 필드
  from Member m
  join m.team t   -> 단일 값 연관 필드
  join m.orders o -> 컬렉션 값 연관 필드
  where t.name = '팀A'
  ```
  - 묵시적 내부 조인이 발생하도록 JPQL 작성하지 말자. 조인은 최대한 직관적이어야 한다. 명시적으로 join 키워드 사용 권장.
- 컬렉션 페치 조인(일대다 관계)시 데이터 뻥튀기 문제 주의 [참고](https://github.com/namjunemy/TIL/blob/master/Jpa/inflearn/12_jpql2.md#%EC%BB%AC%EB%A0%89%EC%85%98-%ED%8E%98%EC%B9%98-%EC%A1%B0%EC%9D%B8)
  - 엔티티 입장에서 일대다를 예로 들어보면, TEAM 조회하면서 Members를 페치 조인하는 상황에서 TEAM 1개 MEMBER 5명을 페치 조인시 row가 5개 생긴다.
- 페치 조인과 DISTINCT
  - JPQL의 distinct는 2가지 기능을 제공한다. SQL에서 DISTINCT추가, 애플리케이션에서 중복 제거 시도
  - SQL에서의 DISTINCT는 완전히 동일한 row만 중복 제거한다.
  - 애플리케이션에서 같은 식별자를 가진 일대다의 일쪽 엔티티를 가진 row를 중복 제거해준다. 따라서 원하는 결과를 얻을 수 있다.
    - [스터디로그](https://github.com/namjunemy/TIL/blob/master/Jpa/inflearn/12_jpql2.md#%ED%8E%98%EC%B9%98-%EC%A1%B0%EC%9D%B8%EA%B3%BC--distinct)
  - **그런데 주의하자....**
    - QueryDSL에서 select().distinct() 또는 selectDistinct()로 DISTINCT 사용시
    - 쿼리에서 distinct가 추가되면서 Using temporary(중복제거시 가상 테이블 생성) 실행계획 사용.
      - https://dotoridev.tistory.com/2
    - 쿼리 튜닝시 Using temporary는 extra(실행 계획)에서 피해야 한다.
      - [MYSQL 실행 계획 분석시 주의사항](https://12bme.tistory.com/168)
    - 정리
      - 일대다 관계 조인시 중복결과가 존재한다.
      - QueryDSL에서 중복제거를 목적으로 distinct()를 사용하면 Using Temporary 이슈가 있으므로 주의하자
      - QueryDSL에서 중복된 결과를 받은 다음에 애플리케이션 로직에서 stream의 distinct() 으로 중복 제거하자.
### integerous


---
### :house: [Home](https://github.com/team-zunior/orm-jpa-basic)
