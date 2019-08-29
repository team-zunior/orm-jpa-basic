## 4주차 점검 키워드 

### hee


### namjunemy


### integerous
- 프록시 객체를 호출하면 프록시 객체는 실제 객체의 메서드 호출
  - 프록시 객체(MemberProxy)가 보관하고 있는 실제 객체(Member)의 참조(Member target)를 이용
  - `target.실제객체메서드()`
- JPA는 같은 트랜잭션 안에서 `==` 비교시 True를 보장
  - 영속성 컨텍스트에 찾는 Entity가 이미 있으면, `em.getReference()`를 호출해도 실제 Entity를 반환
  - `em.getReference()` 이후에 `em.find()`를 하면 둘 다 프록시 객체를 반환
  - 객체 동일성을 보장하는 이유
    - 실제 개발할 때 프록시 객체인지 실제 객체인지 신경쓰지 않고 개발할 수 있도록 ??
- N+1 문제 해결법
  - https://jojoldu.tistory.com/165
- 도메인 주도 설계(DDD)의 Aggregate Root 개념
  - https://medium.com/@SlackBeck/%EC%95%A0%EA%B7%B8%EB%A6%AC%EA%B2%8C%EC%9E%87-%ED%95%98%EB%82%98%EC%97%90-%EB%A6%AC%ED%8C%8C%EC%A7%80%ED%86%A0%EB%A6%AC-%ED%95%98%EB%82%98-f97a69662f63
  - 강의에서 `CascadeType.ALL + orphanRemoval=true`를 사용하면 Aggregate Root 개념을 구현할 때 유용하다고 함.
---
### :house: [Home](https://github.com/team-zunior/orm-jpa-basic)
