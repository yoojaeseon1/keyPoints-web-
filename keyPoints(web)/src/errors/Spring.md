- nested exception is org.springframework.beans.factory.NoSuchBeanDefinitionException


root-context.xml에서

<context:component-scan base-package="org.zerock"></context:component-scan>

base-package를 "org.zerock" 으로 경로를 설정해줘야 controller,service,DAO 모두를 검색해서 spring bean 클래스로 등록할 수 있다.

"org.zerock.controller"로 설정할 경우 controller만 spring bean class로 만들어 나머지 클래스에 접근하면 오류가 발생한다.


-----

- The server time zone value '´???¹?±¹ ???Ø½?' 


root-context.xml에서

		<property name="url"
			value="jdbc:log4jdbc:mysql://127.0.0.1:3306/curd?serverTimezone=UTC"></property>



serverTimezone=UTC 추가

-----

- The superclass "javax.servlet.http.HttpServlet" was not found on the Java Build Path



서버 추가하고

프로젝트 우클릭 -> Build Path -> Configure Build Path...-> Libraries 탭 -> add library 

-> server runtime -> 사용중인 서버(tomcat) 선택

-----


org.springframework.beans.factory.NoSuchBeanDefinitionException: No matching bean of type [org.apache.ibatis.session.SqlSession] found for dependency...




web.xml 파일에 빈 객체를 생성하는 applicationContext.xml(xml파일명은 상관없음)이

	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring/applicationContext.xml</param-value>
	</context-param>

와 같은 형식으로 등록이 되어있어야 빈 객체를 정상적으로 생성할 수 있다.


-----

Attribute : class

The fully qualified name of the bean's class, except if it serves only as a parent definition for child bean 

 definitions.



Data Type : string



ojdb14.jar를 import 해주면 된다.


-----

실행하자마자 404 not found 또는 VO cannot be resolved~~(VO domain이 import되지 않을 때)

프로젝트 우클릭 > properties > Java Build Path > Libraries > JRE System Library를 remove > Add Library > JRE System Library추가(workspace default JRE)


-----

expected at least 1 bean which qualifies as autowire candidate

또는

Unsatisfied dependency expressed through field


: mapper.xml에 오타 있는지 확인

Controller와 serviceImple, daoImpl에 에너테이션 잘 달려있는지 확인



-----

Unable to find Log4j2 as default logging library

src/main/resource 경로에 mybatis-config.xml 파일 생성

입력 : 

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
	PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

	<typeAliases>
		<package name="org.zerock.domain"></package>
	</typeAliases>

</configuration>

-----

org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'dataSource' defined in file


src/main/resource 경로에 log4jdbc.log4j2.properies 파일 생성

입력 : log4jdbc.spylogdelegator.name=net.sf.log4jdbc.log.slf4j.Slf4jSpyLogDelegator


같은 경로에 logback.xml파일 생성

입력 : 

<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <include resource="org/springframework/boot/logging/logback/base.xml"/>
 
    <!-- log4jdbc-log4j2 -->
    <logger name="jdbc.sqlonly"        level="DEBUG"/>
    <logger name="jdbc.sqltiming"      level="INFO"/>
    <logger name="jdbc.audit"          level="WARN"/>
    <logger name="jdbc.resultset"      level="ERROR"/>
    <logger name="jdbc.resultsettable" level="ERROR"/>
    <logger name="jdbc.connection"     level="INFO"/>
</configuration>

-----

Uncaught SyntaxError: Invalid shorthand property initializer

원인 : 변수를 초기화 할 때 ':'를 사용하지 않았을 경우('=' 로 자주 실수한다.)

-----

ajax 할 때 415에러

ajax안에 오타 없는지 확인하자



에러 났었던 경우

headers >> Content-Type(context-Type으로 잘못 썼었다.)

-----

Error Code: 1175. You are using safe update mode

입력

SET SQL_SAFE_UPDATES =0;


-----

java.lang.IllegalArgumentException: Mapped Statements collection does not contain value for 패키지 경로


mapper.xml의 namespace와 DAOImple 클래스의 namespace가 일치하는지 확인


-----

WARN : org.springframework.core.io.support.PathMatchingResourcePatternResolver - Cannot search for matching files underneath URL [classpath:mappers/] because it does not correspond to a directory in the file system

java.io.FileNotFoundException: URL [classpath:mappers/] cannot be resolved to absolute file path because it does not reside in the file system: classpath:mappers/

-----

WARN : org.springframework.web.servlet.PageNotFound - No mapping found for HTTP request with URI [/] in DispatcherServlet with name 'appServlet'

방법 1

servlet-context.xml 파일에서

<context:component-scan base-package="org.zerock.controller" />

부분의 패키지 경로와 controller의 패키지 경로가 일치하는지 확인


방법 2

.jsp 또는  .js 파일의 ${pageContext.request.contextPath} 뒤에 오는 경로에 문제가 없는지 확인하자

ex)

"/resources" 가 빠져있어서 오류가 발생했던 경우

	<link rel="stylesheet" href="${pageContext.request.contextPath}/resources/css/selectDesign.css">



-----

404 Not Found : The origin server did not find a current representation for the target resource or is not willing to disclose that one exists.

프로젝트 우클릭 -> Properties -> Web Project Settings -> Context root를 '/'로 변경

-----

Current request is not of type [org.springframework.web.multipart.MultipartHttpServletRequest]

1. 전송하는 <form> 태그에서 enctype="multipart/form-data"로 속성 값을 입력해준다.

2. applicationContext.xml 파일에 멀티 파일 전송을 위한 bean 객체를 추가해준다.

	<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"/>
	
---

##### 한글 깨질 때

window -> preferences 에서 할 수 있는 모든 것들 UTF-8로 설정

그래도 안되면

server.xml 파일에서

<connector> 태그에 URIEncoding="UTF-8" 추가

그래도 안되면

web.xml 파일에

	<filter>
		<filter-name>encodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>encodingFilter</filter-name>
		<url-pattern>*</url-pattern>
	</filter-mapping>



###### 한글이 ?(물음표)로 깨질 때

	@RequestMapping(value = "/review_list/commentList", method = RequestMethod.GET, produces = "application/json; charset=utf-8")

RequestMapping에

	 produces = "application/json; charset=utf-8"
	 
produces / charset 속성을 추가해주면 된다.

리턴 타입이 json이면 json, 문자면 String으로 바꿔주면 된다.

###### URL로 들어가는 한글이 깨질 때

	String keyword = URLEncoder.encode(cri.getKeyword(), "UTF-8");
	
와 같이 URLEncoder로 UTF-8 방식으로 인코딩한 String을 사용하면 된다.

---

##### javax.el.PropertyNotFoundException: [com.bit.yes.model.entity.ReviewVo] 타입에서 프로퍼티 [idx]을(를) 찾을 수 없습니다.

review_comment.jsp 파일에서

주석으로 된 부분에 bean.idx가 있는데 그 주석을 지우거나, bean.reviewIndex로 vo 필드값을 정확히 써주면 해결

왜 주석인데 오류가 발생하는지 모르겠다. : 주석을 /**/, // 이 아니라 <%--  --%>를 사용해야 정상적으로 비활성화 된다.

HTML, JavaScript는 <%--  --%>로 주석을 처리해야한다.

/**/, // 는 java 코드를 위한 주석이다.

---

##### JSP파일 갱신 안될 때


\workspace\.metadata\.plugins\org.eclipse.wst.server.core\tmp0\work\Catalina\localhost\www\org\apache\jsp\WEB_002dINF\views 에 있는 파일 전부 지우기

컴파일하면 다시 생긴다.

그래도 안되면 script 소스(html, java 말고)에 문법적으로 문제가 있는지 일일히 확인해야된다.

---

##### java.lang.IllegalStateException: Optional int parameter 'reviewIndex' is present but cannot be translated into a null value due to being declared as a primitive type

ajax통신으로 Controller의 메소드에서 변수가 mapping 될때 기본형 변수에 넣을 값이 ajax통신을 통해 넘어오지 않으면 기본형 변수가 null이 될 수 없으므로 오류가 난다.

---

##### Unterminated [&lt;c:if] tag

별일 없어도 오류가 난다.

해결법 : 해당 프로젝트 우클릭 -> Properties -> validation -> 전부 disable All 적용 -> clean project

##### java.lang.NumberFormatException: For input string: "+pageMaker.startPage+"

	pagingHTML += "<c:forEach begin="+pageMaker.startPage+" end="+pageMaker.endPage+" var='idx'>";

"+pageMaker.startPage+"를 왜 문자열로 보는걸까

"<c:forEach begin="

pageMaker.startPage

" end="

pageMaker.endPage

" var='idx'>"

이렇게 구분되는걸 의도한건데 뭘 잘못 생각한걸까

###### 원인

JSTL에서

	<c:forEach begin="+pageMaker.startPage+" end="+pageMaker.endPage+" var='idx'>

만 쏙 빼내서 인식하기 때문에(맨앞의 pagingHTML += "  맨 뒤의 "; 은 무시한다.)

begin 속성을 +pageMaker.startPage+

end 속성을 +pageMaker.endPage+

로 봤기 때문에 이게 숫자가 아니니까 NumberFormatException이 발생한 것이다.


##### org.apache.jasper.JasperException: /WEB-INF/views/review/review_detail.jsp (행: [415], 열: [1]) /WEB-INF/views/review/review_comment.jsp (행: [167], 열: [40]) quote symbol expected

둘다 따옴표 잘못써서 난 오류. 아직 해결책은 찾지 못했다.

###### 원인

javaScript 소스

	pagingHTML += "<c:if test="+pageMaker.prev+">";
	
JSTL이 인식한 소스	
	
	<c:if test="+pageMaker.prev+">

만 쏙 빼네서 인식한다. 그렇기 때문에 test 속성을 +pageMaker.prev+로 보니까 

위와 같은 NumberFormatException이 발생한 것이다.

--

###### 해결방법

javaScript 소스

	pagingHTML += "<c:if test=true>";
	
JSTL이 인식한 소스

	<c:if test=true>
	
test 속성을 감싸는 따옴표가 없으니까 따옴표가 예상된다는 에러가 발생한 것이다.

---

##### spring java.lang.NoClassDefFoundError : com/bit/yes/model/adminDAO

###### 원인

- adminDao를 포함해서 Dao라고 되있는 클래스명을 전부 DAO로 바꿨다.

- Impl클래스의 postfix에 01이 추가되어있는 것들이 있었는데 전부 제거했다.


###### 해결 방법

- 클래스명을 원상복귀 시키지 않고 pom.xml에 

<dependency>
   <groupId>org.apache.httpcomponents</groupId>
   <artifactId>httpclient</artifactId>
   <version>4.5.2</version>
   <scope>runtime</scope>
 </dependency>
 

을 추가하니까 잘 된다. 추가 이후에 지워도 잘 된다. 

정확한 해결 원리는 모르겠다.

---

##### Ajax통신으로 controller -> jsp파일로 전송이 안될 때

@RequestMapping의 produces 속성을 return 타입에 맞게 수정해주자

String : "text/plain; charset=utf-8"

Json : "application/json; charset=utf-8"

---

