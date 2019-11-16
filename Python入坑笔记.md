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

