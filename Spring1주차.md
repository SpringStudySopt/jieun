## Spring Study 1 주차 :books:



### 공부 방법

- [인프런 예제로 배우는 스프링 입문(개정판)](https://www.inflearn.com/course/spring_revised_edition/dashboard)
- 스프링 퀵 스타트  :green_book:
- [패스트캠퍼스 Java 웹개발 마스터](https://www.fastcampus.co.kr/dev_online_jvweb)

### 백엔드 개발자

#### 백엔드 개발자란?

: 클라이언트측에서 요청을 하면 데이터베이스를 조회, 가공하여 해당 요청에 응답하는 일을 수행

**Socket 통신**
- 접속을 계속 유지하여 데이터를 전달
- 서버의 자원에 따라 연결될 수 있는 클라이언트의 숫자가 한정됨
- 실시간 정보 교환에 사용하며 http보다 속도가 빠름

**HTTP 통신**
- 클라이언트 요청이 있을때만 데이터 응답을 전달함
- 불필요한 자원의 점유를 없애 다른 접속을 원활하게 하여 많은 데이터를 처리
- 데이터 요청 후 응답이 오면 연결은 끊어짐

**REST API**

**C** : Create - POST

**R** : Read - GET

**U** : Update - PUT(전체) / PATCH(일부)

**D** : Delete - DELETE

**JSON** 형식으로 데이터를 주고받음 (javascript에서 객체를 주고받는 형식)

### Spring 프레임워크

#### 스프링 프레임워크란?
: **Ioc**와 **AOP**를 지원하는 **경량**의 **컨테이너** 프레임워크

**경량**
: 1) 여러개의 모듈로 구성되어 있고 각 모듈은 하나 이상의 JAR파일로 구성 -> 크기 측면에서 가볍고 배포 역시 쉽다.

  2) POJO(Plain OLD Java Object)형태의 객체를 관리함 -> 클래스를 구현하는 데 특별한 규칙이 없는 단순하고 가벼운 객체 
  
   * 오래된 방식의 순수한 자바 객체
   
**AOP**
: Aspect Oriented Programming , 공통으로 사용하는 기능들을 외부의 독립된 클래스로 분리하고, 해당 기능을 프로그램 코드에 직접 명시하지 않고 선언적으로 처리하여 적용하는 것 -> 공통 기능을 분리하여 관리하므로 응집도가 높은 비즈니스 컴포넌트 생성가능, 유지보수 향상

**컨테이너**
: 특정 객체의 생성과 관리를 담당하며 객체 운용에 필요한 다양한 기능을 제공함

#### 스프링 프로젝트 맛보기

예제로 배우는 스프링 입문 ~ 스프링 Ioc 컨테이너

먼저 인텔리제이에 https://github.com/spring-projects/spring-petclinic를 클론하여 프로젝트를 생성

클론해온 프로젝트를 localhost:8080 해서 들어가보면 스프링부트를 기반으로한 동물병원 웹에플리케이션이 나온다.

![image-20200515033806175](https://user-images.githubusercontent.com/53978090/81972904-45c74180-965e-11ea-9b78-3c50b00674fb.png)

![image-20200515033831239](https://user-images.githubusercontent.com/53978090/81972905-46f86e80-965e-11ea-8ebe-549e45be9bbd.png)



이걸로 스프링이 어떻게 동작하는지 확인

Controller는 Repository 객체를 주입받아 동작 , resources의 html파일들이 보여지고 servlet 맵핑을 함 

 

#### 스프링 Ioc

: Inversion of Control 의존성의 제어권이 뒤바뀜, 객체 생성을 자바 코드로 직접 처리하는것이 아니라 컨테이너가 대신 처리(의존관계포함)


**기존의 의존성**

~~~
class OwnerController {
	private OwnerRepository repository = new OwnerRepository();
}
~~~

일반적으로 자기가 사용할 의존성 -> new 로 만들어서 사용함



**Ioc사용**

~~~
class OwnerController {
	private OwnerRepository repo;
	
	public OwnerController(OwnerRepository repo)
	this.repo = repo;
}

class OwnerControllerTest{
	@Test
	public void create(){
		OwnerRepository repo = new OwnerRepository();
		OwnerController controller = new OwnerController(repo);
	}
}
~~~

이런식으로 Controller 클래스에서는 객체를 사용하긴 하지만  누군가 밖에서 줄 수 있게끔 생성자를 통해 의존성을 받아옴 (객체 넣어주는걸 spring이 알아서 해준다!)



#### IoC 컨테이너

: 빈(bean)을 만들고 엮어주며 제공해준다.

스프링 Ioc 컨테이너 안에는 bean들이 존재, 의존성을 관리해줘야함(필요한 의존성 서로 주입)

빈설정 -> 이름 또는 id, 타입, 스코프 @Bean으로 빈 객체 등록

~~~
@AutoWired
ApplicationContext applicationContext;

@Test
public void getBean(){
	applicationContext.getBeanDefinitionNames()
	//IoC컨테이너 안에 등록된 bean들이 어떤게 있는지 확인
	OwnerController bean = applicationContext.getBean(OwnerController.class)
	assertThat(bean).isNotNull();
	//이런식으로 OwnerController클래스에 들어가는 bean도 따로 확인
	
}
~~~







