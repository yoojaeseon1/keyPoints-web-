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

