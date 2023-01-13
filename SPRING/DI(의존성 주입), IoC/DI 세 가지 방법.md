### **1.생성자 주입**

```java
@Controller
public class Controller{
   private Service service;

   @Autowired
   public Controller(Service service){
     this.service = service;
   }
}
```

- 생성자에 @Autowired를 붙여 의존성을 주입받을 수 있음.
- Spring 4.3이후로는 클래스 내 생성자가 하나이고, 그 생성자로 주입받을 객체가 빈으로 등록되어 있다면 @Autowired 생략 가능.
- 생성자 주입은 인스턴스 생성시 1회 호출되는 것이 보장되기 때문에, 주입받은 객체가 변하지 않거나, 반드시 객체주입이 필요한 경우 강제하기 위해 사용됨.

### **2. 필드 주입**

```java
@Controller
public class Controller{
  @Autowired
  private Service service;
}
```

- 코드가 간결하고 편하지만 의존 관계를 정확히 파악하기 힘듦.
- 필드 주입 시 final 키워드를 선언할 수 없어 객체가 변할 수 있음.
- 주입이 동시에 일어나 겹치는 경우 순환참조 에러가 남.

### **3. 수정자(setter) 주입**

```java
@Controller
public class Controller{
   private Service service;

   @Autowired
   public setService(Service service){
     this.service = service;
   }
}

```

setter 혹은 사용자정의 메서드를 통해 의존관계 주입.

setter의 경우 객체가 변경될 필요성이 있을 때만 사용한다.(주입하는객체를 변경하는 경우는 드물다)

**스프링 팀에서는 생성자 주입을 권장한다.(인텔리제이에서는 필두주입 사용시 경고창이 뜬다.)** 

**왜?**

### **생성자 주입을 권장하는 이유**

**1.객체 불변성 확보**

- 객체의 생성자는 객체 생성시 최초 1회만 호출된다. 때문에 주입받은 객체가 불변 객체여야 하거나 반드시 해당 객체의 주입이 필요한 경우 사용한다.

```java
@Controller
public class Controller{
   private Service service;

   @Autowired
   public Controller(Service service){
     this.service = service;
   }
}
```

위 코드에서 Controller가 사용하는 Service를 변경하는 코드는 Controller의 생성자뿐이다. 
즉, 생성자로 한번 의존관계를 주입하면 생성자는 최초 1회 이후 다시 생성될 일이 없기 때문에 불변객체를 보장한다.

또한 Controller가 생성되는 시점에 무조건 service 객체가 생성되어 주입된다.

**2. 테스트 용이**

- 필드 주입으로 작성된 경우, 순수 자바 코드로 단위테스트를 실행하는 것이 불가하다.
- 메인코드는 Spring과 같은 DI프레임워크 위에서 동작하는데 단위테스트 시 단독적으로 실행되기 때문에 의존관계 주입이 null상태여서 NullPointerException이 발생하게 된다. 
생성자 주입 시 단독으로 실행할때도 의존관계 주입이 성립된다.

**3. 순환참조 에러방지**

- 순환참조란 A객체는 B객체를 참조하고, B객체는 A객체를 서로 동시에 참조하고 있을 때 발생한다.

```java
@Service
publiv class ServiceA{
  @Autowired
  private ServiceB serviceB;

  public void test(){
   serviceB.test();//A가 B의 메서드 호출
  }
}
```

```java
@Service
publiv class ServiceB{
  @Autowired
  private ServiceA serviceA;

  public void test(){
   serviceA.test();//B가 A의 메서드 호출
  }
}
```

어플리케이션을 실행하고 A또는 B에서 test()메서드를 호출하면 서로 메서드를 계속해서 호출하다. StackOverFlow가 발생하면서 시스템이 다운된다. 이 때 컴파일 시에는 아무런 에러가 없다가 메서드 호출 시에 발생한다는 것이 문제가 된다. 이러한 순환참조 에러는 사실 3가지 방법론 모두에서 발생하는데 에러 시점이 다르다.

필드주입과 수정자 주입은 프로그램 실행 중에  runtime 에러가 발생하고, 생성자 주입시에는 프로그램 실행 시점에 compile 에러가 발생한다.즉, 실행 시점에 컴파일 에러 발생시 프로그램 실행 자체가 되지 않기 때문에 개발자 입장에서는 실제 서비스 되기 전, 순환 참조 문제를 해결하도록 한다.

이러한 이유들 때문에 여러 DI 방법 중 생성자주입 방식을 권장하고 있다.

- DI Dependency Injection 의존주입

객체를 직접 생성(new 연산자)하는 것이 아니라 외부에서 생성된 객체를 주입받아 이용하는 것.

- IoC Inversion of Control.제어의역전

메소드나 객체의 호출작업을 개발자가 직접 하는게 아니라, 스프링 프레임워크에게 제어권을 넘기는 것.

대부분의 프레임워크에서 IoC를 적용하고 있고 스프링인 여러 프레임웤중 하나인 것.
