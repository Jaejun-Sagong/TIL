

## JPA는 기술 명세이다.

- **자바 어플리케이션에서 관계형 데이터베이스를 사용하는 방식을 정의한 인터페이스.**
- JPA는 특정 기능을 하는 **라이브러리가 아닌 인터페이스.**


## Hibernate는 JPA의 구현체이다

- Hibernate는 **JPA라는 명세의 구현체.**
    - `javax.persistence.EntityManager`와 같은 인터페이스를 직접 구현한 라이브러리.
- **JPA와 Hibernate는 마치 자바의 interface와 해당 interface를 구현한 class와 같은 관계.**


## Spring Data JPA는 JPA를 쓰기 편하게 만들어놓은 모듈이다

- **Spring에서 제공하는 모듈** 중 하나로, 개발자가 **JPA를 더 쉽고 편하게 사용할 수 있도록** **도와줌.**
    - **JPA를 한 단계 추상화시킨 `Repository`라는 인터페이스를 제공함**
        - `Repository` 인터페이스에 정해진 규칙대로 메소드를 입력하면, Spring이 알아서 해당 메소드 이름에 적합한 쿼리를 날리는 구현체를 만들어서 Bean으로 등록해 줌.
        - JPA를 추상화했다는 말은, **Spring Data JPA의 `Repository`의 구현에서 JPA를 사용하고 있다**는 것.
