## 7주차 점검 키워드 

### hee
- JPQL enum 타입 표현 
```sql
// 하드 코딩 하는 경우,
String query = "select m.username, 'HELLO', true from Member m " + 
                "where m.type = jpql.MemberType.USER";
// binding 하는 경우,
String query = "select m.username, 'HELLO', true from Member m " + 
                "where m.type = :userType";

List<Object[]> result = em.createQuery(query)
        .setParameter("userType", MemberType.ADMIN)
        .getResultList();
```

### namjunemy
- JPQL 쿼리 짤 때, 자바 코드에서 멤버 테이블이 있구나 보다 멤버 객체가 있구나라는 생각을 가지고 개발해야 한다.
  - from절의 Member는 객체다.
- 운영 측면에서 Criteria보단 QueryDSL을 권장한다. SQL스럽지 않아서 유지보수가 어렵고 복잡하다. 
- 선 JPQL, 후 QueryDSL
- 프로젝션으로 select 절에 조회할 대상을 지정. DTO로 바로 조회
  ```java
  List<MemberDTO> members =
  em.createQuery("select new jpql.dto.MemberDTO(m.name, m.age) from Member m", MemberDTO.class)
  .getResultList();
  ```
  - QueryDSL에서는 `Projections.bean()` 으로 해결 가능
  ```java
  List<ContentResponse> results = this.queryFactory.select(Projections.bean(ContentResponse.class,
                content.idx.as("contentIdx"),
                content.title.as("contentTitle"),
                channel.contentProvider.name.as("cpName"),
                content.updatedTime))
                .from(content)
                .orderBy(content.updatedTime.desc())
                .fetch();
  ```
- 서브쿼리 보다는 아래의 방법들을 우선 고려 해보자
  - Join으로 해결할 수 있는지
  - 비즈니스로직으로 풀어낼 수 있는지
  - 쿼리를 두 번 실행해서 해결할 수 있는지
  - [서브쿼리의 한계](https://github.com/namjunemy/TIL/blob/master/Jpa/inflearn/11_jpql.md#jpa-%EC%84%9C%EB%B8%8C-%EC%BF%BC%EB%A6%AC%EC%9D%98-%ED%95%9C%EA%B3%84)
  - https://jojoldu.tistory.com/379
### integerous


---
### :house: [Home](https://github.com/team-zunior/orm-jpa-basic)
