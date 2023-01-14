>maven과 gradle이 빌드관리도구인 것은 알고있지만 자세한 개념은 모르기에 maven과 gradle 각각의 개념과 정확한 차이점을 알아보고자 합니다.


## **[1. 메이븐(Maven)](https://dev-coco.tistory.com/65#--%--%EB%A-%--%EC%-D%B-%EB%B-%---Maven-)**

![https://blog.kakaocdn.net/dn/PA9i4/btq8bsoZE6Y/xNQw1nKBQP2i2OURKZDZn0/img.png](https://blog.kakaocdn.net/dn/PA9i4/btq8bsoZE6Y/xNQw1nKBQP2i2OURKZDZn0/img.png)

### **[Maven이란?](https://dev-coco.tistory.com/65#Maven%EC%-D%B-%EB%-E%--%-F)**

- 아파치 메이븐은 자바용 프로젝트 관리 도구이다.
- 아파치 Ant의 대안으로 만들어졌다.
- 아파치 라이센스로 배포되는 오픈 소스 소프트웨어이다.

프로젝트를 진행하면서 사용하는 수많은 라이브러리들을 관리해주는 도구입니다.

여기서 메이븐의 특징적인 점은 그 라이브러리들과 연관된 라이브러리들까지 거미줄처럼 모두 연동이 되어 관리가 된다는 점입니다.

즉, 메이븐은 네트워크를 통해 연관된 라이브러리까지 같이 업데이트를 해주기 때문에  사용이 편리합니다.

### **[POM - Project Object Model](https://dev-coco.tistory.com/65#POM%---%--Project%--Object%--Model)**

Maven의 기능을 이용하기 위해 POM이 사용됩니다.

POM은 약자 이름 그대로 Project Object Model의 정보를 담고 있는 파일입니다.

pom.xml에서 주요하게 다루는 기능들은 아래와 같습니다.

- 프로젝트 정보 : 프로젝트의 이름, 라이센스 등
- 빌드 설정 : 소스, 리소스, 라이프사이클별 실행한 플로그인 등 빌드와 관련된 설정
- 빌드 환경 : 사용자 환경 별로 달라질 수 있는 프로파일 정보
- pom 연관 정보 : 의존 프로젝트(모듈), 상위 프로젝트, 포함하고 있는 하위 모듈 등

---

## **[2. 그래들(Gradle)](https://dev-coco.tistory.com/65#--%--%EA%B-%B-%EB%-E%--%EB%--%A--Gradle-)**

![https://blog.kakaocdn.net/dn/Eh1RA/btq79q6tnlQ/CQZ2CP3BJgDKBAiKAmCWLk/img.png](https://blog.kakaocdn.net/dn/Eh1RA/btq79q6tnlQ/CQZ2CP3BJgDKBAiKAmCWLk/img.png)

### **[Gradle이란?](https://dev-coco.tistory.com/65#Gradle%EC%-D%B-%EB%-E%--%-F)**

- 빌드, 프로젝트 구성/관리, 테스트, 배포 도구
- 안드로이드 앱의 공식 빌드 시스템
- 빌드 속도가 Maven에 비해 10~100배 가량 빠름
- JAVA, C/C++M Python 등을 지원
- 빌트툴인 Ant Builder와 Groovy 스크립트 기반으로 만들어져 기존 Ant의 역할과 배포 스크립트의 기능을 모두 사용 가능

> Groovy?
> 

기존 메이븐의 경우 XML로 라이브러리를 정의하고 활용하도록 되어 있으나,

Gradle의 경우 별도의 빌드 스크립트를 통하여사용할 어플리케이션 버전, 라이브러리 등의 항목을 설정할 수 있습니다.

Groovy 스크립트 언어로 구성되어 있기에 XML과 달리 변수선언, if, else, for 등의 로직이 구현가능하며 간결하게 구성 가능합니다.

---

## **[3. 메이븐(Maven) VS 그래들(Gradle)](https://dev-coco.tistory.com/65#--%--%EB%A-%--%EC%-D%B-%EB%B-%---Maven-%--VS%--%EA%B-%B-%EB%-E%--%EB%--%A--Gradle-)**

먼저 스프링부트를 이용하여 같은 기능과 라이브러리 의존성을 가지는 maven과 gradle 프로젝트를 생성해보겠습니다.

(Java 11 & SpringBoot 2.5.2)

### **[pom.xml](https://dev-coco.tistory.com/65#pom-xml)**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.2</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.example2</groupId>
    <artifactId>demo-maven</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>demo-maven</name>
    <description>Demo project for Spring Boot</description>
    <properties>
        <java.version>11</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
 
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
 
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
 
</project>
```

### **[build.gradle](https://dev-coco.tistory.com/65#build-gradle)**

```xml
plugins {
    id 'org.springframework.boot' version '2.5.2'
    id 'io.spring.dependency-management' version '1.0.11.RELEASE'
    id 'java'
}
 
group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '11'
 
repositories {
    mavenCentral()
}
 
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
 
test {
    useJUnitPlatform()
}
```

**1. 스크립트 길이와 가독성 면에서 gradle이 우세하다.**

**2. 빌드와 테스트 실행 결과 gradle이 더 빠르다. (gradle은 캐시를 사용하기 때문에 테스트 반복 시 차이가 더 커진다.)**

**3. 의존성이 늘어날 수록 성능과 스크립트 품질의 차이가 심해질 것이다.**

maven은 프로젝트가 커질수록 빌드 스크립트의 내용이 길어지고 가독성이 떨어집니다.

반면에 gradle은 훨씬 적은 양의 스크립트로 짧고 간결하게 작성할 수 있습니다.

maven이 정적인 형태의 XML 기반으로 작성되어 동적인 빌드를 적용할 경우 어려움이 많다면,

gradle은 Groovy를 사용하기 때문에 동적인 빌드는 Groovy 스크립트로 플러그인을 호출하거나 직접 코드를 짜면 됩니다.

maven의 경우 멀티 프로젝트에서 특정 설정을 다른 모듈에서 사용하려면 상속을 받아야 하지만,

gradle은 설정 주입 방식을 사용하기 때문에 멀티 프로젝트에 매우 적합합니다.

아래 사진은 gradle과 maven의 속도 비교 표 입니다.

**Gradle vs Maven : Performance Comparison**

![https://blog.kakaocdn.net/dn/dKtmEU/btq8bsvQoKc/DjilAAylpcHLJFRtXQCd01/img.png](https://blog.kakaocdn.net/dn/dKtmEU/btq8bsvQoKc/DjilAAylpcHLJFRtXQCd01/img.png)

![https://blog.kakaocdn.net/dn/bS9riQ/btq8aN1zMjC/Kn1fpOCrvzF1lWKkoDNy4K/img.png](https://blog.kakaocdn.net/dn/bS9riQ/btq8aN1zMjC/Kn1fpOCrvzF1lWKkoDNy4K/img.png)

![https://blog.kakaocdn.net/dn/cEGqWA/btq8dulsWv5/RPudjkBsjzmp0gKd3Qk0z1/img.png](https://blog.kakaocdn.net/dn/cEGqWA/btq8dulsWv5/RPudjkBsjzmp0gKd3Qk0z1/img.png)

![https://blog.kakaocdn.net/dn/1me5I/btq8aOznGOG/MMblyASIa9QaGkmi9xEw30/img.png](https://blog.kakaocdn.net/dn/1me5I/btq8aOznGOG/MMblyASIa9QaGkmi9xEw30/img.png)

출처 : https://bkim.tistory.com/13

지금 시점에서 Gradle을 사용하지 않을 이유는 익숙함 뿐인 것 같다.   
과거에는 maven을 많이 사용했었지만 요즘은 gradle로 넘어오는 추세라고 합니다.
