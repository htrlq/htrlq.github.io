# mvc simple example

## config maven

### edit pom.xml
````xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">

  <modelVersion>4.0.0</modelVersion>
  <packaging>war</packaging>

  <name>Test</name>
  <groupId>Test</groupId>
  <artifactId>Test</artifactId>
  <version>1.0-SNAPSHOT</version>

  <properties>
    <spring.version>4.2.5.RELEASE</spring.version>
  </properties>

  <build>
    <plugins>

    </plugins>
  </build>

  <dependencies>
    <!-- Spring核心依赖 start -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId><!-- 包含Spring框架基本的核心工具类 -->
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-beans</artifactId><!-- springIoC（依赖注入）的基础工具类-->
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId><!-- spring 提供在基于IoC 功能上的扩展服务 -->
      <version>${spring.version}</version>
    </dependency>
    <!-- Spring核心依赖 end -->
    <!-- spring 持久层依赖start -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId><!-- 封装了spring对于事物的控制 -->
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId><!-- 包含对Spring对JDBC数据访问进行封装的所有类  -->
      <version>${spring.version}</version>
    </dependency>
    <!-- spring 持久层依赖end -->
    <!-- Spring web相关依赖 start -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId><!-- Spring Web MVC是一种基于Java的实现了Web MVC设计模式的请求驱动类型的轻量级Web框架 -->
      <version>${spring.version}</version>
    </dependency>
    <!-- Spring web相关依赖 end -->
    <!-- Spring 其它依赖 -->
    <dependency>
      <groupId>org.springframework</groupId><!-- spring面向切面编程，提供AOP（面向切面编程） -->
      <artifactId>spring-aop</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context-support</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-expression</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-orm</artifactId>
      <version>${spring.version}</version>
    </dependency>
  </dependencies>

</project>
````

## configuration mvc
### edit web.xml
````xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.4"
         xmlns="http://java.sun.com/xml/ns/j2ee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">

  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>

  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
  </context-param>

  <!--Spring MVC -->
  <servlet>
    <servlet-name>SpringMvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:/spring-mvc.xml</param-value>
    </init-param>
    <load-on-startup>0</load-on-startup>
  </servlet>

  <servlet-mapping>
    <servlet-name>SpringMvc</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>


  <!-- 自定义编码拦截器 也是spring为我们写好的 -->
  <filter>
    <filter-name>setcharacter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>

    <init-param>
      <param-name>encoding</param-name>
      <param-value>utf-8</param-value>
    </init-param>

    <init-param>
      <param-name>forceEncoding</param-name>
      <param-value>true</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>setcharacter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>

</web-app>
````

### create spring-mvc.xml
create file in resource in src\main\spring-mvc.xml
````xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="
   http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
   http://www.springframework.org/schema/context
   http://www.springframework.org/schema/context/spring-context-4.0.xsd
   http://www.springframework.org/schema/mvc
   http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd">

    <mvc:default-servlet-handler/>
    <mvc:annotation-driven />

    <context:component-scan base-package="com.controller" />


    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/" />
        <property name="suffix" value=".jsp" />
    </bean>



</beans>
````

### create applicationContext.xml
spring global configuration
create file in resource in src\main\applicationContext.xml

````xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
   http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
   http://www.springframework.org/schema/context
   http://www.springframework.org/schema/context/spring-context-4.0.xsd">
</beans>
````

create empty spring configuration

--update 2020.3.30
