> component 기반 security 설정을 장려하는 차원에서 deprecated 됨  -> Bean 장려

[공식 블로그](https://spring.io/blog/2022/02/21/spring-security-without-the-websecurityconfigureradapter)

#### 원래 코드
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
#### 원래 코드

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

