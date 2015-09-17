介绍retrofit的使用方法
* [retrofit Getting started and Created and Create and Android Client](https://futurestud.io/blog/retrofit-getting-started-and-android-client/)

# Retrofit Converter
 retrofit使用Gson作为java与json的转换器，因此json转换为java是第一步工作，下面介绍一些json转化JavaBean的实用在线工具
 * [Json2PoJo](https://github.com/joelittlejohn/jsonschema2pojo/wiki/Getting-Started) 
 * 生成Java类的命令，生成的类具有以下规则，无tostring方法，int类型默认使用int，注意对于字符性在json中使用""说明
   Json中最好不要出现中文字符，否则会出现字符集错误的提示
```
jsonschema2pojo --source Status --target java-gen -T JSON -a GSON -p com.meizu.weiboshare.retrofit.model -R -P -pl -S -E

```

# Retrofit Annotation
## Request Method
一般的request请求
* 1 @Path
* 2 @Query
* 3 @QueryMap

## Post Method
* @Body
## Mutipart Post
* @Part

