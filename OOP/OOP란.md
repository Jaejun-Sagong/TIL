## 객체 지향 프로그래밍(OOP)이란 무엇인가요?

> **객체 지향 프로그래밍**
(Object-Oriented Programming, OOP)은 컴퓨터 프로그래밍의 패러다임 중 하나이다. 
객체 지향 프로그래밍은 컴퓨터 프로그램을 명령어의 목록으로 보는 시각에서 벗어나 여러 개의 독립된 단위, 즉 "**객체**"들의 모임으로 파악하고자 하는 것이다. 
각각의 객체는 메시지를 주고받고, 데이터를 처리할 수 있다.
> 

라고 설명하고 있습니다.

한마디로 정의하자면 프로그래밍을 여러 개의 객체 단위로 보고 작업한다라고 생각하시면 될 것 같습니다.

일단 객체지향프로그래밍을 알고자 한다면 객체(Object)가 무엇인지에 대해 알고 넘어가야 합니다.

### 객체(Object)란 무엇인가요?

객체(Object)란 우리 일상생활에서 인식할 수 있는 모든 사물이 될 수 있습니다.

이러한 객체는 속성(Field)과 행위(Method)으로 구성이 되어 있습니다.

- 속성 = Field
- 행위 = Method

위 객체를 만드는 설계도 또는 틀이 바로 클래스(Class)입니다.

Java에서 사람이라는 클래스를 만든다고 가정한다면

```java
public class Person {
}
```

위 와 같이 만들 수 있습니다.

예를 들어 설명하기 위하여 가장 간단한 형태의 클래스를 작성했다고 생각하시면 됩니다.

클래스의 선언만 하고 아무 내용도 적지 않았습니다. 빈 껍데기인 이 클래스에는 객체(Object)를 생성하는 기능을 가지고 있습니다.

아래와 같이 객체를 생성할 수 있습니다.

```java
Person daehee = new Person();
```

이렇게 생성을 하면 

Person 클래스의 인스턴스(instance)인 daehee 또는 Person의 객체가 생성됩니다.

### 객체 지향 언어와 객체 지향 언어의 특징

객체지향 언어에는 Java, C++, C#, Python, Ruby 등 많은 언어들이 있습니다.

그중에 저는 Java를 사용하고 있기 때문에 Java를 예를 들어 설명하겠습니다.

이러한 객체지향 언어들에는 몇 가지 특징이 있습니다.

## 1. 상속성

상위 클래스의 모든 것을 하위 클래스가 이어받는 것을 말합니다. 자식의 부모의 재산을 상속받는 것과 같다고 생각하시면 됩니다.

```java
public class Beer {
        String type;
        String model;

        // 생성자 
        public Beer(String type, String model) {
                this.type = type;
                this.model = model;
        }

        public void drink() {
                System.out.println("마십니다.");
        }

        public void mix() {
                System.out.println("섞습니다.");

        public String getType() {
                return type;
        }

        public String getModel() {
                return model;
        }
				
```

하위 클래스에서 사용하기 위해  Get도 만들어준다!

그렇다면 하위클래스인 호가든클래스를 만들어보자.

호가든에는 로제, 청포도, 레돈 등등의 다양한 과일 맛이 있으므로 맛이라는 flavor필드를 만들어보고

model은 상위클래스에서 받아와서 써보자

```java
public class Hoegaarden extends Beer {

        // 필드 입력
        String flavor;
        int ABV = 6;

        // 생성자 입력
        Hoegaarden(String model, String flavor) {
                super("밀맥주", model);
                this.model = model; 
                this.flavor = flavor;
        }

        // 생성자 오버로딩 (ABV 추가)
        Hoegaarden(String model, String flavor, int ABV) {
                super("밀맥주", model);
                this.model = model;
                this.flavor = flavor;
                this.ABV = ABV;
        }

        // 메서드 입력
        public void flavordrink() {
                System.out.println(model + "맥주의" + flavor + "맛을 마십니다.");
        }

        // Getter
        public String getFlavor() {
                return flavor;
        }

        public int getABV() {
                return ABV;
        }
}
```

하위(자식)클래스인 호가든클래스는 상위(부모)클래스인 Beer 클래스에 정의된 4가지 메소드를 모두 상속받아 사용 가능하며 생성자 오버로딩이나 메소드를 생성함으로써 좀 더 세부적인 기능을 하는 객체가 됩니다.

### **super 키워드**

super 키워드는 자식 클래스에서 부모 클래스를 가리킬 때 사용하는 키워드입니다. 주로 부모 클래스의 필드에 접근, 메소드를 호출할 때 사용합니다.

### **부모 클래스에서 상속이 안 되는 것**

- 부모 클래스의 private 접근 제한을 갖는 필드 및 메소드는 자식이 물려받을 수 없습니다.
- 부모와 자식 클래스가 서로 다른 패키지에 있다면, 부모의 default 접근 제한을 갖는 필드 및 메소드도 자식이 물려받을 수 없습니다.

상속을 하더라도 자식 클래스가 부모의 모든 것들을 물려받는 것은 아닙니다. 필드나 메소드의 접근제어자가 public이거나 protected일 때만 상속이 가능합니다.

### **접근제어자**

![https://blog.kakaocdn.net/dn/oK0kS/btrN67jJ0wo/gTjEYQAuJUWYUjQ9pDpssK/img.png](https://blog.kakaocdn.net/dn/oK0kS/btrN67jJ0wo/gTjEYQAuJUWYUjQ9pDpssK/img.png)

접근제어자는 외부로부터 데이터를 보호하기 위해서 사용되며 public, protected, default, private가 있습니다. 위의 그림에서처럼 public -> private로 진행될수록 접근 가능 범위가 축소됩니다.

## 2. 다형성

하나의 메서드(Method)나 클래스(Class)가 있다고 가정할 때 이 메서드나 클래스를 다양한 방법으로 동작하게 하는 것이라고 생각하시면 됩니다.

예를 들어 "공을 던지다." 라는 동작이 있다면 "야구공을 던지다." 와 "농구공을 던지다." 라는 공을 던지다의 똑같은 행동에서 어떤 공이냐에 따라 동작의 목적이 달라집니다.

Java에서는 대표적으로 **Overloading** 과 **Overriding**이 있습니다.

**오버 로딩(Overloading)은** 같은 이름의 함수(메서드)를 **매개변수의 유형과 개수를 다르게** 하여 다양하게 **재정의** 하는 것을 의미합니다.

```java
Class Test {
	public void print(int value) {
		System.out.println(“ 숫자 출력= " +value);
	}
	public void print(String value) {
		System.out.println(“ 문자 출력= " +value);
	}
}

Class Main{
	public void main(String[]args) {
		Test test = new Test();
		test.print(100);     // 결과 : 숫자 출력 = 100
		test.print(“test”);   // 결과 : 문자 출력 = “test”
	}
}
```

위 예제와 같이 print 라는 메서드를 매개변수 유형을 다르게 하여 재정의 후 사용하는 것을 볼 수 있습니다.

**오버 라이딩(Overriding)은** 상위 클래스가 가지고 있는 메서드를 하위 클래스에서 재정의하여 사용하는 것을 뜻합니다.

쉽게 말하자면 메서드의 이름과 매개변수는 같지만 메서드를 덮어쓴다고 생각하면 됩니다.

```java
Class A {
	public void print() {
		System.out.println(“  print Class A “);
	}
}

Class B extends A {
	public void print() {
		System.out.println(“  print Class B “);
	}
}

Class Main {
	public void main(String[]args) {
		A a=new A();
		B b=new B();
		a.print();     // 결과 : print Class A
		b.print();     // 결과 : print Class B
	}
}
```

## 3. 캡슐화

캡슐화는 객체의 속성(Field)과 행위(Method)를 하나로 묶고, 외부로 부터 내부를 감싸 숨기는 것을 캡슐화라고 합니다.

외부의 잘못된 접근으로 값이 변하는 것을 막기 위해 클래스 내의 변수나 함수를 감추거나 드러내는 은닉성을 가지고 있습니다.

Java에서는 public, protected, default, private의 접근제어자를 통해 구현이 가능합니다.

객체가 맡은 역할을 수행하기 위한 하나의 목적을 한데 묶는다고 생각하면 됩니다.

```java
public class Student {
	private String name;
	private int id;
	private int age;

	public void setName(String name) {
		this.name = name;
    }

	public void setId(int id) {
		this.name = name;
    }

	public void setAge(int age) {
		this.name = name;
    }

	public void getName() {
		return name;
    }

	public void getId() {
		return id;
    }

	public void getAge() {
		return age;
    }
}
```

```java
public class Main {
	public static void main(String[] args) {
		Student student = new Student();
		student.setName("홍길동");
		student.setId(20110756);
		student.setName(20);

		System.out.println("이름 :" +student.getName());
		System.out.println("학번 :" +student.getId());
		System.out.println("나이 :" +student.getAge());
	}
}
```

위 와 같이 Student라는 클래스를 생성 하고 변수들을 private를 붙여 은닉화를 시켰습니다.

그래서 student.age 등으로 호출이 불가능하며 getAge() 메서드를 통해 변수에 접근이 가능합니다.

## 4. 추상화

추상화는 객체의 공통적인 속성과 기능을 추출하여 정의하는 것입니다.

예를 들면, 물고기, 사자, 토끼, 뱀이 있을 때 우리는 이것들을 각각의 객체라 하며 이 객체들을 하나로 묶으려 할 때, 만약 동물 또는 생물이라는 어떤 추상적인 객체로 크게 정의할 수 있습니다. 이때 동물 또는 생물이라고 묶는 것을 추상화라고 합니다.

```java
abstract class Action {
   // 달리다
   public abstract void running();

   // 치다
   public abstract void hitting();

   // 돌다
   public abstract void turnning();
}
```

위 와 같이 Action이라는 추상 클래스를 생성했습니다.

```java
Class Man extends Action {  // 사람 객체
  public void running() {
    System.out.println("운동장을 달린다.");
  }

  public void hitting() {
    System.out.println("바닥을 치다.");
  }

  public void turnning() {
    System.out.println(“찻길 옆으로 돌다.＂);
  }
}
```

```java
public class Animal extends Action {
  public void running() {
    System.out.println("동물을 피해 달린다.");
  }

  public void hitting() {
    System.out.println("앞에선 동물을 치다.");
  }

  public void turnning() {
    System.out.println("앞에선 동물을 피해 돌다.");
  }
}
```

```java
Class Main {
  public static void main(String[] args) {
    Action manAction = new Man();         // 사람의 액션
    Action animalAction = new Animal();  // 동물의 액션

    manAction.running();                       // 결과 : 운동장을 달린다.
    manAction.hitting();                        // 결과 : 바닥을 치다.
    manAction.turning();                       // 결과 : 찻길 옆으로 돌다.

    animalAction.running();                   // 결과 : 동물을 피해 달린다.
    animalAction.hitting();                    // 결과 : 앞에선 동물을 치다.
    animalAction.turnning();                   // 결과 : 앞에선 동물을 피해 돌다.
  }
}
```

이 와 같이 추상 클래스를 상속받아 내부의 메서드들을 사용할 수 있습니다.

### 객체 지향 언어의 특징

- 상속을 통한 코드의 재사용성이 높일 수 있습니다.
- 잘 설계된 클래스는 개발 생산성 향상에도 도움이 됩니다.
- 객체란 일상생활의 모든 것이기 때문에 현실 세계의 생각대로 프로그래밍을 할 수 있습니다.
- 캡슐화로 인하여 수정할 내용이 있어도 다른 클래스에 영향을 덜 미치기 때문에 프로그래밍 유지보수성이 높아집니다.
