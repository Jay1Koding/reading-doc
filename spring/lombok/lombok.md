# Lombok

## 1. val / var

- 코틀린 같이 val은 불변, var는 가변
- DTO나 Entity에서 구현할 시 타입 문제가 발생하니 그 외에서 사용할 것

## 2. @NonNull

- null-check- statement를 생성
- 내부적으로 동작함
- 메소드 최상단에 삽입

```java
import lombok.NonNull;

public class NonNullExample extends Something {
  private String name;

  public NonNullExample(@NonNull Person person) {
    super("Hello");
    this.name = person.getName();
  }
}
```

## 3. @Cleanup

- 자동 리소스 관리
- FileIO, DB, Network connection 등

```java
import lombok.Cleanup;
import java.io.*;

public class CleanupExample {
  public static void main(String[] args) throws IOException {
    @Cleanup InputStream in = new FileInputStream(args[0]);
    @Cleanup OutputStream out = new FileOutputStream(args[1]);
    byte[] b = new byte[10000];
    while (true) {
      int r = in.read(b);
      if (r == -1) break;
      out.write(b, 0, r);
    }
  }
}
```

## 4. @Getter/@Setter

- @Getter
  - 1. lazy
  - 2. onMethod
  - 3. value
- @Setter : AccessLevel을 PUBLIC, PROTECTED, PACKAGE, PRIVATE 4가지로 지정
  - 1. onMethod
  - 2. onParam
  - 3. value

## 5. @ToString

- 디폴트로 non-static 필드 값이 출력
- @ToString.Exclude 같은 선언을 통해 특정한 필드값은 출력하지 않을 수 있다.

## 6. @EqualsAndHashCode

- 패스

## 7. @NoArgsConstructor, @RequiredArgsConstructor and @AllArgsConstructor

1. @NoArgsConstructor
   - 매개변수 없이 생성자 생성
2. @RequiredArgsConstructor
   - 특별한 처리가 필요한 각 필드에 대해 1개의 매개변수가 있는 생성자 생성
   - 특정 Bean에 생성자가 오직 하나만 있고, 생성자의 파라미터 타입이 빈으로 등록 가능한 존재라면 이 빈은 @Autowired 어노테이션 없이도 의존성 주입이 가능하다.
3. @AllArgsConstructor
   - 각 필드에 대해 1개의 매개변수가 있는 생성자 생성

## 8. @Data

- @Getter , @Setter , @ToString , @EqualsAndHashCode , @RequiredArgsConstructor 을 모두 선언해주는 단축키(?)이다
- 보통의 엔티티에서는 절대 사용하면 안되고, DTO나 DAO 등에는 충분한 고려 후에 사용될 수 있음

## 9. @Value

- 모든 필드 값은 디폴트로 private, final로 선언된다

## 10. @Builder

- 쉽게 Set 해줌

```java
Person.builder()
.name("Adam Savage")
.city("San Francisco")
.job("Mythbusters")
.job("Unchained Reaction")
.build();
```

## 11. @SneakyThrows

- 굳이 ❌

## 12. @Synchronized

- synchronized 키워드보다 더 안전하다
- synchronized 키워드와는 다르게 파라미터로 입력받는 Object 단위로 락을 건다

## 13. @Log
