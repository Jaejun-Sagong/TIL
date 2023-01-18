## **Nginx란?**

**Nginx란 트래픽이 많은 웹사이트의 서버(WAS)를 도와주는 비동기 이벤트 기반 구조의 경량화 웹 서버 프로그램입니다.** 클라이언트로부터 요청을 받았을 때 요청에 맞는 **정적 파일을 응답해주는 HTTP Web Server**로 활용되기도 하고, 또는 `Reverse Proxy Server`로 활용하여 **WAS의 부하를 줄일 수 있는 `로드밸런서` 역할**을 하기도 합니다.

## **Nginx가 만들어진 배경**

![https://blog.kakaocdn.net/dn/kX4lO/btrzOH1bM1w/RbxkG7Gqle7HAk768EXrV1/img.png](https://blog.kakaocdn.net/dn/kX4lO/btrzOH1bM1w/RbxkG7Gqle7HAk768EXrV1/img.png)

### Apache의 구조

~~최초의 웹 서버~~는 1995년 UNIX 기반으로 만들어진 NCSA HTTPd입니다(CERN httpd 웹 서버가 먼저 나왔습니다) .하지만 이 프로그램은 버그가 굉장히 많아서 개발자들이 사용할 때 불편함을 많이 느꼈습니다. 그래서 문제를 해결하기 위해서 만들어진 것이 바로 **Apache Server**입니다. **Apache 같은 경우 요청이 들어오면 connection을 생성**합니다. **그래서 새로운 클라이언트의 요청이 올 때마다 새로운 process를 생성**합니다.

![https://blog.kakaocdn.net/dn/HsQhw/btrzQH0nHt2/sUNTsv4ufuA8ZdFMv0FpJ1/img.png](https://blog.kakaocdn.net/dn/HsQhw/btrzQH0nHt2/sUNTsv4ufuA8ZdFMv0FpJ1/img.png)

### Apache의 PREFORK

그런데 프로세스를 생성하는 과정은 시간이 오래 걸리기 때문에 프로세스를 미리 만들어 놓는 **PREFORK**라는 방식을 이용했습니다. 그래서 새로운 클라이언트의 요청이 오면 미리 만들어 놓은 프로세스를 할당했습니다. 만약 만들어 놓은 프로세스가 없다면 추가로 프로세스를 생성했습니다.

![https://blog.kakaocdn.net/dn/dS4uEZ/btrzQFBeEsU/CKKps6QnC1JDkPMJgFwrK0/img.png](https://blog.kakaocdn.net/dn/dS4uEZ/btrzQFBeEsU/CKKps6QnC1JDkPMJgFwrK0/img.png)

### Apache의 확장성

이런 구조 덕분에 개발자는 다양한 모듈을 만들어서 서버에 빠르게 기능을 추가할 수 있었습니다. 즉, 확장성이 높았고, **Apache Server는 동적 컨텐츠를 처리할 수 있게 되었습니다.** 확장성이 좋다는 장점은 결국 **요청을 받고 응답을 처리하는 과정을 하나의 서버에서 해결하기 좋았습니다.**

하지만 1999년에 들어오면서 **컴퓨터가 많이 보급됨에 따라 요청이 많아져**서 **서버에 동시에 연결된 connection이 많을 때 더 이상 새로운 connection을 생성하지 못하게 되었습니다.**

![https://blog.kakaocdn.net/dn/ydfCO/btrzLIFPFLs/XeENKRemCRef3ZCMuECEk1/img.png](https://blog.kakaocdn.net/dn/ydfCO/btrzLIFPFLs/XeENKRemCRef3ZCMuECEk1/img.png)

더 이상 새로운 connetion을 생성하지 못함

이를 **C10K(connection 10000 problem)문제**라고 하는데 **connection 10000개의 문제**라는 뜻 입니다. Apache Server는 구조적으로 아래와 같은 문제점이 있었습니다.

1. **메모리 부족**: connection이 연결될 때마다 프로세스를 생성
2. **무거운 프로그램**: 확장성이 좋다는건 곧 리소스가 많다는 걸 의미
3. **CPU 부하 증가**: 많은 connection 요청이 들어오면 context switching을 많이 하기에 CPU 부하가 증가

![https://blog.kakaocdn.net/dn/wO0CR/btrzSyOWKo9/cRVpe1wbmm5qgwi9UjQwQ0/img.png](https://blog.kakaocdn.net/dn/wO0CR/btrzSyOWKo9/cRVpe1wbmm5qgwi9UjQwQ0/img.png)

### Apache Server의 문제점

즉, **Apache Server는 현대로 올수록 사용하기 꺼려졌고**, 이러한 구조적 문제점을 해결한 **Nginx**가 2004년에 나왔습니다.

**초창기 Nginx는 Apache와 함께 사용하기 위해 만들어졌습니다.** 웹 서버이기는 하지만 아파치 서버를 완전히 대체할 목적은 아니었습니다. **아파치 서버가 지닌 구조적 한계를 Nginx를 사용하면서 극복하려고 했습니다.**

![https://blog.kakaocdn.net/dn/cfXvlr/btrzUiES8cJ/L3e0u4bUl4xyk7hAHnH41k/img.png](https://blog.kakaocdn.net/dn/cfXvlr/btrzUiES8cJ/L3e0u4bUl4xyk7hAHnH41k/img.png)

### Nginx + Apache

위 이미지와 같이 **수많은 동시 connection을 Nginx가 유지**하고, **Nginx도 웹 서버이기에 정적 파일에 대한 요청은 스스로 처리하고**, **클라이언트로부터 동적 파일의 요청을 받았을 때만 아파치 서버와 connection을 형성하여 아파치 서버의 부하를 줄였습니다.**

## **3. Nginx의 구조**

그렇다면 **Nginx**는 어떠한 구조로 되어있길래 그 많은 동시 connection을 유지할 수 있을까요?

![https://blog.kakaocdn.net/dn/bUefWi/btrzUix8nEV/k7IDvC3SjVYZbR5S3ekQkK/img.png](https://blog.kakaocdn.net/dn/bUefWi/btrzUix8nEV/k7IDvC3SjVYZbR5S3ekQkK/img.png)

### Nginx의 구조

- **Nginx**는 설정파일을 읽고, 설정에 맞게 **worker process**를 생성하는 **master process**가 있습니다.
- **worker process**는 **실제로 일을 하는 프로세스**이며 **worker process**가 만들어질 때 지정된 listen 소켓을 배정받습니다.
- 그리고 그 소켓에 **새로운 클라이언트의 요청이 들어오면 connection을 형성하고 처리**합니다.
- **connection은 정해진 Keep-Alive 시간만큼 유지**됩니다. 하지만 **connection**이 형성되었다고 해서 **worker process**가 해당 **connection** 하나만 담당하지는 않습니다
- 형성된 **connection**으로부터 아무런 요청이 없다면 새로운 **connection**을 형성하거나 이미 만들어진 다른 **connection**으로부터 들어온 요청을 처리합니다.

![https://blog.kakaocdn.net/dn/MJmKR/btrzRqXWYVL/qE1yuidBZCH2tc1PhSE2wk/img.png](https://blog.kakaocdn.net/dn/MJmKR/btrzRqXWYVL/qE1yuidBZCH2tc1PhSE2wk/img.png)

### Nginx의 이벤트(event)
**Nginx**에서는 이러한 **connection 형성과 제거**, 그리고 **새로운 요청을 처리하는 것**을 `이벤트(event)`라고 합니다.

![https://blog.kakaocdn.net/dn/JvKKk/btrzOHNCKAg/K4a81dAxcSjeqc14g5LMP0/img.png](https://blog.kakaocdn.net/dn/JvKKk/btrzOHNCKAg/K4a81dAxcSjeqc14g5LMP0/img.png)

### Nginx의 메모리 부족 현상 해결

그리고 이 이벤트들은 **os커널**이 **queue**형식으로 **worker process**에게 전달합니다. 이 이벤트들은 **queue**에 담긴 상태에서 **비동기 상태로 대기**합니다. 그리고 **worker process**는 **하나의 스레드로 이벤트를 꺼내서 처리**해 나갑니다. 이런 방식은 **worker process**가 쉬지 않고 일을 하기에, 요청이 없을 때 프로세스를 방치시키는 **Apache Server**보다 **훨씬 효율적으로 자원을 사용**할 수 있습니다.

Apache: 스레드 방식

![https://blog.kakaocdn.net/dn/cTC1Pa/btrzSfBP1Be/wyssi2KZGkY3MSUNI7H5Y1/img.gif](https://blog.kakaocdn.net/dn/cTC1Pa/btrzSfBP1Be/wyssi2KZGkY3MSUNI7H5Y1/img.gif)

Nginx: Event-driven 방식

![https://blog.kakaocdn.net/dn/beiRuM/btrzQFgZoRW/rJOif4Umazi42zFK41GUk0/img.gif](https://blog.kakaocdn.net/dn/beiRuM/btrzQFgZoRW/rJOif4Umazi42zFK41GUk0/img.gif)

위 이미지를 보면 알 수 있듯이 **Apache 방식인 스레드 기반**은 하나의 커넥션 당 하나의 스레드를 잡아먹지만 **이벤트 기반 방식**은 여러 개의 connection을 전부 **Event Handler를 통해 비동기 방식**으로 처리해 먼저 처리되는 것부터 로직이 진행되게끔 합니다.

이러한 이벤트 기반 구조가 바로 Nginx의 핵심입니다.

## **4. Nginx의 장단점**

**Nginx의 단점**

- 동적 컨텐츠를 기본적으로 처리 할 수 없음
- 동적 콘텐츠에 대한 PHP 및 기타 요청을 처리하려면 NGINX가이를 실행하기 위해 외부 프로세서로 전달하고 렌더링 된 콘텐츠가 다시 전송 될 때까지 기다려야함(프로세스 속도 저하).
- 즉, 동적 웹 페이지 컨텐츠를 가진 모든 요청을 위해 외부 자원과 연계(php-fpm)

**Nginx의 장점**

- 이벤트 중심 접근 방식을 사용하여 클라이언트 요청 제공
- 제한된 하드웨어 리소스로도 여러 클라이언트 요청을 동시에 효율적으로 처리
- 단일 스레드를 통해 여러 연결을 처리 가능
- 최소한의 리소스로 웹 서버의 아키텍처를 개선하기 위해 독립형 HTTP 서버로 배치 가능

## **5. 성능 비교**

![https://blog.kakaocdn.net/dn/cXigsD/btrzLIsk7jl/wZdM4duDXHbBhqMntnRAh0/img.png](https://blog.kakaocdn.net/dn/cXigsD/btrzLIsk7jl/wZdM4duDXHbBhqMntnRAh0/img.png)

동시 커넥션 수당 메모리 사용률

위 이미지는 동시 connection 수 당 메모리 사용률을 나타냅니다. Apache Server에 비해 Nginx는 동시 connection 수가 늘어나도 메모리 사용률이 낮고 일정한 사실을 알 수 있습니다.

![https://blog.kakaocdn.net/dn/bhN6ah/btrzRVX0jIJ/hWjfmhhTbBNCQv9u5q3SnK/img.png](https://blog.kakaocdn.net/dn/bhN6ah/btrzRVX0jIJ/hWjfmhhTbBNCQv9u5q3SnK/img.png)

동시 커넥션 수에 따라 처리되는 초당 요청 수

동시 connection 수가 많아졌을 때 처리하는 초당 요청 수는 Nginx가 Apache에 비해 압도적으로 높은 모습을 보여줍니다.

## **6. 정리**

**Apache의 한계**

**클라이언트 접속마다 Process 혹은 Thread 를 생성하는 구조입니다.** 1만 클라이언트로부터 동시접속 요청이 들어온다면 CPU 와 메모리 사용이 증가하고 추가적인 **Process/Tread 생성비용이 드는 등 대용량 요청에서 한계를 보입니다.** 또한, **Apache 서버의 프로세스가 blocking 될 때 요청을 처리하지 못하고 처리가 완료될 때까지 대기상태에 있습니다.** 이는 Keep Alive(접속대기) 로 해결이 가능하지만, 효율이 떨어집니다.

**Nginx 의 정리**

**Nginx 는 위에서 언급했듯이 Event-Driven 방식으로 동작합니다.** 즉, 프로그램 흐름이 이벤트에 의해 결정이 됩니다. 한 개 또는 고정된 프로세스만 생성하고, 그 내부에서 **비동기로 효율적인 방식으로 task 를 처리**합니다. **Apache 와 달리 동시접속자 수가 많아져도 추가적인 생성비용이 들지 않습니다.**

- 비동기 이벤트 기반으로 요청하여 적은양의 스레드가 사용되기 때문에 CPU소모가 적습니다.
- Apache 와 달리 CPU 와 관계없이 I/O 들을 전부 Event Listener로 미루기 때문에 흐름이 끊이지 않습니다.
- context switching 비용이 적습니다.
