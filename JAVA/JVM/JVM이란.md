## **JVM(Java Virtual Machine)이란?**

> **자바 프로그램 실행환경을 만들어 주는 소프트웨어**입니다.
> 

Java 소스코드, 즉 원시코드(`*.java`)는 CPU가 인식을 하지 못하므로 기계어로 컴파일을 해줘야한다.

자바 코드를 컴파일하여 .class 바이트 코드로 만들면 이 코드가 자바 가상 머신 환경에서 실행됩니다. JVM은 자바 실행 환경 JRE(Java Runtime Environment)에 포함되어 있습니다. 현재 사용하는 컴퓨터의 운영체제에 맞는 자바 실행환경 (JRE)가 설치되어 있다면 자바 가상 머신이 설치되어 있다는 뜻입니다.

> Java는 어떠한 플랫폼에 영향을 받지 않는다.
> 

JVM을 사용함으로써 얻는 가장 큰 이점이 무엇일까요? 
**JVM을 사용하면 하나의 바이트 코드(.class)로 모든 플랫폼에서 동작하도록 할 수 있습니다.**

> .class 파일은 바이트 코드라고 하는데 사람이 쓰는 자바 코드에서 컴퓨터가 읽는 기계어로의 중간 단계라고 생각하시면 됩니다.
> 

※C언어의 경우

![https://blog.kakaocdn.net/dn/bSyyF2/btruTDnDSKz/73EXAY7aiTWRzHKlM2OFpK/img.png](https://blog.kakaocdn.net/dn/bSyyF2/btruTDnDSKz/73EXAY7aiTWRzHKlM2OFpK/img.png)

예를 들어 C언어로 작성된 Test.c가 있다고 해봅시다. 
이 Test.c를 윈도우 컴파일러를 사용해서 컴파일하면 Test.exe가 만들어집니다. 
윈도우 컴파일러로 컴파일되었기에 Test.exe는 윈도우에서만 실행되는 실행 파일입니다. 
리눅스 운영체제에서는 실행할 수 없습니다. 

- **즉 C / C++에서는 컴파일 플랫폼과 타겟 플랫폼이 다를 경우에는 프로그램이 동작하지 않습니다.**

만약 이 Test.exe 파일을 리눅스 운영체제에서 실행하려면 리눅스 환경을 타겟으로 크로스 컴파일을 해서 리눅스 운영체제에 맞는 실행 파일을 새로 만들어야 하는 것입니다.

※Java의 경우

![https://blog.kakaocdn.net/dn/56cSc/btruTEtjRXJ/r1JNTkEuEeY8cSKtqcXCRK/img.png](https://blog.kakaocdn.net/dn/56cSc/btruTEtjRXJ/r1JNTkEuEeY8cSKtqcXCRK/img.png)

Java의 경우에는 Java언어로 작성된 Test.java는 컴파일하면 Test.class 파일이 생성됩니다. 

- **이렇게 생성된 바이트 코드는 각자의 플랫폼에 설치되어 있는 자바 가상 머신(JVM)이 운영체제에 맞는 실행 파일로 바꿔줍니다.**

즉 Java에서는 C언어와는 달리 JVM을 사용하기 때문에 각자의 플랫폼에 맞게끔 컴파일을 따로따로 해줘야 할 필요가 없습니다. 하나의 바이트 코드로 JVM이 설치되어 있는 모든 플랫폼에서 동작이 가능하다는 이야기입니다.

### ***※ Java는 플랫폼에 종속적이지 않지만 JVM은 플랫폼에 종속적이다.***

이렇게 Java는 컴파일된 바이트코드로 어떤 JVM에서도 동작시킬 수 있기 때문에 플랫폼에 의존적이지 않습니다. 하지만 반대로 자바 가상 머신(JVM)은 플랫폼에 의존적입니다. **즉 리눅스의 JVM과 윈도우의 JVM은 서로 다릅니다.** 자바로 작성된 모든 프로그램은 자바 가상 머신에서만 실행될 수 있으므로, 자바 프로그램을 실행하기 위해서는 반드시 자바 가상 머신이 설치되어 있어야 합니다. 따라서 오라클은 대부분의 주요 운영체제뿐만 아니라 웹 브라우저, 스마트 폰, 가전기기 등에서도 자바 가상 머신을 손쉽게 설치할 수 있도록 지원하고 있습니다.

![https://blog.kakaocdn.net/dn/ll408/btruXTkzZDm/4Tg6kIPhOoQbW611htEch0/img.png](https://blog.kakaocdn.net/dn/ll408/btruXTkzZDm/4Tg6kIPhOoQbW611htEch0/img.png)

여러분의 PC에도 위와 같이 Java 프로그램이 설치되어 있을 것입니다.

## **자바 프로그램의 실행 과정과 JVM**

![https://blog.kakaocdn.net/dn/bXdEIg/btru3sF159q/aS1KNKZS4xGeQTnRnZuoy1/img.png](https://blog.kakaocdn.net/dn/bXdEIg/btru3sF159q/aS1KNKZS4xGeQTnRnZuoy1/img.png)

1. 우리가 자바로 .java 코드를 작성하고 파워쉘이나 터미널에 있는 자바 컴파일러인 javac에 컴파일 명령을 내리면 .class 파일이 만들어집니다. 
2. 이후 이 바이트 코드는 클래스 로더를 통해 JVM Runtime Data Area로 로딩 
3. 로딩된 .class 바이트 코드를 실행할 컴퓨터에 깔린 JVM에 가져다주면 그 컴퓨터가 이 프로그램을 실행할 때 이 JVM이 그때그때 기계어로 해석합니다.

### **바이트 코드를 읽는 방식**

JVM은 바이트코드를 명령어 단위로 읽어서 해석하는데, **Interpreter 방식과 JIT 컴파일 방식 두 가지 방식을 혼합하여 사용합니다.** 

먼저 **Interpreter 방식**은 **바이트코드를 한 줄씩 해석, 실행**하는 방식입니다. **초기 방식으로, 속도가 느리다는 단점**이 있습니다.

이렇게 **느린 속도를 보완하기 위해 나온 것이 JIT**(Just In Time) 컴파일 방식입니다. 
바이트코드를 **JIT 컴파일러를 이용해 프로그램을 실제 실행하는 시점**(바이트코드를 실행하는 시점)**에 각 OS에 맞는 Native Code로 변환**하여 **실행 속도를 개선**하였습니다. **하지만,** 바이트코드를 Native Code로 변환하는 데에도 **비용이 소요**되므로, JVM은 모든 코드를 JIT 컴파일러 방식으로 실행하지 않고, **인터프리터 방식을 사용하다가 일정 기준이 넘어가면 JIT 컴파일 방식으로 명령어를 실행**합니다.

### **JIT (Just In Time) 컴파일러란?**

![https://blog.kakaocdn.net/dn/cUaNHZ/btrvl7QE2QV/xgly7fewMJsJ1CkP7om0Ek/img.png](https://blog.kakaocdn.net/dn/cUaNHZ/btrvl7QE2QV/xgly7fewMJsJ1CkP7om0Ek/img.png)

기존의 자바는 인터프리터 방식으로 명령어를 하나씩 실행하게끔 이루어져 있어 실행 속도가 느렸습니다. 하지만 하드웨어가 발전하면서 자바 컴파일러도 JIT 컴파일러 방식으로 개선되어 속도적인 측면에서 상당한 개선을 이루었습니다. **JVM은 JIT(Just In Time) 컴파일러**라고 합니다. 또한, JIT 컴파일러는 **같은 코드를 매번 해석하지 않고, 실행할 때 컴파일을 하면서 해당 코드를 캐싱**해버립니다. **이후에는 바뀐 부분만 컴파일**하고, **나머지는 캐싱된 코드를 사용**합니다. 이렇게 JIT 컴파일러는 운영체제에 맞게 바이트 실행 코드로 한 번에 변환하여 실행하기 때문에 이전의 자바 해석기(Java interpreter) 방식보다 **성능이 10배 ~ 20배 정도 더 좋습니다.**
