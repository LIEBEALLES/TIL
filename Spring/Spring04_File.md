# Spring04_File

> 패키지명 : 기관성격(com,org ...) / 프로젝트명 / 세부프로젝트

1. ### pom.xml에 commons-io / commons-io , commons fileupload / commons-fileupload 추가

   - **commons-io** : 아파치 소프드웨어 재단에서 제공하는 자바 기반의 io관련 오픈소스(io를 제공하는 유틸리티 컴포넌트)
   - **commons-fileupload** : 파일 업로드를 할 때 필요한 라이브러리

   ```xml
   <!-- https://mvnrepository.com/artifact/commons-io/commons-io -->
   <dependency>
       <groupId>commons-io</groupId>
       <artifactId>commons-io</artifactId>
       <version>2.8.0</version>
   </dependency>
   
   <!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
   <dependency>
       <groupId>commons-fileupload</groupId>
       <artifactId>commons-fileupload</artifactId>
       <version>1.4</version>
   </dependency>
   ```

2. ### applicationContext.xml에 bean태그

   ```xml
   <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
   			<property name="maxUploadSize" value="100000000"/>
   			<property name="defaultEncoding" value="UTF-8"/>
   </bean>
   ```

   - 화면 단에서 multipart/form-data방식으로 서버에 전송되는 데이터를 스프링 MVC multipartResolver로 처리
   - value = "100000000" : 10mb제한

3. ### web.xml 경로지정 / :star:루트 그대로 간다!

   ```xml
   <context-param>
   		<param-name>contextConfigLocation</param-name>
   		<param-value>/WEB-INF/spring/appServlet/applicationContext.xml</param-value>
   </context-param>
   
   
   <servlet-mapping>
   	<servlet-name>appServlet</servlet-name>
   	<url-pattern>/</url-pattern>
   </servlet-mapping>
   ```

   - 루트 그대로 가는 이유 : / 뒤에 어느문자가 오든 다 받겠다

4. ### web.xml 에 encoding Filter 추가 (한글 인코딩 처리!)

   ```xml 
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

5. ### index.jsp 

   ```JSP
   <%@page import="javax.servlet.jsp.tagext.TagLibraryInfo"%>
   <%@ page language="java" contentType="text/html; charset=UTF-8"
       pageEncoding="UTF-8"%>
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   <!DOCTYPE html>
   <html>
   <head>
   <meta charset="UTF-8">
   <title>Insert title here</title>
   </head>
   <body>
   
   <c:redirect url="form"></c:redirect>
   </body>
   </html>
   ```

   - `c:redirect` : 지정된 url로 페이지를 이동시킨다
   - log를 찍어보니 index.jsp가 실행되지마자 HomeController `@RequestMapping` 의 value값이 form인 곳 찾아간다. = `<c:redirect url="form"></c:redirect>`

6. ###  HomeController.java

   ```JAVA
   @Controller
   public class HomeController {
   	
   	
   	@RequestMapping("/form")
   	public String uploadForm() {
   		return "upload";
   	}
   	
   }
   
   ```

   - return 값으로 upload.jsp로 가라!

7. ### upload.jsp 만들기 

8. ### package com.mvc.updown.validate; /

   ### FileValidator impl Validator(org.springframework.validation.Validator)

   - **:star:Validator(인터페이스)** : 도메인 객체를 검증할 수 있음

     객체를 검증하는데 실패하면 Errors객체에 에러를 등록함으로써 동작

   | method                                                  |                                                              |
   | ------------------------------------------------------- | ------------------------------------------------------------ |
   | support(class)                                          | 매개변수로 전달된 클래스를 검증할 수 있는지 여부를 반환      |
   | validate(Object, org.springframework.validation.Errors) | 매개변수로 전달된 객체를 검증하고 실패하면 Errors 객체에 에러를 등록 |

9. ### package com.mvc.updown.dto; / UploadFile 

   ```java
   package com.mvc.updown.dto;
   
   import org.springframework.web.multipart.MultipartFile;
   
   public class UploadFile {
   	
   	private String name;
   	private String desc;
   	private MultipartFile mpfile;
   	public String getName() {
   		return name;
   	}
   	public void setName(String name) {
   		this.name = name;
   	}
   	public String getDesc() {
   		return desc;
   	}
   	public void setDesc(String desc) {
   		this.desc = desc;
   	}
   	public MultipartFile getMpfile() {
   		return mpfile;
   	}
   	public void setMpfile(MultipartFile mpfile) {
   		this.mpfile = mpfile;
   	}
   	
   
   }
   ```

10. ### FileValidator 클래스에`@service` 추가

    : Controller 에서 FileValidator 객체를 사용하려고 `@service` 걸어주었다.

    ```java
    package com.mvc.updown.validate;
    
    import org.springframework.stereotype.Service;
    import org.springframework.validation.Errors;
    import org.springframework.validation.Validator;
    
    import com.mvc.updown.dto.UploadFile;
    
    @Service
    public class FileValidator implements Validator {
    
    	@Override
    	public boolean supports(Class<?> clazz) {
    		//왜 false일까
    		return false;
    	}
    
    	@Override
    	public void validate(Object target, Errors errors) {
    	
    		UploadFile file = (UploadFile) target;
    		
    		//파일이 없으면 
    		if(file.getMpfile().getSize() == 0) {
    			errors.rejectValue("mpfile", "fileNPE", "Please select a file");
    		}
    		
    
    	}
    
    }
    
    ```

    - `rejectValue(field, errorCode, defaultMessage);`

11. ### HomeController 에 `@Autowired`

    ```java
    @Autowired // 서비스를 걸어주었기 때문에 가능!
    	private FileValidator fileValidator;
    ```

12. ### upload.jsp

    ```jsp
    <%@ page language="java" contentType="text/html; charset=UTF-8"
    	pageEncoding="UTF-8"%>
    <%@ taglib prefix="form" uri="http://www.springframework.org/tags/form"%>
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <title>Insert title here</title>
    </head>
    <body>
    
    	<h1>UPLOAD PAGE</h1>
    
    <!--
    *태그없이써야함 안 그러면 오류가 난다*
    	spring form tag
    	form:form, form:errors, form:input, ...
    	우리가 기본적으로 form태그에서 쓸 수있는거 웬만한 것은 다 지원해준다.
    	나중에 찾아볼 것!
    
    	form:errors -> Errors, BindingResult를 사용할 때 command객체의 
    	특정 field에서 발생하는 예외에 대한 메세지를 출력가능
    
    -->
        
    <!-- 
    	enctype 속성 : 전송되는 데이터 형식
    	1.application/www-form-urlencoded : 문자들이 encoding 됨(default)
    	2.multipart/form-data : file upload - post / 파일 담을 때 멀티파트쓴다!
    	3.text/plain : 문자들을 encoding 하지 않음 , 기본적인 문자열 자체로 보내겠다
    -->
    	<form:form method="post" enctype="multipart/form-data"
    		modelAttribute="uploadFile" action="upload">
    	file<br>
    		<input type="file" name="mpfile">
    		<br>
    		<p style="color: red; font-weight: bold;">
    			<form:errors path="mpfile" />
    		</p>
    		<br>
    	
    	description<br>
    		<textarea rows="10" cols="40" name="desc"></textarea>
    		<br>
    		<input type="submit" value="send" />
    	</form:form>
    
    </body>
    </html>
    ```

    - `<form:form>` : spring form tag
    - `<form:errors>` : 지정한 프로퍼티와 관련된 한 개 이상의 에러메세지를 출력
    - `action="upload" ` : 현재 요청 url 
    - `id="command"` : 따로 지정해주지 않으면 command 가 기본값
    - `enctype="multipart/form-data` : 전송되는 데이터 형식
    - `modelAttribute="uploadFile` : command객체를 찾아간다. (앞글자를 대문자로 바꿔서)

13. ### HomeController (upload 부분)

    ```java
    	
    	@RequestMapping("/upload")
    	public String fileUpload(HttpServletRequest request, Model model, UploadFile uploadFile, BindingResult result) {
    		fileValidator.validate(uploadFile, result);
    		/*
    		upload.jsp에서 send하면 uploadFile 커맨드객체 하나가 가는데 파일이름, 파일설명이
    		이 두개가 전달된다.
            전달된 값을 Validator로 보냈어
    		Validate메소드를 통해서 혹시 예외가 발생한다면 
    		바인딩리절트가 대기하고있다가 해당 파일에 에러가 발생한다면 Validator로 가
            (return : result)
    		그 값을 가지고 upload.jsp에서 우리가 지정해준 에러메세지를 출력해줘 
    		(유효성 검사결과를 뷰로 가져오려고 form error를 쓴 것)
    		*/
    		if(result.hasErrors()) {
    			return "upload";
    		}
    		
    		MultipartFile file = uploadFile.getMpfile();
    		String name = file.getOriginalFilename();
    		
    		UploadFile fileObj = new UploadFile();
    		fileObj.setName(name);
    		fileObj.setDesc(uploadFile.getDesc());
    		
    		
    		InputStream inputStream = null;
    		OutputStream outputStream =null;
    		
    		try {
    			inputStream = file.getInputStream();
    			String path = WebUtils.getRealPath(request.getSession().getServletContext(), "/resources/storage");
    			
    			File storage = new File(path);
    			if(!storage.exists()) {
    				storage.mkdir();
    			}
    			
    			File newFile = new File(path + "/" + name);
    			if(!newFile.exists()) {
    				newFile.createNewFile();
    			}
    			
    			outputStream = new FileOutputStream(newFile);
    			
    			int read = 0;
    			byte[] b = new byte[(int)file.getSize()];
    			
    			while((read=inputStream.read(b)) != -1) {
    				outputStream.write(b,0,read);
    			}
    		
    	
    		} catch (IOException e) {
    			e.printStackTrace();
    		}finally {
    			try {
    				inputStream.close();
    				outputStream.close();
    			} catch (IOException e) {
    				
    				e.printStackTrace();
    			}
    		}
    		
    		model.addAttribute("fileObj", fileObj);
    		
    		
    		return "download";
    	}
    
    //밑에 다운로드 시작 더 있음 ! 15번으로 가라
    ```

    

14. ### download.jsp

    ```jsp
    <%@ page language="java" contentType="text/html; charset=UTF-8"
    	pageEncoding="UTF-8"%>
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <title>Insert title here</title>
    </head>
    <body>
    
    	<h1>DOWNLOAD PAGE</h1>
    	file : ${fileObj.name }<br>
    	desc : ${fileObj.desc }
    	
    	<form action="download" method="post">
    		<input type="hidden" name="name" value="${fileObj.name }"/>
    		<input type="submit" value="download"/>
    	</form>
        
        <!--
    	tomcat web.xml 아래쪽 mime-type(Multipurpose Internet Mail Extension)
    	request header 에 지정되는 데이터가 어떤 종류의 stream인지를 나타내는 프로토콜
    
    	* mime-type추가하기 / hwp는 한국 밖에 없어서
    	<mime-mapping>
    		<extension>hwp</extension>
    		<mime-type>application/unknown</mime-type>
    	</mime-mapping>
    	
    	-->
    
    </body>
    </html>
    ```

15. ### HomeController (download 부분)

    ```java
    @RequestMapping("/download")
    	@ResponseBody
    	public byte[] fileDownload(HttpServletRequest request, HttpServletResponse response, String name) {
    		byte[] down = null;
    
    		try {
    
    			String path = WebUtils.getRealPath(request.getSession().getServletContext(), "/resources/storage");
    			File file = new File(path + "/" + name);
    			//해당 경로에 그 파일 값을 가지고 오겠다 
                
                //copyToByteArray : 바이트배열로 바꾸겠다.
    			down = FileCopyUtils.copyToByteArray(file);
                //한글깨짐방지
    			String filename = new String(file.getName().getBytes(), "8859_1");
    			/*해당 브라우저가 Content-Disposition", "attachment" : 
    			파일 다운로드 하는 애구나 하고 자동으로 알아듣는다. + filename
                설정가능
    			*/
    			response.setHeader("Content-Disposition", "attachment; filename=\"" + filename + "\"");
    			response.setContentLength(down.length);
    		} catch (FileNotFoundException e) {
    			e.printStackTrace();
    		} catch (IOException e) {
    			e.printStackTrace();
    		}
            //그 만들어 온 걸 리턴할거다
    		return down;
    
    	}
    ```

    

    

