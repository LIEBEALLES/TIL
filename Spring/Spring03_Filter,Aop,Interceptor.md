# Spring03_Filter,Aop,Interceptor

### :black_medium_small_square: Filter :black_medium_small_square: AOP:black_medium_small_square: Interceptor 

## :black_medium_small_square: Filter 

> client 와 server 사이에 위치해서 각각의 작업을 처리하고 넘겨준다.
>
> 요청과 응답객체를 가지고 무언가 처리하고 필터체이닝을 통해서 다음 필터 혹은 서버한테 리케스트와 리스펀스를 연결해주는 역할

## :black_medium_small_square: AOP

> 해당 클래스에서 cc와ccc를 분리해서 공통 관심사항을 호출할 시점에 붙여서 실행시켜주는 것 (위빙! 엮어주는 역할)

## :black_medium_small_square: Interceptor

> 디스패처 서블릿이 컨트롤러로 리퀘스트를 요청할 때 인터셉터가 가로채서 특정 무언가를 처리하고 클라이언트로 넘어가던가 못넘어가던가 그런 것들을 해줄 수 있는 객체

#### :point_right: 공통부분을 빼서 따로 관리하기 위한 기능

> 웹 개발을 하다보면, 공통적으로 처리해야 할 업무들이 많다. 
>
> 공통업무에 관련된 코드를 모든 페이지마다 작성해야하면 중복코드도 많아지고, 
>
> 프로젝트 단위에 따라 서버에 부하를 줄 수도 있으며 소스관리도 되지 않는다. 
>
> 즉 , 공통부분은 따로 관리하는 게 좋다. 
>
> 이러한 공통업무를 프로그램 흐름의 앞, 중간, 뒤에 추가하여 자동으로 처리할 수 있는 방법으로
>
> Filter, AOP, Interceptor 가 있다.

### ![image-20200914021657504](C:\Users\dywjd\AppData\Roaming\Typora\typora-user-images\image-20200914021657504.png)

1. 서버실행 -> servlet이 올라오는 동안 init()이 실행이 되고, 그 후에 doFilter실행

2. 컨트롤러에 들어가기 전 preHandler 실행

3. 컨트롤러로 나와 postHandler, afterCompletion, doFilter 순으로 진행

4. 서블릿 종료시 destroy가 실행

   <br>

|             | Filter                                           | Interceptor                     | AOP                                                          |
| ----------- | ------------------------------------------------ | ------------------------------- | ------------------------------------------------------------ |
| 실행위치    | servlet                                          | servlet                         | method                                                       |
| 실행순서    | 1(제일 먼저, 제일 나중)                          | 2                               | 3                                                            |
| 설정위치    | xml / java                                       | web.xml                         | xml / java                                                   |
| 실행 메소드 | preHandler<br />postHandler<br />afterCompletion | init<br />doFilter<br />destroy | pointcut으로<br />@after, @before, <br />@around 등 위치를<br />선정하여 자유롭게 <br />메소드 생성가능 |

![image-20200914023332033](C:\Users\dywjd\AppData\Roaming\Typora\typora-user-images\image-20200914023332033.png)

