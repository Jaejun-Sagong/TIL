## **1. Spring Security란?**

**Spring Security** is a **Java**/**Java EE** framework that provides **authentication**, **authorization** and other security features for enterprise applications.

> Spring Security는 엔터프라이즈 애플리케이션에 대한 인증, 권한 부여 및 기타 보안 기능을 제공하는 Java / Java EE 프레임 워크입니다.
> 

간단하게 말해서 Spring 진영의 보안 관련 프레임워크이다.

## **2. Spring Security의 동작원리**

![https://chathurangat.files.wordpress.com/2017/08/blogpost-spring-security-architecture.png](https://chathurangat.files.wordpress.com/2017/08/blogpost-spring-security-architecture.png)

**일반적인 Form Login 절차**

**2.1. 요청 수신**

- 브라우저에서 사용자가 아이디와 비밀번호를 입력하고 요청을 보냄

**2.2. 토큰 생성**

- AuthenticationFilter가 요청을 받아서 UsernamePasswordAuthenticationToken토큰을 생성
    
    > UsernamePasswordAuthenticationToken은 해당 요청을 처리할 수 있는 Provider을 찾는 데 사용
    > 

**2.3. Authentication Manager에게 처리 위임**

- **Authentication Manager**는 List형태로 Provider들을 갖고 있음
    
    > ProviderManager가 갖고 있는 Provider들을 차례로 탐색하면서 각 Provider들의 supports
    메소드로 확인
    > 

**2.4. Token을 처리할 수 있는 Authentication Provider 선택**

- **supports**메소드를 호출하여 처리 가능한 **Provider**선택

**2.5. UserDetailsService의 loadUserByUsername메소드 수행**

- **UserDetailsService**를 구현하여 **loadUserByUsername**를 오버라이딩
- **loadUserByUsername**메소드 안에 파일이던 DB던 유저를 찾는 로직을 정의

**2.6. DB등 사용자의 정보가 저장 된 곳에서 데이터를 꺼내어 UserDetails형태로 반환**

**2.7. 인증 완료 시 Authentication객체를 SecurityContextHolder안에 저장**

## **3. Spring Security에서 사용되는 객체**

### **Authentication Filter**

- 사용자의 요청을 받아 인증에 사용되는 UsernamePasswordAuthenticationToken을 생성해주는 필터
    
    > Filter: 앱의 제일 앞단에서 HTTP요청을 가로채는데 사용되는 객체
    > 

### **UsernamePasswordAuthenticationToken**

- AuthenticationFilter에 의해 생성된 객체이며, 로그인을 시도한 아이디와 비밀번호 데이터를 담고 있음

### **AuthenticationManager**

- authenticate라는 인증 메소드를 제공하는 인터페이스

### **ProviderManager**

- AuthenticationManager를 구현하는 객체로 authenticate메소드를 오버라이딩
- authenticate메소드 안에서 멤버로 갖고있는 Provider를 순회하며 UsernamePasswordAuthenticationToken를 처리할 수 있는 Provider를 찾아서 요청 위임

### **AuthenticationProvider**

- supports메소드를 통해 해당 Token객체를 처리 할 수 있는지 확인
- 처리할 수 있는 경우 내부의 authenticate메소드를 통해 인증 처리

### **UserDetailsService**

- 사용자 정보를 찾는데 사용되는 loadUserByUsername메소드를 제공하는 인터페이스
- 이를 구현하는 클래스는 loadUserByUsername를 오버라이딩하여 username으로 사용자 정보를 찾는 로직을 구현하면 됨

### **UserDetails**

- Spring Security에서 관리하는 사용자 정보
- 직접 구현하여 사용해도 되고, UserDetails따로 User 엔티티 따로 사용해도 됨

### **SecurityContextHolder**

- SecurityContext를 저장하는 객체
- Securitycontextholder.getcontext().getauthentication()를 통해 SecurityContext안에 저장된 authentication객체를 꺼내올 수 있음

### **SecurityContext**

- 현재 인증 된 사용자의 정보(Authentication객체)를 저장하는데 사용
