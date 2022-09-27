# backend_study_starting_with_spring_modulabs
---
# 목차
- 1. 스프링 설치법(For IntelliJ)
- 2. 

---
# 내용
### 1. 스프링 설치법(For IntelliJ)
스프링 부트 스타터 사이트에서 스프링 프로젝트 생성(https://start.spring.io)

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

![image](https://user-images.githubusercontent.com/101415950/192464593-05687cb8-63fc-4f75-b479-444342a26928.png)

https://gist.github.com/ihoneymon/652be052a0727ad59601
