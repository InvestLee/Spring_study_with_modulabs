# 스프링 핵심원리
---
# 목차
- 1. 스프링 vs 스프링부트
- 2. 객체지향원리와 스프링
- 3. 객체지향 SOLID 원칙
- 4. 스프링 컨테이너와 스프링 빈
---
# 내용

※ 인프런에서 김영한 강사님의 강의를 기반으로 제가 이해한 내용을 작성된 글입니다.   

### 1. 스프링 vs 스프링부트

<br/>

#### <스프링>
- 자바 오픈소스 애플리케이션 프레임워크 중 하나로 스프링의 기본철학은 특정 기술에 종속되지 않고 객체를 관리할 수 있는 프레임워크를 제공하는 것

- 컨테이너로 자바 객체를 관리하면서 DI와 IOC를 통해 결합도를 낮춤으로써 개발을 편리하게 하는 프레임워크

- 즉 부품 바꿔서 조립하듯이 객체 지향 어플리케이션을 개발하도록 도와주는 프레임워크

<br/>

#### <스프링 부트>
- 단독으로 실행할 수 있는 스프링 애플리케이션을 쉽게 생성

- 내장 서버(Tomcat, jetty 등)로 구동하므로 서버를 별도로 설치할 필요 없음, 간단한 배포 서버 구축

- dependency 외부 라이브러리 버전 관리 권장 버전으로 자동 설정

- starter 종속성 제공(한 번에 필요한 라이브러리 모두 가져옴)으로 편리한 빌드 구성

- application.properties나 application.yml을 통한 편리한 Configuration 설정

- 기본적으로 default로 미리 설정되어 있어 손쉽게 구동할 

<br/>

---
### 2. 객체지향원리와 스프링

객체 지향 프로그래밍은 프로그램을 명령어의 목록으로 인식하기보다 여러 개의 독립적인 객체들의 상호작용을 통해 프로그램을 유연하고 변경 용이하게 만드는 프로그래밍을 의미

#### <객체 지향의 특징>

- 추상화

	- 객체의 공통된 속성이나 메소드를 추출 
	
	- 추상클래스, 인터페이스 등 미완성 설계도를 통해 기본 틀을 잡는 특성

```
abstract class Predator{
    abstract String getFood();

    void printFood() {
        System.out.printf("my food is %s\n", getFood());
    }

    static int LEG_COUNT = 4;  // 추상 클래스의 상수는 static 선언이 필요하다.
    static int speed() {
        return LEG_COUNT * 30;
    }
}

class Tiger extends Predator{
    public String getFood(){
    	return "meat"
    }
}
```

- 캡슐화

	- 특정 메서드와 속성을 접근제어자(private 등)을 통해 숨기는 특성
	
	- 데이터 값을 외부에서 직접 접근하는 것을 방지
	
	- public method로 값을 통제

```
public class Suit{
    private String name;

    public Suit(String name){
    	this.name = name;
    }
    
    public void setName(String name){
    	this.name = name;
    }
}
```

- 상속

	- 자식(서브) 클래스가 부모(슈퍼) 클래스의 기능과 메서드를 물려받는 것

	- 코드의 재사용성 증진 및 코드 중복 제거

```
추상화 코드 참조
```

- 다형성

 	- 서로 다른 클래스의 객체가 동일한 요청을 받았을 때 각자의 방식으로 동작하는 능력
 	
	- Overriding, Overloading이 대표적인 케이스

	- 스프링에서는 다형성을 가장 중요하게 사용

```
interface Barkable {
    void bark();
}

class Tiger implements Barkable {
    public void bark() {
        System.out.println("어흥");
    }
}

class Lion implements Barkable {
    public void bark() {
        System.out.println("으르렁");
    }
    
    public void bark(int n) {
        for (int i = 0; i < n ; i++){
		System.out.println("으르렁");
	}
    }
}
```

#### <스프링과 다형성>

- 스프링은 IoC와 DI를 통해 역할과 구현을 분리하도록 지원하여 다형성을 극대화하는 프레임워크

- 마치 드라마 역할에 따른 배우를 선택하듯이 Lego를 조립하듯이 구현을 편리하게 변경 가능

- 이를 이해하기 위해 아래 예제를 통해 먼저 역할과 구현을 구분할 필요가 있음

<br/>
[운전자와 자동차]

- K3에서 아반떼로 바뀌어도 운전 면허를 새로 따거나 운전 방법을 새로 배우지 않음

- 즉 운전자 역할(인터페이스)와 자동차 역할(인터페이스)의 변경은 일어나지 않음

<img src="https://user-images.githubusercontent.com/101415950/194824932-ff7b9584-e8e5-4bf2-95db-41ebff92d545.png" width="80%" height="80%">

[공연 무대]

- 로미오 역할이 장동건에서 원빈으로 바껴도 로미오의 대사가 변경이 되지 않음

- 줄리엣 역할이 김태희에서 송혜교로 바뀌어도 로이오의 행위 대본은 변경되지 않음

- 즉 로미오 역할(인터페이스)와 줄리엣 역할(인터페이스)의 변경은 일어나지 않음

<img src="https://user-images.githubusercontent.com/101415950/194825817-4dafaf82-2947-46c0-bd12-1acea2ffd7d1.png" width="80%" height="80%">

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

<br/><br/>

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

※ @RequestParam("name")은 url이 전달될 때 value가 담긴 name 파라미터를 받아올 때 사용, 즉 HTTP 요청 파라미터를 받기 위해 사용

- 실행 결과(http://localhost:8080/hello-mvc?name=spring!!!!!!)
<img src="https://user-images.githubusercontent.com/101415950/193284509-dcf94e77-8f6c-4057-bdfc-876eabfa4a1f.png" width="80%" height="80%">

- 동작 순서
<img src="https://user-images.githubusercontent.com/101415950/193286010-26b7449d-d3a2-458a-ad77-4b3e12c7ecfb.png" width="80%" height="80%">

1. 웹브라우저에서 http://localhost:8080/hello-mvc?name=spring 주소를 Tomcat(WAS)으로 전송 
2. Tomcat(WAS)에서 스프링 컨테이너로 해당 요청을 전송하여 hello-mvc 관련 Controller를 찾는 과정을 수행(@GetMapping("hello-mvc"))
3. Controller에서 model에 name:spring을 담아 viewResolver(resources/static/hello-template.html)로 전송
4. hello-template에서 Thymeleaf(html 파일에서 동적으로 요청을 처리하기 위한 기술) 템플릿 엔진으로 요청을 처리하여 웹 브라우저에 전송

<br/><br/>

#### <API 방식, Application Programming Interface>

API는 소프트웨어가 다른 소프트웨어로부터 지정된 형식으로 명령을 받을 수 있는 수단

프로그램들이 서로 상호작용하는 것을 도와주는 매개체 역할 수행

어떤 프로그래밍 언어를 쓰든 무슨 프레임워크를 쓰든 API 폼에 맞춰서 개발

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
- 실행 결과(http://localhost:8080/hello-api?name=spring!!!!!!)
<img src="https://user-images.githubusercontent.com/101415950/193285275-2ff6e6d7-dc8a-47c2-a1df-65ddef51c91b.png" width="80%" height="80%">

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

   - 변수 및 객체를 송수신할 데이터의 자료형에 알맞게 생성

   - 데이터를 담아서 전달하는 바구니 역할을 하는 데이터 전송 객체

   - Domain(Entity Class)와 DTO를 분리하는 이유는 DB와 View 사이의 역할을 분리하기 위함   
     (Controller에서 Domain(Entity Class)에 직접 사용하지 못하게 하여 의도하지 않은 DB 컬럼 변경 방지)

2. DAO(Data Access Object)

   - 데이터베이스에 접근하고, SQL을 활용하여 데이터를 실제로 조작하는 코드를 구현하는 과정 (JPA가 대신 그 역할을 수행)

3. Domain(Entity Class)

   - 비즈니스 도메인 객체 (ex. 회원, 주문, 쿠폰 등등 주로 데이터베이스에 저장하고 관리)

   - 실제 DB 테이블과 Mapping되는 클래스로 DB 테이블 내 존재하는 컬럼만을 속성으로 가져야 함

4. Controller

   - 사용자의 요청에 적절한 서비스를 호출하여, 그 결과가 사용자에게 반환하는 코드를 구현 (웹 MVC의 Controller 역할과 동일)

5. Service

   - 사용자의 요청에 응답하기 위한 비즈니스 로직을 구현

   - DB의 데이터가 필요할 때는 리포지토리에게 요청

   - 엔티티 객체와 DTO 객체를 서로 변환하여 양방향으로 전달

6. Repository

   - Domain(Entity Class)에 의해 생성된 DB에 접근하는 메서드들을 사용하기 위한 인터페이스

   - DB 연결, 해제, 자원 관리 및 CRUD 작업 처리(ex. 도메인 객체를 DB에 저장하고 관리)

---
### 6. DI, IoC

#### <의존성 주입(DI : Dependency Injection)>

객체의 의존 관계를 외부에서 주입하는 방식

스프링에서는 의존 설정에 의해 Spring Container 의존성 자동 주입이 발생

Controller가 service와 repository를 통해서만 데이터를 조회하고 수정할 수 있는 것을 의존 관계가 있다고 표현 가능 

Spring에서는 주입이 필요한 객체에 @Autowired 어노테이션을 붙여 주입

주입할 객체가 하나인 경우 @Autowired 어노테이션 생략이 가능 

@Autowired가 여러 개 있을 경우 가장 많은 의존성을 주입할 수 있는 생성자를 사용하여 의존성 주입

기본적으로 설정을 바꾸지 않는 이상 Spring Container에 명칭이 동일한 객체 등록 불가능(싱글톤 패턴)

<br/>

- DI를 사용해야 하는 이유

	- 역활을 분리하여 응집도를 높이고 결합도를 낮춰 유지보수에 유연한 구조로 만들기 위해

	- 객체를 직접 주입하는 경우 객체의 변경이 필요하면 특정 객체를 사용한 모든 클래스 변경 필요(강한 결합)

	- 객체를 외부에서 주입하는 경우 객체를 한 번만 생성하여 재사용 가능하므로 유지보수 용이(약한 결합)


[객체를 직접 주입하는 경우]
```
//1. 양복 클래스
public class Suit{
    private String name;

    public Suit(){
    }
}

//2. 사람A, 사람B 클래스에서 양복 객체 생성
public class HumanA{
    public Suit suit;
    
    public HumanA() {
        this.suit = new Suit();
    }
}

public class HumanB{
    public Suit suit;
    
    public HumanB() {
        this.suit = new Suit();
    }
}
```

```
//3. 양복 클래스에서 양복 객체에 메이커 명을 지어준다면,
public class Suit{
    private String name;

    public Suit(String name){
        this.name = name;
    }
}

//4. 사람A, 사람B 클래스에 있는 양복 객체에도 해당 변경사항을 적용해야함
public class HumanA{
    public Suit suit;
    
    public HumanA() {
        this.suit = new Suit(String name);
    }
}

public class HumanB{
    public Suit suit;
    
    public HumanB() {
        this.suit = new Suit(String name);
    }
}
```


제어 순서 : 사람 -> 양복 -> 양복 명칭 -> .....

<br/>

[객체를 외부에서 주입하는 경우]
```
//1. 양복 클래스(양복 객체 한 번만 생성)
public class Suit{
    private String name;

    public Suit(){
    }
}
Suit suit = new Suit();

//2. 사람A, 사람B 클래스에서 양복 객체 생성(양복 객체 사용/재사용)
public class HumanA{
    public Suit suit;
    
    public HumanA(Suit suit) {
        this.suit = Suit;
    }
}

public class HumanB{
    public Suit suit;
    
    public HumanB(Suit suit) {
        this.suit = Suit;
    }
}
```

```
//3. 양복 클래스에서 양복 객체에 메이커 명을 지어줘도
public class Suit{
    private String name;

    public Suit(String name){
        this.name = name;
    }
}
Suit suit = new Suit(String name);

//4. 사람A, 사람B 클래스는 변경이 필요하지 않음
public class HumanA{
    public Suit suit;
    
    public HumanA(Suit suit) {
        this.suit = Suit;
    }
}

public class HumanB{
    public Suit suit;
    
    public HumanB(Suit suit) {
        this.suit = Suit;
    }
}
```

제어 순서 : ....양복 명칭 -> 양복 -> 사람 

 <br/><br/>

- Field Injection(필드 주입)
```
@Controller
public class MemberController {
	
	@Autowired
	private MemberService memberService;
}
```
   
  1. 생성자 or Setter가 없으므로 수동 의존성 주입이 필요한 테스트 불가능
	
  2. 의존성이 프레임워크에 강하게 종속되는 문제점 발생
   
 <br/><br/>
   
- Setter Injection(수정자 주입)
```
@Controller
public class MemberController {
	private  MemberService memberService;
	
	@Autowired
	public void setMemberController(MemberService memberService) {
		this.memberService = memberService;
	}
}
```
  
  1. 속성에 final를 작성할 수 없으므로 의존성 불변을 보장할 수 없음 

  2. 런타임 중 setter를 다시 호출하여 이미 주입된 의존성 변경이 가능

  3. 주로 런타임 중 의존성 수정하거나 선택적 의존성(의존성 주입이 반드시 필수가 아닌 것을 의미)에 사용   
  
<br/><br/> 
  
- Construction Injection(생성자 주입)
```
@Controller
public class MemberController {
	private final MemberService memberService;
	
	@Autowired
	public MemberController(MemberService memberService) {
		this.memberService = memberService;
	}
}
```

  1. 객체의 최초 생성 시점에 스프링이 모든 의존성을 주입

  2. 생성자 호출 시 final에 의해서 의존성 주입이 최초 1회만 이루어짐

  3. 생성자 주입은 의존관계 불변이므로 NullPointerException 방지(다른 방식은 직접 객체를 생성하여 방지)
  
<br/>

- 주입 대상이 여러 개인경우

  - 의존성 주입 우선 순위는 생성자 -> 필드 -> 수정자 순

  - 의존성 주입 대상을 찾을 때 자료형 -> @Qualifier -> @Primary -> Bean name 순으로 탐색   
     (매개변수명과 등록된 Bean의 이름이 일치하는지 체크)

  - @Primary 어노테이션은 해당 타입의 의존성을 주입할 때 우선적으로 주입

  - @Qualifier(value = "bean 객체 이름") 의존성 주입 객체를 선택가능

<br/>

- 싱글톤 패턴

	- 어떤 클래스의 객체가, 해당 프로세스에서 딱 하나만 만들어져 있어야 할 때 사용

	- 즉 어느 페이지에서든 해당 객체를 동일한 것으로 사용해야 할 때 사용

	- 스프링에서는 싱글톤 패턴을 적용하지 않아도 기본적으로 객체를 싱글톤으로 관리하여 싱글톤 단점 해결

	- 스프링 컨테이너 종료 시 Spring Bean 전부 삭제하므로 단위테스트가 가능

	- 스프링에서 싱글톤으로 관리하는 이유는 대규모 트래픽을 처리하기 위함(새로 생성하지 않고 동일한 객체를 반환하므로)

	- ex) 스마트폰에서 다크모드 설정 시 다른 페이지로 이동하더라도 다크모드가 유지되어야 함

#### <제어의 역전(IoC : Inversion of Control)>

개발자가 작성한 객체의 제어 권한이 프레임워크로 넘어가서 스프링 컨테이너에서 객체를 호출하는 것

위 DI 파트의 사람과 양복의 예시처럼 작은 부품부터 거꾸로 조립되는 것이 특징(제어흐름이 바뀜)

component Scan 방식이나 코드로 직접 등록하는 방식에 의해 스프링 컨테이너에 Spring Bean 형태로 등록 후 의존성 자동 주입(8장에서 설명)

자주 사용하는 객체를 미리 메모리에 올려 두고 사용자가 객체에 대한 사용 요청했을 때, 올려둔 객체를 재활용하는 것이 IoC의 개념

※ Sprint Bean : 스프링 컨테이너가 관리하는 객체

<img src="https://user-images.githubusercontent.com/101415950/193053533-899af1ee-89e0-491e-8761-cfa9f232354d.png" width="80%" height="80%">

---

### 7. Test(with JUnit)

테스트 코드의 의미는 작성된 코드를 자동으로 테스트해주는 코드를 추가로 작성한 것

테스트 코드를 이용하여 작성한 모든 코드를 한번에 테스트할 수 있으므로 직접 프로그램을 실행하여 테스트하는 것보다 효율적임

- junit
	- 자바 프로그래밍 언어용 단위 테스트 프레임워크이다

	- 단위 테스트는 각각 모듈 단위(로그인, 회원가입, 검색 등)로 테스트하는 것을 의미, 즉 메소드에 대한 테스트
	
	- Spring에서는 Junit을 사용하여 Spring Container에 있는 Bean을 테스트   
	  (@Test 어노테이션 메서드 호출 시 새로운 인스턴스를 생성하여 독립적인 테스트가 이루어짐)
	
	- 단정 메소드(assert)로 테스트 케이스의 수행결과를 판별

	- @Test : 테스트를 수행하는 메소드라는 의미이며 독립적인 테스트를 위해 @Test 메소드마다 객체 별도 생성

	- @Before : @Test 메소드가 실행되기 전 반드시 실행되는 메소드, 주로 Set-up 코드에 활용
	
	- @After : @Test 메소드가 실행된 후 반드시 실행되는 메소드, 주로 clear 코드에 활용

	- @Ignore : 이 어노테이션이 선언된 메소드는 무시(Skip)

```
class MemberServiceTest {
	MemberService memberService;
	MemoryMemberRepository memberRepository;

	@BeforeEach //각 @Test 메소드가 실행되기 전 반드시 실행
	public void beforeEach() {
		memberRepository = new MemoryMemberRepository();
		memberService = new MemberService(memberRepository);
	}

	@AfterEach //각 @Test 메소드가 실행된 후 반드시 실행
	public void afterEach() {
		memberRepository.clearStore();
	}

	@Test //테스트 메소드
	public void 회원가입() throws Exception {
		//Given
		Member member = new Member();
		member.setName("hello");
		//When
		Long saveId = memberService.join(member);
		//Then
		Member findMember = memberRepository.findById(saveId).get();
		assertEquals(member.getName(), findMember.getName()); //assertEquals(예상값,실제값) 일치하면 Pass
	}

	@Test //테스트 메소드
	public void 중복_회원_예외() throws Exception {
		//Given
		Member member1 = new Member();
		member1.setName("spring");
		Member member2 = new Member();
		member2.setName("spring");
		//When
		memberService.join(member1);
		IllegalStateException e = assertThrows(IllegalStateException.class,() -> memberService.join(member2));//예외가 발생해야 한다.
		assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다."); //assertThat(조건Boolean) 참이면 Pass
	}
}
```


- Spring 통합 테스트(Spring Container와 DB까지 연결한 테스트)

	- @SpringBootTest : Spring Container와 테스트를 함께 실행
	
	- @Transactional : 테스트 시작 전 트랜잭션을 시작하고 테스트 완료 후 Rollback하여 DB에 데이터가 남지 않으므로 다음 테스트 영향 X
 
```
@SpringBootTest
@Transactional
class MemberServiceIntegrationTest {
	
	@Autowired MemberService memberService;
	@Autowired MemberRepository memberRepository;

	@Test //테스트 메소드
	public void 회원가입() throws Exception {
		//Given
		Member member = new Member();
		member.setName("hello");
		//When
		Long saveId = memberService.join(member);
		//Then
		Member findMember = memberRepository.findById(saveId).get();
		assertEquals(member.getName(), findMember.getName()); //assertEquals(예상값,실제값) 일치하면 Pass
	}

	@Test //테스트 메소드
	public void 중복_회원_예외() throws Exception {
		//Given
		Member member1 = new Member();
		member1.setName("spring");
		Member member2 = new Member();
		member2.setName("spring");
		//When
		memberService.join(member1);
		IllegalStateException e = assertThrows(IllegalStateException.class,() -> memberService.join(member2));//예외가 발생해야 한다.
		assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다."); //assertThat(조건Boolean) 참이면 Pass
	}
}
```

---
### 8. Spring Bean(component Scan VS 코드로 직접 등록)

Spring Bean은 Spring 컨테이너가 관리하는 객체

#### <component Scan 방식으로 Spring Bean 등록>

```
@Repository
public class MemoryMemberRepository implements MemberRepository {}
```

- 정형화된 Controller, Service, Repository 코드일 경우에 사용

- 클래스 앞에 @Component 애노테이션(@Controller, @Service, @Repository 포함)이 있으면 Spring Bean으로   
  자동 등록 

- 마치 아무것도 없는 공장(Spring Container)에 일을 시작하기 위해 마킹한 설비(Controller 등)들을 공장안으로   
  운반하는 것

#### <코드로 직접 Spring Bean 등록>
```
@Configuration
public class SpringConfig {
	
	private final DataSource dataSource;

	public SpringConfig(DataSource dataSource) {
		this.dataSource = dataSource;
	}
	
	@Bean
	public MemberService memberService() {
		return new MemberService(memberRepository());
	}

	@Bean
	public MemberRepository memberRepository() {
		//return new MemoryMemberRepository();
		return new JdbcMemberRepository(dataSource);
	}
}
```

<img src="https://user-images.githubusercontent.com/101415950/193201763-6ca2a3d7-48b7-4b50-844c-371f2f44c85b.png" width="80%" height="80%">

- Controller는 정형화된 모듈이므로 굳이 직접 Spring Bean에 등록하지 않고 component Scan 방식으로 진행한다.   
  (사용자의 요청에 적절한 서비스를 호출하여, 그 결과가 사용자에게 반환하는 코드를 구현하는 역할을 하기 때문)

- 해당 예제를 진행할 때, Service 클래스와 Repository 클래스의 @Service, @Repository 제거한다.

- 정형화 되지 않거나, 상황에 따라 구현 클래스를 변경해야 하는 경우에 사용(9장 DB 접근 기술 예제 참고)   
  (ex. 데이터 베이스 변경 : 순수 JDBC -> JPA)

- 스프링의 DI를 이용하면 기존 코드를 전혀 손대지 않고, 설정만으로 구현 클래스를 변경할 수 있음   
  -> 개방-폐쇄 원칙(OCP, Open-Closed Principle) : 확장에는 열려있고, 수정과 변경에는 닫혀있음

---
### 9. DB 접근 기술(JDBC, JPA)

#### 순수 JDBC(Java Database Connectivity)

- 순수 JDBC 개념 

	- JDBC는 자바에서 데이터베이스에 접속할 수 있도록 하는 자바 API로 데이터 조회 및 수정 기능 등 제공

	- connection 객체 : DB와 어플리케이션의 연결을 관리
	
	- prepareStatement 객체 : SQL문을 미리 생성하여 변수를 따로 입력하는 방식(SQL Injection 공격 방어)
	
	- ResultSet 객체 : SQL문 실행 결과값을 받는 객체
	
	- DB에서 정보를 가져올 때마다 연결 및 해제해야하는 번거로움과 서버 과부하, 속도 저하 문제 있음
	
	- connection, prepareStatement, ResultSet 등 반복되는 코드가 많아 유지보수하기 까다로움

```
public class JdbcMemberRepository implements MemberRepository {

	private final DataSource dataSource;
	
	public JdbcMemberRepository(DataSource dataSource) {
		this.dataSource = dataSource;
	}

	@Override
	public Member save(Member member) {
		String sql = "insert into member(name) values(?)";
		Connection conn = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		try {
			conn = getConnection();
			pstmt = conn.prepareStatement(sql,
			Statement.RETURN_GENERATED_KEYS);
			pstmt.setString(1, member.getName());
			pstmt.executeUpdate();
			rs = pstmt.getGeneratedKeys();
			if (rs.next()) {
				member.setId(rs.getLong(1));
			} else {
				throw new SQLException("id 조회 실패");
			}
			return member;
		} catch (Exception e) {
			throw new IllegalStateException(e);
		} finally {
			close(conn, pstmt, rs);
		}
	}

	@Override
	public Optional<Member> findById(Long id) {
		String sql = "select * from member where id = ?";
		Connection conn = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		try {
			conn = getConnection();
			pstmt = conn.prepareStatement(sql);
			pstmt.setLong(1, id);
			rs = pstmt.executeQuery();
			if(rs.next()) {
				Member member = new Member();
				member.setId(rs.getLong("id"));
				member.setName(rs.getString("name"));
				return Optional.of(member);
			} else {
				return Optional.empty();
			}
		} catch (Exception e) {
			throw new IllegalStateException(e);
		} finally {
			close(conn, pstmt, rs);
		}
	}

	private Connection getConnection() {
		return DataSourceUtils.getConnection(dataSource);
	}

	private void close(Connection conn, PreparedStatement pstmt, ResultSet rs){
		try {
			if (rs != null) {
				rs.close();
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}
		
		try {
			if (pstmt != null) {
				pstmt.close();
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}

		try {
			if (conn != null) {
				close(conn);
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}

	private void close(Connection conn) throws SQLException {
		DataSourceUtils.releaseConnection(conn, dataSource);
	}
}
```

#### 스프링 JDBC Template

- 스프링 JDBC Template 개념 
 
	- 순수 JDBC와 동일한 환경설정

	- 스프링 JdbcTemplate과 MyBatis 같은 라이브러리는 JDBC API에서 반복 코드를 대부분 제거해주지만,
	  하지만 SQL은 직접 작성 필요

```
public class JdbcTemplateMemberRepository implements MemberRepository {

	private final JdbcTemplate jdbcTemplate;
	
	public JdbcTemplateMemberRepository(DataSource dataSource) {
		jdbcTemplate = new JdbcTemplate(dataSource);
	}

	@Override
	public Member save(Member member) {
		SimpleJdbcInsert jdbcInsert = new SimpleJdbcInsert(jdbcTemplate);
		jdbcInsert.withTableName("member").usingGeneratedKeyColumns("id");
		Map<String, Object> parameters = new HashMap<>();
		parameters.put("name", member.getName());

		Number key = jdbcInsert.executeAndReturnKey(new MapSqlParameterSource(parameters));
		member.setId(key.longValue());
		return member;
	}

	@Override
	public Optional<Member> findById(Long id) {
		List<Member> result = jdbcTemplate.query("select * from member where id = ?", memberRowMapper(), id);
		return result.stream().findAny();
	}

	private RowMapper<Member> memberRowMapper() {
		return (rs, rowNum) -> {
			Member member = new Member();
			member.setId(rs.getLong("id"));
			member.setName(rs.getString("name"));
			return member;
		};
	}
}
```

#### <JPA(Java Persistence API)>

- JPA 개념 

	- JPA는 java 생태계에서 ORM(Object-Relational Mapping)의 기술 표준을 사용하는 인터페이스 모음

	- 즉 인터페이스이므로 하이버네이트(Hibernate)와 같은 구현체가 필요

	- 기존의 반복 코드를 없애고 기본적인 SQL은 JPA가 직접 만들어서 실행

	- SQL과 데이터 중심 설계에서 객체 중심의 설계로 패러다임 전환 가능 -> 개발 생산성 상승

<br/>

- 스프링 부트에 JPA 설정 추가(resources/application.properties)
```
spring.datasource.url=jdbc:h2:tcp://localhost/~/test
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa

spring.jpa.show-sql=true //JPA가 생성하는 SQL 출력
spring.jpa.hibernate.ddl-auto=none //테이블 자동 생성 기능 Off(create 사용 시 엔티티 정보를 바탕으로 테이블 직접 생성)
```
<br/>

-JPA 엔티티 매핑

```
@Entity
public class Member {

	@Id @GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;
	private String name;

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
}
```

- JPA를 JDBC 대신 적용한 예시
```
public class JpaMemberRepository implements MemberRepository {
	private final EntityManager em;

	public JpaMemberRepository(EntityManager em) {
		this.em = em;
	}

	public Member save(Member member) {
		em.persist(member);
		return member;
	}

	public Optional<Member> findById(Long id) {
		Member member = em.find(Member.class, id);
		return Optional.ofNullable(member);
	}

	public List<Member> findAll() {
		return em.createQuery("select m from Member m", Member.class).getResultList();
	}

	public Optional<Member> findByName(String name) {
		List<Member> result = em.createQuery("select m from Member m where m.name = :name", Member.class)
			.setParameter("name", name)
			.getResultList();
		return result.stream().findAny();
	}
}
```

<br/>

- 서비스 계층에 트랜잭션 추가
	- 해당 클래스의 메서드를 실행할 때 트랜잭션을 시작
	
	- 메서드가 정상 종료되면 트랜잭션을 커밋
	
	- 만약 런타임 예외가 발생하면 롤백
	
	- JPA를 통한 모든 데이터 변경은 트랜잭션 안에서 실행해야 함

```
import org.springframework.transaction.annotation.Transactional

@Transactional
public class MemberService {}
``` 

#### 스프링 데이터 JPA

- 스프링 데이터 JPA 개념

	- Repository에 구현 클래스 없이 인터페이스 만으로 개발 가능(반복 개발해온 기본 CRID 기능은 자동으로 구현체 생성)
	
	- 핵심 비즈니스 로직을 개발하는 데 집중할 수 있음

	- findByName(), findByEmail() 처럼 메서드 이름 만으로 조회 기능 제공(findBy이름())
	
	- 페이징 기능 자동 제공
	
	- 실무에서 복잡한 동적 쿼리는 Querydsl 라이브러리 사용
	
	- 대단히 어려운 쿼리는 JPA가 제공하는 네이티브 쿼리나 스프링 JdbcTemplate를 사용

<img src="https://user-images.githubusercontent.com/101415950/193208346-7d58b3ed-b5aa-4e41-a746-bdb442cf8d93.png" width="50%" height="50%">

- 스프링 데이터 JPA를 JPA 대신 적용한 예시
```
public interface SpringDataJpaMemberRepository extends JpaRepository<Member, Long>, MemberRepository {
	Optional<Member> findByName(String name);
}
```

---
### 10. AOP(관점 지향 프로그래밍, Aspect Oriented Programming)

AOP는 공통 관심 사항과 핵심 관심 사항을 분리하여 모듈화하는 방식을 의미

각 메서드에 공통적으로 실행할 기능을 모듈화하여 원하는 곳에 적용할 때 사용   
(ex. 기능 실행 시 소요되는 시간을 측정하는 기능을 몇 천개의 메서드에 적용할 때)

<img src="https://user-images.githubusercontent.com/101415950/193165846-0dbd5c28-5998-4f62-b92e-7c7bb6d2476f.png" width="80%" height="80%">

<br/>

- 시간 측정 로직을 별도로 작성
```
@Component
@Aspect
public class TimeTraceAop {

	@Around("execution(* hello.hellospring..*(..))") //hello.hellospring 디렉터리 내 있는 모든 메서드에 적용
	public Object execute(ProceedingJoinPoint joinPoint) throws Throwable {
		long start = System.currentTimeMillis();
		System.out.println("START: " + joinPoint.toString()); //현재 실행 있는 메서드 출력
		try {
			return joinPoint.proceed(); //메서드 실행
		} finally {
			long finish = System.currentTimeMillis();
			long timeMs = finish - start;
			System.out.println("END: " + joinPoint.toString()+ " " + timeMs + "ms"); //현재 종료되고 있는 메서드 출력
		}
	}
}
```

1. 핵심 관심 사항(비즈니스 로직)을 깔끔하게 유지

2. 가독성 및 유지보수 측면에서 유리

3. 원하는 대상에 적용할 수 있음

<br/>

- AOP가 실행되는 과정
<img src="https://user-images.githubusercontent.com/101415950/193166540-b8e2efad-84b5-49d8-b6a5-95d5fffca486.png" width="80%" height="80%">

1. 프록시 패턴을 사용한 프록시 AOP 방식

2. AOP의 적용대상이 되는 객체에 대한 프록시 생성

3. 비즈니스 로직에 접근할 때 객체에 바로 접근하지 않고 프록시를 통해 간접적으로 접근

4. 프록시는 공통 기능을 실행한 뒤 대상 객체의 실제 메서드를 호출하거나 실제 메서드를 호출한 후 공통 기능 실행

<br/>

- 프록시 패턴(대리인 패턴, 유튜브 얄팍한 코딩 사전 참고)

	- 클래스 중에서 시간이 많이 걸리거나 메모리를 많이 차지하여 객체로 여럿 생성하기 부담스러운 경우가 있음

	- 그 해당 클래스의 프록시(대리자) 클래스를 따로 만들어 가벼운 작업은 프록시가 처리하고 무거운 작업은 실제 클래스가 처리
	
	- 필요할 때만 실제 객체를 생성하므로 보다 효율적이고 유연한 프로그래밍 가능
	
	- ex) 유튜브에서 무거운 작업 : 프리뷰 실행, 가벼운 작업 : 제목 표시

	- 썸네일 객체에서 제목과 프리뷰를 두 메소드를 통해 각각 보여주되 처음에는 제목만 보여주는 프록시로 생성
	
	- 커서를 썸네일로 올리면 무거운 작업인 프리뷰를 실제 클래스가 담당하여 동작
	
![image](https://user-images.githubusercontent.com/101415950/193172857-8ba06812-1a8f-4509-96c1-01fbb8cc9f2a.png)

## 마크다운 언어 참조
https://gist.github.com/ihoneymon/652be052a0727ad59601
