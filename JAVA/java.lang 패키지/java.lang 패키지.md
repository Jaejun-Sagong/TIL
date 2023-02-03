## **java.lang패키지란?**

JAVA의 lang 패키지는 JAVA 프로그래밍에 필요한 가장 기본적인 클래스들이 모여있는 패키지이다. import구문으로 호출해야 사용할 수 있는 다른 패키지들과는 달리 lang패키지는 import구문 없이도 자동으로 프로그램에 포함된다. lang패키지에 포함되는 클래스는 따로 명시하지 않아도 최상위 클래스가 된다.

- **java.lang 패키지의 클래스 구조도**[ lang패키지의 모든 클래스 보기 : [JAVA API DOC : Java.lang](http://docs.oracle.com/javase/7/docs/api/java/lang/package-frame.html) ]

![https://blog.kakaocdn.net/dn/bcTMpy/btqEO0OlnNG/TAf1q1K4mq2Vjp9tyHLjP0/img.jpg](https://blog.kakaocdn.net/dn/bcTMpy/btqEO0OlnNG/TAf1q1K4mq2Vjp9tyHLjP0/img.jpg)

---

## **1. Object클래스**

- Object 클래스는 모든 자바 클래스의 최고 조상 클래스이다. 따라서 자바의 모든 클래스는 Object 클래스의 모든 메소드를 바로 사용할 수 있다.
- Object 클래스는 필드를 가지지 않으며, 총 11개의 메소드만으로 구성되어 있다.

| 메소드 | 설 명 |
| --- | --- |
| protected Object clone() | 객체 자신의 복사본 반환 (깊은 복사) |
| boolean equals(Object obj) | 두 개의 객체가 같은지 비교하여 같으면 true를, 같지 않으면 false를 반환 |
| protected void finalize() | 가비지 컬렉션 직전에 객체의 리소스를 정리할 때 호출 |
| Class getClass() | 객체의 클래스형을 반환 |
| int hashCode() | 찾을 값을 입력하면 저장된 위치를 알려주는 해시코드를 반환 |
| String toString() | 현재 객체의 문자열을 반환 |
| void notify() | wait된 스레드 실행을 재개할 때 호출 |
| void notifyAll() | wait된 모든 스레드 실행을 재개할 때 호출 |
| void wait() 
void wait(long timeout) 
void wait(long timeout, int nanos) | 스레드를 일시적으로 중지할 때 호출
주어진 시간만큼 스레드를 일시적으로 중지할 때 호출
주어진 시간만큼 스레드를 일시적으로 중지할 때 호출 |

**1-1) 공벽 반환 타입 covariant return type**

- 오버라이딩은 원래 이름과 매개변수 그리고 반환타입이 전부 같아야 하지만, covariant return tyoe을 사용하면 조상 메서드의 리턴 타입을 서브 클래스(자손 클래스)의 타입으로의 변경이 허용된다.
- 배열 또는 java.util패키지의 클래스를 복제할 때 번거롭지 않은 형변환이 가능하다.

**1-2) 얕은 복사와 깊은 복사**

- **얕은 복사(Shallow Copy)** : **참조에 의한 복사**로 원본이나 복사된 것 둘 중 하나만 변경해도 당연하게 둘 다 변경된다.

```java
public class Test {
  public static void main(String[] args) {
    List<Integer> a = new ArrayList<>();
    a.add(1);
    a.add(2);
    List<Integer> b = a;
    b.add(4);

    System.out.println(a);
    System.out.println(b);
  }
}
```

- **깊은 복사(Deep Copy)**
- 깊은 복사는 원본과 똑같은 값을 가진 **새로운 객체를 만들어 내는 복사**이다.
- 깊은 복사를 구현할 때는 Object의 clone 메소드를 이용하거나, 직접 복사기능을 하는 생성자나 로직을 만들면 된다.

```java
public class Test {
  public static void main(String[] args) {
    List<Integer> a = new ArrayList<>();
    a.add(1);
    a.add(2);
    List<Integer> b = new ArrayList<>(a);
    b.add(4);

    System.out.println(a);
    System.out.println(b);

  }
}
```

---

## **2. String 클래스**

- String 클래스에는 문자열과 관련된 작업을 할 때 유용하게 사용할 수 있는 다양한 메소드가 포함되어 있다.
- **불변 클래스(immutable class)** : String 인스턴스는 한 번 생성되면 그 값을 읽기만 할 수 있고, 변경할 수는 없다.
- 즉, 기존 문자열의 내용이 변경되는 것이 아니라 새로운 String 인스턴스가 생성되는 것이다.

| 메소드 | 설명 |
| --- | --- |
| char charAt(int index) | 해당 문자열의 특정 인덱스에 해당하는 문자를 반환함. |
| int compareTo(String str) | 해당 문자열을 인수로 전달된 문자열과 사전 편찬 순으로 비교함. |
| int compareToIgnoreCase(String str) | 해당 문자열을 인수로 전달된 문자열과 대소문자를 구분하지 않고 사전 편찬 순으로 비교함. |
| String concat(String str) | 해당 문자열의 뒤에 인수로 전달된 문자열을 추가한 새로운 문자열을 반환함. |
| int indexOf(int ch)
int indexOf(String str) | 해당 문자열에서 특정 문자나 문자열이 처음으로 등장하는 위치의 인덱스를 반환함. |
| int indexOf(int ch, int fromIndex)
int indexOf(String str, int fromIndex) | 해당 문자열에서 특정 문자나 문자열이 전달된 인덱스 이후에 처음으로 등장하는 위치의 인덱스를 반환함. |
| int lastIndexOf(int ch) | 해당 문자열에서 특정 문자가 마지막으로 등장하는 위치의 인덱스를 반환함. |
| int lastIndexOf(int ch, int fromIndex) | 해당 문자열에서 특정 문자가 전달된 인덱스 이후에 마지막으로 등장하는 위치의 인덱스를 반환함. |
| String[] split(String regex) | 해당 문자열을 전달된 정규 표현식(regular expression)에 따라 나눠서 반환함. |
| String substring(int beginIndex) | 해당 문자열의 전달된 인덱스부터 끝까지를 새로운 문자열로 반환함. |
| String substring(int begin, int end) | 해당 문자열의 전달된 시작 인덱스부터 마지막 인덱스까지를 새로운 문자열로 반환함. |
| String toLowerCase() | 해당 문자열의 모든 문자를 소문자로 변환함. |
| String toUpperCase() | 해당 문자열의 모든 문자를 대문자로 변환함. |
| String trim() | 해당 문자열의 맨 앞과 맨 뒤에 포함된 모든 공백 문자를 제거함. |
| length() | 해당 문자열의 길이를 반환함. |
| isEmpty() | 해당 문자열의 길이가 0이면 true를 반환하고, 아니면 false를 반환함. |

**2-1) 문자열 리터럴**

- 자바 소스파일에 포함된 모든 문자열 리터럴은 컴파일 시에 클래스 파일에 저장된다. 이 때 같은 내용의 문자열 리터럴은 한번만 저장된다.
- 따라서 같은 내용의 인스턴스(문자열 리터럴)는 한번만 생성해 공유할 수 있다.

---

## **3. StringBuffer클래스와 StringBuilder클래스**

- **가변 클래스(mutable class)** : String 클래스의 인스턴스는 immutable object이지만, StringBuffer 클래스의 인스턴스는 그 값을 변경할 수도 있고, 추가할 수도 있다.
- 이를 위해 StringBuffer 클래스는 내부적으로 **버퍼(buffer)**라고 하는 독립적인 공간을 가진다. 버퍼 크기의 기본값은 16개의 문자를 저장할 수 있는 크기이며, 생성자를 통해 그 크기를 별도로 설정할 수도 있다.
- StringBuffer 인스턴스를 사용하면 문자열을 바로 추가할 수 있으므로, 공간의 낭비도 없으며 속도도 매우 빨라집니다. (String 인스턴스는 새로운 객체를 생성하므로 공간이 낭비되고, 속도도 느려진다.)

| 메소드 | 설명 |
| --- | --- |
| StringBuffer append(boolean b)
StringBuffer append(char c)
StringBuffer append(char[] str)
StringBuffer append(CharSequence s)
StringBuffer append(double d)
StringBuffer append(float f)
StringBuffer append(int i)
StringBuffer append(long long)
StringBuffer append(Object obj)
StringBuffer append(String str)
StringBuffer append(StringBuffer sb) | 인수로 전달된 값을 문자열로 변환한 후, 해당 문자열의 마지막에 추가함. |
| int capacity() | 현재 버퍼 크기를 반환함. |
| StringBuffer delete(int start, int end) | 전달된 인덱스에 해당하는 부분 문자열을 해당 문자열에서 제거함. |
| StringBuffer deleteCharAt(int index) | 전달된 인덱스에 해당하는 문자를 해당 문자열에서 제거함. |
| StringBuffer insert(int offset, boolean b)
StringBuffer insert(int offset, char c)
StringBuffer insert(int offset, char[] str)
StringBuffer insert(int offset, CharSequence s)StringBuffer insert(int offset, double d)
StringBuffer insert(int offset, float f)
StringBuffer insert(int offset, int i)
StringBuffer insert(int offset, long lng)
StringBuffer insert(int offset, Object obj)
StringBuffer insert(int offset, String str) | 인수로 전달된 값을 문자열로 변환한 후, 해당 문자열의 지정된 인덱스 위치에 추가함. |
| StringBuffer reverse() | 해당 문자열의 인덱스를 역순으로 재배열함. |

**3-1) StringBuilder**

|  | StringBuffer | StringBuilder |
| --- | --- | --- |
| 공통점 | - mutable class- 
문자열 연산이 자주 있을 때 String보다 성능이 좋다. |  |
| 차이점 | - thread-safe하다
(멀티스레드환경에서 synchronized키워드가 가능하므로 동기화가 가능함.) | - thread-safe하지 않다.(
멀티스레드 환경에서는 적합하지 않다.)- 동기화를 고려하지 않기 때문에 싱글스레드 환경에서 StringBuffer에 비해 연산처리가 빠르다. |
| 적절한 경우 | multi-thread 환경 | single-thread 환경 |

---

## **4. Math 클래스**

- Math 클래스는 수학에서 자주 사용하는 상수들과 함수들을 미리 구현해 놓은 클래스
- Math 클래스의 모든 메소드는 클래스 메소드(static method)이므로, 객체를 생성하지 않고도 바로 사용할 수 있다.

| 메소드 | 설명 |
| --- | --- |
| static double random() | 0.0 이상 1.0 미만의 범위에서 임의의 double형 값을 하나 생성하여 반환함. |
| static double abs(double a)
static double abs(float a)
static double abs(int a)
static double abs(long a) | 전달된 값이 음수이면 그 값의 절댓값을 반환하며, 전달된 값이 양수이면 인수를 그대로 반환함. |
| static double ceil(double a) | 전달된 double형 값의 소수 부분이 존재하면 소수 부분을 무조건 올리고 반환함. |
| static double floor(double a) | 전달된 double형 값의 소수 부분이 존재하면 소수 부분을 무조건 버리고 반환함. |
| static long round(double a)
static int round(float a) | 전달된 값을 소수점 첫째 자리에서 반올림한 정수를 반환함. |
| static double rint(double a) | 전달된 double형 값과 가장 가까운 정수값을 double형으로 반환함. |
| static double max(double a, double b)
static float max(float a, float b)
static long max(long a, long b)
static int max(int a, int b) | 전달된 두 값을 비교하여 큰 값을 반환함. |
| static double min(double a, double b)
static float min(float a, float b)
static long min(long a, long b)
static int min(int a, int b) | 전달된 두 값을 비교하여 작은 값을 반환함. |
| static double pow(double a, double b) | 전달된 두 개의 double형 값을 가지고 제곱 연산을 수행하여, ab을 반환함. |
| static double sqrt(double a) | 전달된 double형 값의 제곱근 값을 반환함. |
| static double sin(double a)
static double cos(double a)
static double tan(double a) | 전달된 double형 값에 해당하는 각각의 삼각 함숫값을 반환함. |
| static double toDegrees(double angrad) | 호도법의 라디안 값을 대략적인 육십분법의 각도 값으로 변환함. |
| static double toRaidans(double angdeg) | 육십분법의 각도 값을 대략적인 호도법의 라디안 값으로 변환함. |

---

## **5. Wrapper 클래스**

자바에서는 8개의 기본 타입을 객체로 지정하지 않았지만, 기본 타입의 데이터를 객체로 취급해야 하는 경우가 있다.

8개의 기본 타입에 해당하는 데이터를 객체로 포장해 주는 클래스를 래퍼 클래스(Wrapper class)라고 한다.

1. 매개변수로 객체가 요구 될때.
2. 기본형 값이 아닌 객체로 저장해야 할 때.
3. 객체간의 비교가 필요할 때. 등

![https://blog.kakaocdn.net/dn/dqVVqV/btqEPvfU4Hr/rNpRF6naFjFhS2sGrWbmB1/img.png](https://blog.kakaocdn.net/dn/dqVVqV/btqEPvfU4Hr/rNpRF6naFjFhS2sGrWbmB1/img.png)

**5-1) 오토 박싱(AutoBoxing)과 오토 언박싱(AutoUnBoxing)**

JDK 1.5부터는 박싱과 언박싱이 필요한 상황에서 자바 컴파일러가 이를 자동으로 처리해 준다.

```java
Integer num = new Integer(17);// 박싱int n = num.intValue();// 언박싱

System.out.println(n);

Character ch = 'X';// Character ch = new Character('X'); : 오토박싱char c = ch;// char c = ch.charValue();           : 오토언박싱

System.out.println(c);

--실행 결과--
17
X
```

- 래퍼 클래스인 Interger 클래스와 Character 클래스에는 각각 언박싱을 위한 intValue() 메소드와 charValue() 메소드가 포함되어 있다.
- 오토 박싱을 이용하면 new 키워드를 사용하지 않고도 자동으로 Character 인스턴스를 생성할 수 있고, 반대로 charValue() 메소드를 사용하지 않고도, 오토 언박싱을 이용하여 인스턴스에 저장된 값을 바로 참조할 수 있다.
