## *JVM 메모리 구조*

JVM의 구조는 크게 보면, Garbage Collector, Execution Engine, Class Loader, Runtime Data Area로, 4가지로 나눌 수 있습니다.

![https://blog.kakaocdn.net/dn/pjywN/btqSduBXLIK/2QEL5c2nEJXRm0cyhvwxF1/img.png](https://blog.kakaocdn.net/dn/pjywN/btqSduBXLIK/2QEL5c2nEJXRm0cyhvwxF1/img.png)

위에서 설명하였듯이, 자바 소스 파일은 자바 컴파일러에 의해서 바이트 코드 형태인 클래스 파일이 됩니다. 그리고 이 클래스 파일은 클래스 로더가 읽어들이면서 JVM이 수행됩니다.

**(1) Class Loader**

**자바는 동적 로드**, 즉 **컴파일 타임이 아니라 런타임(바이트 코드를 실행할 때)에 클래스 로드하고 링크하는 특징**이 있다. 이 `동적 로드를 담당하는 부분`이 JVM의 클래스 로더이다. 정리하자면, `클래스 로더`는 **런타임 중에 JVM의 메소드 영역에 동적으로 Java 클래스를 로드하는 역할**을 한다. 클래스 로더에는 로딩, 링크, 초기화 단계로 나뉘어져 있다.

- 로딩
    - 자바 바이트 코드(.class)를 메소드 영역에 저장한다.
        - 각 자바 바이트 코드(.class)는 JVM에 의해 메소드 영역에 다음 정보들을 저장한다.
            - 로드된 클래스를 비롯한 그의 부모 클래스의 정보
            - 클래스 파일과 Class, Interface, Enum의 관련 여부
            - 변수나 메소드 등의 정보
- 링크
    - 검증: 읽어 들인 클래스가 자바 언어 명세 및 JVM 명세에 명시된 대로 잘 구성되어 있는지 검사한다.
    - 준비: 클래스가 필요로 하는 메모리를 할당하고, 클래스에서 정의된 필드, 메소드, 인터페이스를 나타내는 데이터 구조를 준비한다.
    - 분석: 심볼릭 메모리 레퍼런스를 메소드 영역에 있는 실제 레퍼런스로 교체한다.
- 초기화
    - 클래스 변수들을 적절한 값으로 초기화 한다. 즉, static 필드들이 설정된 값으로 초기화한다

**(2) Execution Engine**

클래스 로더를 통해 JVM 내의 Runtime Data Area에 배치된 바이트 코드들을 명렁어 단위로 읽어서 실행합니다. 
최초 JVM이 나왔을 당시에는 인터프리터 방식이었기 때문에 속도가 느리다는 단점이 있었지만 JIT 컴파일러 방식을 통해 이 점을 보완하였습니다. 

**(3) Garbage Collector**

Garbage Collector(GC)는 힙 메모리 영역에 생성된 객체들 중에서 참조되지 않은 객체들을 탐색 후 제거하는 역할을 합니다. 이때, GC가 역할을 하는 시간은 언제인지 정확히 알 수 없습니다.

메모리 관리 방법 중 하나로, 시스템에서 더이상 사용하지 않는 동적 할당된 메모리 블럭을 찾아 자동으로 다시 사용 가능한 자원으로 회수하는 것으로 시스템에서 가비지컬렉션을 수행하는 부분을 가비지 컬렉터라고 한다.

**(4) Runtime Data Area**

JVM의 메모리 영역으로 자바 애플리케이션 실행을 위한 데이터와 명령어를 저장하기 위해 할당받은 메모리 공간이다. 이 영역은 크게 Method Area, Heap Area, Stack Area, PC Register, Native Method Stack로 나눌 수 있습니다.

![https://blog.kakaocdn.net/dn/kOOdl/btqR1E0kWdB/7El4pzDEIvx0UGXLVanKjK/img.png](https://blog.kakaocdn.net/dn/kOOdl/btqR1E0kWdB/7El4pzDEIvx0UGXLVanKjK/img.png)

1. **Method area**
- 가장 먼저 데이터가 저장됨
- 클래스 로더에 의해 로드된 클래스, 메소드 정보와 클래스 변수 정보 등의 바이트 코드를 보관
- 클래스 변수 남발 시 메모리 공간 부족할 수 있음
    - Java 7의 경우 부족할 수 있었으나 Java 8부터는 개선됨
- 프로그램 시작부터 종료될 때까지 메모리에 적재
- 명시적 null 선언 시 GC가 청소
- 모든 스레드가 공유하는 메모리 영역
2. **Heap area**
- new 키워드로 생성된 객체(인스턴스)가 저장되는 공간.
- 객체가 더 이상 쓰이지 않거나 명시적 null 선언 시 GC가 청소
- 모든 스레드가 공유하는 메모리 영역
3. **Stack area**

![https://blog.kakaocdn.net/dn/ulBPu/btqSmAVJhzs/t5uaU3DyAmRUbNKnu10bak/img.png](https://blog.kakaocdn.net/dn/ulBPu/btqSmAVJhzs/t5uaU3DyAmRUbNKnu10bak/img.png)

- 컴파일 시 결정되는 기본 자료형(&참조변수)이 저장됨
    - 컴파일 시 결정됨에 따라 자료형의 범위를 초과한 값 할당 등의 코드가 컴파일 단계에서 검출됨
    - 참조 변수는 기본 자료형을 Wrapper Class로 boxing한 변수(Integer, Byte 등)
- 메소드 호출 시 메모리에 FILO로 삽입
- 메소드 종료 시 메모리에서 LIFO로 제거
- 메소드가 호출될 때마다 각각의 스택 프레임이 생성됨
    - 각 스택 프레임은 하나의 메소드에 대한 정보를 저장
- {} 혹은 메소드가 종료될 때 삭제됨
    - 메소드 종료 시 프레임 별로 삭제
- 각 스레드 별로 생성

**4. PC Register**

- JVM이 수행할 명령어의 주소를 저장하는공간
    - OS의 PC 레지스터와 유사한 역할이나 CPU와는 별개로 JVM이 관리
- 스레드가 시작될 때마다 생성됨
- 각 스레드 별로 생성

**5. Native method stack**

- 다른 언어(c/c++)로 작성된 코드를 수행하기 위함
- Java Native Interface를 통해 바이트 코드로 변환됨
- Java Code를 수행하다 JNI 호출 시 Java Stack에서 Native Stack으로 동적 연결(Dynamic Linking)을 통해 확장됨
    - 따라서 나뉘어졌다고는 하나 stack에서 연결할 수 있음
- JNI(Java Native Interface) 호출 시 생성
- 각 스레드 별로 생성
