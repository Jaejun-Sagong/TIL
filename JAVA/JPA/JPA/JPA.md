# JPA란?

- Java 진영에서 ORM(Object-Relational Mapping) 기술 표준으로 사용하는 인터페이스 모음
- 자바 ORM 기술에 대한 표준 명세로, `JAVA`에서 제공하는 API이다. 스프링에서 제공하는 것이 아님!
- 자바 애플리케이션에서 **관계형 데이터베이스를 사용하는 방식을 정의한 인터페이스**
    - 인터페이스 이기 때문에 Hibernate, OpenJPA(구현체) 등이 JPA를 구현함
- ORM이기 때문에 자바 클래스와 **DB테이블**을 매핑한다.(sql을 매핑하지 않는다)

---

# 등장 배경

- 기술이 발전할수록 **DB의 중요성이 점점 더 높아짐**에 따라 **DB와 연동할 수 있는 기술들이 필요**
- 처음에는 단순한 **DB Connection을 위한 프레임워크들이 나오기 시작**했지만 쿼리는 컴파일 단계에서 에러를 잡을 수 없어 실수가 많고, 코드도 길고 반복적인 업무가 **많은 단점이 존재**
- 사람들은 **쿼리를 강력하고 신뢰성 있게 사용하기를 원했고** 그래서 나온 것이 SQLMapper 시리즈 중 하나인 MyBatis 이다.
- 하지만 **MyBatis 또한** 관심사를 분리하여 XML파일로 쿼리를 분리하긴 했으나 **SQL의존적 개발의 단점을 갖고 있었기 때문에** 등장한 것이 ORM이고 ORM의 등장이 자바에게는 곧 **JPA의 등장**이라고 할 수 있다.

---

# 동작 과정

![https://gmlwjd9405.github.io/images/inflearn-jpa/jpa-insert-structure.png](https://gmlwjd9405.github.io/images/inflearn-jpa/jpa-insert-structure.png)

- JPA는 애플리케이션과 JDBC 사이에서 동작한다.
- JPA 내부에서 JDBC API를 사용하여 SQL을 호출하여 DB와 통신한다.
    - 개발자가 ORM 프레임워크에 저장하면 적절한 INSERT SQL을 생성해 데이터베이스에 저장해주고, 검색을 하면 적절한 SELECT SQL을 생성해 결과를 객체에 매핑하고 전달해 준다.

---

# **사용 이유**

### 1. 생산성

지루하고 반복적인 코드를 개발자가 직접 작성하지 않아도 되며, DDL문도 자동으로 생성해주기 
때문에 데이터베이스 설계 중심을 객체 설계 중심으로 변경할 수 있다.

### 2. 유지보수, 리팩토링 용이

예를 들어 필드를 하나만 추가할 경우 추가되는 SQL과 JDBC 코드를 작성해야했지만 JPA는 이를 
대신 처리해주기 때문에 개발자가 유지보수 및 리팩토링을 비교적 편리하게 할 수 있습니다.

### 3. 패러다임의 불일치 해결

자바는 추상화, 상속, 다형성과 같은 개념이 존재하는 객체지향적이지만,
데이터베이스는 그러한 개념이 없고 데이터 중심적 개념들이 없기 때문에 서로가 지향하는 목적이 다르므로 둘의 기능과 표현 방법도 다르다. 

이것을 객체와 관계형 데이터베이스의 패러다임 불일치 문제라 한다.

### 4. 성능

애플리케이션과 데이터베이스 사이에서 성능 최적화 기회를 제공한다.

같은 트랜잭션 안에서는 같은 엔티티를 반환하기 때문에 데이터 베이스와의 통신 횟수를 줄일 수 
있다. 또한, 트랜잭션을 commit하기 전까지 메모리에 쌓고 한 번에 SQL을 전송한다.

### 5. 데이터 접근 추상화와 벤더 독립성

RDB는 같은 기능이라도 벤더마다 사용법이 다르기 때문에 처음 선택한 데이터베이스에 종속되고 변경이 어렵다. JPA는 애플리케이션과 데이터베이스 사이에서 추상화 된 데이터 접근을 제공하기 
때문에 종속이 되지 않도록 한다.

만약 DB가 변경되더라도 JPA에게 알려주면 간단하게 변경이 가능하다.