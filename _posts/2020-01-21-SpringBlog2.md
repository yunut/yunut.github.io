---
layout: post
title:  "Spring Blog 만들기 2.DB 설정 및 테스트"
date:   2020-01-18
excerpt: ""
project: true
tag:
- spring
- project
comments: true
---

# 기본 설정
사용할 DB는 Mysql로 홈페이지에서 설치합니다.
Workbench까지 설치하면 더 편하게 구성할수 있습니다.

## 할일
블로그에 글을 저장하고, 글 목록을 나타낼수 있는 DB를 구축하고 환경설정 합니다. 



## 스프링 환경 설정
![Config1](/photo/springBlog/2.DBConfig/1.Config.PNG)  
 
기본적으로 구성되어있는 스프링의 환경설정 파일의 폴더구조를 변경합니다.
스프링 웹 시스템은 web.xml을 참조하여 스프링 설정과 관련된 파일의 위치와 내용을 참조합니다.  
이 web.xml파일을 보면 root-context.xml과 servlet-context.xml 파일 두개의 설정파일과 관련된 코드가 있습니다.  
이 환경설정파일을 하나의 폴더에 관리해두는것이 편하고, 작업이 용이하기에 `src/main/resources/` 디렉토리 내에 관리하기위해  `src/main/resources/` 아래 `spring` 폴더를 만들어 안에 root-context.xml을 넣습니다. servlet-context.xml 은 `src/main/resources/` 디렉토리에 관리합니다.
 
![Config2](/photo/springBlog/2.DBConfig/2.WebXMLConfig.PNG)  
환경설정 파일의 위치를 옮겼기때문에 스프링 설정과 관련된 파일의 위치를 찾아주는 web.xml의 경로를 바꿔주어야 합니다. 위 그림처럼 classpath경로를 바꾸어줍니다. 여기서 classpath는 src/main/resources 경로를 의미합니다.  


## Mysql 데이터베이스 설정  
블로그에 데이터를 저장할 데이터베이스를 생성하고 사용자를 추가합니다. root계정으로 데이터베이스를 생성하고 사용자를 추가합니다.
~~~
mysql> CREATE DATABASE 'springblog';
~~~  

이제 사용자를 추가하고 모든 권한을 줍니다.
~~~
mysql> CREATE USER 'yunut'@'%' IDENTIFIED BY '1q2w3e4r'
mysql> GRANT ALL PRIVILEGES ON springblog.* TO 'yunut'@'%' WITH GRANT OPTION;
~~~
※ mysql 8.0 버전에서 명령어가 많이 바뀌였습니다. 예전에는 권한을 주면서 묵시적으로 암호도 설정하는 `GRANT ALL PRIVILEGES ON *.* TO *@* IDENTIFIED BY '*'` 의 명령어가 가능했었는데, 버전이 올라가면서 오류메시지가 나타납니다.  




## Dependency 추가
pom.xml 파일에 아래의 소스코드를 추가합니다.
~~~
<!-- mysql -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.13</version>
</dependency>

<!-- mybatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.4.6</version>
</dependency>

<!-- mybatis-spring -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>1.3.2</version>
</dependency>

<!-- spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>${org.springframework-version}</version>
</dependency>

<!-- spring-test -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>${org.springframework-version}</version>
    <scope>test</scope>
</dependency>
~~~  

junit의 버전을 4.12로 수정합니다.  
~~~
<!-- Test -->
<dependency>
	<groupId>junit</groupId>
	<artifactId>junit</artifactId>
	<version>4.12</version>
	<scope>test</scope>
</dependency> 
~~~

자바 및 스프링의 버전도 수정합니다.
~~~
<properties>
	<java-version>1.8</java-version>
	<org.springframework-version>5.1.4.RELEASE</org.springframework-version>
	<org.aspectj-version>1.9.2</org.aspectj-version>
	<org.slf4j-version>1.7.25</org.slf4j-version>
</properties>
~~~  

버전 변경 후 변경된 내용을 적용하기위해 Maven- update project를 클릭해 적용해줍니다.

## 데이터베이스 설정 파일 추가
~~~
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:mybatis-spring="http://mybatis.org/schema/mybatis-spring"
	
		xsi:schemaLocation="
	http://mybatis.org/schema/mybatis-spring 
	http://mybatis.org/schema/mybatis-spring.xsd
	http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	
		
	
	<!--dataSource 객체 설정 -->
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
	<property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
	<property name="url" value="jdbc:mysql://127.0.0.1:3306/springblog?useSSL=false&amp;serverTimezone=Asia/Seoul" />       
	        <property name="username" value="yunut"></property>
	        <property name="password" value="1q2w3e4r"></property>
	</bean>  
	
	
	
	<!-- SqlSessionFactory 객체 설정 -->
	<bean id="SqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
	<property name="dataSource" ref="dataSource" />       
	<property name="mapperLocations" value="classpath*:mappers/**/*Mapper.xml" />
	</bean>

	
	<!-- SqlSession Template 설정 -->
	<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate" destroy-method="clearCache">
	<constructor-arg name="sqlSessionFactory" ref="SqlSessionFactory" />
	</bean>

</beans>
~~~
dataSource 빈은 데이터베이스의 접속 환경입니다.  
SqlSessionFactory 및 sqlSession은 mybatis 관련 환경설정입니다.  
mybatis는 객체지향언어인 자바의 관계형 데이터베이스 프로그래밍을 더 쉽게 도와주는 개발 프레임 워크입니다. 개발자가 작성한 SQL 명령어와 자바 객체를 매핑해 주는 기능을 제공합니다.  SqlSessionFactory 안에 있는 mapperLocations는 데이터베이스에서 실행할 SQL 쿼리문이 있는 위치입니다. 여기서는 게시판을 위한 `src/main/resources` 내에 mappers 폴더를 만들어 boardMappers.xml파일내에 관리하겠습니다.  
// ClassPath 설정 mappers폴더를 찾는데 오류가있어 classpath*로 설정하였습니다.  

![Config3](/photo/springBlog/2.DBConfig/3.mappers.PNG)

boardMappers.xml 파일 내의 소스는 아래와 같습니다.
~~~
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE mapper
    PUBLIC "-//mybatis.org/DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.freehoon.web.board.boardMapper">
</mapper>
~~~

##데이터베이스 접속 테스트
이제 기본적인 데이터베이스 환경설정, 스프링 설정이 끝났으므로 접속 테스트를 합니다.

![Config4](/photo/springBlog/2.DBConfig/4.test.PNG)  
/src/test/java 아래의 패키지 안에 MysqlConnectionTest.java 파일을 생성합니다.
~~~
package com.jys.web;

import java.sql.Connection;

import javax.inject.Inject;
import javax.sql.DataSource;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = { "classpath:spring/dataSource-context.xml" })
public class MysqlConnectionTest {
	
	private static final Logger logger = LoggerFactory.getLogger(MysqlConnectionTest.class);

	@Inject
	private DataSource ds;

	

	@Test
	public void testConnection() {
		try (Connection con = ds.getConnection()){
			logger.info("\n MySQL 연결 : " + con);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
~~~

Run 메뉴에서 Run As -> Junit Test를 선택헤 테스트를 진행합니다.

![Config5](/photo/springBlog/2.DBConfig/5.test2.PNG)  
정상적으로 연결된 모습입니다.  

※ 지정한 패스워드가 너무 짧을경우 에러메시지가 나올수 있습니다. 변경할 시 `ALTER user '사용자'@'%' IDENTIFIED WITH mysql_native_password BY '변경할비밀번호';` 명령을 입력하여 변경할수 있습니다.

## Mybatis 설정
아까와 동일한 위치에 MybatisTest.java 파일생성
~~~
package com.jys.web;

import javax.annotation.Resource;
import javax.inject.Inject;

import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;



@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = { "classpath:spring/dataSource-context.xml" })
public class MybatisTest {

	private static final Logger logger = LoggerFactory.getLogger(MybatisTest.class);

	@Inject
	private SqlSessionFactory sessionFactory;

	@Test
	public void testSessionFactory() {
		logger.info("\n Session Factory : " + sessionFactory);
	}

	@Test
	public void testSqlSession() {
		try (SqlSession session = sessionFactory.openSession()){
			logger.info("\n Sql Session : " + session);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
~~~

![Config5](/photo/springBlog/2.DBConfig/6.mybatis.PNG)  
정상적으로 잘 접속되는 모습입니다.