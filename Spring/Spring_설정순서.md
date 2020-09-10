# Spring(설정순서)

## 🍔SpringMVC(설정순서)

##### spring regacy project / spring MVC project 

1. #### pom.xml : ojdbc6 / mybatis / mybatis-spring / commons-dbcp / spring-orm / aspectjweaver

   - 자르파일 다 잘 가져왔는지 확인하려면 pom.xml에 jar 넣고 실행해보면 된다.

   <br>

2. #### web.xml에서 경로 지정 잘 해주고 바꿔준다.

   ```html
   <context-param>
     		<param-name>contextConfigLocation</param-name>
     		<param-value>/WEB-INF/spring/appServlet/applicationContext.xml</param-value>
     	</context-param>
   ```

   - spring / root-context.xml이 있는데 선생님은 그냥 appServlet 폴더에 다 넣으신다 
   - 이름을 applicationContext.xml로 변경 후 web.xml에서도 바꿔준다.

   <br>

3. #### web.xml에서 

   ```xml
   <url-pattern>*.do</url-pattern> 
   ```

   #### 로 바꿔주고

   <br>

4. #### web.xml에서 필터추가 / mapping 까지 꼭

   ```html
   <filter>
     		<filter-name>encodingFilter</filter-name>
     		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
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

   <br>

5. #### myboard.sql / (/WEB-INF/spring/sqls/myboard.sql)

   <br>

6. #### BoardDto , BoardDao(Impl), BoardBiz(Impl), BoardController  

   - (com.mvc.upgrade.model. ~  dto,dao,biz) (com.mvc.upgrade.controller) 

     ->**다오 , 비즈클래스에 어노테이션 걸어주고 오토와이드 설정**
     
     ->`@repository`  : 해당 클래스에서 발생하는 **exception을 DataAccessException으로 변환** / 일반적으로 dao에서 사용이된다.
     
     ->`@service` : 비즈니스로직이다 를 알려준다, 클래스를 빈으로 등록해준다 딱 ! 이거 뿐 (`@Component`랑 차이가 없음)

   ```java
   //DaoImpl
   @Autowired
   	private SqlSessionTemplate sqlSession;
   ```

   ```java
   //BizImpl
   @Autowired
   	private BoardDao dao;
   ```

   <br>

7. #### Controller에서 비즈객체생성하는 `@Autowired`

   <br>

8. #### board-mapper.xl (src/main/resources/mybatis(폴더)/board-mapper.xml) 

   -  마이바티스 홈페이지 -> 시작하기

     > preferences - XML - Catalog - Add
>
     > key : -//mybatis.org//DTD Mapper 3.0//EN
>
     > location http://mybatis.org/dtd/mybatis-3-mapper.dtd
>
     > 
>
     > key -//mybatis.org//DTD Config 3.0//EN
>
     > location : http://mybatis.org/dtd/mybatis-3-config.dtd
>
     > 
>
     > :star:**new - other - xml - <next> -DTDfile - Catalog - mapper - finish**:star:

   <br>

9. #### <cache-ref namespace="" >지우고 namesapce 속성 추가 /  DAO의 namespace에 추가해주기 

   <br>

10. #### db.properties(src/main/resources/mybatis/db.properties)

    <br>

11. #### config.xml(/WEB-INF/spring/sqls/config.xml)

    - 모든 설정이 다 들어가기 때문에 (하나만 만들어놓고 쓸거니까) config.xml 이라고 파일명을 지정해주는 것이다.

    ```xml-dtd
    <configuration>
    
    	<typeAliases>
    		<typeAlias type="com.mvc.upgrade.model.dto.BoardDto"
    			alias="BoardDto" />
    
    	</typeAliases>
    	<mappers>
    		<mapper resource="/mybatis/board-mapper.xml"></mapper>
    	</mappers>
    
    
    </configuration>
    
    
    ```

    <br>

12. #### applicationContext.xml  설정 4개 잡자 

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
    	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    	xsi:schemaLocation="http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd">
    
    	<!-- Root Context: defines shared resources visible to all other web components -->
    
    	<!-- db.properties -->
    	<bean
    		class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
    		<property name="location"
    			value="classpath:mybatis/db.properties"></property> <!-- location : 값을 하나만 저장 -s : 여러개 -->
    	</bean>
    
    	<!-- dataSource -->
    	<bean id="dataSource"
    		class="org.apache.commons.dbcp.BasicDataSource">
    		<property name="driverClassName" value="${driver}" />
    		<property name="url" value="${url}" />
    		<property name="username" value="${username}" />
    		<property name="password" value="${password}" />
    
    	</bean>
    	<!--mybatis --> <!-- sqlsessionfactory 객체 -->
    	<bean id="sqlSession"
    		class="org.mybatis.spring.SqlSessionFactoryBean">
    		<property name="dataSource" ref="dataSource"></property>
    		<property name="configLocation"
    			value="/WEB-INF/spring/sqls/config.xml"></property>
    
    	</bean>
    
    	<!-- mybatis template session.insert << 이런식으로만 해주면 된다. / template 에서 openSession 
    		, close를 해준다는 의미 -->
    
    	<bean id="sqlSessionTemplate"
    		class="org.mybatis.spring.SqlSessionTemplate">
    		<constructor-arg ref="sqlSession"></constructor-arg>
    	</bean>
    
    </beans>
    ```

    <br>

    ----------

    ### 🍟_Filter

13. #### LogFilter(com.mvc.upgrade.common.filter) impl javax.servlet.Filter 

    - ```java
      Logger logger = LoggerFactory.getLogger(LogFilter.class);
      
      //() 안에 LogFilter가 의미하는 것 : 내가 로그를 찍고싶은 클래스명을 써주면된다.
      ```

      <br>

14. #### web.xml에 <filter>추가해주기

    ```xml
    	<filter>
    		<filter-name>logFilter</filter-name>
    		<filter-class>com.mvc.upgrade.common.filter.LogFilter</filter-class>
    	</filter>
    	<filter-mapping>
    		<filter-name>logFilter</filter-name>
    		<url-pattern>/*</url-pattern>
    	</filter-mapping>
    	
    ```

    <br>

    ### 🍟_AOP

15. #### pom.xml : aspectjrt, aspectjweaver

    - aspectj 를 또 추가해주는 이유 : 중복으로 넣어도 버전이 같으면 잘 돌아간다.  -> 그거 확인하려고!

      같은 dependency가 두개 있을 때 서로 버전이 다르면 동작하지 않는다 / 충돌이 난다/

    <br>

16. #### LogAop (com.mvc.upgrade.common.aop.LogAop)

    - ```java
      import org.aspectj.lang.JoinPoint;
      import org.slf4j.Logger;
      import org.slf4j.LoggerFactory;
      
      //클래스 헷갈리지말 것
      ```

    - clazz : class 타입의 변수다. 

    - ```
      //getTarget() : 대상객체
      //getArgs() : 대상 파라미터들
      //getSignature() : 대상 메서드 정보 
      ```

    <br>

17. #### aop-context.xml(WEB-INF/spring/appServlet/aop-context.xml)

    - :star:**Spring Bean Configuration File** :star:로 만드는 것 잊지말기! 
    - Namespaces 에 aop 체크

    <br>

18. #### web.xml <init-param>에 aop-context.xml 추가해주기! 헷갈리지 말자 여기서 실수했음

    - ```
      <servlet>
      		<servlet-name>appServlet</servlet-name>
      		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
      		<init-param>
      			<param-name>contextConfigLocation</param-name>
      			<param-value>/WEB-INF/spring/appServlet/servlet-context.xml
      			/WEB-INF/spring/appServlet/aop-context.xml
      			</param-value>
      		</init-param>
      		<load-on-startup>1</load-on-startup>
      	</servlet>
      ```

    <br>

19. #### log4j.xml(src/main/resources/log4j.xml)

    - ```java
      //log4j Log Level 
        		//-Level.FATAL : 치명적인 에러 
        		//-Level.ERROR : 일반적인 에러
        		//-Level.WARN : 경고
        		//-Level.INFO : 일반적인 정보
        		//-Level.DEBUG : 디버깅 정보
      	//-Level.TRACE : DEBUG + @(상세한 정보)
      ```

    ### 🍟_Login

20. #### pom.xml : jackson-core-asl, jackson-mapper-asl 

    - ```xml 
      <!-- https://mvnrepository.com/artifact/org.codehaus.jackson/jackson-core-asl -->
      		<dependency>
      			<groupId>org.codehaus.jackson</groupId>
      			<artifactId>jackson-core-asl</artifactId>
      			<version>1.9.13</version>
      		</dependency>
      		<!-- https://mvnrepository.com/artifact/org.codehaus.jackson/jackson-mapper-asl -->
      		<dependency>
      			<groupId>org.codehaus.jackson</groupId>
      			<artifactId>jackson-mapper-asl</artifactId>
      			<version>1.9.13</version>
      		</dependency>
      
      ```

    - spring 4.x 부터는 jackson-databind / jackson-core 사용해야함

    <br>

21. #### mymember.sql (WEB-INF/spring/sqls/mymember.sql)

22. #### MemberDto, MemberDao, MemberBiz, MemberController(원래 있던 패키지에 만든다)

23. #### member-mapper.xml (src/main/resources/mybatis/member-mapper.xml)

24. #### config.xml (WEB-INF/spring/sqls/config.xml)

    - member-mapper 추가

    ### 🍟_Interceptor

25. #### LoginInterceptor(com.mvc.upgrade.common.interceptor.LoginInterceptor impl HandlerInterceptor)

26. #### servlet context.xml 적용

    - ```xml
      <!--interceptor -->
      	<interceptors>
      		<interceptor>
      			<mapping path="/*" />
      			<beans:bean class="com.mvc.upgrade.common.interceptor.LoginInterceptor"></beans:bean>
      		</interceptor>
      	</interceptors>
      ```

      

27. 

    

    <br>

----

##### 🍕spel ${  } : 스프링에서도 사용되어서 저렇게도 부른다.

```html
	<!-- dataSource -->
	<bean id="dataSource"
		class="org.apache.commons.dbcp.BasicDataSource">
		<property name="driverClassName" value="${driver}"></property>
		<property name="url" value="${url}"></property>
		<property name="username" value="${username}"></property>
		<property name="password" value="${password}"></property>
	</bean>
```





