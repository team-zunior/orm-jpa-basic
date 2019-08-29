## 4주차 점검 키워드 

### hee


### namjunemy
- 프록시 객체의 초기화는 직접 참조를 했을 때 일어난다.
- 프록시 초기화는 첫 참조에서만
  - 그럼 컬렉션의 경우 첫번째 객체 터치할때 프록시 초기화 일어나고, 두번째 객체 부터는 target -> 실제 엔티티 바로 호출
- 실무에서는 @Transactional의 범위 밖에서 프록시 객체를 사용하는 것을 주의하자
  - 실제로, Service Layer에 @Transactional을 붙여서 쓰는데 아래와 같은 상황에서는 어떻게 될까
  
    ```java
    @Controller
    public class MemberController {
      
      private MemberService memberService;
      
      public MemberController(Memberservice memberService) {
        this.memberService = memberService;
      }
      
      @RequestMapping("/member/{id}")
      public String test(@PathVariable("id") Long id) {
      
        Member member = memberService.getMember(id); // 서비스 레이어 내에서 트랜잭션이 열리고 닫힌다.
        List<Order> orders = member.getOrders(); // LazyInitializationException 터진다.
      
        ...
      }
    }
    ```
  - 근데, 스프링 부트 어플리케이션에서 해보면 에러 안난다. ????
    - 부트는 기본적으로 `spring.jpa.open-in-view=true` 옵션설정을 가져간다. [spring boot common-application-properties](https://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html)
    - 영속성 컨텍스트를 서비스 레이어를 넘어서 뷰 렌더링 시점까지 유지시키는 방법이다. ([이쯤에서 킹 소환](https://kingbbode.tistory.com/27))
    - 그런데, 뷰 렌더링 시점까지 트랜잭션을 유지한다는 얘기는 그때까지 DB 커넥션을 유지한다는 이야기이다.
    - 성능상 이슈가 있고, 트래픽이 커지면 바로 장애 포인트가 된다.(tdd 강의 중 jojoldu님 경험담)
      - 옵션 끄자
        - https://kwonnam.pe.kr/wiki/springframework/springboot/jpa
        - https://whiteship.tistory.com/1636
    - 그러면?
      - 서비스 레이어내에서(트랜잭션 범위 내에서) request/response dto로 만들어서 리턴하는게 좋다.
      - 엔티티는 고유한 영역으로 보존하고, 이러면 엔티티 레이지로딩 문제도 발생하지 않는다.

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
