# Spring01_DI

> 참고사이트 : https://yzlosmik.tistory.com/6?category=782430

## 🍀DI : dependency injection , 의존주입

> ##### 각 클래스 사이의 의존관계를 낮추기 위함
>
> ##### 객체 간의 의존관계를 관리 
>
> ##### 객체 간의 결합을 느슨하게 하는 스프링의 핵심기술 중 하나 
>
> 1. 생성자를 이용한 의존성 주입 `constructor-arg`
> 2. setter메소드를 이용한 의존성 주입 `property`
> 3. 초기화 인터페이스를 이용한 의존성 주입 (field주입)

---

## 🍀before using spring

> ##### 한 클래스 내에서 다른 클래스의 기능이 필요로 할 때 , new연산자를 통해 그 클래스의 생성자를 호출해서 사용

```java
public class MessageBean{
    public void sayHello(String name){
        System.out.println("안녕," + name);
    }
}
```

```java
//메인클래스
MessageBean bean = new MessageBean();
bean.sayHello("Spring");
```

> 메인클래스 내에서 사용될 클래스(MessageBean)를 만들고 new연산자를 사용했다.
>
> 메인클래스는 MessageBean의 sayHello()기능을 사용하려면 MessageBean 클래스가 필요하다. 즉. 메인클래스는 MessageBean에 **의존**하고있다.

<br>

> #### 의존하다 ? 
>
> - 어떤 기능을 실행시키기 위해 다른 클래스가 필요로할 때 , 즉 A가 B에 의존한 다는 것은 A가 B객체를 사용한다는 것이다. 

<br>

> 만약 의존하는 클래스 즉, messageBean에 변경사항이 생기면 해당 클래스를 의존하고있는 모든  클래스(여기선 메인클래스)의 소스코드를 수정해줄 필요가 생김 
>
> -> **why?** 
>
> -> 서로 간의 의존관계가 높기때문 
>
> -> 결합도가 높음 
>
> ->**결합도를 낮추기 위해서 DI(Dependency Injection) 사용!** 

<br>

> ##### 인터페이스 : 추상메소드로만 구성되어있다. 상속받는 클래스는 추상메소드를 반드시 구현해야한다. 구현하지 않으려면 , 또 다시 상속을 해야한다.

> ##### 추상클래스 : 추상클래스는 일반클래스에서 추상메소드를 포함한 것이다. 나머지는 인터페이스와 동일하다.

```java
public interface Pencil {
	//펜슬은 타입만 제공해준다 
	
	//오버라이드하라고 알려주는 것
	public void use();

}
```

```java
public class Pencil4B implements Pencil {

	@Override
	public void use() {
		System.out.println("4B 굵기로 쓰입니다.");
	}

}

```

```java
public class Pencil6B implements Pencil {

	@Override
	public void use() {
		System.out.println("6B굵기로 쓰입니다.");
	}

}
```

```xml
<bean id="pencil" class="com.javalec.ex.Pencil6B"></bean>
<bean id="pencil2" class="com.javalec.ex.Pencil4B"></bean>
```

```java
public class MainClass {
	
	public static void main(String[] args) {
		
		
		ApplicationContext factory  = new ClassPathXmlApplicationContext("com/javalec/ex/applicationContext.xml");
		
		Pencil pencil = (Pencil)factory.getBean("pencil", Pencil.class);
		Pencil pencil2 = (Pencil)factory.getBean("pencil2", Pencil.class);
		
		pencil.use();
		pencil2.use();
	}

}

//실행결과
6b굵기
4b굵기 

```

<br>

## 🍀DI 장점

> DI를 사용하니 더욱 복잡하고 시간이 더 많이 소요된다고 생각할 수 있다.
>
> 하지만 , 프로젝트 규모가 어느 정도 커지고 , 추후 유지보수 업무가 발생 시에는 DI를 이용한 개발이 더욱 적합하다.

- ##### JAVA파일의 수정 없이 스프링 설정 파일 만을 수정하여 부품들을 생성/조립 가능 

<br>

## 🍀DI와 IoC

> ##### 스프링 프레임워크는 객체의 생성,소멸 그리고 객체 간의 의존관계를 제어한다.
>
> 개발자가 객체를 제어했던 것을, 스프링이 제어하게 되었으므로 제어권이 개발자에서 스프링으로 역전이 된 것 
>
> #### -> IoC(Inversion of Control) : 제어의 역전
>
> ##### 이 때, 제어권이 역전되도록(IoC) 하는 방식이 "DI, 의존성 주입" : 스프링 프레임워크가 연관 관계를 직접 규정하는 것
>
> ##### -> DI스프링이 직접 의존성을 주입한다 (IoC)

<br>

