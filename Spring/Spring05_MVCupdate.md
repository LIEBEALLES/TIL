# Spring05_MVCupdate

### 1. web.xml : 2.5 -> 4.0

​	1-1.프로젝트 우클릭 -> new -> Dynamic Web Project ->  Dynamic web module version 4.0

​	-> next -> Generate web.xml deployment descriptor 체크  ->  다시 원 프로젝트로 돌아가서 web.xml 에 붙여주기

```xml 
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
	id="WebApp_ID" version="4.0">
```

![image-20200916133514101](C:\Users\dywjd\AppData\Roaming\Typora\typora-user-images\image-20200916133514101.png)

​	4.0으로 바뀌어진 거 확인할 수 있다. 

​	(4.0부터는 filter 자동완성 된다!)

### 2. pom.xml 에서 버전 바꿔주기

​	java 1.6 -> 1.8

​	springframework 3.1.1 -> 5.2.9

​	servlet-api 2.5-> 4.0.1 (servlet-api -> javax.servlet-api)

​	jsp-api : 2.1 -> 2.3.3 (jsp-api -> javax.servlet.jsp-api)

​	maven-compiler-plugin
​			source : 1.6->1.8
​			target : 1.6->1.8

### 3. project facets

![image-20200916140743968](C:\Users\dywjd\AppData\Roaming\Typora\typora-user-images\image-20200916140743968.png)

​	Java , Dynamic Web Module 을 알맞게 버전을 바꿔준다.

### 4. maven update 

Project -> Java Build Path -> Maven Dependencies 체크 확인하고 넘어갈 것 !