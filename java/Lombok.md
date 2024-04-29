## 생성자 주입
- 생성자 주입 시 @RequiredArgsConstructor를 주로 사용
- 하지만 lombok은 자식 클래스의 부모 호출을 지원하지 않음
- 즉, super 사용 불가
- 부모 클래스에 접근 시 @RequiredArgsConstructor는 사용 불가
- 기존 방식의 생성자 주입을 통한 dependency injection 구현 필요
```java
pubic class Something {
	private final UserService userService;
	private final Environment env;

	public AuthenticationFilter extends UsernamePasswordAuthenticationFilter(UserService userService,
					Environment env,
					AuthenticationManager authenticationManager) {
			super.setAuthenticationManager(authenticationManager);
			this.userService = userService;
			this.env = env;					
					}
					
}
```


