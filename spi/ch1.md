# Lab 0：基础知识

当我们使用浏览器浏览网页之时，本质上是从服务器上拿到一堆“字符”，这些字符由 html/css/js 三者组成。然后经过浏览器解析成漂亮的界面显示出来，这就是我们看到的网页。

而爬虫就是跳过浏览器，不断的访问服务器，将服务器发送过来的“字符”截住，自己解析，提取出有用的信息。然后将这些信息以不同的格式存入磁盘中。

总结一下，以上的流程可以分为三步，即获取数据，提取数据，存储数据。

# 涉及到的库

* 获取数据：requests，urllib 和 selenium 等这些库用于处理 http 请求。

* 提取数据：re 中的正则表达式，Beautifusoup 和 lxml ，解决中文乱码。

* 存储数据：python 的文件操作可以讲提取到的数据存入 txt/csv 文件中，或者存入数据库中。

# 安装库

经过我的不断尝试，我发现这个方法是最便捷的，例如安装 urllib 这个包如下，安装其他包的话可以直接将 urllib 名字修改成包名即可。

```python
pip install urllib http://pypi.douban.com/simple --trusted-host pypi.douban.com
```

# 获取数据

下面提供两个模块 requests/urllib 的简单使用方法。横向比较，个人推荐 requests 模块。

## requests 

首先是导入模块 [prequests](https://requests.readthedocs.io/en/master/) ，然后通过 get 请求拿到一个浏览器对象。

如果没有下载 requests 模块需要先下载。win10 系统上 win + r 然后输入 cmd ，使用如下命令下载。

```python
pip install urllib http://pypi.douban.com/simple --trusted-host pypi.douban.com
```

首先将需要获取页面的 url 存入变量中。

然后使用 get 方法获取网页对象。

最后网页文本。

```python
import requests

url = "http://www.baidu.com"
r = requests.get(url)
print(r.text)
```

结果出现了乱码问题，中文编码出现了问题。

点开百度的网页，按 F12 通过查看页面的 head 部分发现，编码方式为 utf-8 。

<div align="center"> <img src="https://gitee.com/weijiew/pic/raw/master/img/20200617162258.png"/> </div>

而 reauests 通过 text 显示数据的时的编码方式为 unicode 编码，所以改成 utf-8 即可。

```python
import requests

url = "https://www.baidu.com/"
r = requests.get(url)
html = str(r.content,'utf-8')
print(html)
```

这样爬出来的结果没有问题，可以创建一个 html 文件，将输出的字符粘贴进去然后查看页面。

注意 ：r.text返回的是Unicode型的数据。r.content返回的是bytes型也就是二进制的数据。

所以可以通过 `r.content` 方法获取二进制数据，然后修改成 utf-8 的编码方式显示出来。

## urllib 

使用 urllib 模块，这个模块是 python 3 自带的，可以直接使用。

```python
import urllib
url = "https://www.baidu.com/"
r = urllib.request.urlopen(url)
print(r.read())
```

同样结果的中文编码有问题，

```python
import urllib
url = "https://www.baidu.com/"
r = urllib.request.urlopen(url)
print(r.read().decode('utf-8'))
```

设置编码格式后，发现得到的内容和网页中按 F12 得到的内容不一致。

原因是 https，urllib 对 https 的解析出了问题。经过尝试改成 htpp 后该问题得以解决。

注：https 可以简单的理解为是在 http 协议的基础上添加了 ssl 加密协议。实现了由明文传输到密文传输。

```python
import urllib
url = "http://www.baidu.com/"
r = urllib.request.urlopen(url)
print(r.read().decode('utf-8'))
```

这样得到的内容和浏览器访问到的源码完全一致。

## 伪装

访问完 百度后，访问豆瓣试试。

```python
import urllib
url = "http://www.douban.com/"
r = urllib.request.urlopen(url)
print(r.read().decode('utf-8'))
```

修改 url ，运行后发现报错！

查看一下状态码，看一下哪里差错了。

```python
import urllib
url = "http://www.douban.com/"
r = urllib.request.urlopen(url)
print(r.status_code())
```
`urllib.error.HTTPError: HTTP Error 418: ` 服务器返回了一个 418 的数字。

这些数字都有一定的含义，被称为错误状态码，也就是发送错误时返回过来的代码，不同的数字代表不同的错误类型。

例如常见的 404 代表找不到所访问的网页。这里 [418](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/418) 在这里可以理解为浏览器知道你是爬虫，不允许你访问我的页面。

所以需要伪装成浏览器访问，当去服务器中“拿东西”时都会表明身份。使用爬虫访问时加上一部分内容（User-Agent）表示自己是浏览器就行了。

至于 User-Agent 获取的具体方式请搜索。

```python
import urllib

header = {'User-Agent': "Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36"}
url = "http://www.douban.com/"
req = urllib.request.Request(url=url,headers=header)
res = urllib.request.urlopen(req)
print(res.read().decode('utf-8'))
```

然后就没问题了，得到豆瓣的页面源码。下一步是就是从中如何提取有效的信息了。