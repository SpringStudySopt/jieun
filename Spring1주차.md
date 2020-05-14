## Spring Study 1 주차 :books:



### 공부 방법

- [인프런 예제로 배우는 스프링 입문(개정판)](https://www.inflearn.com/course/spring_revised_edition/dashboard)
- 스프링 퀵 스타트  :green_book:



### 1주차 내용 

#### 스프링 프로젝트 맛보기

예제로 배우는 스프링 입문 ~ 스프링 Ioc 컨테이너

먼저 인텔리제이에 https://github.com/spring-projects/spring-petclinic에 들어가 클론한다.

클론해온 프로젝트를 localhost:8080 해서 들어가보면 스프링부트를 기반으로한 동물병원 웹에플리케이션이 나온다.

![image-20200515033806175](https://user-images.githubusercontent.com/53978090/81972904-45c74180-965e-11ea-9b78-3c50b00674fb.png)

![image-20200515033831239](https://user-images.githubusercontent.com/53978090/81972905-46f86e80-965e-11ea-8ebe-549e45be9bbd.png)



이걸로 스프링이 어떻게 동작하는지 확인

Controller는 Repository 객체를 주입받아 동작 , resources의 html파일들이 보여지고 servlet 맵핑을 함 

 

#### 스프링 Ioc

: Inversion of Control 의존성의 제어권이 뒤바뀜



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







