### Spring Mvc

* [Spring MVC 配置详解](http://www.cnblogs.com/superjt/p/3309255.html)
* [SpringMVC关于json、xml自动转换的原理研究[附带源码分析]](http://www.cnblogs.com/fangjian0423/p/springMVC-xml-json-convert.html)
* [SpringMVC 基于注解的Controller @RequestMapping @RequestParam.. ](http://blog.csdn.net/lufeng20/article/details/7598801)

* 拦截器配置

   由于不知道什么的原因将url地址方法名写错，导致始终返回gsonjson解析错误，由于具体的json其采用的是系统框架，无法拿到具体的json，最后还是通过volley
   将服务器返回的json打印出来才知道方法名写错了，真是无语了

```
05-21 20:40:45.365  30486-30486/io.rong.app I/Rongyunapi﹕ user User{
userId='null', passwd='null', code=200, message='null', result=ApiResult{id='26985', username='comsince', portrait='http://www.gravatar.com/avatar/b7f78b04527a7f4300115718ee0dbd3b?s=82&d=wavatar', status=1, token='null', name='null', introduce='null', number='null', max_number='null', create_user_id='null', creat_datetime='null'}}

```



* ERMaster【数据库建模工具】
  ERMaster时候在选择数据库时一定要制定数据库的类型，因为在导出DDL时，ERMaster是根据数据库类型生成对应的DDL语句，不同数据库类型生成的DDL语句格式是不一样的

