## Dependency 설정
- spring boot 3.x 버전에서 javax 패키지가 deprecated되고 모두 jakarta로 바뀜
- querydsl 패키지의 경우 javax를 지원하는 버전과 jakarta를 지원하는 버전이 나누어져 있음
- dependency 추가 시 jakarta를 명시해줘야 함

```groovy
dependencies {
	// 기본 패키지 생략
	
	implementation 'com.querydsl:querydsl-jpa:5.1.0:jakarta'
	annotationProcessor "com.querydsl:querydsl-apt:${dependencyManagement.importedProperties['querydsl.version']}:jakarta"
	annotationProcessor "jakarta.annotation:jakarta.annotation-api" 
	annotationProcessor "jakarta.persistence:jakarta.persistence-api"
}
```

## QClass 설정 변경
spring boot 3.x 버전으로 오면서 QClass에 대한 설정 방법이 변경됨
```groovy
// plugin, dependency 관련 설정 생략

// QClass가 들어갈 디렉토리
def querydslSrcDir = 'src/main/generated'

// ./gradlew clean 시 QClass 삭제
clean {
	delete file(querydslSrcDir)
}

// ./gradlew javacompile 시 querydslSrcDir에 QClass 생성
tasks.withType(JavaCompile) {
	options.generatedSourceOutputDirectory = file(querydslSrcDir)
}
```