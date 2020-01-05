# Flask学习

## 安装pipenv虚拟环境

```
pip Install pipenv
```

### 运行pipenv

```
pipenv --version
```

### 进入虚拟容器

```
pipenv install
```

### 安装flask

```
pipenv install flask
```

### 调用flask

```
form flask import Flask
```

### 声名路由(对比url)

```
@app.route('hello')
```

### 设置默认参数

```
@app.route('bugman',defaults={'name':'bugman')

@app.route('/bug/<name>')
```

## 启动Flask服务器

```
flask run
```

## 实例

#### helloword



```
from flask impor Flask

@app.route('hello')

def index():
	return '<h1>bugman</h1>'
```



#### 变量

```
from flask import Flask
app=Flask(__name__)

@app.route('/bug',defaults={'name':'paga'})
@app.route('/greet/<name>')
def greet(name):
    return '<h1>sss,%s</h1>' % name
```

