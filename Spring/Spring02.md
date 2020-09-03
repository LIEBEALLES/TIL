# Spring(2)MVC

- 프로젝트 : **spring07_HelloMVC**(주로참고) , SpringMVC01_hello, Spring MVC02

## 🎅MVC 

#### : Model, View, Controller의 약자로 , 어플리케이션을 세 가지 역할로 구분한 개발방법론 

> #### Model :  어플리케이션이 무엇을 할 것인지 정의, 내부 비즈니스 로직을 처리
>
> #### Controller : Model 이 어떻게 처리할 지 알려주는 역할
>
> #### View : 처리한 것을 화면에 보여주는 역할          

### 🎄특징

- Spring Framework의 다른 모듈과의 연계용이
- Controller, command 객체, 모델 객체, Validator 등 각각의 역할에 대한 명확한 분리
- Form 객체 없이 사용자 지정 가능한 데이터 바인딩과 유효성 체크지원
- 어떠한 View 기술과도 연계가 용이
- Tag lib통한 Message 출력, Theme 적용 등과 입력 폼을 보다 쉽게 구현 

### 🎄장점

- 비즈니스 로직과 인터페이스를 분리시켜 서로 영향없이 개발하기 수월한 장점이 있다. 

- 스프링이 제공하는 트랜잭션 처리가 DI , AOP 적용 등을 쉽게 사용할 수 있도록 돕는다.

- structures 와 같은 프레임워크와 스프링 프레임워크를 연동하기 위해 추가적인 설정을 하지 않아도 된다.
- Spring MVC 프레임워크는 스프링 기반으로 사용할 수 있다.

## ⛄wichtig⛄

###  ⭐⭐Spring MVC 주요구성요소⭐⭐

#### DispatcherServlet 

> ##### 클라이언트 요청을 전달받는다. 컨트롤러에게 클라이언트의 요청을 전달하고 ,  
>
> ##### 컨트롤러가 리턴한 결과값을 View에 전달하여 알맞은 응답을 생성한다.

#### HandlerMapping

> ##### 클라이언트의 요청 URL 을 어떤 컨트롤러가 처리할 지를 결정한다.
>
> ##### RequestURL과 Controller 클래스 매핑을 관리한다.

#### Controller

> ##### 클라이언트의 요청을 처리한 뒤, 그 결과를 DIspatcherServlet에 알려준다.
>
> ##### 비즈니스 로직을 호출하여 처리결과 ModelAndView 인스턴스를 반환한다.

#### ModelAndView

> ##### 컨트롤러가 처리한 결과 정보 및 뷰 선택에 필요한 정보를 담는다.

#### ViewResolver 

> ##### 컨트롤러의 처리결과를 생성할 뷰를 결정한다.



### ⭐⭐MVC흐름도⭐⭐

![image-20200903213559821](C:\Users\dywjd\AppData\Roaming\Typora\typora-user-images\image-20200903213559821.png)

### 🎄TODO 순서

1.  dependency 추가 →pom.xml
2.  listenter → web.xml
3.  DispatcherServlet (hello-servlet) →web.xml
4.  handler mapping("/hello.do")를 통해 controller의 메서드를 찾아온다. → HelloController.java
5.  biz(@Service) 호출 → HelloController.java
6.  Dao(@Repository) 호출 →HelloBiz.java
7.  Dao에서 return → HelloDao.java
8.  Biz에서 return → HelloBiz.java
9.  return받은 값을 model 객체에 담아서 전달 (ModelAndView) → HelloController.java
10.  view

> **TODO  <<  소문자 안 먹힘 /  TODO^:^01 << 숫자 / 공백 제대로 써주어야 순서가 잘 나온다.** 
>
> **주의할 것 !**
>
> **tasks  보는 법 : window ->preferences -> TODO -> Task Tags -> Enable searching for task tags 체크**

### 1.dependency 추가 -> pom.xml

> pom.xml에 spring-web, spring-webmvc dependency 를 추가
>
> search.maven.org
>
> **dependency 사이트 : mvnrepository.com**

```html
	<dependencies>
	<!-- TODO : 01. dependency 추가 -->
		<!-- org.springframework.spring-web/ spring- webmvc -->
		<!-- https://mvnrepository.com/artifact/org.springframework/spring-web -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>5.2.8.RELEASE</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>5.2.8.RELEASE</version>
		</dependency>
	</dependencies>
```



### 2.ContextLoader listener ->web.xml

> **어떤 xml을 쓸건지 호출한다. -> contextparam , servlet  둘 중에 누굴 호출할 건지 서치함**
>
> `ContextLoaderListener`는 ServletContext의 라이프사이클에 맞추어 applicationContext를 ServletContext에 추가,삭제
>
> spring에서 DispatcherServlet은 그 자체가 servlet이기 때문에 1개 이상의 DispatcherServlet을 설정하는 것이 가능하다.
>
> `ContextLoaderListener` 가 상속받을 class로 실질적으로 applicationContext를 생성하고 , ServletContext에 setting 하는 역할 

#### 작동원리

##### 1.링크클릭 

```html
<!--1-->
<a href="hello.do">hello</a>
<a href="bye.do?name=mvc">bye</a>

```

##### 2.web.xml 에 있는 리스너가 지금 들어온 요청이 어떤 xml 을 실행시켜줘야하는지 본다 .

(context-param , servlet 둘 중 하나 누구 호출할건지? / 둘 중 하나만 올라와 있을 수 있다)

```html
<!-- TODO : 02. listener  -->
<listener> <!-- 어떤 xml을 호출해주어야하는지 -->
	<listener-class>
		org.springframework.web.context.ContextLoaderListener
	</listener-class>
</listener>
```

```html
<context-param> <!--context전체에서 사용할 수 있는 param / 해당 웹 어플리케이션 전체 -->
	<param-name>contextConfigLocation</param-name>
	<param-value>/WEB-INF/applicationContext.xml</param-value>
</context-param>

<!--TODO : 03. DispatcherServlet (hello-servlet)  -->
<servlet>
	<servlet-name>hello</servlet-name>
	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
</servlet>

<!--/*.do 아님!! -->
<servlet-mapping>
	<servlet-name>hello</servlet-name>
	<url-pattern>*.do</url-pattern> 
</servlet-mapping>
```

##### 3.controller 값 받기

```java
@RequestMapping("/bye.do")
	public ModelAndView getBye(@RequestParam("name") String nickname) {
		//
		ModelAndView mav = new ModelAndView();
		mav.setViewName("/WEB-INF/views/bye.jsp");
		mav.addObject("message", "bye, "+ nickname);
		
		return mav;
	}
```

##### 4.서버)HandlerMapping 을 통해서 DIspatcherServlet(컨트롤러 호출해주는 애)

##### 5. biz(@Service) - > dao(@Repository)



### 3.DispatcherServlet(hello-servlet) ->web.xml

> 클라이언트의 요청을 받아서 컨트롤러에게 클라이언트 요청을 전달하고 , 컨트롤러가 리턴한 결과값을 view에 전달하여 알맞은 응답을 생성한다.
>
> 모든 요청을 한 곳에서 받아서 필요한 처리를 하고 , 요청에 맞는 handler로 요청을 dispatch하고, 해당 handler의 실행결과를 http Response형태로 만드는 역할을 한다.
>
> - 장점 
>
> 1. 기존에는 모든 서블릿에 대해 url매핑 활용을 하기 위해서 web.xml에 모두 등록해주어야했지만, dispatcher-servlet이 해당 어플리케이션으로 들어오는 모든 요청을 핸들링해주면서 작업이 상당히 편리해졌다. 
> 2. 이 서블릿을 이용하면 `@mvc`역시 사용할 수 있게되어 좋다.

### 

### 4.handler mapping -> HelloController.java

> ("/hello.do")를 통해 controller의 메서드를 찾아온다. 
>
> 클라이언트의 요청을 바탕으로 어떤 Handler(Controller메소드)를 실행할 지 결정한다.

```java
@Controller
public class HelloController {
	//TODO : 05 biz(@servlet) 호출
	@Autowired
	private HelloBiz biz;
	
	//TODO : 04. handler mapping("/hello.do")통해 controller의 메서드 찾아온다.
	@RequestMapping("/hello.do")
	public String getHello(Model model) {
		
		//TODO : 09.return받은 값을 model 객체에 담아서 전달(ModelAndView)
		model.addAttribute("message", biz.getHello());
		
		return "/WEB-INF/views/hello.jsp"; //뷰네임
	}
	
	//여기에 들어올 값이 여기에 들어올거야
	//쿼리 스트링의 밸류값이 여기로 들어갈 것 . 둘다 이름이 다르면 강제적으로 써주어야한다 .
	@RequestMapping("/bye.do")
	public ModelAndView getBye(@RequestParam("name") String nickname) {
		//
		ModelAndView mav = new ModelAndView();
		mav.setViewName("/WEB-INF/views/bye.jsp");
		mav.addObject("message", "bye, "+ nickname);
        
		
		return mav;
	}
	
}
```



### 5.biz(@Service) 호출 → HelloController.java

> `@Service`
>
> : 비즈니스 로직이 들어간 클래스를 의미한다.

#### 

### 6.Dao(@Repository) 호출 →HelloBiz.java

> `@Repository`
>
> : 일반적으로 Dao에 사용되며 DB exception을 DataAccessException으로 변환한다.

```java
@Service
public class HelloBiz {
//TODO : 06. Dao(@Repository) 호출
	@Autowired
	private HelloDao dao;
	
	public String getHello() {
		
		//TODO : 08. Biz에서 return
		return "Hello, " + dao.getHello();
	}
}

```



### 7.Dao에서 return → HelloDao.java

```java
@Repository
public class HelloDao {
	
	//TODO : 07. Dao에서 return
	public String getHello() {
		return "Spring";
	}

}

```



### 8.Biz에서 return → HelloBiz.java

```java
@Service
public class HelloBiz {
//TODO : 06. Dao(@Repository) 호출
	@Autowired
	private HelloDao dao;
	
	public String getHello() {
		
		//TODO : 08. Biz에서 return
		return "Hello, " + dao.getHello();
	}
}
```



### 9.return받은 값을 model 객체에 담아서 전달 (ModelAndView) 

### -> HelloController.java

```java
@Controller
public class HelloController {
	//TODO : 05 biz(@servlet) 호출
	@Autowired
	private HelloBiz biz;
	
	//TODO : 04. handler mapping("/hello.do")통해 controller의 메서드 찾아온다.
	@RequestMapping("/hello.do")
	public String getHello(Model model) {
		
		//TODO : 09.return받은 값을 model 객체에 담아서 전달(ModelAndView)
		model.addAttribute("message", biz.getHello());
		
		return "/WEB-INF/views/hello.jsp"; //뷰네임
	}
```



### 10.view ->HelloController.java / jsp

```java
model.addAttribute("message", biz.getHello());
//message 를 보내주고 jsp에서 받는다 .
```

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

<!--TODO : 10. view  -->
<h1>${message }</h1>

</body>
</html>
```



### 🎄Spring Model 객체(보충필요)

- Controller의 메서드는 Model이라는 타입의 객체를 파라미터로 받을 수 있다.

  (직접 model을 생성할 필요 x , 파라미터로 선언만 해주면 된다.)

- 순수하게 JSP Servlet으로 웹 어플리케이션을 만들 때 보통 request나 session 내장객체에 정보를 담아 jsp에 넘겨주곤 했는데 Spring에서는 Model 객체를 쓴다.

  -> 즉 , `request.setAttribute()` 와 비슷한 역할을 한다.

- `model.addAttribute("key", "value")` : 데이터 입력
- `return "경로"` : view지정

```java
	@RequestMapping("/hello.do")
	public String getHello(Model model) {
		
		//TODO : 09.return받은 값을 model 객체에 담아서 전달(ModelAndView)
		model.addAttribute("message", biz.getHello());
		
		return "/WEB-INF/views/hello.jsp"; //뷰네임
	}
```



### 🎄Spring ModelAndView(보충필요)

- 컴포넌트 방식으로 ModelAndView객체를 생성해서 객체형태로 리턴한다.
- Controller 처리 결과 후 응답할 view와 view에 전달할 값을 저장한다.
- `addObject("key", value)` : 데이터입력
- `setViewName("view")` : 화면지정 , view 세팅

```java
	@RequestMapping("/bye.do")
	public ModelAndView getBye(@RequestParam("name") String nickname) {
		//
		ModelAndView mav = new ModelAndView();
		mav.setViewName("/WEB-INF/views/bye.jsp");
		mav.addObject("message", "bye, "+ nickname);
		
		return mav;
	}
```



### 🎄Model vs ModelAndView(보충필요)

- Model을 사용한 Controller는 Model객체에 put을 해주고 리턴해줄 때 문자열로 view이름을 리턴
- ModelAndView를 사용한 Controller는 ModelAndView의 생성자에 view이름과 Model객체를 넣어준 뒤 리턴



### 🎄Annotation

#### @RequestMapping

- url class 또는 method와 mapping 시켜주는 역할

#### @RequestParam (주로 겟방식)

  - key=value 형태로 넘어오는 queryStirng(Parameter)을 mapping된 method의 파라미터와 연결 

#### @ModelAttribute (주로 포스트방식)

- form tag를 통해 넘어온 model을 mapping된 method의 파라미터와 연결

#### @sessionAttribute

- session객체에 model 정보를 유지하고 싶을 경우 사용

----

### 🎄인코딩 3가지 방법

1. jsp
2. controller
3. :star:**xml**

```xml
<filter>
		<filter-name>encodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter
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

```















