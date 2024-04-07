# Spring Security

> 모든 url에 대해서 로그인이 되있지 않으면 강제 로그인 유도
> 인가 : 특정 url request 시 가로채서 검증
> security에서 ROLE은 필수 // role-based일 경우 접두사 필수
> 로그인을 진행한 뒤, 정보는 SecurityContextHolder에 의해 서버 세션에 관리 ➡️ 쿠키로 반환
> JWT : stateless로 관리
> 앱일 경우 stateless 이기 때문에 csrf disabled해도 문제없음

## Authentication과 SecurityContextHolder

- Authentication

  - session에 저장되는 정보
  - 역할
    1. principal
       - 식별된 사용자 정보를 보관, UserDetails의 인스턴스
       - 시스템에 따라 UserDetails 클래스를 상속해 커스텀한 형태로 유지할 수 있음
    2. credentials
       - 주체(사용자)가 올바르다는 걸 증명하는 자격 증명
       - 보통은 비밀번호, AuthenticationManager와 관련된 항목일 수 있음
    3. authorities
       - AuthenticationManager가 설정한 권한
       - Authentication 상태에 영향을 주지 않거나 수정할 수 없는 인스턴스를 사용해야함

- SecurityContextHolder
  - 인증된 사용자의 구체적인 정보를 보관
  - 어떻게 만들어지는지에 대해 신경 ❌
  - 값을 포함하고 있다면 현재 인증된 사용자 정보로 사용
  - 인증되었음을 나타내는 가장 간단한 방법은 직접 설정하는 것

---

## User, Role, Privilege(권한) Entity 객체

- UserDetailsService

  - username/password 인증방식일 때 사용자를 조회하고 검증한 뒤 UserDetails를 반환
  - 커스텀하여 `Bean`으로 등록 후 사용가능

- UserDetails

  - 인증된 사용자 정보(권한,비밀번호,사용자명,상태)를 제공하기 위한 interface
  - 기존에 구현된 User가 UserDetails의 구현체가 되면 됨
  - 시나리오에 따라 추가적으로 구현하면 됨

| return type                            | method                     | desc                                                                              |
| -------------------------------------- | -------------------------- | --------------------------------------------------------------------------------- |
| String                                 | getUsername() ➡️ 로그인 id | 계정의 이름을 리턴합니다.                                                         |
| String                                 | getPassword()              | 계정의 비밀번호를 리턴합니다.                                                     |
| boolean                                | isAccountNonExpired()      | 계정이 만료되지 않았는지를 리턴합니다.(true: 만료되지 않음)                       |
| boolean                                | isAccountNonLocked()       | 계정이 잠겨있지 않은지를 리턴합니다.(true: 계정이 잠겨있지 않음)                  |
| boolean                                | isCredentialsNonExpired()  | 계정의 비밀번호가 만료되지 않았는지를 리턴합니다.(true: 패스워드가 만료되지 않음) |
| boolean                                | isEnabled()                | 계정이 사용 가능한지를 리턴합니다.(true: 사용 가능한 계정임)                      |
| Collection<? extends GrantedAuthority> | getAuthorities()           | 계정이 가지고 있는 권한 목록을 리턴합니다.                                        |

- User

  - 사용자 정보를 포함하는 entity, UserDetails의 구현체

- Role

  - 시스템에서 관리하는 Role 정보를 저장하는 entity
  - Privilege의 컨테이너 역할, 하나의 role은 여러 개의 권한을 갖는다
  - Role과 Privilege은 N:N 관계
  - Role과 Privilege은 "role_privilege" 매핑 테이블을 통해서 관리

- Privilege

  - 시스템에서 관리하는 권한 정보를 저장하는 entity

```java
@Entity
@Builder
@Data
@NoArgsConstructor
@AllArgsConstructor
public class User implements UserDetails {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    @Column
    private String name;

    @Column
    private String email;

    @Column
    private String password;

    @Column
    private String phoneNumber;

    @Transient
    private Collection<SimpleGrantedAuthority> authorities;

    @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(
            name = "user_role",
            joinColumns = @JoinColumn(name = "user_id", referencedColumnName = "id"),
            inverseJoinColumns = @JoinColumn(name = "role_id", referencedColumnName = "id")
    )
    private List<Role> roles;

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return this.authorities;
    }

    @Override
    public String getUsername() {
        return this.name;
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
}
```

---

## Authorities vs ROLE

| Granted Authorities | ROLES           |
| ------------------- | --------------- |
| READ_PROFILE        | ROLE_ADMIN      |
| EDIT_PROFILE        | ROLE_USER       |
| DELETE_PROFILE      | ROLE_SALES      |
| ACCESS_PROFILE      | ROLE_MANAGEMENT |

- Authorities(작은 단위) : 기능 단위의 permission을 의미하며 작명 규칙은 따로 존재 ❌
- ROLE(좀더 큰 단위/ 그룹) : 여러 기능을 포함할 수 있는 포괄적인 permission을 가지며 접두사로 ROLE\_을 명시해야함

### authority based authorization

- role based 보다 세분화하여 접근을 제한할 수 있다
- 대부분 app을 운영할 때 많이 사용
- hasAuthority( ) : 해당 URL에는 명시된 authority를 소유한 사용자만 접근할 수 있다.

### role based authorization

- anyRequest( ).authenticated( ) : 로그인한 모든 사용자는 모든 resource에 접근가능하다. 이 부분은 ROLE Based Authorization과 충돌하는 부분이므로 주석처리한다.

- antMatchers( ) : 특정경로를 지정한다.
  ( /\*\* : 경로의 모든 하위 경로(디렉토리) 매핑한다.)

- permitAll( ) : 모든 임의의 사용자들의 접근을 허용한다. 로그인하지 않은 사용자도 접근할 수 있다.

- authenticated( ) : 로그인한 모든 사용자의 접근을 허용한다.

- hasRole( ) : 로그인한 사용자 중 해당 ROLE을 가진 사용자만 접근을 허용한다.

- hasAnyRole( ) : 로그인한 사용자 중 선언된 ROLE 중 하나이상의 ROLE을 가지고 있는 사용자만 접근을 허용한다.

> http request 시 접근제한을 위에서 아래로 (순서 고려!)

### form based authorization

#### 로그인

1. client가 접근이 제한된 GET /home request를 요청한다.

2. 인증되지 않은 사용자는 login html page로 redirect 된다. (팝업x)

3. 사용자가 입력한 form data(username + password 등)는 POST 메서드를 통해 server로 전달된다.

4. server는 전달된 post data를 사용해 authentication process를 수행한뒤, 정상적으로 인증된 사용자에 대한 Session ID를 생성한다.(session id가 정상적으로 생성되면, client에게 OK sign과 함께 auth cookie를 전달한다.(auth cookie는 session id를 담고 있다.))

5. 이후 client는 접근이 제한된 resource를 요청할 때 auth cookie를 함께 server에 전달해 인증된 사용자임을 server에 알린다.

6. server는 사용자가 전달한 auth cookie와 server에 저장되어 있는 session id를 비교해, 인증된 사용자인 경우 200 과 함께 resource를 response한다.

#### 로그아웃

1. client가 GET /logout request를 요청한다.

2. 서버는 해당 사용자의 session id를 server에서 만료시킨 후
   logout 된 사용자를 login page로 redirect 한다.
   (일정 시간이 지난 inactive session id도 똑같은 방식으로 만료된다.)

## inMemory

1. db를 인메모리로
2. 소수의 유저를 인메모리로

---

세션 고정 보호
계층 권한
