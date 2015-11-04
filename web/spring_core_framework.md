# 概览
本项目旨在分析现在成熟的开发技术，包括做一个应用系统所需要的各方面知识的汇总，虽然每一个技术并没有进行深入的技术分析，但是足以让你了解你自己的所喜欢的技术领域；当然最重要的是分享我在开发过程中遇到的各种技术问题，本文旨在理清各个技术领域的之前的相互共性，更为重要的是提供一种学习的路径和方向；在下面的说明中，我会给出一些个人认为比较流行且可用以商业的开源项目，针对项目做具体的说明；当然也会包括我个人的一些项目以及在公司中所见到的技术方案。

# Web开发说明
## 1 关于web技术的一些博客站点
* [1.前后端分离的思考与实践系列文章](http://ued.taobao.org/blog/2014/04/full-stack-development-with-nodejs/) 
* [2.Web开发演进方向—Web 研发模式演变](https://github.com/lifesinger/lifesinger.github.io/issues/184)
这篇文章说明web开发发展进程中，全后端分离实现所作的探究，不断得再探索一种高校的开发模式
* [3.淘宝核心技术团队博客](http://csrd.aliapp.com/?page_id=2)
说明当前淘宝核心技术团队正在研发的基础服务，大部分已经开源
* [4.阿里 开源项目](https://github.com/alibaba?utf8=%E2%9C%93&query=cobar)
  主要包括web服务中各个领域的开源项目，涵盖数据库，中间件
* [5.前阿里架构师的收藏夹](http://afoo.me/favorite.html#%E6%94%B6%E8%97%8F%E5%A4%B9)
* [6.业务系统扩展-阿里中间件技术解密](http://wenku.baidu.com/link?url=A1s1yXfgtNJLnZ8K-nKBEyw49L3ZKlO9UUWkEDcdQJ3ER4XWQE2KL-13hS5eIYzIjq1lVoms3B--y-PnVCghU6ozdOBIsrZdl3vb5llsMoS) 


### 分布式系统框架
* [1.Dubbo-阿里巴巴SOA服务化治理方案的核心框架](http://dubbo.io/Home-zh.htm)
* [2.消息中间件MetaQ](https://github.com/killme2008/Metamorphosis)
* [3.魅族服务化框架kiev]()
* [4.为监控而生的数据库连接池](https://github.com/alibaba/druid)
* [5.基于MySQL的分布式数据库服务中间件](https://github.com/alibaba/cobar)
* [6.基于iBatis和Spring的轻量级分布式数据访问框架(DDAL) ](https://github.com/alibaba/cobarclient)


# 一. Spring核心框架
## 1.Spring 简化java开发的关键策略
* 基于POJO 的轻量级和最小侵入性偏程
 Spring 竭力避免因自身的API 而弄乱体的应用代码。Spring 不会强迫你实现Spring 规范的接口或推承Spring 规范的类，相反，在基于Spring 构建的应用中，它的类通常没有任何痕迹表明你使用了Spring。最坏的场景是， 一个类或许会使用Spring注解，但它依旧是POJO
* 通过依赖注入和面向接口实现松耦合
通过依赖注入 ，对象的依赖关系将由负责协调系统中各个对象的第三方组件在创建对象时设定。对象无需自行创建和管理他们的依赖关系--依赖关系将被自动注入到需要他们的对象中去。
* 基于切面和惯例进行声明式编程
依赖注入让相互协作的软件组件保持松散耦合，而AOP编程允许你把遍布应用各处的功能分离出来形成可重用的组件
* 通过切面和模板减少样板式代码



## 2 说明
Spring是后端核心的容器框架，管理后端服务，与其相关的附属框架都依赖于它
### 2.1 SpringFramwork modules
> **以下为SpringFramework的模块图  **

![image](http://docs.spring.io/spring/docs/4.2.2.BUILD-SNAPSHOT/spring-framework-reference/htmlsingle/images/spring-overview.png)

*  Core Container
*  AOP and Instrumentation
*  Messaging
*  Data Access/Integration
*  Web
*  Test


### 2.2 Core Technologies(Ioc container)
下图为SpringIoc的启动过程:
![image](http://docs.spring.io/spring/docs/4.2.2.BUILD-SNAPSHOT/spring-framework-reference/htmlsingle/images/container-magic.png)

> **NOTE:** 实际上Spring是通过反射来创建bean的


Ioc bean 包含的元素
* （类名）A package-qualified class name: typically the actual implementation class of the bean being defined.
* Bean behavioral configuration elements, which state how the bean should behave in the container (scope, lifecycle callbacks, and so forth). 
* References to other beans that are needed for the bean to do its work; these references are also called collaborators or dependencies.
*  Other configuration settings to set in the newly created object, for example, the number of connections to use in a bean that manages a connection pool, or the size limit of the pool. 

##3 Dependency Injection（依赖注入）
### 3.1 依赖注入
Dependency injection (DI) is a process whereby objects define their dependencies, that is, the other objects they work with, only through constructor arguments, arguments to a factory method, or properties that are set on the object instance after it is constructed or returned from a factory method. 

### 3.2 声明bean并初始化

* 原理说明
```
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">  
</bean>
```

当Spring 容器如载该Bean 时. Spring 将使用默认的构造器来实例化CommonsMultipartResolver Bean。实际上. 会使用如下代码来创建
new org.springframework.web.multipart.commons.CommonsMultipartResolver () ;
#### 3.2.1 通过构造器注入
```
<bean id="exampleBean" class="examples.ExampleBean"/>
<bean name="anotherExample" class="examples.ExampleBeanTwo"/>
```
#### 3.2.2 通过工厂方法创建Bean
```
package com.meizu.cloud.test.spring;
Pulic class Stage {
   private Stage(){
   }
   public static class StageSingletonHoder{
        static Stage instance = new Stage();
   }
   public static Stage getInstance(){
       return StageSingletonHolder.instance;
   }
}

通过工厂方法创建bean的声明配置文件
<bean id ="stage" class="com.meizu.cloud.test.spring"
fatory-method="getInstance">
```


###3.3 Bean属性两种注入方式
#### 3.3.1 Constructor-based dependency injection
```
package x.y;
public class Foo {
    public Foo(Bar bar, Baz baz) {
        // ...
    }
}

xml声明配置
<beans>
    <bean id="foo" class="x.y.Foo">
        <constructor-arg ref="bar"/>
        <constructor-arg ref="baz"/>
    </bean>
    <bean id="bar" class="x.y.Bar"/>
    <bean id="baz" class="x.y.Baz"/>
</beans>
```
[基于构造注入的其他方式-按类型，按名称，按索引](http://docs.spring.io/spring/docs/4.2.2.BUILD-SNAPSHOT/spring-framework-reference/htmlsingle/#beans-factory-collaborators)
#### 3.3.2 Setter-based dependency injection
* 注入简单值
在Spring 中找们可以使用```<property>``` 元素配置Bean 的属性.```<property>```在许多方面都与```<constructor-arg>``` 类似，只不过一个是通过构造参数来注入值，另一个是通过调用属性的setter 方法来注入值.

```

public class ExampleBean {
    private AnotherBean beanOne;
    private YetAnotherBean beanTwo;
    private int i;
    public void setBeanOne(AnotherBean beanOne) {
        this.beanOne = beanOne;
    }
    public void setBeanTwo(YetAnotherBean beanTwo) {
        this.beanTwo = beanTwo;
    }
    public void setIntegerProperty(int i) {
        this.i = i;
    }
}

<bean id="exampleBean" class="examples.ExampleBean">
    <!-- setter injection using the nested ref element -->
    <property name="beanOne">
        <ref bean="anotherExampleBean"/>
    </property>

    <!-- setter injection using the neater ref attribute -->
    <property name="beanTwo" ref="yetAnotherBean"/>
    <property name="integerProperty" value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

> **NOTE:** 一旦ExampleBean被实例化， Spring 就会调用```<property>``` 元章所指定属性的setter 方桂为该属性注入值。

* 注入对象(依赖其他对象)
```
<!-- in the parent context -->
<bean id="accountService" class="com.foo.SimpleAccountService">
    <!-- insert dependencies as required as here -->
</bean>

<!-- in the child (descendant) context -->
<bean id="accountService" <!-- bean name is the same as the parent bean -->
    class="org.springframework.aop.framework.ProxyFactoryBean">
    <property name="target">
        <ref parent="accountService"/> <!-- notice how we refer to the parent bean -->
    </property>
    <!-- insert other configuration and dependencies as required here -->
</bean>
```

* 注入内部类
```
<bean id="outer" class="...">
    <!-- instead of using a reference to a target bean, simply define the target bean inline -->
    <property name="target">
        <bean class="com.example.Person"> <!-- this is the inner bean -->
            <property name="name" value="Fiona Apple"/>
            <property name="age" value="25"/>
        </bean>
    </property>
</bean>
```

* 注入集合对象
```
<bean id="moreComplexObject" class="example.ComplexObject">
    <!-- results in a setAdminEmails(java.util.Properties) call -->
    <property name="adminEmails">
        <props>
            <prop key="administrator">administrator@example.org</prop>
            <prop key="support">support@example.org</prop>
            <prop key="development">development@example.org</prop>
        </props>
    </property>
    <!-- results in a setSomeList(java.util.List) call -->
    <property name="someList">
        <list>
            <value>a list element followed by a reference</value>
            <ref bean="myDataSource" />
        </list>
    </property>
    <!-- results in a setSomeMap(java.util.Map) call -->
    <property name="someMap">
        <map>
            <entry key="an entry" value="just some string"/>
            <entry key ="a ref" value-ref="myDataSource"/>
        </map>
    </property>
    <!-- results in a setSomeSet(java.util.Set) call -->
    <property name="someSet">
        <set>
            <value>just some string</value>
            <ref bean="myDataSource" />
        </set>
    </property>
</bean>
```
### 3.4 最小化xml配置
#### 3.4.1 四种类型的自动装配
* ```ByName``` 把与Bean的属性具有相同名字（或者ID）的其他Bean自动装配到Bean的对应属性中。如果没有跟属性的名字相匹配的Bean，则该属性不进行装配
*  ```ByType``` 把与Bean的属性具有相同类型的其他bean自动装配到对应的属性中
*   ```constructor``` 把与Bean的构造器入参具有相同类型的其他Bean自动装配到构造器的对应入参中
*  ```autodetect``` 首先尝试使用constructor进行自动装配，如果失败在尝试使用byType进行自动装配

#### 3.4.2基于Annotation配置

* 1.使用```@Autowired``` 自动装配
```
public class MovieRecommender {
    //不受限于private关键字，仍然可以实现自动装配
    @Autowired
    private MovieCatalog movieCatalog;
    private CustomerPreferenceDao customerPreferenceDao;

    //构造器自动装配
    @Autowired
    public MovieRecommender(CustomerPreferenceDao customerPreferenceDao) {
        this.customerPreferenceDao = customerPreferenceDao;
    }

    // ...

}
```
* 2 限制歧义性依赖```@Qualifier```
 为了解决spring实现自动装配时自动按照类型寻找bean，出现多个可装配bean的问题
```
public class MovieRecommender {
    @Autowired
    @Qualifier("main") //限定该bean的id为main
    private MovieCatalog movieCatalog;
    // ...

}
```
* 3 在注解注入中使用表达式```@Value```
```
/*** 管理基础路径 ***/
@Value("${adminPath}")
protected String adminPath;
```

#### 3.4.3 为自动检测标注Bean
```
注意：

```
>**NOTE：```<context:component-scan>```  元素会扫描指定的包及其所有的子包，并能找出能够自动注册为Spring Bean 的类**
> The use of``` <context:component-scan>``` implicitly enables the functionality of ```<context:annotation-config>```. There is usually no need to include the ```<context:annotation-config>``` element when using``` <context:component-scan>```.

默认情况下```<context:component-scan>```查找使用构造型（stereotype）注解所标注的类，如下
* ```@Component``` 通用的构造型注解，标识该类为Spring组件
* ```@Controller``` 标识该类定义为Spring MVC controller
*  ```@Repository```标识将该类定义为数据仓库
*  ```@Service``` 标识将该类定义为服务
*  ```@Component``` 标注的任意自定义注解

>**NOTE:** If such an annotation contains no name value or for any other detected component (such as those discovered by custom filters), the default bean name generator returns the uncapitalized non-qualified class name

```
//其显示设置beanId
@Service("myMovieLister")
public class SimpleMovieLister {
    // ...
}

//默认的beanGenerator将其beanId设置为movieFinderImpl 
@Repository
public class MovieFinderImpl implements MovieFinder {
    // ...
}
```
### 3.5 Bean作用域
所有的Spring Bean 默认都是单例。当容器分配一个Bean 时( 不论是通过装配还是调用容器的getBean()方法)，它总是返回Bean的同一个实例
* ```singleton```在每一个Spring容器中，一个Bean定义只有一个对象实例
* ```prototype```允许Bean的定义可以被实例化任意次
* ```request```在一次Http请求中，每个Bean定义对应一个实例，该作用域仅在基于Web的Spring上下文（例如SpringMVC）中才有效
* ```session```在一次Http Session中，每个Bean定义对应一个实例，该作用域仅在基于Web的Spring上下文（例如SpringMVC）中才有效
* ```global session``` 在一个全局Http Session中，每个Bean定义对应一个实例，该作用域仅在Porlet上下文中才有效