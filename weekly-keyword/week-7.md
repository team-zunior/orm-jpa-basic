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


### integerous


---
### :house: [Home](https://github.com/team-zunior/orm-jpa-basic)
