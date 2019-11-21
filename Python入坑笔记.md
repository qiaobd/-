### Python入坑记
#### 两个for 循环同时输出+正则文章
```
zip(list1,list2)
```
zip函数同时便利两个列表

```
import sys
import requests
import re
from bs4 import BeautifulSoup
hot="https://wiki.17-apt.tk"
a=requests.get(hot).content
b=BeautifulSoup(a)
#d=print(b)

c=re.findall('filePath":(.+?).md',str(b))
d=re.findall('documentURL":(.+?).createdAt',str(b))

for line,urls in zip(c,d):
    print(line,urls)


```

#### Linux基本信息收集

```
***************************
import sys
import os

#c2=os.system("uname -a")
#c3=os.system("pwd")
print('***************************')
print(os.system("whoami"))

#print('系统内核'+c2)
print('***************************')
#print('当前路径'+c3)
print(os.system("uname -a"))
print('***************************')
print(os.system("pwd"))
print('***************************')
print(os.system("ls -l"))
print('***************************')                                                                                                                          
```

![16741210545451.png](https://i.loli.net/2019/11/16/6nXJHmgaqOzVy8M.png)

#### 基于PHPSTUDY l.php的一个MySQL弱口令检测工具

```

import requests
from bs4 import BeautifulSoup
import re
from requests import ReadTimeout

proxies = {
"http":"http://127.0.0.1:8080",

"https":"https://127.0.0.1:8080",
}

data1={
    "host":"localhost",
    "port":"3306",
    "login":"root",
    "password":"abc@123456",
    "act":"MySQL检测",
    "funName":""
}
headers={
    "User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:70.0) Gecko/20100101 Firefox/70.0",
    "Accept-Language":"zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2"
}
a=open('ip.txt',encoding='utf-8')
for ip in a.readlines():
    try:

        url="http://%s/l.php"%ip
        respone=requests.post(url,data=data1,headers=headers,timeout=5).content.decode('utf-8')
        soup=BeautifulSoup(respone)
        t=re.findall('alert\(\'(.+?).\)',str(soup))
        if '连接到MySql数据库正常' in str(t):
            print(ip+"------"+"存在弱口令")
        else:
            print(ip+"------"+"不存在弱口令！！！！")
    except (ConnectionError, ReadTimeout):
        print(ip+"------"+"超时！！！")
    except:
        pass
        
```

#### 查询手机归属地

```

import pandas as pd
import xlwt import *
from phone import Phone
#book=xlwt.Workbook('hhcl.xlsx')
#sheet=book.add_sheet('hhlc')

book = xlwt.Workbook(encoding='utf-8',style_compression=0)
sheet = book.add_sheet('mysheet',cell_overwrite_ok=True)

f1=open(r'12232.txt','r')
f2=open('doct1.csv','wb')
for phoneNUM in f1.readlines():
	phoneNum=phoneNUM.strip("\t\r\n")
	#phoneNUM=str(phoneNUM)
	#print(phoneNum)
	p=Phone()
	info=p.find(phoneNum)
	if info is 	None:
		pass
	else:
		print(info['phone']+"------"+info['province']+"------"+info['city']+"-------"+info["area_code"]+"-------"+info["phone_type"])
		#'cityzip_code','area_code','phone_type'

```
