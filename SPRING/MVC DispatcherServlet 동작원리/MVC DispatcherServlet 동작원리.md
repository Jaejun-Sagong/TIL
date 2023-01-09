스프링 웹 MVC 프레임워크의 레퍼런스의 첫 문장은 다음과 같습니다.

> The Spring Web model-view-controller (MVC) framework is designed around a **DispatcherServlet** that dispatches requests to handlers, with configurable handler mappings, view resolution, locale and theme resolution as well as support for uploading files.
> 

> 스프링 웹 MVC 프레임워크는 디스패처서블릿(DispatcherServlet)을 중심으로 설계되었다.
> 

### Dispatcher Servlet이란 ?

- `Dispatcher Servlet`에 있는 dispatch는 "보내다"라는 뜻을 가진다.
- **HTTP 프로토콜로 들어오는 모든 요청을 가장 먼저 받아 적합한 컨트롤러에 보내주는 
`Front Controller`** 이다.
    - `Front Controller`  : 주로 서블릿 컨테이너의 제일 앞단에서 서버로 들어오는 클라이언트의 모든 요청을 받아서 처리해주는 컨트롤러 ( MVC 구조에서 함께 사용되는 디자인 패턴 )
- `Front Controller`로서 **클라이언트로부터 어떠한 요청이 오게 되면 모든 요청을 자신이 먼저 받게 되고, 이러한 요청들을 세부 컨트롤러에게 전달**하는 역할 !
- 한마디로 `Dispatcher Servlet`은 해당 어플리케이션으로 들어오는 모든 요청을 핸들링 해주기 때문에, 우리는 컨트롤러를 구현해두기만 하면 `Dispatcher Servlet`가 알아서 적합한 컨트롤러로 위임을 해주는 구조가 된다.

### Dispatcher Servlet의 동작 과정

![https://velog.velcdn.com/images/ejung803/post/f97cbbcd-8f4c-440f-83d5-1c41987db16b/image.png](https://velog.velcdn.com/images/ejung803/post/f97cbbcd-8f4c-440f-83d5-1c41987db16b/image.png)

1. 클라이언트의 요청을 `Dispatcher Servlet`에 전달
2. 요청한 url에 맞는 `controller` 검색하여 `Handler Mapping`에 전달
3. `HandlerMapping`에서 해당 `controller`에 처리 요청
4. `controller`에서 처리 결과를 `Handler Adapter`에서 ModelAndView 객체로 변환하여 `Dispatcher Servlet`에 전달
5. `Dispatcher Servlet`에서 전달받은 ModelAndView 객체를 이용하여 매핑되는 View를 검색
6. `viewResolver`에서 처리 결과를 `view`에 전달
7. 처리결과가 포함된 `view`를 `Dispatcher Servlet`에 전달
8. `Dispatcher Servlet`에서 최종 응답 결과를 클라이언트에게 반환

---

## **[Spring] DispatcherServlet이란?**

디스패처서블릿은 **HTTP 요청들을 매핑된 컨트롤러로 배차해주는 역할을 수행하는 중앙 서블릿**입니다.

사용자의 요청을 처리하는 과정에서 다음과 같은 **특수 Bean**들을 사용합니다.

| Bean Type | 역할 |
| --- | --- |
| HandlerMapping | 요청들과 핸들러들을 HandlerMapping 구현체의 기준에 따라 매핑해줍니다. |
| HandlerAdapter | 매핑된 핸들러를 호출합니다. 어댑터 패턴을 적용했기 때문에 디스패처서블릿이 핸들러의 어노테이션이나 파라미터에 대해 몰라도 됩니다. |
| HandlerExceptionResolver | 예외들을 뷰에게 매핑해주고 다양한 예외처리 코드를 가능하게 해줍니다. |
| ViewResolver | 문자열 기반의 논리적 뷰이름을 실제 뷰로 변환해줍니다. |
| LocaleResolver | 사용자의 로케일을 기반으로 국제화된 뷰를 제공해줍니다. |
| ThemeResolver | 웹 애플리케이션이 사용가능한 theme을 리졸브 해줍니다. |
| MultiPartResolver | multi-part 요청들을 파싱하여 파일업로드와 같은 기능들을 지원합니다. |
| FlashMapResolver | 입출력데이터를 FlashMap에 저장 및 반환하여 속성들을 한 요청끼리 전달할 수 있게 해줍니다. 주로 리다이렉트 시 사용됩니다. |

별도로 설정하지 않았다면 DispatcherServlet.properties를 통해 어떤 우선순위로 특수 Bean이 적용되는지 확인할 수 있습니다.

```java
org.springframework.web.servlet.LocaleResolver=org.springframework.web.servlet.i18n.AcceptHeaderLocaleResolver

org.springframework.web.servlet.ThemeResolver=org.springframework.web.servlet.theme.FixedThemeResolver

org.springframework.web.servlet.HandlerMapping=org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping,\
org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping,\
org.springframework.web.servlet.function.support.RouterFunctionMapping

org.springframework.web.servlet.HandlerAdapter=org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter,\
org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter,\
org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter,\
org.springframework.web.servlet.function.support.HandlerFunctionAdapter

org.springframework.web.servlet.HandlerExceptionResolver=org.springframework.web.servlet.mvc.method.annotation.ExceptionHandlerExceptionResolver,\
org.springframework.web.servlet.mvc.annotation.ResponseStatusExceptionResolver,\
org.springframework.web.servlet.mvc.support.DefaultHandlerExceptionResolver

org.springframework.web.servlet.RequestToViewNameTranslator=org.springframework.web.servlet.view.DefaultRequestToViewNameTranslator

org.springframework.web.servlet.ViewResolver=org.springframework.web.servlet.view.InternalResourceViewResolver

org.springframework.web.servlet.FlashMapManager=org.springframework.web.servlet.support.SessionFlashMapManager
```

### **FrontController 패턴**

- Front-Controller 패턴은 웹 애플리케이션에서 주로 사용되는 디자인 패턴으로, 컨트롤러에서 중복으로 처리해야 하는 공통 기능들을 처리하는 입구 역할을 하는 컨트롤러를 두는 개념입니다.

![https://blog.kakaocdn.net/dn/woaOH/btrfqEgU27J/4mNbLQG9tY7xKTiwzdqwNK/img.jpg](https://blog.kakaocdn.net/dn/woaOH/btrfqEgU27J/4mNbLQG9tY7xKTiwzdqwNK/img.jpg)

- 일반적이 페이지 컨트롤러 패턴 적용
- Front-Controller 패턴을 적용하면 다수의 서블릿이 아닌 **하나의 프론트 컨트롤러 서블릿으로 다수의 클라이언트 요청을 받을** 수 있고, **다양한 HTTP 요청에 대해 공통 처리**가 가능하여 코드 재사용성과 유연성을 높이며 중복을 줄일 수 있습니다. 또한 중앙 집중식 제어를 통해 **보안이 강화**되며 **사용자 트랙킹에도 유리**합니다.

![https://blog.kakaocdn.net/dn/0xsnW/btrfnPqXODP/2kPQhCK8k6P0xqOqzmzNH1/img.jpg](https://blog.kakaocdn.net/dn/0xsnW/btrfnPqXODP/2kPQhCK8k6P0xqOqzmzNH1/img.jpg)

- 프론트 컨트롤러 패턴 적용
- **DispatcherServlet이** 바로 위 그림에서 공통의 로직을 처리하는 **Front-Controller 역할을 수행**하게 됩니다.
    - **Spring MVC의 Front-Controller = DispatcherServlet**
