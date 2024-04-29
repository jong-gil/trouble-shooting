# Spring Security 6.2.4 기준
> component 기반 security 설정을 장려하는 차원에서 deprecated 됨  -> Bean 장려

[공식 블로그](https://spring.io/blog/2022/02/21/spring-security-without-the-websecurityconfigureradapter)

## 
#### 기존 코드
```java
@Configuration
public class SecuirtyConfiguration extends WebSecurityConfigurerAdapter {
	@Override
	protected void configure(HttpSecurity http) throws Exception {
		http
			.authorizeHttpRequests((authz) -> authz 
				.anyRequest().authenticated() 
			)
			.httpBasic(withDefaults());
		}
}
```

#### 권장 코드
```java
@Configuration 
public class SecurityConfiguration {

	@Bean
	public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
		http
			.authorizeHttpRequests((authz) -> authz
				.anyRequest().authenticated()
				)
				.httpBasic(withDefaults());
		return http.build();
	}
}
```

### CSRF 비활성화
#### 기존 코드

```java
// ...
@Override  
protected void configure(HttpSecurity http) throws Exception {  
	http.csrf().disable();
}
```

#### 권장 코드
```java
// ...
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http.csrf(AbstractHttpConfigurer::disable);
    return http.build();
}```

### authorizeRequests -> authorizeHttpRequests 
[블로그](https://docs.spring.io/spring-security/reference/servlet/authorization/authorize-http-requests.html)
- 간소화된 AuthorizationManager API를 사용함으로써 재사용과 customization을 용이하게 함
- Authentication 찾기를 미룸 -> 매 request 마다 인증을 조회하기 보다 인증이 필요한 요청만 들여다 볼 수 있음
- FilterSecurityInterceptor 대산 AuthorizationFilter가 사용됨
#### 기존 코드
```java
@Override
protected void configure(HttpSecurity http) throws Exception {
	http.authorizeRequests().antMatchers("/users/**").permitAll();
}
```

#### 권장 코드
```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
	http.authorizeHttpReqeusts((authorize) -> authorize
		.requestMatchers("/users/**").permitAll());
}
```