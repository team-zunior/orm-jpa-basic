## 2주차 점검 키워드 

### heee
-

### namjunemy
-

### integerous
- `GenerationType.IDENTITY`
  - 기본키 매핑 전략 중 IDENTITY의 경우, 예외적으로 `entityManager.persist()`를 호출하는 시점에 즉시 INSERT SQL을 실행하고 식별자를 조회한다.
- `GenerationType.SEQUENCE` 
  - DB에서 Sequence를 가져와서 1차캐시의 @Id에 값을 넣어주고 난 후에 `entityManager.persist()`가 호출된다.
  - 기본값이 50인 `allocationSize` 속성을 사용하여 성능최적화에 사용되는데,
    - 강의에서 DB에 50개를 한 번에 올려놓고 메모리에서 1개씩 쓴다고 설명했는데, 메모리에서 1개씩 쓴다는 표현이 잘 이해가 안된다.
---

### :house: [Home](https://github.com/team-zunior/orm-jpa-basic)
