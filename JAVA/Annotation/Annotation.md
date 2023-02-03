# Annotation?

> 자바 어노테이션은 자바 코드에 메타 데이터를 제공하기 위해 사용됩니다.
> 

@문자와 어노테이션 이름으로 구성이 되며, 컴파일러에서는 @문자로 시작이 되면 어노테이션으로 판단을 합니다.

어노테이션은 값을 저장할 수 있는 엘레멘트를 가지며, 어노테이션 이름 다음에 괄호 안에 엘레멘트를 정의합니다.

```java
@Entity(name = "table")
```

위의 어노테이션은 Entity라는 이름의 어노테이션이고 name이라는 엘레멘트를 가지며 name 엘레멘트의 값은 table입니다.

어노테이션은 클래스, 인터페이스, 메소드, 메소드 파라메터, 필드, 지역 변수 위에 위치할 수 있습니다.

```java
@Entity
public class Animal {
}
```

# Built-in Java Annotations

Java의 내장형 어노테이션에 대해 알아보겠습니다.

### @Deprecated

클래스, 메소드, 필드가 deprecated 되었음을 알릴 때 사용합니다.

deprecated code를 사용하면 컴파일러에서는 warning을 표시합니다.

어노테이션을 사용할 때, @deprecated JavaDoc symbol을 사용하여, 왜 deprecated 되었는지

대신에 무엇을 사용해야 하는지 작성하는 것이 좋습니다.

```java
@Deprecated
/**
  @deprecated Use MyNewComponent instead.
*/public class MyComponent {

}
```

### @Override

부모 클래스의 메소드를 오버라이드 할 때 사용합니다.

오버라이드 할 때, 필수로 어노테이션을 사용해야 하는 것은 아니지만, 오버라이드 하고 있다는 것을 알려주기에 사용합니다.

### @SuppressWarnings

메소드에 사용하며, 사용한 메소드에 워닝을 컴파일러에서 표시하지 않도록 합니다.

deprecated 된 코드를 사용할 때 워닝이 생기지 않게 사용할 수 있습니다.

외에도 @SafeVarargs, @FunctionalInterface, @Naitve 내장형 어노테이션이 있습니다.

# Meta-Annotation

다른 어노테이션에 적용을 할 수 있는 어노테이션을 메타 어노테이션이라고 합니다.

### @Inherited

자식 클래스에 부모 클래스의 어노테이션을 가지도록 하기 위해(propagate) 사용하는 어노테이션입니다.

DerivedClass(상속을 통해 새롭게 작성되는 클래스)는 BaseClass에 선언된 @MyAnnotation, @Test가 적용됩니다.

```java
@Inherited
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface InheritedAnnotation {
}

@MyAnnotation
@Test
public class BaseClass {
}

public class DerivedClass extends BaseClass {
}
```

### @Documented

Javadocs에 어노테이션의 사용을 문서화해주게 하는 어노테이션입니다.

( Javadocs은 Default로 사용하는 어노테이션에 대해서는 문서화해주지 않습니다. )

아래처럼 ExcelCell이라는 어노테이션에 @Documented를 선언하면,

Javadocs에 Exployee 클래스에서 ExcelCell이라는 어노테이션을 사용한다고 문서화해줍니다.

```java
@Documented
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
public @interface ExcelCell {
    int value();
}

public class Employee {
    @ExcelCell(0)
    public String name;
}
```

- javadocs

![https://blog.kakaocdn.net/dn/bDFKfc/btrytjlGOtf/SdOLjeO3uoNedpIcPgQ9R0/img.png](https://blog.kakaocdn.net/dn/bDFKfc/btrytjlGOtf/SdOLjeO3uoNedpIcPgQ9R0/img.png)

### @Repeatable

엘레멘트에 어노테이션을 반복 사용할 때 사용하는 어노테이션입니다.

어노테이션 엘레멘트에 Schedules 클래스를 지정하고, Schedules 클래스에는 컨테이너를 선언해두면

Schedule 어노테이션은 Schedules 어노테이션의 배열 엘레멘트에 저장이 가능해 Schedule 어노테이션의 반복 사용이 가능해집니다.

```java
public @interface Schedules {
    Schedule[] value();
}

@Repeatable(Schedules.class)
public @interface Schedule {
    String time() default "09:00";
}

@Schedule
@Schedule(time = "15:05")
@Schedule(time = "23:00")
void scheduledAlarm() {
}
```

## Custom Annotation

어노테이션을 적용할 때는 어노테이션이 어디에 적용되며 언제까지 어노테이션 소스가 유지될 것인지를 설정하여야 하는데 소스코드에는 다음과 같이 어노테이션을 정의해 주면 된다.

```java
@Target({ElementType.[적용대상]})
@Retention(RetentionPolicy.[정보유지되는 대상])
public @interface [어노테이션명]{
	public 타입 elementName() [default 값]
    ...
}
```

@Target에는 어떠한 값(ex : 클래스, 필드, 메서드 ...)에 어노테이션을 적용할 것이지 나타낼 수 있는데 넣을 수 있는 값은 다음 표와 같다.

| ElementType 열거 상수 | 적용대상 |
| --- | --- |
| TYPE | 클래스, 인터페이스, 열거 타입 |
| ANNOTATION_TYPE | 어노테이션 |
| FIELD | 필드 |
| CONSTRUCTOR | 생성자 |
| METHOD | 메소드 |
| LOCAL_VARIABLE | 로컬 변수 |
| PACKAGE | 패키지 |

@Retention에는 어노테이션 값들을 언제까지 유지할 것인지 값을 입력하는데 각 값이 가지는 의미는 다음 표와 같다. 보통 어노테이션은 Runtime시에 많이 사용하므로 **대부분의 어노테이션의 Retention 값은 RUNTIME**으로 되어있다.

| RetentionPolicy 열거 상수 | 설명 |
| --- | --- |
| SOURCE | 소스상에서만 어노테이션 정보를 유지한다. 소스 코드를 분석할
때만 의미가 있으며, 바이트 코드 파일에는 정보가 남지 않는다. |
| CLASS | 바이트 코드 파일까지 어노테이션 정보를 유지한다. 하지만 리플렉션을
이용해서 어노테이션 정보를 얻을 수는 없다. |
| RUNTIME | 바이트 코드 파일까지 어노테이션 정보를 유지하면서 리플렉션을 이용해서
런타임에 어노테이션 정보를 얻을 수 있다. |

### **어노테이션의 배치 및 사용**

어노테이션의 사용은 클래스를 참고하는 소스의 흐름상에서 Reflection을 사용하는 방법을 통해서 어노테이션 값을 활용하도록 한다. 말로는 설명이 애매하니 예제 소스를 작성하도록 한다.

작성하려는 예제 소스는 다음과 같다.

1. MyAnnotation.java (어노테이션)2. MyService.java (사용되는 클래스)

3. MyMain.java (MyService를 사용하는 클래스)

다음 코드는 MyMain 클래스가 MyService 클래스의 메서드에  MyAnnotation어노테이션 설정이 있는지 확인하고 각 메서드에 설정된 어노테이션에 삽입된 값에 따라 특정값을 일정한 숫자만큼 출력하는 예제이다.

[MyAnnotation.java]

```java
package myannotation;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {
	String value() default "-";
	int number() default 15;
	
}
```

[MyService.java]

```java
package myannotation;

public class MyService {
	@MyAnnotation
	public void method1() {
		System.out.println("실행내용1");
	}

	@MyAnnotation("*")
	public void method2() {
		System.out.println("실행내용2");
	}

	@MyAnnotation(value = "*", number = 20)
	public void method3() {
		System.out.println("실행내용3");
	}
}
```

[MyMain.java]

```java
package myannotation;

import java.lang.reflect.Method;

public class MyMain {
	public static void main(String[] args) {
		Method[] methodList=MyService.class.getMethods();
		
		for(Method m : methodList) {
			if(m.isAnnotationPresent(MyAnnotation.class)) {
				System.out.println(m.getName());
				MyAnnotation annotation=m.getDeclaredAnnotation(MyAnnotation.class);
				
				String value=annotation.value();
				int number=annotation.number();
				for(int i=0;i<number;i++) {
					System.out.print(value);
				}
				System.out.println();				
			}
		}
	}
}
```

결과값

![https://blog.kakaocdn.net/dn/bzVRUZ/btqza9XvQQd/wcBJev1TRkS5S2KHWGfPq1/img.png](https://blog.kakaocdn.net/dn/bzVRUZ/btqza9XvQQd/wcBJev1TRkS5S2KHWGfPq1/img.png)

필드, 생성자, 메소드에 적용된 어노테이션 정보는 위에서 처럼 .class로부터 얻을 수 있다.

- 클래스.class의 다음 메소드를 이용해서
- java.lang.reflect 패키지의 Field, Constructor, Method 클래스의 배열을 얻어냄

| 리턴타입 | 메소드명(매개변수) | 설명 |
| --- | --- | --- |
| Field[] | getFields() | 필드 정보를 Field 배열로 리턴 |
| Constructor[] | getConstructors() | 생성자 정보를 Constructor 배열로 리턴 |
| Method[] | getDelclaredMethods() | 메소드 정보를 Method 배열로 리턴 |

**[어노테이션 정보를 얻기 위한 메소드]**
| 리턴타입 | 메소드명(매개변수) |
| --- | --- |
| boolean | isAnnotationPresent(Class<? Extends Annotation> annotationClass) |
|  | 지정한 어노테이션이 적용되었는지 여부. Class에서 호출했을 경우 상위 클래스에 적용된 경우에도 true를 리턴한다. |
| Annotation | getAnnotation(Class<T> annotationClass |
|  | 지정한 어노테이션이 적용되어 있으면 어노테이션을 리턴하고 그렇지 않다면 null을 리턴한다. Class에서 호출했을 경우 상위 클래스에 적용된 경우에도 어노테이션을 리턴한다. |
| Annotation[] | getAnnotations() |
|  | 적용된 모든 어노테이션을 리턴한다. Class에서 호출했을 경우 상위 클래스에 적용된 어노테이션도 모두 포함한다. 적용된 어노테이션이 없을 경우 길이가 0인 배열을 리턴한다. |
| Annotation[] | getDeclaredAnnotations() |
|  | 직접 적용된 모든 어노테이션을 리턴한다. Class에서 호출했을 경우 상위 클래스에 적용된 어노테이션은 포함되지 않는다. |
