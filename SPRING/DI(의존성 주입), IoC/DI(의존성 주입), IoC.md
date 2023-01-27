## DI (의존성 주입) 의 이해

<aside>
🔥  DI의 핵심은 느슨한 결합이다.
  
    그럼 느슨한 결합을 알아보기 전에 강한 결합의 문제점부터 알아보자!

</aside>

- **강한 결합의 문제점**
    
    ['**강한 결합**' 이해를 위한 예제]
    
    1. **Controller1**  이  **Service1** 객체를 생성하여 사용
        
        ```java
        public class Controller1 {
        	private final Service1 service1;
        
        	public Controller1() {
        		this.service1 = new Service1();
        	}
        }
        ```
        
    2.  **Service1**  이 **Repository1** 객체를 생성하여 사용
        
        ```java
        public class Service1 {
        	private final Repository1 repository1;
        
        	public Service1() {
        		this.repository1 = new Repository1();
        	}
        }
        ```
        
    3.  **Repository1** 객체 선언
        
        ```java
        public class Repository1 { ... }
        ```
        
    4. 만약, 다음과 같이 변경된다면..
        1.  **Repository1**  객체 생성 시 DB 접속 id, pw 를 매개변수를 받아서 생성되게 강제
            
            ( DB 접속 시 사용하기 위해)
            
            - 생성자에 DB 접속 id, pw 를 추가
            
            ```java
            public class Repository1 {
            
            	public Repository1(String id, String pw) {
                // DB 연결
                Connection connection = DriverManager.getConnection("jdbc:h2:mem:springcoredb", id, pw);
              }
            }
            ```
            
    
  ['**강한 결합**'의 문제점]

- **Controller 5 개**가 각각 Service1 을 생성하여 사용 중
- **Repository1** 생성자 변경에 의해..
    
    ⇒ **모든 Controller** 와 **모든 Service** 의 코드 변경이 필요
    

![3](https://user-images.githubusercontent.com/109019062/210706166-3b1ccff2-b92d-4d39-813d-ba2080c663f7.PNG)
 ---   
- **강한 결합 해결방법**
    

👉 그렇다면 '강한 결합'을 해결할 방법이 없을까?

    1. 각 객체에 대한 객체 생성은 딱 1번만!!
    2. 생성된 객체를 모든 곳에서 재사용!!! → 위와 같은 전부 수정해야하는 상황 방지


    
  1. **Repository1**  클래스 선언 및 **객체 생성** → **repository1**
        
      ```java
      public class Repository1 { ... }

      // 객체 생성
      Repository1 repository1 = new Repository1();
      ```
        
![image](https://user-images.githubusercontent.com/109019062/210707187-7b86842e-5e76-4bf5-92a4-e2c0743c2e49.png)
        
    
  2. **Service1**  클래스 선언 및 **객체 생성 (repostiroy1 사용)** → **service1**
        
        ```java
        Class Service1 {
        	private final Repository1 repository1;
        
        	// repository1 객체 사용
        	public Service1(Repository1 repository1) {
 
        		this.repository1 = repository1;
        	}
        }
        
        // 객체 생성
        Service1 service1 = new Service1(repository1);
        ```
        
        ![image](https://user-images.githubusercontent.com/109019062/210707280-c16211bc-98f1-486b-8933-a4280bd6eca9.png)
        
  3. **Controller1**  선언 ( **service1** 사용)
        
        ```java
        Class Controller1 {
        	private final Service1 service1;
        
        	// service1 객체 사용
        	public Controller1(Service1 service1) {
        		this.service1 = new service1();가 아닌
        		this.service1 = service1;
        	}
        }
        ```
        
  4. 만약, 다음과 같이 변경된다면,
        1.  **Repository1**  객체 생성 시 DB 접속 id, pw 를 받아서 DB 접속 시 사용
            - 생성자에 DB 접속 id, pw 를 추가
            
            ```java
            public class Repository1 {
            
            	public Repository1(**String id, String pw**) {
                // DB 연결
                Connection connection = DriverManager.getConnection("jdbc:h2:mem:springcoredb", **id, pw**);
              }
            }
            
            // 객체 생성
            String id = "sa";
            String pw = "";
            Repository1 repository1 = new Repository1(id, pw);
            ```
            
    
  [개선 결과]
    
  ⇒ **Repository1** 생성자 변경은 이제 누구에게도 피해(?) 를 주지 않음
    
  ⇒ **Service1** 생성자가 변경되면? **모든 Controller** → Controller 변경 필요 X
    
   결론적으로, **강한 결합 ⇒ 느슨한 결합**
    
   ![image](https://user-images.githubusercontent.com/109019062/210707348-047b315e-4f19-47e2-8f61-b5f6ed483455.png)
  ---  
- **DI (의존성 주입)의 이해**
    
    ![image](https://user-images.githubusercontent.com/109019062/210707406-a7c00505-5b2e-4758-ac0e-caf5f302aeac.png)
    
   
👉 "제어의 역전 (IoC: Inversion of Control)"

    프로그램의 제어 흐름이 뒤바뀜
    원래는 Controller에서 service가 필요하면 service를 생성해서 사용하는 흐름이었다면
    느슨한 결합을 위해 
    Repo의 객체를 생성하고 service 생성 시 Repo의 객체를 매개변수로 받는 흐름 역전
    즉, 제어의 역전이 일어났다.
    
   
    
  - 일반적: 사용자가 자신이 필요한 객체를 생성해서 사용
  - **IoC** (제어의 역전)
  - **용도에 맞게 필요한 객체를 그냥 가져다 사용**
    - "**DI (Dependency Injection)**" 혹은 한국말로 "**의존성 주입**"이라고 부릅니다.
      - 사용할 객체가 어떻게 만들어졌는지는 알 필요 없음
---
- **DI의 장점**
    
    **1. 의존성이 줄어든다.**
    
    DI로 구현하게 되었을 때, 주입받는 대상이 변하더라도 그 구현 자체를 수정할 일이 없거나 줄어들게됨.
    
    **2. 재사용성이 높은 코드가 된다.**
    
    **3. 테스트하기 좋은 코드가 된다.**
    
    테스트를 분리하여 진행할 수 있다.
    
    **4. 가독성이 높아진다.**
    
    기능들을 별도로 분리하게 되어 자연스레 가동성이 높아진다.
