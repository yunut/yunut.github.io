---
layout: post
title:  "Spring Blog 만들기 1.기본 설정 및 프로젝트 생성"
date:   2020-01-18
excerpt: ""
project: true
tag:
- spring
- project
comments: true
---

# 1. 기본 설정 및 프로젝트 생성
기본적으로 Apache Tomcat, JDK, STS, Eclipse 가 설치된 상태에서 시작합니다.

## 프로젝트 생성

![CreateProject1](/photo/springBlog/1.CreateProject/1.CreateProject.PNG)  
![CreateProject2](/photo/springBlog/1.CreateProject/2.CreateProject.PNG)  
![CreateProject3](/photo/springBlog/1.CreateProject/3.CreateProject.PNG)  

이클립스에서 New Project -> Spring Legacy Project 선택 -> Spring MVC Project 선택 후 프로젝트 이름을 입력 -> 패키지 이름을 지정해 줍니다.  
프로젝트 패키지는 되도록 '네이밍 규칙' 을 참고하여 지정해 줍니다.  
ex) [com,org].[회사명].[프로젝트명].[최상위모듈].[하위모듈]  



## 서버 생성

![CreateServer1](/photo/springBlog/1.CreateProject/4.CreateServer.PNG)  
![CreateServer2](/photo/springBlog/1.CreateProject/5.CreateServer.PNG)  
![CreateServer3](/photo/springBlog/1.CreateProject/6.CreateServer.PNG)  

Servers 탭에있는 No server are available. Click this link to create a new server... 문구를 클릭하여 서버를 생성합니다.
본인에게 맞는 Tomcat 버전을 선택하여 서버를 생성하면 됩니다. 다음 서버가 구동할 웹 프로젝트를 선택하면 됩니다.  


## 서버 설정
![ConfigServer1](/photo/springBlog/1.CreateProject/6.ConfigServer.PNG)  
Servers 탭의 생성된 서버를 더블클릭해 Server Config화면으로 들어갑니다. 다음 관리자포트, 웹포트, AJP 프로토콜 포트설정을 해줍니다.  

![ErrorServer1](/photo/springBlog/1.CreateProject/7.ErrorServer.PNG)  
포트설정을 8080으로했더니 에러메시지가 출력됩니다. 이 에러는 Apache Tomcat 설치 시 웹 포트를 8080으로 설정해 충돌이일어나서 발생하는 에러입니다.  

![ErrorServer2](/photo/springBlog/1.CreateProject/8.ErrorServer.PNG)  
HTTP포트를 8081로 바꿔주어 충돌을 해결합니다.  

![StartServer1](/photo/springBlog/1.CreateProject/9.StartServer.PNG)   
이제 서버를 구동하면 에러없이 정상적으로 구동되는것을 확인할수 있습니다. 


## 텍스트 인코딩
![WebConfig](/photo/springBlog/1.CreateProject/11.WebConfig.PNG)  
서버는 잘 구동되었지만, 글자 깨짐현상이 나타나는것을 확인할수 있습니다.  이는 web.xml 파일에 아래의코드를 적용하여 수정할수 있습니다.  
~~~
<!-- Character Set Filter -->

<filter>

		<filter-name>encodingFilter</filter-name>

		<filter-class>

			org.springframework.web.filter.CharacterEncodingFilter

		</filter-class>

		<init-param>

			<param-name>encoding</param-name>

			<param-value>UTF-8</param-value>

		</init-param>

		<init-param>

			<param-name>forceEncoding</param-name>

			<param-value>true</param-value>

		</init-param>

	</filter>

	<filter-mapping>

		<filter-name>encodingFilter</filter-name>

		<url-pattern>/*</url-pattern>


	</filter-mapping>
~~~
  
![WebStart](/photo/springBlog/1.CreateProject/12.WebStart.PNG)  

