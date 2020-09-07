# Spring(설정순서)

## 🍔SpringMVC(설정순서)

##### spring regacy project / spring MVC project

1. #### pom.xml : ojdbc6 / mybatis / mybatis-spring / commons-dbcp / spring-orm

   - 자르파일 다 잘 가져왔을 지 확인하려면 pom.xml에 jar 넣고 실행해보면 된다

   <br>

2. #### web.xml에서 경로 지정 잘 해주고 바꿔준다.

   <context-param>
     		<param-name>contextConfigLocation</param-name>
     		<param-value>/WEB-INF/spring/appServlet/applicationContext.xml</param-value>
     	</context-param>

   - spring / root-context.xml이 있는데 선생님은 그냥 appServlet 폴더에 다 넣으신다 
   - 이름을 applicationContext.xml로 변경 후 web.xml에서도 바꿔준다.

   <br>

3. #### web.xml에서 <url-pattern>*.do</url-pattern> 로 바꿔주고

   <br>

4. #### web.xml에서 필터추가

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

   <br>

5. #### myboard.sql / (/WEB-INF/spring/sqls/myboard.sql)

   <br>

6. #### BoardDto , BoardDao(Impl), BoardBiz(Impl), BoardController  

   - (com.mvc.upgrade.model. ~  dto,dao,biz) (com.mvc.upgrade.controller) 

     ->**다오 , 비즈클래스에 어노테이션 걸어주고 오토와이드 설정**

   ```java
   //다오임플
   @Autowired
   private SqlSessionTemplate sqlSession;
   ```

   ```java
   //비즈임플
   @Autowired
   	private BoardDao dao;
   ```

   <br>

7. #### Controller에서 비즈객체생성하는 autowired

   <br>

8. #### board-mapper.xl (src/main/resources/mybatis/board-mapper.xml) 

   -  마이바티스 홈페이지 -> 시작하기

     preferences - XML - Catalog - Add

     key : -//mybatis.org//DTD Mapper 3.0//EN

     location http://mybatis.org/dtd/mybatis-3-mapper.dtd

     

     key -//mybatis.org//DTD Config 3.0//EN

     location : http://mybatis.org/dtd/mybatis-3-config.dtd

     

     new - other - DTDfile - Catalog - mapper - finish

   <br>

9. #### <cache-ref namespace="" >지우고 namesapce 속성 추가해주고 추가한 값 DAO의 namespace에 추가해주기 

   <br>

10. #### db.properties(src/main/resources/mybatis/db.properties)

    <br>

11. #### config.xml(/WEB-INF/spring/sqls/config.xml)

    <br>

12. #### applicationContext.xml  설정 4개 잡자 

    <br>

    ----------

    ### 🍟_Filter

13. #### LogFilter(com.mvc.upgrade.common.filter) impl javax.servlet.Filter 

    <br>

14. #### web.xml

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

