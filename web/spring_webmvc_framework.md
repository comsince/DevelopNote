# ��. SpringMVC ���
## 1 DispatcherServlet(���Ŀ�����)
* [spring webMvcFramwork](http://docs.spring.io/spring/docs/4.2.2.BUILD-SNAPSHOT/spring-framework-reference/htmlsingle/#spring-web) ˵���ĵ�
### 1.1 spring web Framwork�����������
![image](http://docs.spring.io/spring/docs/4.2.2.BUILD-SNAPSHOT/spring-framework-reference/htmlsingle/images/mvc.png)

### 1.2 Spring Root Context
* �����յ�contextConfigLocation

![image](http://docs.spring.io/spring/docs/4.2.2.BUILD-SNAPSHOT/spring-framework-reference/htmlsingle/images/mvc-root-context.png)


* web.xml ����
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
            <!--����û�ж���ָ��context��ʼ�����õ�xml����˴������ǿյ�ContextConfigLocation-->
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
## 1.3 SpringMVC����
>**NOTE:** ����```@RequestMapping```��ע�ķ����䷽���������������ԣ��û�����������ӣ�����μ�[Defining @RequestMapping handler methods](http://docs.spring.io/spring/docs/4.2.2.BUILD-SNAPSHOT/spring-framework-reference/htmlsingle/#mvc-ann-methods)
### 1.3.1 ����������

### 1.3.2 ����Э������
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
### 1.3.2 View����������
```
<!-- ������Controller��path<->viewֱ��ӳ�� -->
<mvc:view-controller path="/" view-name="redirect:${web.view.index}"/>
```
### 1.3.3 ��̬��Դӳ��
```
<!-- ��̬��Դӳ�� -->
    <mvc:resources mapping="/static/**" location="/static/" cache-period="31536000"/>
```
### 1.3.4 ��̬��Դ������Controller��ӳ��
```
<!-- �Ծ�̬��Դ�ļ��ķ��ʣ� ���޷�mapping��Controller��path����default servlet handler���� -->
<mvc:default-servlet-handler />
```
### 1.4 Spring���REST����
#### 1.4.1 ����Ӧ���з�����Դ״̬
��������£�������������Java���󣨳�String�⣩ʱ�������������ģ���в�����ͼ����Ⱦʹ�á��������������ʹ����```@ResponseBody```���Ǳ���HTTP��Ϣת�����ƻᷢ������

#### 1.4.2 ��Ϣת����
���ʹ����```@ResponseBody```Spring�Ὣ���صĶ���ͨ��```HttpMessageConverter``` ת��ΪHttp����Ӧ�壬����ͨ��```<mvc:message-converters```������Ӧ��ת����

## 1.5 View technologies
# �� SpringORM���
## 3.1 �־ò���Mybatis
### 3.1.1 ���˵��
* [1.Mybatis-github��Դ��Ŀ��ַ](https://github.com/mybatis/mybatis-3)
* [2.Mybatis�ٷ�˵���ĵ�](http://mybatis.github.io/mybatis-3/) 
* [3.Mybatis-spring ������Ŀ](https://github.com/mybatis/spring)
* [4.Mybatis-spring������Ŀ����˵���ĵ�](http://mybatis.github.io/spring/zh/index.html)
### 3.1.2 ʹ��˵��
Spring����Mybatis���ʵ���ǳ�ʼ���������Ҫ�Ĳ���������SpringIoc���ܿ���ʵ�ֵ�Bean��װ���ʼ��������������mapper��Configer
```
<!-- ɨ��basePackage��������@MyBatisDaoע��Ľӿ� -->
    <bean id="mapperScannerConfigurer" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
        <property name="basePackage" value="com.thinkgem.jeesite"/>
        <property name="annotationClass" value="com.thinkgem.jeesite.common.persistence.annotation.MyBatisDao"/>
    </bean>
```
