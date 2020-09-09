# Spring(1)

## 🐬Spring framework

> #### EJB(Enterprise JavaBean)기반 개발에서 POJO(Plain Old Java Object)기반 개발로 ! 
>
> #### pojo기반의 경량컨테이너 : 간단히 이용할 수 있기때문에 이렇게 불림!
>
> #### 자바 엔터프라이즈 개발을 위한 오픈소스 애플리케이션 프레임워크
>
> #### 자바언어를 기반으로 ,다양한 어플리케이션을 제작하기 위한 약속된 프로그래밍
>
> - ##### open source : s/w , h/w 제작자의 권리를 지키면서 원시코드를 누구나 열람할 수 있도록 한 소프트웨어, 오픈소스라이선스에 준하는 모든 통칭을 일컫는다.
>
> - ##### framework : 소프트웨어의 구체적인 부분에 해당하는 설계와 구현을 재사용이 가능하게끔 일련의 협업화된 클래스들을 제공하는 것
>
> - ##### framework: 설계에 기반이 되는 부분을 기술한 확장가능한 프레임워크 기반코드와 사용자가 이 코드를 자기 입맛대로 확장하는데 필요한 라이브러리 이 두가지 요소가 통합되어 제공되는 형태를 말한다
>
>   - 라이브러리 : 특정목적ㄴ을 위해 사용하는 함수들을 모듈화 시킨 것(*.jar)
>
> - ##### framework : 프로그램의 골격이 되는 코드 / 소프트웨어 개발을 간소화 하기 위해 개발
>
> - ##### framework : 특정한 목적에 맞게 프로그래밍을 쉽게 하기 위한 약속 
>
> #### 동적인 웹 사이트를 개발하기 위한 여러가지 서비스를 제공한다.

1. 스프링다운

2. java project -> convert to Maven Project

3. pom.xml 에 jar파일을 관리하는 dependencies 를 추가해준다(core , context) 

   ## 🐬maven(pom.xml) : 빌드 / 배포관리도구

   ### : 프로젝트를 빌드하고 라이브러리를 관리해주는 도구

   #### 왜 maven을 사용하는가?

   라이브러리 수가 몇 개 안된다면, 그냥 외부jar를 추가해서 사용하면된다. 

   하지만 프로젝트의 규모가 커질수록 라이브러리의 관리가 어려워진다 

   -> **pom.xml(Project Object Model)만 공유하는게 훨씬 효율적이기때문에** 

   ->또 다른 장점! 필요한 라이브러리의 하위 라이브러리까지 버전에 맞게 관리해주는 것 

   ex) dependency에 작성한 라이브러리를 사용하기 위해 다른 라이브러리도 필요하다면 알아서 설치를 해준다. 

   > spring-context 모듈은 spring-beans가 필요하고 spring-beans는 
   >
   > spring-core모듈이 필요하다.  
   >
   > 기본적인 모듈을 코어가 가지고있다 
   >
   > context를 추가하면 core, expression, beans, aop

   - maven 프로젝트의 groupId  : 나의 프로젝트 사이에서 고유하게 식별하게 해주는 것 

   - maven 프로젝트의 artifactId : 버전 정보를 생략한 jar파일 

     이름 아무거나 정해도 상관 x , 단 **소문자** / **특문**사용금지

   - maven 프로젝트의 version :  숫자와 점으로 이루어진 일반적인 버전형태



## 🐬Spring특징

- #### 🐢경량컨테이너(크기와 부하의 측면)로서 자바 객체를 직접 관리

  - 각각의 객체 생성, 소멸과 같은 라이프 사이클을 관리하며 스프링으로부터 필요한 객체를 얻어올 수 있다.

- #### 🐢관점지향 프로그래밍(AOP, Aspect Oriented Programming) 

  - 객체지향 프로그래밍의 뒤를 이은 또 하나의 프로그래밍 언어구조
  - 횡단의분리('Separation Of Cross Cutting Concern')

  - 트랜잭션, 로깅 등 여러 모듈 등 비즈니스 로직과 분리될 수 있도록 도와줌 

  - 핵심관심사항과 공통관심사항을 기준으로 프로그래밍함으로써 공통모듈을 여러코드에 쉽게 적용할 수 있도록 지원하는 기술

  - 공통기능들을 모듈화하고 해당 기능을 프로그램 코드에서 직접 명시하지 않고 선언적으로 처리하여 필요한 컴포넌트에 계층적으로 다양한 기능들을 적용

  - 관점 기준으로 나눌거야 . 그래서 관점이 뭐지?

    - **cc(Core Concern)  주 관심사(핵심)** : 해당메소드의 유니크한 부분

    - **ccc(Cross Cutting Concern)** **공통관심사** : 공통적으로 사용하는 기능을 분리

      > JoinPoint(결합점) : 메소드호출하는 시점 / 인스턴스의 생성시점
      >
      > PointCut(교차점) : ccc가 연결될 곳 / 어떤 결합점에 적용되어야 하는지 정의 / 스프링 설정파일 안에 xml로 작성
      >
      > Advice(충고) : 분리해놓은 ccc코드 / 교차점에서 지정한 결합점에서 실행되어야 할 코드(aspect의 실제구현체)
      >
      > Advisor(Aspect) = advice + pointcut  != cc + ccc  / aop의 중심단위 / 구현하고자 하는 횡단 관심사의 기능, 애플리케이션의 모듈화 하고자 하는 부분
      >
      > **일련의 동작들 :  Weaving = cc + ccc**

 

- #### 🐢의존성 주입(DI, Dependency Injection) 
  - 객체 간의 의존관계를 관리 -> 어떤 객체가 필요로 하는 객체를 외부에 있는 다른 곳에서 필요로 하는 객체를 받는다.(값을 주입)
  - 객체 간의 결합을 느슨하게 하는 스프링의 핵심기술
    1. 생성자를 이용한 의존성주입 <constructor-arg/>
    2. setter메소드를 이용한 의존성주입 <property/>
    3. 초기화 인터페이스를 이용한 의존성주입(field 주입)
  - 강결합 : 객체 간의 결합도가 강한 프로그램 
  - 약결합 : 인터페이스 , 스프링을 사용하여 객체 간의 결합도를 낮춘 프로그램 

```java
//Mtest main메소드

public static void main(String[] args) {
		ApplicationContext factory = new ClassPathXmlApplicationContext("com/test03/applicationContext.xml");
			
		Address choi = factory.getBean("choi", Address.class);
		System.out.println(choi);
		
		Address sung = factory.getBean("sung" , Address.class);
		System.out.println(sung);
			
	}

```

```java
//applicationContext.xml 
<bean id="choi" class="com.test03.Address">
<!-- Address choi = new Address(); -->
	<property name="name" value="최유정"></property>
	<property name="addr" value="봉천역"></property>
	<property name="phone" value="010"></property>
	
</bean>

<!--setter 주입 값이 모자라도 널로 뜬다! -->
<bean id="sung" class="com.test03.Address">
	<property name="name" value="성현모"></property>
	<property name="addr" value="수유역"></property>
</bean>
    
    **값이 모자라도 널로 뜸 !**
```

- #### 🐢제어의역전 , 역전제어 (IoC, Inversion of Controller)를 지원

  - 객체를 사용하는 쪽과 생성하는 쪽이 바뀌었다.(프로그램의 제어 흐름 구조가 뒤 바뀌는 것 ) 스프링컨테이너로 ~ 빈을 통해서 객체생성
  - 오브젝트 자신이 제어를 못하고 다른 대상에서 제어 권한을 부여한다. 즉 , 제어권한을 가진 컨테이너에서 상황에 따라 필요할 때 호출한다.
  - 객체의 생성부터 생명주기 관리까지 객체의 제어권이 프레임워크에 있음
  - 생성하는 곳과 실제로 사용하는 곳이 달라졌다 왜요? 결합도를 낮추기 위해
  - 어떤 객체가 사용할 객체(의존관계인 객체)를 직접 선언하여 사용하는 것이 아니라 어떤 방법을 사용하여(ex.생성자)주입받아 사용하는것

- #### 🐢OCP(Open-Closed Principle)

  - 개방-폐쇄원칙 - 개발자가 건들면 안되는 곳 CLOSED / 확장해도 된다 OPEN
  - 인터페이스를 통해 제공되는 확장 포인트는 확장을 위해 개방되어있고, 인터페이스를 이용하는 클래스는 자신의 변화가 불필요하게 일어나지 않도록 굳게 폐쇄되어있다.

---

#### 200814

http://www.springframework.org/schema/beans/

: beans 에서 사용할 수 있는 모든 태그들이 담겨져있음

- `collectionElements`

- `array`

- `constructor-arg`

- `lazy-init`

  등 ... 추가복습필요(+)

---

## 🐬싱글톤패턴

> **메모리에 단 하나의 객체만 존재하게 하는 것** 
>
> ex) 캘린더



## 🐬spring annotation

> Spring Annotation 
> -어노테이션 자바 1.5부터 지원
> -스프링은 어노테이션을 이용하여 빈과 관련된 정보를 설정할 수 있다.

Spring Framework에서 annotation을 사용하려면 다음과 같은 설정들을 필요로한다.
1. CommonAnnotationBeanPostProcessor 클래스를 설정파일에 bean 객체로 등록하여 annotation 적용
 <bean class="org.springframework.beans.factory.annotation.CommonAnnotationBeanPostProcessor"/>
2. `<context:annotation-config />` 를 이용
	@Autowired, @Required, @Resource, @PostConstructor, @PreDestroy 등의 annotation 을 자동처리해주는 bean post processor
	
	//3,4를 많이 쓴다.
	
3. ##### **`<context:component-scan base-package="" />`를 이용**

  @Component, @Controller , @Service, @Repository 등의 annotation을 자동처리 

4. ##### **`<mvc:annotatin-driven />` 를 이용**

  @RequestMapping , @Valid 등 Spring mvc component 등을 자동처리 
  HandlerMapping , HandlerAdapter 를 등록하여 @Controller에 요청을 연결 

  * 해당 설정이 없어도 component-scan 태그가 있으면 mvc application 동작 



Spring MVC Framework 는 annotation의 사용으로 설정파일을 간결화하고, view 페이지와 객체 또는 메서드의 맵핑을 명확하게 할 수 있다.

-4개의 stereoType annotation (component-scan에 의해 자동으로 등록)
	@Component : stereoType annotation조상
	@Controller : Spring MVC에서 Controller로 인식
	@Service : 역할 부여 없이 스캔대상이 됨. 비즈니스 클래스(biz)에 사용
	@Repository : 일반적으로 dao에서 사용되며, Exception을 DataAccessException으로 변환

-----



1. #### @Component 

  패키지 : org.springframework.stereotype
  버전 : spring 2.5
  클래스에 선언하며 해당 클래스를 자동으로 bean 등록
  bean의 이름은 해당 클래스명이 사용됨(첫글자는 소문자)
  범위는 디폴트로 singleton이며 @Scope를 이용해 지정할 수 있다.

2. #### @Autowired : 의존성 주입

  패키지 : org.springframework.beans.factory.annotation
    버전 : spring 2.5
    @Autowired 어노테이션은 spring에서 의존관계를 자동으로 설정할 때 사용
    **생성자, 필드 , 메서드** 세 곳에 적용이 가능하며 타입을 이용한 프로퍼티 자동 설정 기능을 제공
    즉 , 해당 타입의 빈 객체가 없거나 2개 이상일 경우 예외를 발생시킨다
    프로퍼티 설정 메서드(setter)형식이 아닌 일반메서드에 적용이 가능하다.
    프로퍼티 설정이 필수가 아닐 경우 ,@Autowired(required = false) 로 선언한다. (기본값 : true)
    byType으로 의존관계를 자동으로 설정할 경우 같은 타입의 빈이 2개이상 존재하게 되면 예외가 발생하는데, @Autowired도 같은 문제가 발생한다.
    이럴 때 @Qualifier 를 사용하면 동일한 타입의 빈 중 특정 빈을 사용하도록 하여 문제를 해결할 수 있다.

    **byType먼저찾고 byName**

ex) 
	 @Autowired
	 @Qualicier("test")
	 private Test test;
	 

> Spring에서는 5 개의 Auto-wiring mode가 지원된다.
>
> - no - default, Auto-wiring 없음, “ref”속성을 통해 수동으로 설정
> - byName - 속성 이름에 의한 Auto-wiring. Bean의 이름이 다른 Bean 속성의 이름과 같으면 자동으로 연결한다.
> - byType - 특성 데이터 유형별 Auto-wiring. Bean의 데이터 타입이 다른 Bean 속성의 데이터 타입과 호환되면 자동으로 연결한다.
> - constructor - 생성자 인수의 byType Mode
> - autodetect - 기본 생성자가 발견되면 “autowired by constructor”로 연결되고, 그렇지 않으면 “autowired by type”으로 연결된다.
>

#### autowired 속성(선생님과 함께)

- **default** : 생성자에 할당할 타입이 있는 지 확인 후(constructor로 동작한다는 거겠지?) 

->없으면 , 메서드에서 타입이 있는지 확인하여 할당(byType)

*만일 ,기본생성자가 있으면 기본생성자 호출 그래서 수업시간에서 널떴던 것임!

*@Autowired 라는 어노테이션이 이 방식으로 동작

- **byName** : setter 와 같은 이름(id/name)의 bean을 찾아서 자동할당

해당 빈의 이름을 찾을 것 

- **byType**  : setter 와 같은 type의 bean을 찾아서 자동할당 

- **constructor** : 생성자의 파라미터와 같은 타입의 같은 이름의 bean을 찾아서 자동할당

id name 둘다 같아야함

3. #### @Qualifier

  패키지 : org.springframework.beans.factory.annotation
  버전 : spring 2.5
  @Autowired 어노테이션이 타입 기반이기 때문에 2개이상의 동일타입 빈 객체가 존재할 시 
  특정 빈을 사용하도록 선언한다.
  @Qualifier("beanName")의 형태로 @Autowired와 같이 사용하며, 
  메서드에서 두 개 이상의 파라미터를 사용할 경우에는
  파라미터 앞에 선언해야한다.

4. #### @Required

  패키지 : org.springframework.beans.factory.annotaion
  버전 : spring 2.0
  필수 프로퍼티임을 명시하는 것으로, 프로퍼티 설정 메서드(setter)에 붙이면 된다.
  필수 프로퍼티를 설정하지 않을 겨우 빈 생성시 예외를 발생시킨다.

5. #### @Repository

  패키지 : org.springframework.stereotype
  버전 : spring 2.0
  dao에 사용되며 Exception 을 DataAccessException으로 변환

6. #### @Service

  패키지 : org.springframework.stereotype
  버전 : spring 2.0
  @Service 어노테이션을 적용한 class는 biz로 등록이 된다.

7. #### @Resource

  패키지 : javax.annotaion.Resource
  버전 : java 1.6 & jee5
  어플리케이션에서 필요로 하는 자원을 자동 연결할 때 사용한다.
  name 속성에 자동으로 연결될 빈 객체의 이름을 입력한다.
  ex)
  @Resource(name="testDao") //byName -> byType

#### component-scan 속성

- component-scan을 선언에 의해 특정 패키지 안의 클래스들을 스캔하고 

  @Component 어노테이션이 있는 클래스에 대하여 bean 인스턴스를 생성

- @Controller , @Service , @Repository

  @Component 구체화 -> @Controller , @Service , @Repository

  bean으로 등록 

  해당 클래스가 controller/service/repository로 사용됨을 spring framework에 알림

#### 200818

```java
<context:component-scan base-package=""/>
@Component @Service @Controller @Repository
```

@Service : BIz , 비즈니스로직

@Repository : Dao -> Exception => DataAccessException 데베랑 연결하다가 나는 에러다! 라고 알아들을 수 있 음 : 일반적인 예외를 저거로 바꿔주는 역할을 한다 

@Controller : 연결되는 url 받아서 명령수행?

```java
<mvc:annotation-driven/>
    //스프링 mvc를 위해서 만들어진 아이
```

```
 <property name="pointcut">
 	<bean class="org.springframework.aop.support.JdkRegexpMethodPointcut">
 		<property name="pattern">
 			<value>.*sayHello.*</value>
 		</property>
 	</bean>
 	
 	//정규식표현 공부해봐라!!!!!!!!!!!!!!!!!!!!!!!
```



---

### 자바)접근제한자 

**public > protected > default > private** 순으로 접근 허용 가능 범위 순서!  접근범위는 왼쪽이 더 큼 

public : 접근 제한이 없음 / 모든 접근을 허용

protected : 동일한 패키지 내에서 , 만약 상속받을 시 다른 패키지에서 접근허용 

default : 생략가능하고, 동일한 패키지 내에서만 접근이 가능

private : 자기 자신의 클래스 내에서만 접근이 가능 

### 자바)접근제한자 사용

-클래스 : public , default 

-생성자 : public , protected, default ,private

-멤버변수 : public, protected , default, private 

-멤버메소드 : public, protected, default, private

-지역변수 : 접근제한자 사용 불허 

### 자바)인터페이스의 특성

- 인터페이스는 추상메소드만 존재할 수 있음 -> abstract 생략가능
