> **Spring Container**는 자바 **객체(Bean)의 생성과 소멸 같은 생명주기(Life Cycle)를 관리**하며, 생성된 자바 객체들에게 추가적인 기능을 제공하는 역할을 합니다.
> 

## **Bean Life Cycle**

- Spring의 **Bean은** Java 또는 XML bean 정의를 기반으로 **컨테이너가 시작될 때 인스턴스화 되어야 합니다.**
- Bean을 사용 가능한 상태로 만들기 위해 사전, 사후 초기화 단계를 수행해야 할 수도 있습니다.

- 그 후 **Bean이 더 이상 필요하지 않으면 IoC Container에서 제거됩니다**.

- 다른 시스템 리소스를 해제하기 위해 사전 및 사후 소멸 단계를 수행해야 할 수도 있습니다.

> 즉, **Bean의 Life Cycle이란** **해당 객체가 언제, 어떻게 생성되어 소멸되기 전까지 어떤 작업을 수행하고 언제, 어떻게 소멸되는지 일련의 과정을 이르는 말**입니다.
> 
- Spring Container는 이런 빈 객체의 생명주기를 컨테이너의 생명주기 내에서 관리하고 객체 생성이나 소멸 시 호출될 수 있는 **콜백 메서드**를 제공하고 있습니다. ****
    - Spring Container  - 초기화 : Bean을 등록, 생성, 주입
                                 - 종료 : Bean 객체들을 소멸
    - 콜백: 콜백함수를 부를 때 사용되는 용어이며 콜백 함수를 등록하면 특정 이벤트가 발생했을 때 해당 메소드가 호출됨
    즉, 조건에 따라 실행될 수도 실행되지 않을 수도 있는 개념.
    이를 토대로 간단히 스프링 **Bean Life Cycle** 을 요약하면

![https://blog.kakaocdn.net/dn/Q7Epv/btrFwu1aI4R/tm6LUhBs1dkOiyJ7TocoY0/img.png](https://blog.kakaocdn.net/dn/Q7Epv/btrFwu1aI4R/tm6LUhBs1dkOiyJ7TocoY0/img.png)

1. 스프링 컨테이너 생성
2. 스프링 빈 생성
3. 의존성 주입
4. 초기화 콜백 : 빈이 생성되고, 빈의 의존관계 주입이 완료된 후 호출
5. 사용
6. 소멸전 콜백 : 빈이 소멸되기 직전에 호출
7. 스프링 종료

> 이처럼 스프링 빈은 생성과 의존성 주입이 끝난 후에 필요한 데이터를 사용할 준비가 됩니다. 
또한 의존 관계 주입이 완료되는 시점과 종료되는 시점은 콜백 메서드를 통해 알 수 있습니다.
> 

![https://blog.kakaocdn.net/dn/dMc5tZ/btrFxjZndp1/JK7XXHeEggPlQ1R12pHnJk/img.jpg](https://blog.kakaocdn.net/dn/dMc5tZ/btrFxjZndp1/JK7XXHeEggPlQ1R12pHnJk/img.jpg)

Bean 생명주기 관리

### **Spring 빈 생명주기 콜백**

Spring Framework는 빈 라이프 사이클을 제어하기 위한 다음과 같은 방법을 제공해줍니다.

1. InitializingBean, DisposableBean callback interfaces

2. 설정 정보에 초기화 메서드 init( ), 종료 메서드 destory( ) 지정하는 방법

3. @PostConstruct, @PreDestroy 애노테이션각각의 방법에 대해 더 자세히 알아봅시다.

### **1. InitializingBean, DisposableBean callback interfaces**

- **InitializingBean**: 빈에 필요한 모든 속성이 컨테이너에 설정된 후에 빈의 초기화 작업을 수행하도록 합니다.

```java
void afterPropertiesSet() throws Exception;
```

: afterPropertiesSet() 메서드로 초기화를 지원합니다.: 의존 관계 주입이 끝난 후 초기화 진행**• DisposableBean**: 스프링 컨테이너가 빈을 소멸시키기 전에 콜백을 얻을 수 있다.

```java
void destroy() throws Exception;
```

: destory() 메소드로 소멸을 지원합니다.: Bean 종료 전 마무리 작업 ex) 자원해제 close( ) 등

```java
import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.InitializingBean;

public class DemoBean implements InitializingBean, DisposableBean
{
//Other bean attributes and methods@Override
	public void afterPropertiesSet() throws Exception
	{
//Bean initialization code
	}

	@Override
	public void destroy() throws Exception
	{
//Bean destruction code
	}
}
```

**단점**: 해당 인터페이스는 스프링 전용 인터페이스로 해당 코드가 인터페이스에 의존한다.   
        : 초기화, 소멸 메소드를 오버라이드 하기 때문에 메소드명을 변경할 수 없다.   
        : 코드를 고칠 수 없는 외부 라이브러리에 적용 불가능하다.   
        : 또한 인터페이스를 사용하는 초기화 및 종료 방법은 스프링 초창기에 나온 방법들이며, 지금은 거의 사용하지 않습니다.

### **2. 설정 정보에 사용자 정의 초기화 메서드, 종료 메서드 지정**

위 같은 인터페이스를 구현하지 않고 @Bean 어노테이션에 initMethod, destroyMethod 속성을 사용하여 초기화, 소멸 메서드를 각각 지정할 수 있습니다.

```java
public class ExampleBean {
    public void init() throws Exception {
//초기화 콜백
    }

    public void close() throws Exception {
// 소멸 전 콜백
    }
}

@Configuration
class LifeCycleConfig {
	 @Bean(initMethod = "init", destroyMethod = "close")
     public ExampleBean exampleBean() {

     }
}
```

- 이는 수정할 수 없는 외부 클래스, 정확히 위의 두 인터페이스를 구현시킬 수 없는 클래스의 객체 스프링 컨테이너에 등록할 때 유용합니다. 메소드 명을 자유롭게 부여할 수있고, 스프링 코드에 의존하지 않습니다.
설정 정보를 사용하기 때문에 코드를 고칠 수 없는 외부 라이브러리에도 초기화, 종료 메서드를 적용할 수 있습니다.

### **3. @PostConstruct, @PreDestroy 애노테이션**

- **@PostConstruct**: 기본 생성자를 사용하여 빈이 생성된 후 인스턴스가 요청 객체에 반환되기 직전에 주석이 달린 메서드가 호출됩니다.
- **@PreDestroy**: @PreDestroy주석이 달린 메소드는 bean이 bean 컨테이너 내부 에서 파괴되기 직전에 호출됩니다 .

```java
import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;

public class DemoBean
{
	@PostConstruct
	public void customInit()
	{
		System.out.println("Method customInit() invoked...");
	}

	@PreDestroy
	public void customDestroy()
	{
		System.out.println("Method customDestroy() invoked...");
	}
}
```

- 최신 스프링에서 가장 권장하는 방법으로 애노테이션 하나만 붙이면 되므로 매우 편리합니다. 패키지를 잘 보면 javax.annotation.PostConstruct 로 스프링에 종속적인 기술이 아니라 JSR-250 라는 자바 표준입니다.따라서 스프링이 아닌 다른 컨테이너에서도 동작합니다. 유일한 단점은 외부 라이브러리에는 적용하지 못한다는 점입니다.학습에 많은 도움이 된 블로그 링크를 참조했습니다.
- 🙂참조 및 출처[Spring – Bean Life Cycle](https://howtodoinjava.com/spring-core/spring-bean-life-cycle/)[Spring의 빈 생명주기(Bean Lifecycle)](https://haruhiism.tistory.com/186)[[Spring] 빈 생명주기(Bean LifeCycle) 콜백 알아보기](https://dev-coco.tistory.com/170)
