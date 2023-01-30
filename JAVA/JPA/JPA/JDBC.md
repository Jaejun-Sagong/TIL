# JDBC

> Java DataBase Connectivity 의 약어로 Java와 DB연결을 위한 표준 API이다.
> 
- **자바에서** DB를 연결하고, SQL문을 실행해서 **DB 내 데이터를 가져오기 위해서 사용하는 API**이다.
    - 자바 언어와 DB를 연결해주는 통로와 같은 것.
- 자바를 이용한 DB접속과 SQL문장의 실행, 그리고 실행 결과로 얻어진 데이터의 핸들링을 제공하는 방법과 절차에 관한 규약
- SQL과 프로그래밍 언어의 통합 접근 중 한 형태

![https://blog.kakaocdn.net/dn/bdvtgI/btrwpZ9682E/4hpiG4oEw78yYB2tbLCVqK/img.png](https://blog.kakaocdn.net/dn/bdvtgI/btrwpZ9682E/4hpiG4oEw78yYB2tbLCVqK/img.png)

### 사용 이유

- DB 학습시 **SQL이용해서 DB에다 직접 값을 넣거나 조회하는 등의 일**을 수행했음.
    - 우리가 웹을 동작, 수행시킬 때마다 번거러운 작업을 해야했음.
- 그래서 프로그램이 이 일을 **대신할 수 있게 만들어줘야 하는데 이 때 사용하는 것이 JDBC**이다.
- JAVA는 표준 인터페이스인 JDBC API를 제공한다. 그래서 사용하기 편하다. 인터페이스가 이미 정의되어 있기 때문에, **어떤 DB벤더든 간에 다 똑같은 방법으로 사용하면 된다**.
- DB벤더, 또는 기타 써드파티에서는 JDBC 인터페이스를 구현한 드라이버(driver)를 제공한다. 그래서 사용자들은 이런 드라이버를 이용하면 된다.
