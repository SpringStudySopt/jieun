## Spring Study 2 주차 :books:


#### 결합도가 높은 프로그램

**결합도란?**

: 하나의 클래스가 다른 클래스와 얼마나 많이 연결되어 있는지 나타내는 표현

결합도를 낮추는것이 좋다 , 결합도를 낮추면 유지보수가 쉬워지기 때문 !

##### 다형성 이용하기

![image-20200521153625284](https://user-images.githubusercontent.com/53978090/82542076-59a60280-9b8c-11ea-9e6f-4911727ac37d.png)

**TV.java**

~~~
package com.springbook.biz;

public interface TV {
	public void powerOn();
	public void powerOff();
	public void volumeUp();
	public void volumeDown();
}
~~~



**SamsungTV.java**

~~~
package com.springbook.biz;

public class SamsungTV implements TV{	
	public void powerOn() {
		System.out.println("SamsungTV---전원 켠다.");
	}
	public void powerOff() {
		System.out.println("SamsungTV---전원 끈다.");
	}
	public void volumeUp() {
		System.out.println("SamsungTV---소리 올린다.");
	}
	public void volumeDown() {
		System.out.println("SamsungTV---소리 내린다.");
	}	
}
~~~



**LgTV.java**

~~~
package com.springbook.biz;

public class LgTV implements TV {
	public void powerOn() {
		System.out.println("LgTV---전원 켠다.");
	}
	public void powerOff() {
		System.out.println("LgTV---전원 끈다.");
	}
	public void volumeUp() {
		System.out.println("LgTV---소리 올린다.");
	}
	public void volumeDown() {
		System.out.println("LgTV---소리 내린다.");
	}
}

~~~



**TVUser.java**

~~~
package com.springbook.biz;

public class TVUser {

	public static void main(String[] args) {
		TV tv = new LgTV();		
		//TV tv = new SamsungTV();
		tv.powerOn();
		tv.volumeUp();
		tv.volumeDown();
		tv.powerOff();
	}
}

~~~

![image-20200521151319075](https://user-images.githubusercontent.com/53978090/82542090-5ca0f300-9b8c-11ea-80bd-6450465902f5.png)




##### 디자인 패턴 이용하기

 **BeanFactory.java**

Factory패턴 : 클라이언트에서 사용할 객체 생성을 캡슐화하여 TVUser와 TV 사이를 느슨한 결합 상태로 만들어준다.

~~~
package com.springbook.biz;

public class BeanFactory {
	public Object getBean(String beanName) {
		if(beanName.contentEquals("samsung")) {
			return new SamsungTV();
		}else if(beanName.contentEquals("lg")) {
			return new LgTV();
		}
		return null;
	}
}
~~~



**TVUser.java**

~~~
package com.springbook.biz;

public class TVUser {

	public static void main(String[] args) {
		BeanFactory factory = new BeanFactory();
		TV tv = (TV)factory.getBean(args[0]);
		tv.powerOn();
		tv.volumeUp();
		tv.volumeDown();
		tv.powerOff();
		factory.close();
	}

}

~~~

RunAs -> Run Configurations 로 lg / samsung 넘겨주기 

![image-20200521153835729](https://user-images.githubusercontent.com/53978090/82542095-5f034d00-9b8c-11ea-8d71-96432933541a.png)

#### 스프링 컨테이너 및 설정 파일

1. 스프링 설정 파일 생성

Spring Legacy Project 선택 후 properties에서 Java 버전을 1.8로 바꿔준다.

![image-20200521145602444](https://user-images.githubusercontent.com/53978090/82542105-632f6a80-9b8c-11ea-8a5d-aecb5d811e9d.png)


만들어둔 Spring Legacy Project에서 src/main/resources 소스 폴더를 선택한 후 New -> Other 해서 'Spring Bean Configuration File'을 선택한다. applicationContext를 입력해서 스프링 설정파일 생성 !



**applicationContext.xml**

~~~~
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id = "tv" class = "com.springbook.biz.SamsungTV"/>
	//객체 등록
</beans>
~~~~



2. 스프링 컨테이너 구동 및 테스트



**TVUser.java**

~~~
package com.springbook.biz;

import org.springframework.context.support.AbstractApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;

public class TVUser {

	public static void main(String[] args) {		
	//스프링 컨테이너 구동
		AbstractApplicationContext factory = new GenericXmlApplicationContext("applicationContext.xml");
		//Spring 컨테이너로부터 필요한 객체를 요청(LookUp)
		TV tv = (TV)factory.getBean("tv");
		tv.powerOn();
		tv.volumeUp();
		tv.volumeDown();
		tv.powerOff();
	
    	//Spring 컨테이너 종료
		factory.close();
	}
}

~~~



![image-20200521154442124](https://user-images.githubusercontent.com/53978090/82542110-66c2f180-9b8c-11ea-8247-4fc646cb891b.png)

**작동 순서**

![image-20200521154926277](https://user-images.githubusercontent.com/53978090/82542121-69bde200-9b8c-11ea-9478-393e3801da07.png)


① TVUser 클라이언트가 스프링 설정 파일을 로딩하여 컨테이너 구동

② 스프링 설정 파일에 <bean> 등록된 Samsung TV 객체생성

③ getBean() 메소드로 이름이 'tv'인 객체 요청(LookUp)

④ Samsung TV 객체 반환



이로 인해 SamsungTV 를 LgTV 로 변경할때, applicationContext.xml 파일만 수정하면 된다 !

기존 BeanFactory 클래스 사용보다 유지보수가 더 편해진다 ㅎㅎ



#### 스프링 컨테이너의 종류

* BeanFactory

  : 스프링 설정 파일에 등록된 <bean> 객체를 생성하고 관리하는 가장 기본적인 컨테이너 기능만 제공

  컨테이너가 구동될 때 <bean> 객체 생성하는게 아니라, 클라이언트의 요청에 의해서만 생성되는 지연로딩(Lazy Loding) 방식을 사용



* ApplicationContext

  : BeanFactory가 제공하는 <bean> 객체 관리 기능 외에도 트랜잭션 관리나 메세지 기반의 다국어 처리 등 다양한 기능을 지원함

  컨테이너가 구동되는 시점에 <bean> 등록된 클래스들을 객체 생성하는 즉시 로딩(pre-loading) 방식으로 동작

  웹 애플리케이션 지원



#### 스프링 XML 설정

1.  <beans> 루트 엘리먼트 

2.  <import> 엘리먼트

    : 기능별로 나눈 XML 파일을 하나로 통합할때 사용

3.  <bean> 엘리먼트

    : id, class 속성으로 객체를 생성

   *<bean> 엘리먼트의 속성

   1) init-method

    : 객체를 생성한 후 멤버 변수 초기화 작업 처리

   2) destroy-method

    : 스프링 컨테이너가 객체를 삭제하기 직전에 호출

   3) lazy-init 

    : 아까 위에 ApplicationContext 방식을 사용하면 즉시 로딩 방식으로 동작한다고 했다.  그런데 어떤 <bean>은 자주 사용되지 않으면서 메모리를 많이 차지하여 시스템에 부담을 주는 경우가 있다. 그래서 컨테이너가 구동되는 시점이 아닌 해당 <bean>이 사용되는 시점에 객체를 생성하도록 lazt-init = "true" 값을 줄 수 있다. 

   4) scope 

    : 하나의 객체만 생성해도 되는 경우 (싱글톤) -> scope = "singleton"으로 사용 가능



#### Spring MVC 구조

![image-20200521171511906](https://user-images.githubusercontent.com/53978090/82542135-6f1b2c80-9b8c-11ea-8ea2-04adda1bd1d4.png)


①  클라이언트로부터의 모든 ".do" 요청을 DispatcherServlet이 받는다.

② DispatcherServlet은 HandlerMapping 을 통해 요청을 처리한 Controller를 검색한다.

③  DispatcherServlet은 검색된 Controller를 실행하여 클라이언트의 요청을 처리한다.

④  Controller는 비즈니스 로직의 수행 결과로 얻어낸 Model 정보와 Model을 보여줄 View 정보를 ModelAndView객체에 저장하여 리턴한다.

⑤ DispatcherServlet은 ModelAndView로부터 View 정보를 추출하고, ViewResolver를 이용하여 응답으로 사용할 View를 얻어낸다.

⑥ DispatcherServlet은 ViewResolver를 통해 찾아낸 View를 실행하여 응답을 전송한다.



#### Spring 프로젝트 구조

![image-20200521174900912](https://user-images.githubusercontent.com/53978090/82542147-73474a00-9b8c-11ea-92db-422394d537cd.png)



① : java 폴더의 하위 디렉토리 -> java 파일만 모아서 관리함 컨트롤러, 모델

② : resource 폴더의 하위 디렉토리, java 코드에서 사용하기 위한 리소스(mapper,ws,sql 등) 파일을 모아서 관리하는곳

③ : 테스트코드 및 테스트 코드에서 사용할 리소스

Maven Dependencies : 라이브러리 관리 도구 (maven에서 다운받은 jar파일)

④ : webapp 폴더의 하위 디렉토리, Web에서 사용하는 자원들을 모아서 관리하는곳 jsp, js,css같은 자원이나 xml 설정파일등이 있다.

*WEB-INF 폴더 : 외부에서 직접 접속이 차단되어있음 -> 컴파일된 클래스와 스프링 환경설정파일(DB연결)이 존재하기 때문에!

#### 출처

[스프링 폴더 구조](https://jayviii.tistory.com/15)

[스프링 폴더 구조1]()(https://doublesprogramming.tistory.com/16)

[스프링 컨테이너 및 설정 파일](https://backback.tistory.com/74?category=695393)
