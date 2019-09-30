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


### integerous


---
### :house: [Home](https://github.com/team-zunior/orm-jpa-basic)
