# **Spring Filter 와 Interceptor**

비슷한 듯 다른 Filter 와 Interceptor 에 대해서 자세히 알아보도록 하겠습니다.

위 2개와 AOP 는 공통적으로 여러 작업을 처리함으로써 중복된 코드를 제거할 수 있다는 공통점이 있습니다. 3가지 기능들은 모두 어떤 행동을 하기전에 먼저 실행하거나, 실행한 후에 추가적인 행동을 할 때 사용되는 기능들입니다.

# 1. 필터(Filter)

- J2EE 표준 스펙 기능입니다.
    - J2EE 란, 자바를 이용하여 서버 개발을 하기 위해 만들어진 플랫폼입니다.
    - J2EE 스펙에 따라 제품으로 구현한 것을 [웹 애플리케이션 서버](https://ko.wikipedia.org/wiki/%EC%9B%B9_%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98_%EC%84%9C%EB%B2%84) 또는 [WAS](https://ko.wikipedia.org/wiki/WAS)(대표적으로 톰캣)라 불립니다.
- **Dispatcher Servlet 에 요청이 전달되기 전/후 에 URL 패턴에 맞는 모든 요청에 대해 부가작업을 처리할 수 있는 기능을 제공** 합니다.
- Dispatcher Servlet 은 스프링 가장 앞단에 있는 프론트 컨트롤러 이기 때문에, **Filter 는 Spring 범위 밖에서 처리가 됩니다.**
- 더 자세히 설명하면 **Filter 는 스프링 컨테이너가 아닌 Tomcat과 같은 웹 컨테이너에 의해 관리가 됩니다.**
    - 하지만 **Filter 도 스프링 빈으로 등록이 됩니다.**

![https://blog.kakaocdn.net/dn/b8E2rv/btrFIdUjzmB/jM26Mj8ylpqaZQ5lAFw0Hk/img.png](https://blog.kakaocdn.net/dn/b8E2rv/btrFIdUjzmB/jM26Mj8ylpqaZQ5lAFw0Hk/img.png)

# 2. 인터셉터(Interceptor)

- J2EE의 표준 스펙이 아닌 **Spring 이 제공하는 기술입니다.**
- 정식 명칭은 Handler Interceptor 입니다.
- **Dispatcher Servlet 이 컨트롤러를 호출하기 전과 후에 요청과 응답을 참조하거나 가공할 수 있는 기능을 제공합니다.**
- 스프링 컨텍스트에서 동작을 합니다.

Dispatcher Servlet 의 역할은 핸들러 매핑을 통해 들어온 요청에 대한 적절한 컨트롤러를 찾습니다. 그 결과로 HandlerExecutionChain 을 돌려받게 됩니다.

만약 HandlerExecutionChain 이 1개 이상의 인터셉터가 등록되어 있다면 순차적으로 등록된 인터셉터들을 거친 후 컨트롤러가 실행이 됩니다.

어떠한 인터셉터도 등록하지 않았다면 바로 컨트롤러를 실행합니다.

정리하자면, 인터셉터는 스프링 컨테이너 내에서 동작하므로 작동순서는 아래 그림과 같습니다.

![https://blog.kakaocdn.net/dn/cSCcOW/btrFJ9w5oG4/Xkyrk5DyYk0jtF7kp9UmEK/img.png](https://blog.kakaocdn.net/dn/cSCcOW/btrFJ9w5oG4/Xkyrk5DyYk0jtF7kp9UmEK/img.png)

# 필터(Filter) vs 인터셉터(Interceptor) 차이 및 용도

- 위에 설명했던 것처럼 Filter 와 Interceptor 는 다음과 같은 차이점이 있습니다.
    
 ![image](https://user-images.githubusercontent.com/109019062/212006528-22323733-3e0c-475a-bfec-6ddf639b1bbc.png)   


### Request/Response 객체 조작 가능 여부

- Filter 는 Request 와 Response 를 조작할 수 있지만 Interceptor는 조작할 수 없습니다.
    - 여기서 조작이란 **Request 와 Response 의 내부 상태를 변경한다는 것이 아닌 다른 객체로 바꿔친다는 의미**입니다.
    
    ```java
    @Component
    public MyFilter implements Filter {
    
      public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {
     chain.doFilter(request, response);
      }
    }
    ```
    
- 코드를 보면 doFilter 의 파라미터를 확인해보면 ServletRequest request, 
ServletResponse response 받고 또한 필터 체이닝(다음 필터 호출)을 할 때 파라미터로 request, response 객체를 넘겨주어야 합니다. 따라서 이때 개발자가 원하는 request/response 로 변경하여 넣어줄 수 있습니다.
- 그와 다르게 인터셉터는 Dispatcher Servlet이 여러 인터셉터 목록을 가지고 있습니다.
- 해당 인터셉터를 for 문으로 순차적으로 실행시키고 true 를 반환하면 다음 인터셉터를 실행되거나 컨트롤러로 요청이 전달됩니다.

```java
public class MyInterceptor implements HandlerInterceptor {

    default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
// Request/Response를 교체할 수 없고 boolean 값만 반환할 수 있습니다.
        return true;
    }

}
```

- false 가 반환되면 요청이 중단됩니다.
- 따라서 다른 **request/response 객체를 넘겨줄 수 없습니다.**

# 필터와 인터셉터의 용도 및 예시

### 필터의 용도 및 예시

- 공통된 보안 및 인증/인가 관련 작업
- 모든 요청에 대한 로깅 또는 감사
- 이미지/데이터 압축 및 문자열 인코딩
- Spring 과 분리되어야 하는 기능

필터는 기본적으로 **스프링과 무관하게 전역적으로 처리해야 하는 작업들** 을 처리할 수 있습니다.

- 대표적으로 보안 공통 작업이 있습니다.
    - 필터는 인터셉터 보다 앞단에서 동작하므로 전역적으로 해야하는 보안 검사(XSS 방어 등) 을 하여 올바른 요청이 아닐 경우 차단할 수 있습니다.
- 웹 애플리케이션에 **전반적으로 사용되는 기능을 구현**하기에 적당합니다.
    - 이미지나 데이터 압축
    - 문자열 인코딩
- **Filter 는** 다음 체인으로 넘기는 ServletRequest 와 ServletResponse 를 조작할 수 있다는 점에서 **Interceptor 보다 훨씬 강력한 기술**입니다.

### 인터셉터 용도 및 예시

- 세부적인 보안 및 인증/인가 공통 작업
- **API 호출에 대한** 로깅 또는 감사
- Controller 로 넘겨주는 정보의 가공

인터셉터에서는 **클라이언트의 요청과 관련되어 전역적으로 처리해야하는 작업** 들을 처리할 수 있습니다.

- 특정 그룹의 사용자는 어떤 기능을 사용하지 못하게 해야 할 경우, 컨트롤러로 넘어가기 전에 검사해야 하므로 인터셉터가 처리하기 적합합니다.

인터셉터는 필터와 다르게 HttpServletRequest 나 HttpServletResponse 등과 같은 객체를 제공받으므로 객체 자체를 조작할 수는 없습니다.

하지만 내부적으로 갖는 값은 조작할 수 있으므로 **컨트롤러로 넘겨주기 위한 정보를 가공** 하기에는 용이합니다.

- JWT 토큰 정보를 파싱해서 컨트롤러에게 사용자의 정보를 제공하도록 가공할 수 있습니다.

그 외에도 다양한 목적으로 API 호출에 대한 정보들을 기록해야 할 수 있습니다. 이러한 경우에 HttpServletRequest 나 HttpServletResponse 를 제공해주는 인터셉터는 클라이언트 IP나 요청 정보들을 포함해 기록하기에 용이합니다.

**Filter 를 인증과 인가에 사용하는 도구로 대표적으로 SpringSecurity** 가 있습니다.

**SpringSecurity 의 특징** 중 하나가 **Spring MVC 에 종속적이지 않다는 것**인데, 바로 **Filter를 기반으로 인증/인가 처리를 하기 때문**입니다.
   
   
- 전체적인 Spring MVC 처리 프로세스

![https://blog.kakaocdn.net/dn/erTGA9/btrFP0Fo0Jn/qlOTMLn8p6BDTg6MFCQO81/img.png](https://blog.kakaocdn.net/dn/erTGA9/btrFP0Fo0Jn/qlOTMLn8p6BDTg6MFCQO81/img.png)
