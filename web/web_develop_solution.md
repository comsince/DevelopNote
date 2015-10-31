# 概览
本项目旨在分析现在成熟的开发技术，包括做一个应用系统所需要的各方面知识的汇总，虽然每一个技术并没有进行深入的技术分析，但是足以让你了解你自己的所喜欢的技术领域；当然最重要的是分享我在开发过程中遇到的各种技术问题，本文旨在理清各个技术领域的之前的相互共性，更为重要的是提供一种学习的路径和方向；在下面的说明中，我会给出一些个人认为比较流行且可用以商业的开源项目，针对项目做具体的说明；当然也会包括我个人的一些项目以及在公司中所见到的技术方案。



#一. J2EE项目开发说明
##1. web容器配置
这里主要说明springMVC的项目工程最基本的配置，详细罗列说明各个配置文件的功能，并逐步说明其工作原理。
这里的项目主要还是依托Spring FrameWork 4.0 以及spring webMVC的技术讲解MVC构建web应用程序的主要步骤

### 1.1 spring FramWork 文档概览
springFramwork包括各个小模块，各个小模块可以单独使用，项目依赖工具Maven或者gradle；
[spring webMvcFramwork](http://docs.spring.io/spring/docs/4.2.2.BUILD-SNAPSHOT/spring-framework-reference/htmlsingle/#spring-web) 说明文档
* 以下是spring web Framwork的请求过程流
![image](http://docs.spring.io/spring/docs/4.2.2.BUILD-SNAPSHOT/spring-framework-reference/htmlsingle/images/mvc.png)

### 1.2 Dispatcher web 容器配置
为了能让dispatcher处理用户的请求，必须在web.xml 配置项的项目，如下：
```
<web-app>
<!-- MVC Servlet -->

	<!-- Context ConfigLocation -->
	<!-- 指定Spring Bean的配置文件所在目录。默认配置在WEB-INF目录下 -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath*:/spring-context*.xml</param-value>
	</context-param>
	<!-- 加载spring配置-->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
	<listener>
        <listener-class>org.springframework.web.context.request.RequestContextListener</listener-class>  
	</listener>
	<servlet>
		<servlet-name>springServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<!-- 初始化参数-->
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<!-- spring mvc 的详细配置路径  可以自定义servlet.xml配置文件的位置和名称，默认为WEB-INF目录下，名称为[<servlet-name>]-servlet.xml，如spring-servlet.xml-->
			<param-value>classpath*:/spring-mvc*.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>springServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
</web-app>
```

**Note:** [web.xml 配置项以及加载顺序](http://www.cnblogs.com/JesseV/archive/2009/11/17/1605015.html)


### 1.3 WebMVC bean配置
如前所述，系统会自动加载web-inf目录下面的spring-servlet的配置文件，下面就此项目配置做一些说明。
由此说明文档基于jeesite快速开发平台，因此只针对项目中出现的配置项目进行说明，其他的可参考官方文档
* 为了能自动扫描到@Controller ，需要在配置下面的属性
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="org.springframework.samples.petclinic.web"/>

    <!-- ... -->

</beans>
```
* 多视图解析支持配置
```
<!-- 定义视图文件解析 -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="${web.view.prefix}"/>
		<property name="suffix" value="${web.view.suffix}"/>
	</bean>
```

* 文件上传配置
```
<bean id="multipartResolver"
        class="org.springframework.web.multipart.commons.CommonsMultipartResolver">

    <!-- one of the properties available; the maximum file size in bytes -->
    <property name="maxUploadSize" value="100000"/>

</bean>
```

# 二. ThinkPHP
## 1. 框架说明
这里使用thinkSNS作为基本的项目说明php的mvc框架

# 三. 前端技术
## 1.简介
作为MVC中的View层，现在前端更多的注重与用户的实时交互，从而演进为现在的Reacive响应式编程，这里主要还是说明目前流行的javaScript框架

# 四. Android应用开发
