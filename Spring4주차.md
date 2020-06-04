4주차 스프링 스터디에서는 스프링 예제 프로젝트 실습해보기가 목표였다.

아직 모르는 개념이 너무나도 많지만 제일 많이 하는 **게시판** 예제를 무작정 따라해봤다. :sweat_smile:



[**참고 블로그**](https://m.blog.naver.com/0221jyh/220325179391)



![image-20200605005025351](https://user-images.githubusercontent.com/53978090/83804838-befc0680-a6e9-11ea-9239-79777cab8583.png)

먼저 Spring 프로젝트를 생성해준다.

그리고 pom.xml에 의존성들을 추가해준다.



**pom.xml**

~~~
<project xmlns="http://maven.apache.org/POM/4.0.0" 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>ch13-myBatisBoard</groupId>
  <artifactId>ch13-myBatisBoard</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>war</packaging>
  <build>
    <sourceDirectory>src</sourceDirectory>
    <resources>
      <resource>
        <directory>src</directory>
        <excludes>
          <exclude>**/*.java</exclude>
        </excludes>
      </resource>
    </resources>
    <plugins>
      <plugin>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.1</version>
        <configuration>
          <source>1.7</source>
          <target>1.7</target>
        </configuration>
      </plugin>
      <plugin>
        <artifactId>maven-war-plugin</artifactId>
        <version>2.3</version>
        <configuration>
          <warSourceDirectory>WebContent</warSourceDirectory>
          <failOnMissingWebXml>false</failOnMissingWebXml>
        </configuration>
      </plugin>
    </plugins>
  </build>
  <dependencies>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>3.2.3.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-expression</artifactId>
          <version>3.2.3.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-beans</artifactId>
          <version>3.2.3.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>org.aspectj</groupId>
          <artifactId>aspectjweaver</artifactId>
          <version>1.8.2</version>
      </dependency>
      <dependency>
          <groupId>commons-fileupload</groupId>
          <artifactId>commons-fileupload</artifactId>
          <version>1.3.1</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>3.2.3.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-web</artifactId>
          <version>3.2.3.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>javax.validation</groupId>
          <artifactId>validation-api</artifactId>
          <version>1.1.0.Final</version>
      </dependency>
      <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>jstl</artifactId>
          <version>1.2</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-jdbc</artifactId>
          <version>3.2.3.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>commons-dbcp</groupId>
          <artifactId>commons-dbcp</artifactId>
          <version>20030825.184428</version>
      </dependency>
      <dependency>
          <groupId>commons-pool</groupId>
          <artifactId>commons-pool</artifactId>
          <version>20030825.183949</version>
      </dependency>
      <dependency>
          <groupId>commons-collections</groupId>
          <artifactId>commons-collections</artifactId>
          <version>20040616</version>
      </dependency>
      <dependency>
          <groupId>com.atomikos</groupId>
          <artifactId>transactions</artifactId>
          <version>3.9.3</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-orm</artifactId>
          <version>3.2.3.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis-spring</artifactId>
          <version>1.1.1</version>
      </dependency>
      <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis</artifactId>
          <version>3.2.2</version>
      </dependency>
      <dependency>
          <groupId>log4j</groupId>
          <artifactId>log4j</artifactId>
          <version>1.2.17</version>          
      </dependency>
  </dependencies>
</project>
~~~



그 다음 폴더를 구성해준다

![image-20200605005746763](https://user-images.githubusercontent.com/53978090/83804844-c0c5ca00-a6e9-11ea-8a22-c2e2da168efd.png)


각 폴더에 대한 설명!

![image-20200605005808276](https://user-images.githubusercontent.com/53978090/83804848-c1f6f700-a6e9-11ea-8456-49c42f0f8a2b.png)

그 다음 게시판 페이지를 보여줄 홈페이지의 css를 작성한다.

![image-20200605005855424](https://user-images.githubusercontent.com/53978090/83804862-c6231480-a6e9-11ea-860d-64be0a5e36d8.png)
**board.css**

~~~
@charset "UTF-8";table.search{
    width:250px;
    margin:0 auto;
}
table{
    width:500px;
    margin:0 auto;
}
th, td{
    padding:5px;
}
.border-style{
    border:1px solid #000;
    border-collapse:collapse;
}
.border-style th, .border-style td{
    border:1px solid #000;
}
.align-right{
    text-align:right;
}
.align-center{
    text-align:center;
}
textarea {
    color:#000000;
    font-family:"굴림"; 
    font-size:9pt; line-height:120%;
    background-color: #ffffff; 
    border: 1 solid #999999
}
th { 
    color:#3f3f3f; 
    font-family:"굴림"; 
    font-size:9pt; 
    line-height:170%;
}
td { 
    color:#3f3f3f; 
    font-family:"굴림"; 
    font-size:9pt; 
    line-height:170%;
}
td a { 
    color:#333377; 
    font-family:"굴림"; 
    font-size:9pt; 
    line-height:170% ; 
    text-decoration: none;
}
td a:hover { 
    color:#3366cc; 
    font-family:"굴림"; 
    font-size:9pt; 
    line-height:170% ; 
    text-decoration: underline;
}
input {
    color:#000000; 
    font-family:"굴림"; 
    font-size:9pt; 
    line-height:120%;
    background-color: #ffffff; 
    border: 1 solid #999999
}
.inputb {
    border-bottom: #999999 1px solid; 
    border-left: #cecece 1px solid;
    border-right: #999999 1px solid; 
    border-top: #cecece 1px solid; 
    color: #000000; 
    font-size: 9pt;
    background-color: #ededed;
}
~~~

![image-20200605005931242](https://user-images.githubusercontent.com/53978090/83804864-c7544180-a6e9-11ea-9945-a3a9a8e157ad.png)

그다음 orcle 을 이용하여 저장소의 테이블을 생성

~~~
create table springboardtest(
seq number primary key,
writer varchar2(50) not null,
title varchar2(100) not null,
content clob not null,
pwd varchar2(10) not null,
hit number(5) not null,
regdate date not null,
filename varchar2(100)
);
 
create sequence boardtest_seq;
~~~

![image-20200605010236857](https://user-images.githubusercontent.com/53978090/83804880-ce7b4f80-a6e9-11ea-9a61-3791b8a3e401.png)


그 다음 데이터베이스와 1:1로 매칭되는 빈의 역할을 수행할 클래스를 작성

**com.blogboard.domain/BoardCommand.java**

~~~
package com.blogboard.domain;

import java.sql.Date;

import org.springframework.web.multipart.MultipartFile;

public class BoardCommand {
	private int seq;
    private String writer;
    private String title;
    private String content;
    private String pwd;
    private int hit;
    private Date regdate;
    private MultipartFile upload;
    private String filename;
	public int getSeq() {
		return seq;
	}
	public void setSeq(int seq) {
		this.seq = seq;
	}
	public String getWriter() {
		return writer;
	}
	public void setWriter(String writer) {
		this.writer = writer;
	}
	public String getTitle() {
		return title;
	}
	public void setTitle(String title) {
		this.title = title;
	}
	public String getContent() {
		return content;
	}
	public void setContent(String content) {
		this.content = content;
	}
	public String getPwd() {
		return pwd;
	}
	public void setPwd(String pwd) {
		this.pwd = pwd;
	}
	public int getHit() {
		return hit;
	}
	public void setHit(int hit) {
		this.hit = hit;
	}
	public Date getRegdate() {
		return regdate;
	}
	public void setRegdate(Date regdate) {
		this.regdate = regdate;
	}
	public MultipartFile getUpload() {
		return upload;
	}
	public void setUpload(MultipartFile upload) {
		this.upload = upload;
	}
	public String getFilename() {
		return filename;
	}
	public void setFilename(String filename) {
		this.filename = filename;
	}
}
~~~

그 다음 저번에 공부했었던! DispatcherServlet을 만들어줍니다.

**web.xml**

~~~
 <servlet>
      <servlet-name>dispatcher</servlet-name>
      <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  </servlet>
 
  <servlet-mapping>
      <servlet-name>dispatcher</servlet-name>
      <url-pattern>*.do</url-pattern>
  </servlet-mapping>
~~~

이걸 추가해서 *.do로 들어오는 클라이언트의 요청을 DispatcherServlet이 처리하도록 설정 !

**dispatcher-servlet.xml**

~~~
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context-3.0.xsd">    
 
    <!-- 컴포넌트 어노테이션 스캔 -->
   <context:component-scan base-package="com.blogboard"/>
   
    <!-- messageSource 지정 -->
    <bean id="messageSource"
        class="org.springframework.context.support.ResourceBundleMessageSource">
        <property name="basenames">
            <list>
                <value>messages.label</value>
                <value>messages.validation</value>
            </list>
        </property>
    </bean>
    
    <!-- Exception 설정 -->
    <bean
        class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
        <property name="exceptionMappings">
            <props>
                <prop key="java.lang.Exception">pageError</prop>
            </props>
        </property>
    </bean>
    
    <!-- viewResolver -->
    <bean 
        class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/boardView/" />
        <property name="suffix" value=".jsp" />
        <property name="order" value="1"/>
    </bean>
    
    <!-- 파일 다운로드 -->
    <bean class="org.springframework.web.servlet.view.BeanNameViewResolver"
        p:order="0"/>
    
    <!-- 파일 업로드 -->
    <bean id="multipartResolver"
        class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="maxUploadSize" value="52428800"/>
        <property name="defaultEncoding" value="UTF-8"/>
    </bean>
</beans>
~~~



