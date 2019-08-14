## 2주차 점검 키워드 

### hee
- Q. id 값(pk 값)은 어떤 타입을 써야 할까?
  - int 안되는 이유?
- IDENTITY 전략은 entityManager.persist() 시점에 즉시 INSERT SQL을 실행하고 DB에서 식별자를 조회한다.
    - JPA는 보통 트랜잭션 commit 시점에 INSERT SQL을 실행한다.
    - [link](https://gmlwjd9405.github.io/2019/08/12/primary-key-mapping.html)
- sequence object 의 의미?
- Q. Team.members를 주인으로 선택하고 Team의 List members를 변경했다. 그 결과는?
  - A. 다른 테이블인 Member의 UPDATE Query가 날라간다..?
  - [link](https://gmlwjd9405.github.io/2019/08/14/bidirectional-association.html)


### namjunemy
- DB 스키마 자동 생성
  - hibernate.hbm2ddl.auto 옵션에서 UPDATE는 추가만 가능하다. 데이터나 스키마를 지우지는 않는다. [Link](https://github.com/namjunemy/TIL/blob/master/Jpa/inflearn/05_entity_mapping.md#데이터베이스-스키마-자동-생성)
    - 그러나 위험..
    - https://github.com/HomoEfficio/dev-tips/blob/master/hibernate.hbm2ddl.auto%20%EC%9C%84%ED%97%98%20%ED%97%B7%EC%A7%80.md#hibernatehbm2ddlauto-%EC%9C%84%ED%97%98-%ED%97%B7%EC%A7%80
    - http://egloos.zum.com/gyumee/v/2483659
  - 결론은 다수가 사용하는 스테이징, 운영(테스트서버 포함)에서는 ddl-auto 사용하지 말자. 사용하더라도 자동 생성된 SQL을 다듬어서 직접 테스트 서버에 검증해보고 스테이지나 운영에 사용하자.
  - 근본적으로 웹 어플리케이션에서 사용하는 DB계정은 운영 장비에서 alter나, drop 못하도록 계정 자체를 분리하는 것이 좋다.
    - 이렇게 하면 JPA가 테이블 날리는 일은 발생하지 않는다.
  - 어플리케이션 실행시점에 VM 옵션을 주는 것이 application.yml파일 보다 우선순위 높으므로 상황에 따라서 `-Dspring.jpa.properties.hibernate.hbm2ddl.auto=none` 옵션을 주고 실행 시키는 것도 효과적일 수 있음.
- 필드-컬럼 매핑 어노테이션 [Link](https://github.com/namjunemy/TIL/blob/master/Jpa/inflearn/05_entity_mapping.md#필드와-컬럼-매핑)
  - @Column의 precision, scale
  - 유니크 제약조건을 복수로 주고싶다면, @Table의 uniqueConstrains 옵션
- 기본키 매핑 IDENTITY 전략의 특징 [Link](https://github.com/namjunemy/TIL/blob/master/Jpa/inflearn/05_entity_mapping.md#기본-키-매핑)
  ```java
  //팀 저장. 팀의 ID Generate 전략 = IDENTITY
  Team team = new Team();
  team.setName("TeamA");
  em.persist(team); // 영속 상태가 됨. DB들려서 가져옴. PK값이 세팅 된 상태.

  //회원 저장
  Member member = new Memeber();
  member.setName("member1");
  member.setTeamId(team.getId()); // PK값이 세팅 됐으므로, getId() 가능.
  em.persist(member);
  ```
- SEQUENCE 전략의 allocationSize - 성능 최적화, 동시성 보장
  - [allocationSize 속성 설명 및 사용시 주의사항](https://dololak.tistory.com/479)

- 양방향 매핑시 @OneToMany쪽의 List 컬렉션을 ArrayList로 초기화 하는것은 관례 [Link](https://github.com/namjunemy/TIL/blob/master/Jpa/inflearn/07_relational_mapping.md#%EC%96%91%EB%B0%A9%ED%96%A5-%EC%97%B0%EA%B4%80%EA%B4%80%EA%B3%84%EC%99%80-%EC%97%B0%EA%B4%80%EA%B4%80%EA%B3%84%EC%9D%98-%EC%A3%BC%EC%9D%B8)

### integerous
- `GenerationType.IDENTITY`
  - 기본키 매핑 전략 중 IDENTITY의 경우, 예외적으로 `entityManager.persist()`를 호출하는 시점에 즉시 INSERT SQL을 실행하고 식별자를 조회한다.
- `GenerationType.SEQUENCE` 
  - DB에서 Sequence를 가져와서 1차캐시의 @Id에 값을 넣어주고 난 후에 `entityManager.persist()`가 호출된다.
  - 기본값이 50인 `allocationSize` 속성을 사용하여 성능최적화에 사용되는데,
    - 강의에서 DB에 50개를 한 번에 올려놓고 메모리에서 1개씩 쓴다고 설명했는데, 메모리에서 1개씩 쓴다는 표현이 잘 이해가 안된다.
---

### :house: [Home](https://github.com/team-zunior/orm-jpa-basic)
