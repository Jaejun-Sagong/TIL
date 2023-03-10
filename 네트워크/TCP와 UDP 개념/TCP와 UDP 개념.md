### **TCP란?**

- TCP는 Transmission Control Protocol의 약자입니다
- 데이터의 송수신을 위해 IP를 사용하는 프로토콜이며, **TCP는** UDP의 비해서 **복잡하지만** **신뢰성이 높기 때문에 대부분 이 프로토콜을 사용한다고 보시면 됩니다**
- TCP는 **IP가 처리할 수 있도록 데이터를 여러 개의 패킷으로 나누고 도착지에서는 완전한 데이터로 패킷들을 재조립** 해야 합니다
- 신뢰성이 있는 것은 패킷의 분실이나 중복, 순서가 바뀌는 것 등의 문제를 해결해 주는 것입니다

![image](https://user-images.githubusercontent.com/109019062/211184876-1b2c2d85-4c7d-4311-9252-1ac64cb0da2d.png)

- 패킷이 전송된 것을 보장하기 위해서 TCP는 ACK(acknowledgment : ‘패킷을 받았다’라고 응답하는 것)라는 것을 사용하여 **패킷을 보냈는데도 상대 상대편에서 분실이 되어 데이터가 완벽하지 않을 때 수신지에서 ACK를 보내 줄 때까지 다시 데이터를 보냅니다.**

### **TCP의 특징**

- TCP는 **상위층이 넘겨준 데이터를 세그먼트라는 단위로 쪼개어** **가공하고 하위층으로 넘겨주며** 원래 I**P에서 동작하도록 설계**되었습니다. 때문에 **대부분**의 경우 **하위층은 IP**가 됩니다
- 나눠진 **세그먼트에 순서를 부여하여 전송, 수신하여 순서가 뒤바뀌는 일이 없도록** 하고 있으며 패킷이 왔다갔다하며 순번이 **뒤바뀌는 경우에도 복구**하여 **상위층이 신뢰할수 있는 연결방식**을 제공합니다
- TCP의 **연결지향형 방식**을 다른 말로 **신뢰성 스트림 서비스**라고 부릅니다

---

### **UDP란?**

- UDP는 User Datagram Protocol의 약자입니다
- TCP와는 다르게 데이터를 패킷으로 나누고 반대편에서 재조립하는 과정을 거치지 않으며 **수신지에서 제대로 받던 받지 않던 상관안하고** **데이터를 보내기만 합니다**
- 또 목적지에 도달하려고 하지만 (Best-effort) 에러가 날 수도 있고 **재전송이나, 순서 뒤바뀜에 대한 대체는 어플리케이션에서 처리해 주어야 합니다**
- 그치만 **속도가 빠릅니다.** 별도의 **연결도 필요하지 않고 ACK메시지를 통해서 확인을 받을 필요도 없기 때문에** TCP 프로토콜 보다는 **더 빠른 속도**를 낼 수 있으며 이러한 빠른 속도로 **UDP는 실시간 방송 등등에서** **사용**을 하는데 **데이터 처리가 신속**하고 **한 두 장의 프레임이 빠져도 보정이 가능**합니다

### **UDP 특징**

- UDP는 TCP와 다르게 **비연결성**을 가지며 수신측이 제대로 도착하였는지 확인 여부를 보장하지 않는 **비신뢰성 서비스** 입니다 사용자 데이터를 데이터그램에 포함해 전송합니다

**TCP와 UDP 비교**

![image](https://user-images.githubusercontent.com/109019062/211184901-bcf5cdf8-e53d-4ab9-ab37-c89494243858.png)

|  | TCP | UDP |
| --- | --- | --- |
| 데이터 전송단위 | 세크먼트 | 블록 형태의 다이어 그램 |
| 서비스의 형태 | 연결형 | 비연결형 |
| 수신 순서 | 송신 순서와 동일 | 순신 수서와 불일치 |
| 오류 제어, 흐름제어 | 있음 | 없음 |

| 전송 속도 | UDP에 비해 느림 | TCP에 비해 빠름 |
| --- | --- | --- |
