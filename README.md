# backend_study_starting_with_spring_modulabs
---
# 목차
- [1. 스프링 설치법(For IntelliJ)](#1-스프링-설치법-for-intellij-)
- [2. Maven vs Gradle](#2-maven-vs-gradle)
- 3. 정적 콘텐츠 vs MVC vs API
- 4. Controller, Service , Repository
- 5. DI, IoC
- 6. Test
- 7. Spring Bean(component Scan VS 코드를 이용한 직접 등록)
- 8. DB 접근 기술(JDBC, JPA)
- 9. AOP
---
# 내용

※ 인프런에서 김영한 강사님의 강의를 기반으로 작성된 글입니다.

### 1 스프링 설치법(For IntelliJ)
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
### 2 Maven vs Gradle

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
### 3. 정적 콘텐츠 vs MVC vs API

---
### 4. Controller, Service , Repository

---
### 5. DI, IoC

---
### 6. Test

테스트 코드의 의미는 작성된 코드를 자동으로 테스트해주는 코드를 추가로 작성한 것

테스트 코드를 이용하여 작성한 모든 코드를 한번에 테스트할 수 있으므로 직접 프로그램을 실행하여 테스트하는 것보다 효율적임

---
### 7. Spring Bean(component Scan VS 코드를 이용한 직접 등록)

---
### 8. DB 접근 기술(JDBC, JPA)

---
### 9. AOP

https://gist.github.com/ihoneymon/652be052a0727ad59601
