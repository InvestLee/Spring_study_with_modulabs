# backend_study_starting_with_spring_modulabs
---
# 목차
- 1. 스프링 설치법(For IntelliJ)
- 2. Maven vs Gradle
- 3. 스프링 프로젝트 구조
- 4. 정적 콘텐츠 vs MVC vs API
- 5. Controller, Service , Repository
- 6. DI, IoC
- 7. Test
- 8. Spring Bean(component Scan VS 코드를 이용한 직접 등록)
- 9. DB 접근 기술(JDBC, JPA)
- 10. AOP
---
# 내용

※ 인프런에서 김영한 강사님의 강의를 기반으로 작성된 글입니다.

### 1. 스프링 설치법(For IntelliJ)
스프링 부트 스타터 사이트에서 스프링 프로젝트 생성(https://start.spring.io)

(이클립스로 설치하는 경우는 https://wikidocs.net/160375 링크 참조)

![image](https://user-images.githubusercontent.com/101415950/192459186-e7f13542-1063-451d-b9aa-1dc41ff08650.png)

[Basic Setting]
- Project - Gradle Project
- Spring Boot - SNAPSHOT, M1 같은 미정식 버전을 제외한 최신 버전 사용 권장
- Language - Java
- Packaging - Jar
- Java - 현재 자신의 개발 환경에 설치된 Java 버전 사용 권장

[Project Metadata]
- Group - 기업의 도메인 명 설정 (기업과 관련되지 않은 프로젝트 수행 시 자유롭게 설정)
- Artifact - 빌드 결과물의 이름 설정

[Dependencies]
- Spring Web
- Thymeleaf

설정이 완료되면  Generate 버튼을 클릭하여 zip파일을 다운받은 후 압축 해제

#### <IntelliJ에서 스프링 프로젝트 파일 열기>

<img src="https://user-images.githubusercontent.com/101415950/192464593-05687cb8-63fc-4f75-b479-444342a26928.png" width="80%" height="80%">

1. IntelliJ 상단바의 파일(File)에서 열기(Open)을 클릭
2. 압축을 푼 스프링 프로젝트 폴더에서 build.gradle 선택 후 확인 버튼 클릭

<img src="https://user-images.githubusercontent.com/101415950/192465073-0e5bcfa3-1049-4fc8-a63d-3e6e6865b05b.png" width="50%" height="50%">

3. 위와 같은 안내메시지가 팝업되면 프로젝트로 열기 클릭
4. 약간의 시간을 소요하여 자동으로 다운로드가 진행됨을 확인

위 과정을 통해 IntelliJ에서 스프링 프로젝트를 생성할 수 있음

#### <Gradle을 통해서 실행 하는 방식에서 Java로 바로 실행하는 방식으로 설정하여 실행 속도를 높이는 방법>

<img src="https://user-images.githubusercontent.com/101415950/192476018-d1ffc1dd-0427-4fa0-8fc5-385e6638200a.png" width="80%" height="80%">

1. IntelliJ 상단바의 파일(File)에서 설정(Preferences)을 클릭
2. 빌드, 실행, 배포(Build, Execution, Deployment)에서 빌드 도구(Build Tools)를 통해 Gradle을 클릭한 뒤 적색 박스와 같이 설정

   -> 다음을 사용하여 빌드 및 실행(Build and run using) - IntelliJ IDEA
 
   -> 다음을 사용하여 테스트 실행(Run tests using) - IntelliJ IDEA

   -> Gradle JVM - 설치된 자바 버전 선택

---
### 2. Maven vs Gradle

maven과 gradle은 빌드(소스 코드 파일을 컴퓨터에서 실행할 수 있는 독립 소프트웨어 가공물(Artifact)로 변환시키는 과정)를 자동화하는 Tool

외부 소스 코드(외부 라이브러리)를 자동으로 추가하고 관리(버전도 자동으로 업데이트)

[빌드의 과정]
1. 소스 코드를 컴파일
2. 테스트 코드를 컴파일
3. 테스트 코드를 실행
4. 테스트 코드 리포트 작성
5. 기타 추가로 설정한 작업 진행(ex : 소나 큐브에 코드 정적 분석 위임 등)
6. 패키징 수행(java 라이브러리 외 다른 사람이 만들어 놓은 오픈소스를 작성한 코드와 묶는 작업)
7. 최종 sw 결과물(Artifact) 생성

하기 코드는 프로젝트에 필요한 라이브러리를 정의할 때 Maven과 Gradle의 작성법에 대한 차이를 예시로 듬

[maven]
```
<dependencies>
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter</artifactId>
      </dependency>
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-test</artifactId>
      </dependency>
<dependencies> 
```

[gradle]
```
dependencies {
      implementation 'org.springframework.boot:spring-boot-starter'
      testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

#### <maven이 아닌 gradle을 선택한 이유>

- maven은 pom.xml(Project Object Model) 파일에서 build를 xml로 정의하므로 구조화하기 쉽지만 문서의 양이 비대해지므로   

  JVM 기반의 grooby를 사용하는 gradle에 비해 설정 내용이 길어지고 가독성이 떨어짐

- gradle이 바뀐 파일들만 빌드하는 점진적 빌드 방식과 빌드 결과물을 저장하여 한번 빌드된 프로젝트의 다음 빌드는 매우 적은 시간이    

  소요되는 Daemon Process 그리고 Build Cashe를 사용하므로 maven에 비해 10~100배 가량 빌드 속도가 빠름

- maven은 라이브러리가 추가되거나 각 라이브러리가 서로 다른 버전의 라이브러리를 참조하는 종속성을 가지고 있을 경우 관리가 어려움   

   -> 상속 구조를 사용하므로 특정 설정을 몇몇 모듈에서만 공통으로 사용하기 위해 불필요한 부모 프로젝트를 생성해야 하여 상속

   -> 설정이 다른 프로젝트가 하나라도 있으면 그 프로젝트는 상속할 수 없으므로 거의 모든 설정을 중복하여 작성해야함

- gradle은 설정 주입시 프로젝트의 조건을 체크할 수 있으므로 프로젝트 별로 유연하게 설정할 수 있음 (settings.gradle에서 설정) 

   -> 구성 주입 방식을 통해 조건에 따라 특정 프로젝트에만 주입하므로 불필요한 프로젝트가 필요없음


#### <gradle 라이브러리 구성>

Package, Artifact, Version 으로 구성
- group : 소스코드가 작성된 패키지 명
- Artifact : 라이브러리의 고유한 명칭
- version : 버전 명칭(생략 시 최신 버전)

라이브러리 앞에 적용된 명령어는 라이브러리가 적용될 Scope를 의미
- implementation : 전 범위에 적용
- testImplementation : 테스트 시에만 적용
- debugimplementation : 디버그 모드에서만 적용
- androidTestimplementation : 안드로이스 테스트 시에만 적용

implementation 'org.springframework.boot:spring-boot-starter'의 뜻은 아래와 같음

-> org.springframework.boot 패키지에서 spring-boot-starter 라이브러리의 최신버전을 전범위에 적용 

---
### 3. 스프링 프로젝트 구조

#### <전체적인 프로젝트 구조>

<img src="https://user-images.githubusercontent.com/101415950/192735277-c8a88b0f-3eff-4130-879f-8e4360e035ba.png" width="40%" height="40%">

1. src/main/java   

   - java 파일을 작성하기 위한 공간으로 Controller, Service, Repository, Entity 파일 등이 속한 디렉토리   

   - 이 디렉토리에 속한 <프로젝트명> + Application.java 파일은 프로그램 시작을 담당하는 파일

2. src/main/resources

   - java 파일을 제외한 HTML, CSS, Javascript, 환경파일 등 작성하는 공간으로 static, templates 그리고 application.properties 파일이   
     기본적으로 생성

3. static

   - css, fonts, images, plugin, scripts 등의 정적 리소스 파일이 속한 디렉토리

4. templates

   - 사용자에게 화면을 출력하기 위한 HTML, JSP, Thymeleaf 등 자바 객체와 연동되는 templates 파일이 저장되는 디렉토리

5. application.properties

   - 프로젝트의 환경 설정, 데이터베이스 등의 설정을 저장하는 파일   
     (Tomcat(WAS) 설정, 데이터베이스 관련 정보를 key-value 형식으로 지정 등)

   - 웹 애플리케이션을 실행하면 자동으로 로딩

6. src/test/java

   - 테스트 코드를 작성하는 공간으로 JUnit과 스프링부트의 테스팅 도구를 사용하여 서버를 실행하지 않고 테스트가 가능

   - 각각의 개발 단계에 알맞는 단위 테스트 및 통합 테스트를 진행하는 용도로 사용

7. build.gradle

   - 환경설정 및 프로젝트를 위해 필요한 플러그인과 라이브러리를 기술하는 파일 (해당 글 2장 참고)

---
### 4. 정적 콘텐츠 vs MVC vs API

#### <정적 콘텐츠>

덧셈, 뺄셈 등 연산과 같이 사용자의 행동에 의해 변화하는 동적 콘텐츠가 아닌    

한번 정해놓으면 변하지 않고 계속 유지되는 콘텐츠로 응답하는 방식

- resources/static/hello-static.html에 다음과 같이 작성 후 스프링을 실행   
```
<!DOCTYPE HTML>
<html>
<head>
<title>static content</title>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
정적 컨텐츠 입니다.
</body>
</html>
```
- URL에 http://localhost:8080/hello-static.html을 입력하면 하기 사진과 같은 결과가 도출
<img src="https://user-images.githubusercontent.com/101415950/192709042-9d1dd8e3-0ae5-40b3-9527-c3860ef04bbd.png" width="50%" height="50%">

- 동작 순서
<img src="https://user-images.githubusercontent.com/101415950/192704982-ba9520e4-253e-414d-8e89-eddd04963f52.png" width="80%" height="80%">

1. 웹브라우저에서 http://localhost:8080/hello-static.html 주소를 Tomcat(WAS)으로 전송 
2. Tomcat(WAS)에서 스프링 컨테이너로 해당 요청을 전송하여 hello-static 관련 Controller를 찾는 과정을 수행
3. Controller가 없는 경우 resources에 있는 hello-static 관련 html 파일을 찾아 웹 브라우저에 전송하여 화면에 출력



#### <MVC 방식>

시스템을 모델, 뷰, 컨트롤러 3가지 역할로 구조화한 방식(정보처리기사 참고)   

각 부분은 별도의 컴포넌트로 분리되어 있음

뷰를 여러 개 만들 수 있으므로 사용자의 요구가 발생하면 시스템이 이를 처리하고 반응하는 대화형 애플리케이션에 적합

<img src="https://user-images.githubusercontent.com/101415950/192724212-ea883698-3ea5-4806-a803-8c7f53566980.png" width="80%" height="80%">

1. Model : 추출, 저장, 삭제 등 데이터를 처리하는 역할 (내부 비즈니스 로직)
2. View : 사용자의 요청에 의해 가공된 정보를 출력하는 역할 (사용자 인터페이스)
3. controller : 사용자의 요청(url)에 적절한 비즈니스 로직(서비스)을 호출하고 그 결과를 받아 뷰로 전달   

- controller 구현
```
@Controller
public class HelloController {
   @GetMapping("hello-mvc")
   public String helloMvc(@RequestParam("name") String name, Model model) {
      model.addAttribute("name", name);
      return "hello-template";
   }
}
```

- View 구현(resources/static/hello-template.html)   
```
<html xmlns:th="http://www.thymeleaf.org">
<body>
<p th:text="'hello ' + ${name}">hello! empty</p>
</body>
</html>
```

- 동작 순서
<img src="https://user-images.githubusercontent.com/101415950/192705339-75616c1c-d6fa-407e-83a0-4a3b196ed899.png" width="80%" height="80%">

1. 웹브라우저에서 http://localhost:8080/hello-mvc?name=spring 주소를 Tomcat(WAS)으로 전송 
2. Tomcat(WAS)에서 스프링 컨테이너로 해당 요청을 전송하여 hello-mvc 관련 Controller를 찾는 과정을 수행(@GetMapping("hello-mvc"))
3. Controller에서 model에 name:spring을 담아 viewResolver(resources/static/hello-template.html)로 전송
4. hello-template에서 Thymeleaf(html 파일에서 동적으로 요청을 처리하기 위한 기술) 템플릿 엔진으로 요청을 처리하여 웹 브라우저에 전송



#### <API 방식>

- controller 구현(@ResponseBody 문자 반환)
```
@Controller
public class HelloController {

	@GetMapping("hello-string")
	@ResponseBody
	public String helloString(@RequestParam("name") String name) {
		return "hello " + name;
	}
}
```

- controller 구현(@ResponseBody 객체 반환)
```
@Controller
public class HelloController {

	@GetMapping("hello-api")
	@ResponseBody
	public Hello helloApi(@RequestParam("name") String name) {
		Hello hello = new Hello();
		hello.setName(name);
		return hello;
	}

	static class Hello {
		private String name;

		public String getName() {
			return name;
		}

		public void setName(String name) {
			this.name = name;
		}
	}
}
```

- 동작 순서
<img src="https://user-images.githubusercontent.com/101415950/192705493-8abdce7a-558a-4b1f-9f39-82585aa7f0c5.png" width="80%" height="80%">

1. 웹브라우저에서 http://localhost:8080/hello-api?name=spring 주소를 Tomcat(WAS)으로 전송 
2. Tomcat(WAS)에서 스프링 컨테이너로 해당 요청을 전송하여 hello-mvc 관련 Controller를 찾는 과정을 수행(@GetMapping("hello-api"))   
3. Controller에서 @ResponseBody 어노테이션에 의해 HTTP의 Body에 문자 or 객체를 직접 반환(객체는 JSon으로 변환되어 반환)   

   (이 과정에서 viewResolver 대신 클라이언트의 HTTP Accept 해더와 서버의 컨트롤러 반환 타입 정보 둘을 조합하여   
   
   HttpMessageConverter가 동작하여 문자 or 객체를 직접 처리)
	
	- 기본 문자 처리 : StringHttpMessageConverter
	- 기본 객체 처리 : MappingJackson2HttpMessageConverter
	- 기타 byte 처리 등 여러 HttpMessageConverter가 기본으로 등록되어 있음

---
### 5. Controller, Service , Repository

역할 별로 분할하여 가독성 및 유지보수 편의성 증진

마치 조립 기계처럼 원하는 데이터 저장소 및 객체 등을 조립하고 분해하기 위함

![image](https://user-images.githubusercontent.com/101415950/192966018-e6c0cb87-aa3b-4277-9868-7c0ca0f03f38.png)

1. DTO(Data Transfer Object)

   - 데이터 교환을 위해 사용할 객체를 만드는 과정으로, 변수 및 객체를 송수신할 데이터의 자료형에 알맞게 생성 

2. DAO(Data Access Object)

   - 데이터베이스에 접근하고, SQL을 활용하여 데이터를 실제로 조작하는 코드를 구현하는 과정 (JPA가 대신 그 역할을 수행)

3. Domain

   - 비즈니스 도메인 객체 (ex. 회원, 주문, 쿠폰 등등 주로 데이터베이스에 저장하고 관리)

4. Controller

   - 사용자의 요청에 적절한 서비스를 호출하여, 그 결과가 사용자에게 반환하는 코드를 구현 (웹 MVC의 Controller 역할과 동일)

5. Service

   - 사용자의 요청에 응답하기 위한 비즈니스 로직을 구현

6. Repository

   - 데이터베이스에 접근, 도메인 객체를 DB에 저장하고 관리

---
### 6. DI, IoC

#### <의존성 주입(DI : Dependency Injection)>

- Field Injection(필드 주입)

- Setter Injection(수정자 주입)

- Construction Injection(생성자 주입)


싱글톤 패턴

---
### 7. Test

테스트 코드의 의미는 작성된 코드를 자동으로 테스트해주는 코드를 추가로 작성한 것

테스트 코드를 이용하여 작성한 모든 코드를 한번에 테스트할 수 있으므로 직접 프로그램을 실행하여 테스트하는 것보다 효율적임

---
### 8. Spring Bean(component Scan VS 코드를 이용한 직접 등록)

---
### 9. DB 접근 기술(JDBC, JPA)

---
### 10. AOP

https://gist.github.com/ihoneymon/652be052a0727ad59601
