# Spring Mvc

## 1.J2EE描述部署文件web.xml

* [Spring MVC 配置详解](http://www.cnblogs.com/superjt/p/3309255.html)
* [SpringMVC关于json、xml自动转换的原理研究[附带源码分析]](http://www.cnblogs.com/fangjian0423/p/springMVC-xml-json-convert.html)
* [SpringMVC 基于注解的Controller @RequestMapping @RequestParam.. ](http://blog.csdn.net/lufeng20/article/details/7598801)



* MyBatis
  [SpringMVC与MyBatis集成](http://blog.csdn.net/techbirds_bao/article/details/9233599/)
数据查询面向接口编程



* 拦截器配置

   由于不知道什么的原因将url地址方法名写错，导致始终返回gsonjson解析错误，由于具体的json其采用的是系统框架，无法拿到具体的json，最后还是通过volley
   将服务器返回的json打印出来才知道方法名写错了，真是无语了

```
05-21 20:40:45.365  30486-30486/io.rong.app I/Rongyunapi﹕ 
user User{
userId='null', passwd='null', code=200, message='null', result=ApiResult{id='26985', username='comsince', portrait='http://www.gravatar.com/avatar/b7f78b04527a7f4300115718ee0dbd3b?s=82&d=wavatar', status=1, token='null', name='null', introduce='null', number='null', max_number='null', create_user_id='null', creat_datetime='null'}}

```



* ERMaster【数据库建模工具】
  ERMaster时候在选择数据库时一定要制定数据库的类型，因为在导出DDL时，ERMaster是根据数据库类型生成对应的DDL语句，不同数据库类型生成的DDL语句格式是不一样的


* [springmvc整合redis架构搭建实例](http://www.cnblogs.com/dennisit/p/3614521.html?utm_source=tuicool)



# thinkPhp
## 1 语言说明
## 2 开发工具
## 3 开源项目
    [thinkPhp](http://doc.thinkphp.cn/manual/basic_concept.html)->[OneThink](http://www.onethink.cn/)->[OCenter](http://www.ocenter.cn/index.php?m=book&f=browse&nodeID=1)
     前端路线
    [html](http://www.w3cschool.cn)->[javaScript]()


    [thinkSNS]
     安装错误说明：
环境：   wampserver-2.1a
系统 ：  win8
错误 ：  500 -Invalid command RewriteEngine
日志 ： [Tue Nov 20 22:52:24 2012] [alert] [client 127.0.0.1] D:/wamp/www/.htaccess: Invalid command 'RewriteEngine', perhaps misspelled or defined by a module not included in the server configuration
解决 ： RewriteEngine命令需要rewrite mod的支持，
       打开apache的配置文件httpd.conf ，取消 LoadModule rewrite_module modules/mod_rewrite.so前的注释

     [模块加载机制]
     [插件机制]

[前端技术]
 * javaScript 
      C 中的块级作用域
      自执行匿名函数
 * [深入理解javaScript](http://www.cnblogs.com/TomXu/archive/2011/12/15/2288411.html)

 * [php]()
    thinkphp 使用，thinksns
 * [html]()
   前端模版技术
   前端框架：jsp，php模版引擎

  * 页面渲染与接口分离

      


