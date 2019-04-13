[**Xiuno BBS 4.0.4**](http://bbs.xiuno.com/)

前台存在储存型XSS

1.发帖文章标题储存在xss漏洞

![1555173180826](C:\Users\jxy\AppData\Roaming\Typora\typora-user-images\1555173180826.png)

构造POC

 					

####  							 							<div/onmouseover='alert(1)'> style="x:">							 						

![1555173254121](C:\Users\jxy\AppData\Roaming\Typora\typora-user-images\1555173254121.png)

源码审计

![1555173761716](C:\Users\jxy\AppData\Roaming\Typora\typora-user-images\1555173761716.png)

可以看到

![1555173890942](C:\Users\jxy\AppData\Roaming\Typora\typora-user-images\1555173890942.png)

仅仅判断了参数不为空

![1555174042902](C:\Users\jxy\AppData\Roaming\Typora\typora-user-images\1555174042902.png)

xn_strlen函数仅仅以UTF-8显示

所以导致xss漏洞产生