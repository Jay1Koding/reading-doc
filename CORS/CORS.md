# CORS

1. 기본적으로 아무것도 설정하지 않음 모든 외부 요청에 대해 CORS BLOCK함
2. 설정을 CUSTOM 해야하는데 이를 허용해줘야함

```java
@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http.httpBasic().disable();
        http.csrf().disable(); // 외부 POST 요청을 받아야하니 csrf는 꺼준다.
        http.cors(); // ⭐ CORS를 커스텀하려면 이렇게
        http.sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);

        http.authorizeHttpRequests()
                .requestMatchers("/**").permitAll()
                .anyRequest().authenticated();

        return http.build();
    }
}
```

- `.cors()` 메서도를 실행 시켜주면 원하는 대로 설정할 수 있음

[출처](https://velog.io/@juhyeon1114/Spring-security%EC%97%90%EC%84%9C-CORS%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)

- 순수 톰캣으로 설정하는 법

[출처](https://bug41.tistory.com/entry/Tomcat-React-%EC%83%88%EB%A1%9C%EA%B3%A0%EC%B9%A8%EC%8B%9C-404-Error)

---

## back

1. 대부분의 경우 API 통신이 성공해도 browser에서 걸러서 Access-Control-Allow-Origin을 true로 바꿔줘야함

## axios (XMLHttpRequest)

1. 대부분의 경우 서버단에서 문제가 일어날 상황이 많음
2. 앞단에선 preflight req, credential 등이 일어날 수 있기 때문에 header에 실어서 보낼 것
3. 혹은 proxy 문제일 수도 있으므로 Change origin, pathRewrite를 통해서 해결할 것
