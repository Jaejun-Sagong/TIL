## *JVM 메모리 구조*

JVM의 구조는 크게 보면, Garbage Collector, Execution Engine, Class Loader, Runtime Data Area로, 4가지로 나눌 수 있습니다.

![https://blog.kakaocdn.net/dn/pjywN/btqSduBXLIK/2QEL5c2nEJXRm0cyhvwxF1/img.png](https://blog.kakaocdn.net/dn/pjywN/btqSduBXLIK/2QEL5c2nEJXRm0cyhvwxF1/img.png)

위에서 설명하였듯이, 자바 소스 파일은 자바 컴파일러에 의해서 바이트 코드 형태인 클래스 파일이 됩니다. 그리고 이 클래스 파일은 클래스 로더가 읽어들이면서 JVM이 수행됩니다.

**(1) Class Loader**

JVM 내로 클래스 파일을 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈입니다. 런타임 시에 동적으로 클래스를 로드합니다.

**(2) Execution Engine**

클래스 로더를 통해 JVM 내의 Runtime Data Area에 배치된 바이트 코드들을 명렁어 단위로 읽어서 실행합니다. 최초 JVM이 나왔을 당시에는 인터프리터 방식이었기 때문에 속도가 느리다는 단점이 있었지만 JIT 컴파일러 방식을 통해 이 점을 보완하였습니다. 

**(3) Garbage Collector**

Garbage Collector(GC)는 힙 메모리 영역에 생성된 객체들 중에서 참조되지 않은 객체들을 탐색 후 제거하는 역할을 합니다. 이때, GC가 역할을 하는 시간은 언제인지 정확히 알 수 없습니다.

**(4) Runtime Data Area**

JVM의 메모리 영역으로 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역입니다. 이 영역은 크게 Method Area, Heap Area, Stack Area, PC Register, Native Method Stack로 나눌 수 있습니다.

![https://blog.kakaocdn.net/dn/kOOdl/btqR1E0kWdB/7El4pzDEIvx0UGXLVanKjK/img.png](https://blog.kakaocdn.net/dn/kOOdl/btqR1E0kWdB/7El4pzDEIvx0UGXLVanKjK/img.png)

1. **Method area**

모든 쓰레드가 공유하는 메모리 영역입니다. 메소드 영역은 클래스, 인터페이스, 메소드, 필드, Static 변수 등의 바이트 코드를 보관합니다.

2. **Heap area**

모든 쓰레드가 공유하며, new 키워드로 생성된 객체와 배열이 생성되는 영역입니다. 또한, 메소드 영역에 로드된 클래스만 생성이 가능하고 Garbage Collector가 참조되지 않는 메모리를 확인하고 제거하는 영역입니다.

3. **Stack area**

![https://blog.kakaocdn.net/dn/ulBPu/btqSmAVJhzs/t5uaU3DyAmRUbNKnu10bak/img.png](https://blog.kakaocdn.net/dn/ulBPu/btqSmAVJhzs/t5uaU3DyAmRUbNKnu10bak/img.png)

메서드 호출 시마다 각각의 스택 프레임(그 메서드만을 위한 공간)이 생성합니다. 그리고 메서드 안에서 사용되는 값들을 저장하고, 호출된 메서드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값들을 임시로 저장합니다. 마지막으로, 메서드 수행이 끝나면 프레임별로 삭제합니다.

4. **PC Register**

쓰레드가 시작될 때 생성되며, 생성될 때마다 생성되는 공간으로 쓰레드마다 하나씩 존재합니다. 쓰레드가 어떤 부분을 무슨 명령으로 실행해야할 지에 대한 기록을 하는 부분으로 현재 수행중인 JVM 명령의 주소를 갖습니다.

5. **Native method stack**

자바 외 언어로 작성된 네이티브 코드를 위한 메모리 영역입니다.
