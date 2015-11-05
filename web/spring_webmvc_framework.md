# 二. SpringMVC 框架
## 1 DispatcherServlet(核心控制器)
* [spring webMvcFramwork](http://docs.spring.io/spring/docs/4.2.2.BUILD-SNAPSHOT/spring-framework-reference/htmlsingle/#spring-web) 说明文档
### 1.1 spring web Framwork的请求过程流
![image](http://docs.spring.io/spring/docs/4.2.2.BUILD-SNAPSHOT/spring-framework-reference/htmlsingle/images/mvc.png)

### 1.2 Spring Root Context
* 创建空的contextConfigLocation

![image](http://docs.spring.io/spring/docs/4.2.2.BUILD-SNAPSHOT/spring-framework-reference/htmlsingle/images/mvc-root-context.png)


* web.xml 配置
```
<web-app>
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/root-context.xml</param-value>
    </context-param>
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <!--这里没有定制指定context初始化配置的xml，因此创建的是空的ContextConfigLocation-->
            <param-value></param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>/*</url-pattern>
    </servlet-mapping>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
</web-app>
```
## 1.3 SpringMVC配置
>**NOTE:** 利用```@RequestMapping```标注的方法其方法参数具有任意性，用户可以随意添加，具体参见[Defining @RequestMapping handler methods](http://docs.spring.io/spring/docs/4.2.2.BUILD-SNAPSHOT/spring-framework-reference/htmlsingle/#mvc-ann-methods)
### 1.3.1 拦截器配置

### 1.3.2 内容协商配置
```
<mvc:annotation-driven content-negotiation-manager="contentNegotiationManager"/>

<bean id="contentNegotiationManager" class="org.springframework.web.accept.ContentNegotiationManagerFactoryBean">
	    <property name="mediaTypes" >
	        <map> 
                <entry key="xml" value="application/xml"/> 
                <entry key="json" value="application/json"/> 
            </map>
	    </property>
        <property name="ignoreAcceptHeader" value="true"/>
        <property name="favorPathExtension" value="true"/>
</bean>
```
### 1.3.2 View控制器配置
```
<!-- 定义无Controller的path<->view直接映射 -->
<mvc:view-controller path="/" view-name="redirect:${web.view.index}"/>
```
### 1.3.3 静态资源映射
```
<!-- 静态资源映射 -->
    <mvc:resources mapping="/static/**" location="/static/" cache-period="31536000"/>
```
### 1.3.4 静态资源访问无Controller的映射
```
<!-- 对静态资源文件的访问， 将无法mapping到Controller的path交给default servlet handler处理 -->
<mvc:default-servlet-handler />
```
### 1.4 Spring添加REST功能
#### 1.4.1 在响应体中返回资源状态
正常情况下，当处理方法返回Java对象（除String外）时，这个对象会放在模型中并在视图中渲染使用。但是如果处理方法使用了```@ResponseBody```，那表明HTTP信息转换机制会发挥作用

#### 1.4.2 消息转换器
如果使用了```@ResponseBody```Spring会将返回的对象通过```HttpMessageConverter``` 转换为Http的响应体，可以通过```<mvc:message-converters```配置相应的转化器

## 1.5 View technologies
# 三 SpringORM框架
## 3.1 持久层框架Mybatis
### 3.1.1 框架说明
* [1.Mybatis-github开源项目地址](https://github.com/mybatis/mybatis-3)
* [2.Mybatis官方说明文档](http://mybatis.github.io/mybatis-3/) 
* [3.Mybatis-spring 整合项目](https://github.com/mybatis/spring)
* [4.Mybatis-spring整合项目中文说明文档](http://mybatis.github.io/spring/zh/index.html)
### 3.1.2 使用说明
Spring加载Mybatis框架实际是初始化框架所需要的参数，利用SpringIoc功能可以实现的Bean组装与初始化，以下是配置mapper的Configer
```
<!-- 扫描basePackage下所有以@MyBatisDao注解的接口 -->
    <bean id="mapperScannerConfigurer" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
        <property name="basePackage" value="com.thinkgem.jeesite"/>
        <property name="annotationClass" value="com.thinkgem.jeesite.common.persistence.annotation.MyBatisDao"/>
    </bean>
```
