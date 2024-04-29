### h2 console에서 connection 연결 안되는 문제
#### 에러메세지
```
Database "C:/Users/stephen/test" not found, either pre-create it or allow remote database creation (not recommended in secure environments)
```
#### 해결방법
`~/stephen` 경로에 `test.mv.db`파일 생성 후 connect

### java 서비스에서 create 했을 때 h2에 반영이 안되는 문제
`application.yml` 파일에 아래의 코드 추가
```yml
spring:
	jpa:  
		properties:  
			hibernate:  
				dialect: org.hibernate.dialect.H2Dialect  
				hbm2ddl:
					# 설정 바꿀 때에는 create 사용
					auto: update
```
