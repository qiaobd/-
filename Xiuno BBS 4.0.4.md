[**Xiuno BBS 4.0.4**](http://bbs.xiuno.com/)

前台存在储存型XSS

1.发帖文章标题储存在xss漏洞

[![AOitun.png](https://s2.ax1x.com/2019/04/14/AOitun.png)](https://imgchr.com/i/AOitun)

构造POC

 					

####  							 							<div/onmouseover='alert(1)'> style="x:">							 						

[![AOiUH0.png](https://s2.ax1x.com/2019/04/14/AOiUH0.png)](https://imgchr.com/i/AOiUH0)

源码审计

[![AOiNBq.png](https://s2.ax1x.com/2019/04/14/AOiNBq.png)](https://imgchr.com/i/AOiNBq)

可以看到

[![AOidEV.png](https://s2.ax1x.com/2019/04/14/AOidEV.png)](https://imgchr.com/i/AOidEV)

仅仅判断了参数不为空

[![AOiJjs.png](https://s2.ax1x.com/2019/04/14/AOiJjs.png)](https://imgchr.com/i/AOiJjs)

xn_strlen函数仅仅以UTF-8显示

所以导致xss漏洞产生
