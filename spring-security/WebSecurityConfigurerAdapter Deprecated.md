# Spring Security 6.2.4 기준
> component 기반 security 설정을 장려하는 차원에서 deprecated 됨  -> Bean 장려

[공식 블로그](https://spring.io/blog/2022/02/21/spring-security-without-the-websecurityconfigureradapter)

### 기본 형식
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
[Spring Security 공식 문서](https://docs.spring.io/spring-security/reference/servlet/authorization/authorize-http-requests.html)
- 간소화된 AuthorizationManager API를 사용함으로써 재사용과 customization을 용이하게 함
- Authentication 찾기를 지연시킴 -> 매 request 마다 인증을 조회하기 보다 인증이 필요한 요청만 들여다 볼 수 있음
- FilterSecurityInterceptor 대신 AuthorizationFilter가 사용됨

#### antMatchers - deprecated
- `?` : 하나의 문자가 같을 때 사용
	- `org/g?g`: org/gag, org/gbg, 등
- `*`: 0개 이상의 문자가 같을 때 사용
	- `org/*jsp`: org 디렉토리에 있는 모든 .jsp 파일
- `**`: 경로 상 0개 이상의 디렉토리가 같을 때 사용
	- `org/**/test.jsp`: org 하위 디렉토리에 있는 모든 test.jsp 파일

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

### FrameOption 비활성화
- h2-console 화면을 보기 위한 옵션

#### 기존 코드
```java
protected void configure(HttpSecurity http) throws Exception {
	http.headers().frameOptions().disable();
}
```
#### 권장 코드
```java
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
	http.headers((header) -> header
		.frameOptions(HeadersConfigurer.FrameOptionsConfig::disbale))
	
	/*
	다음과 같은 코드
	http.headers((header) -> header
		.frameOptions((frame) -> frame.disable()))
	**/
}
```