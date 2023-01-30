### **의존성(Dependency)이란?**

객체의 세계에서 협력은 필수적이며, 객체가 협력한다는 것은 객체 간의 의존성이 존재한다는 것이다. 여기서 의존성이란 `파라미터나 리턴값 또는 지역변수 등으로 다른 객체를 참조하는 것`을 의미한다.

예를 들어 비밀번호 값을 해싱하여 간단히 암호화하는 다음과 같은 SimplePasswordEncoder가 있다고 하자.

```java
@Component
public class SimplePasswordEncoder {

    public void encryptPassword(final String pw) {
        final StringBuilder sb = new StringBuilder();

        for(byte b : pw.getBytes(StandardCharsets.UTF_8)) {
            sb.append(Integer.toString((b & 0xff) + 0x100, 16).substring(1));
        }

        return sb.toString();
    }
}
```

그리고 구성원에 대한 비지니스 로직을 처리하는 MemberService에서 회원가입 시에 SimplePasswordEncoder를 사용해 비밀번호를 암호화하여 데이터베이스에 저장한다고 하자. 이러한 로직은 다음과 같이 구성될 수 있다.

```java
@Service
@RequiredArgsConstructor
@Transactional(readOnly = true)
public class MemberService {

    private final MemberRepository memberRepository;
    private final SimplePasswordEncoder passwordEncoder;

    @Transactional
    public void signUp(String email, String pw) {
        String encryptedPassword = passwordEncoder.encryptPassword(pw);

        final Member member = Member.builder()
            .email(email)
            .pw(encryptedPassword).build();

        memberRepository.save(member);
    }
}
```

이때 구성원에 대한 비지니스 로직을 담당하는 MemberService는 비밀번호를 암호화하는 SimplePasswordEncoder를 지역변수로 가지고 참조하고 있으므로 의존한다고 표현한다.

![https://blog.kakaocdn.net/dn/VeKjP/btrvJUctlVV/Igf0gB848QtV7mNiVlqxRK/img.png](https://blog.kakaocdn.net/dn/VeKjP/btrvJUctlVV/Igf0gB848QtV7mNiVlqxRK/img.png)

### **의존성 전이, 의존성(Dependency)이 위험한 이유**

앞서 설명한대로 의존성은 객체 간의 협력을 위해 필수적이다. 하지만 의존성은 위험하므로 `의존성은 최소화`되어야 한다. 왜냐하면 한 객체가 다른 객체에 의존한다는 것은 `다른 객체가 변할 때 변경이 전파될 수 있다는 것`을 의미하기 때문인데, 이를 `의존성 전이`라고 한다.

예를 들어 보안팀으로부터 SimplePasswordEncoder에 취약점이 발견되어서 암호화 로직을 변경해야 한다는 요구가 왔다고 하자. 그러면 우리는 SimplePasswordEncoder를 다른 PasswordEncoder로 교체해주어야 한다. 이러한 문제를 해결하고자 SHA256 해시 알고리즘을 기반으로 하는 SHA256PasswordEncoder를 만들었다고 하자.

```java
@Component
public class SHA256PasswordEncoder {

    private final static String SHA_256 = "SHA-256";

    public String encryptPassword(String pw)  {
        MessageDigest digest;
        try {
            digest = MessageDigest.getInstance(SHA_256);
        } catch (NoSuchAlgorithmException e) {
            throw new IllegalArgumentException();
        }

        byte[] encodedHash = digest.digest(pw.getBytes(StandardCharsets.UTF_8));

        return bytesToHex(encodedHash);
    }

    private String bytesToHex(byte[] encodedHash) {
        StringBuilder hexString = new StringBuilder(2 * encodedHash.length);

        for (final byte hash : encodedHash) {
            String hex = Integer.toHexString(0xff & hash);
            if (hex.length() == 1) {
                hexString.append('0');
            }
            hexString.append(hex);
        }

        return hexString.toString();
    }
}
```

문제는 해당 클래스만 만들어서 되는게 아니고, 비밀번호 암호화 알고리즘과 관련이 없는 MemberService도 다음과 같이 SHA256PasswordEncoder를 사용하도록 변경해주어야 한다는 것이다.

```java
@Service
@RequiredArgsConstructor
@Transactional(readOnly = true)
public class MemberService {

    private final MemberRepository memberRepository;
    private final SHA256PasswordEncoder passwordEncoder;

    @Transactional
    public void signUp(String email, String pw) {
        String encryptedPassword = passwordEncoder.encryptPassword(pw);

        final Member member = Member.builder()
            .email(email)
            .pw(encryptedPassword).build();

        memberRepository.save(member);
    }
}
```

MemberService는 구성원에 대한 비지니스 로직을 처리하는 클래스이지만 PasswordEncoder에 의해 변경되었는데, 이러한 경우를 의존성이 전이되었다고 한다. 만약 SimplePasswordEncoder에 의존하는 다른 클래스들이 있었다면 모두 변경해주어야 했을 것이다.

이러한 것들은 불필요한 변경이므로 개방 폐쇠 원칙을 준수하도록 의존성 전이를 최소화해야 한다. 의존성 전이를 최소화하기 위해서는 컴파일 타임 의존성이 아닌 런타임 의존성을 가져야 하는데, 두 의존성이 무엇이고 왜 런타임 의존성을 가져야 하는지 살펴보도록 하자.

### **[ 컴파일타임 의존성과 런타임 의존성 ]**

### **컴파일타임 의존성**

컴파일타임 의존성이란 `코드를 컴파일하는 시점에 결정되는 의존성`이며, `클래스 사이의 의존성`에 해당한다. 일반적으로 추상화된 클래스나 인터페이스가 아닌 `구체 클래스에 의존하면 컴파일타임 의존성을 갖게된다.`

아까 살펴보았던 MemberService를 다시 살펴보도록 하자.

```java
@Service
@RequiredArgsConstructor
@Transactional(readOnly = true)
public class MemberService {

    private final MemberRepository memberRepository;
    private final SHA256PasswordEncoder passwordEncoder;

    @Transactional
    public void signUp(String email, String pw) {
        String encryptedPassword = passwordEncoder.encryptPassword(pw);

        final Member member = Member.builder()
            .email(email)
            .pw(encryptedPassword).build();

        memberRepository.save(member);
    }
}
```

위의 코드에서 MemberService는 컴파일될 때 SHA256PasswordEncoder 클래스를 참조한다. 이는 MemberService가 비밀번호 암호화에는 SHA256 알고리즘을 사용한다는 것을 알고 있음을 의미한다. 그러므로 **컴파일타임 의존성**은 `결합도가 높다.`

소프트웨어 세계에서 결합도는 낮을수록 좋은데, 결합도를 낮추고 바람직한 의존성을 갖기 위해서는 결국 런타임 의존성을 가져야 한다.

### **런타임 의존성**

런타임 의존성이란 `코드(애플리케이션)를 실행하는 시점에 결정되는 의존성`이며, `객체 사이의 의존성`에 해당한다. 일반적으로 `추상화된 클래스나 인터페이스에 의존`할 때 **런타임 의존성**을 갖게 되며, 이러한 이유로 런타임 의존성과 컴파일 의존성은 다를 수 있다.

예를 들어 우리가 비밀번호 암호화를 위한 PasswordEncoder라는 인터페이스를 만들었고, 이를 구현하도록 했다고 하자.

```java
public interface PasswordEncoder {
    String encryptPassword(final String pw);
}

@Component
public class SHA256PasswordEncoder implements PasswordEncoder {

	@Override
	public String encryptPassword(final String pw)  {
		...
	}
}
```

그러면 MemberService에서는 이제 구체 클래스가 아닌 PasswordEncoder 인터페이스에 의존할 수 있게 된다.

```java
@Service
@RequiredArgsConstructor
@Transactional(readOnly = true)
public class MemberService {

    private final MemberRepository memberRepository;
    private final PasswordEncoder passwordEncoder;

    @Transactional
    public void signUp(String email, String pw) {
        String encryptedPassword = passwordEncoder.encryptPassword(pw);

        final Member member = Member.builder()
            .email(email)
            .pw(encryptedPassword).build();

        memberRepository.save(member);
    }
}
```

위의 코드에서 MemberService는 컴파일될 때 PasswordEncoder 인터페이스를 참조한다. 이는 컴파일 시점에는 MemberService가 어떠한 비밀번호 암호화 알고리즘을 사용하는지 알 수 없음을 의미하며, 애플리케이션이 실행될 때에야 어떠한 PasswordEncoder 구현체를 참조하는지 알 수 있다는 것이다. 그러므로 추상화에 의존한다면 컴파일 의존성과 런타임 의존성은 다를 수 있다.

다시 설명하자면 런타임 의존성은 추상클래스 또는 인터페이스에 의존하므로 컴파일 시점에 어느 객체에 의존하는지 알지 못한다. 컴파일 시점에는 딱 비밀번호를 암호화해야 한다는 것만 알고 있을 뿐, 실행될 때 어떠한 객체를 주입받아서 어떤 PasswordEncoder와 결합되는지 알 수 있다. 이러한 이유로 런타임 의존성은 `결합도가 낮으며` 다른 객체들과 협력할 가능성을 열어두므로 `변경에 유연한 설계` 를 갖는다.

예를 들어 SHA256PasswordEncoder 역시 레인보우 테이블 공격 기법에 의한 취약점이 발견되었다고 하자. 그래서 BCryptoPasswordEncoder를 새롭게 구현하였다고 하자.

```java
@Component
public class BCryptPasswordEncoder implements PasswordEncoder {

	@Override
	public String encryptPassword(final String pw)  {
      ...
	}
}
```

위와 같이 PasswordEncoder가 SHA256PasswordEncoder에서 BCryptoPasswordEncoder로 변경되었다고 하더라도 MemberService에는 의존성이 전이되지 않는다. 왜냐하면 런타임 의존성을 갖기 때문이다.

이러한 런타임 의존 관계를 그림으로 표현하면 다음과 같다.

![https://blog.kakaocdn.net/dn/5LOmk/btrvNGKqENt/QsAQkWqNJTJyFykFzXAut1/img.png](https://blog.kakaocdn.net/dn/5LOmk/btrvNGKqENt/QsAQkWqNJTJyFykFzXAut1/img.png)

위의 코드에서 살펴보아서 알겠지만, 결국 런타임 의존성을 갖기 위해서는 `추상화에 의존`해야 한다.

### **[ 컴파일타임 의존성과 런타임 의존성 차이 및 비교 정리 ]**

- 컴파일타임 의존성
    - 코드를 컴파일하는 시점에 결정되는 의존성
    - 클래스 사이의 의존성
    - 결합도가 높으며 변경에 유연하지 못함
- 런타임 의존성
    - 코드(애플리케이션)를 실행하는 시점에 결정되는 의존성
    - 객체 사이의 의존성
    - 결합도가 낮으며 변경에 유연함

![https://blog.kakaocdn.net/dn/c1Djf7/btrvPe7LcTN/k18pGCmfv6Q6uTmMG2czK1/img.png](https://blog.kakaocdn.net/dn/c1Djf7/btrvPe7LcTN/k18pGCmfv6Q6uTmMG2czK1/img.png)
