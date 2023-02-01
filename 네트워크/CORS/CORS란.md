## SOP

CORS를 알기 전에 *SOP(Same-Origin Policy)*에 대해 알 필요가 있다. SOP는 동일 출처 정책을 의미하는데, 간단히 말하자면 다른 출처에서 가져오는 리소스를 제한하는 보안 방식이다. 지금은 다른 출처의 리소스를 가져와 사용하는 것은 흔한 일이지만 이전에는 SOP가 강력히 적용되어 있어 다른 출처의 리소스와 상호작용하는 것이 굉장히 불편했다.

SOP가 기본적으로 적용되는 이유는 당연히 보안때문인데, 만약 사용자가 사이트를 이용 중 모종의 이유로 해커의 사이트에 접근하게 되면 사용자의 세션, 토큰등을 탈취당할 수 있다. 이렇게 된다면 해커는 탈취한 토큰을 이용해 사용자가 이용하고 있는 사이트에서 사용자의 민감한 정보에 접근할 수 있어 위험할 수 있다.

SOP는 이런 상황을 막기 위해 다른 출처의 접근을 차단한다. 하지만 현재같이 서로다른 출처끼리의 리소스를 통한 상호작용이 많은 시대에 완벽하게 SOP를 따르기는 힘들기 때문에, *CORS(Cross-Origin Resource Sharing) 정책*을 사용하여 통신한다.

![https://blog.kakaocdn.net/dn/E2KY5/btrI1hxsSYD/ZJrCDW0gdeOsWgnKvsYn51/img.png](https://blog.kakaocdn.net/dn/E2KY5/btrI1hxsSYD/ZJrCDW0gdeOsWgnKvsYn51/img.png)

위의 구성요소 중에서 **Protocol + Host + Port** 3가지가 같으면 동일 출처(Origin)라고 합니다.   

HTTP 기본 Port인 80번은 생략 가능해 있으나 없으나 동일 출처입니다

## CORS

> 교차 출처 리소스 공유(Cross-Origin Resource Sharing, CORS)는 추가 HTTP 헤더를 사용하여, 한 출처에서 실행 중인 웹 애플리케이션이 다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제입니다. - mdn
> 

이제 CORS가 왜 생겼고 쓰이고 있는지와 mdn에서 정의한 CORS에 대한 설명도 이해가 될 것이다. 중요한 점은 다른 출처의 리소스 접근과 이 접근이 가능하도록 브라우저에게 알려주는 것에 있다. 위의 정의에서도 나와있듯 CORS를 통해 다른 출처 리소스에 접근하기 위해선 HTTP의 헤더를 사용해야 하는데, 이 방법을 통해 브라우저에게 권한을 부여하도록 알려줄 수 있다.

이 방법은 ***Simple request***, ***Preflight*** 2가지 방법으로 나뉜다.

## CORS 접근 제어 시나리오

### 1. Simple request

preflight를 사용하지 않고 바로 CORS 요청한다.

조건은 아래와 같다.

- GET, POST, HEAD Method 중 하나
- Header
    - Accept
    - Accept-Language
    - Content-Language
    - Content-Type
        - application/x-www-form-urlencoded
        - multipart/form-data
        - text

### request

위와 같은 조건에 맞춰서 아래와 같이 HTTP header를 작성하고 서버에 요청을 보낸다.

그리고, Oringin에는 오리진 정보(도메인, 프로토콜, 포트)를 작성한다.

```
GET /location HTTP/1.1
Origin: <http://foo.com>

```

### response

서버는 request의 Origin의 정보를 검사 후 옳다고 판단한 경우 `Access-Control-Allow-Origin` 에 Origin 또는 `*`을 담아 response를 보낸다. `*`는 모든 도메인에 접근이 가능하다는 의미이다.

`Access-Control-Allow-Origin` 에 정보가 없다면 응답은 실패이다.

```
HTTP/1.1 200 OK
Access-Control-Allow-Origin: *

```

흐름을 정리해 보면 다음과 같다.

1. 조건에 맞춰 HTTP 헤더 작성 후 서버에 request를 보낸다.
2. 서버에서 request의 헤더 정보를 검사하고 Origin에 정보를 담아 응답한다.
3. response 헤더의 `Access-Control-Allow-Origin`안을 확인하고 서버가 요청을 허용하는지 브라우저가 확인한다.
4. 만약 헤더에 정보가 있다면 리소스에 접근할 수 있고, 없다면 에러가 발생한다.

### 2. Preflight

preflight는 실제 요청을 하기 전에 이 도메인이 안전한지 확인하기 위한 request이다. 안전하지 않은 요청을 할 때 사용이 되는데, 안전하지 않은 요청이란 데이터에 직접적인 영향을 줄 수 있는 요청이라 할 수 있다. GET, POST 뿐만 아닌 PATCH와 DELETE와 같은 메서드를 사용할 때 필요하다.

preflight는 OPTIONS 메서드와 두가지의 헤더가 사용된다.

### request

  - `Access-Control-Request-Method` : 요청하는 메서드
  - `Access-Control-Request-Headers` : 추가적인 헤더

```
OPTIONS /location HTTP/1.1
Origin: <http://foo.com>
Access-Control-Request-Method: PATCH
Access-Control-Request-Headers: Content-Type, API-Key

```

### response

반환되는 response는 HTTP Status 200과 `Access-Control-Request-Method`, `Access-Control-Request-Headers` 값을 가지고 있어야 한다. 만약 값이 없다면 에러가 발생한다.

```
HTTP/1.1 200 OK
Access-Control-Allow-Origin: <http://foo.com>
Access-Control-Request-Method: PATCH
Access-Control-Request-Headers: Content-Type, API-Key
Access-Control-Max-Age: 86400

```

`Access-Control-Max-Age` 는 퍼미션 체크를 얼마동안 캐싱할지에 대한 명시이다. 설정해둔다면 일정시간동안 preflight 요청없이 요청할 수 있다.

흐름을 정리해보자면 다음과 같다.

1. 본 요청을 보내기 전 preflight request를 서버에 보낸다.
2. 서버가 허락한다면 HTTP Status 200과 `Access-Control-Request-Method` , `Access-Control-Request-Headers` 를 응답한다. 만약 아니라면 에러를 발생시킨다.
3. 이후부턴 Simple request 요청과 같다.
