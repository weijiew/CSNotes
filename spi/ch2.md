# Lab 1：爬取小说

很喜欢这个小说，用爬虫练练手，记得初中的时候用手机看，流量太费，就手动的将电脑上的每一章复制下来，贴到记事本上。但是手动模拟太麻烦。爬虫就是将这个过程自动化。

我采用的是 BeatuifulSoup 这个库来爬取的，需要先看 [BeatuifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/) 文档，先看一遍留个印象，具体用法用到的时候在琢磨。

- 首先需要模拟浏览器来访问网站
- 解析网站的内容，获取到章节数目以及链接
- 将章节链接拼接起来，获取每一章的内容
- 存取章节内容

# 详解

首先需要导入所需的库

```python
import requests
import codecs
```

from bs4 import BeautifulSoup

- 第一个是用于模拟浏览器来访问所爬取的网站。
- 第二个是用于操作文件的。
- 第三个则是用于解析爬取到的网页，获取所需内容的。

```python
url = 'https://www.biquge.com.cn/book/7562/'

# 浏览器的请求头，模拟浏览器访问这个网站。
header = {'User-Agent': 'Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.132 Mobile Safari/537.36'}
```

前期准备，设置需要爬取的网站，其次设置浏览器的头部，请求头里面储存了一系列的信息，服务器拿到后根据这些请求头的内容返回对应的信息。

```python
res = requests.get(url, headers=header)
soup = BeautifulSoup(res.text , 'html.parser')

# 核心，检索到需要的内容
maxPages = soup.find('div', id='list').find_all('a')
```

这里我们需要爬取所有章节的内容，一千多章的小说就需要一个循环了，每个章节对应一个页面，首先需要指定章节数，上面的代码就获取所有章节数目的。

```python
k = 1
for i in maxPages:
    print('爬到了第：'+ str(k) +' 章')
    try:
        chapter = 'https://www.biquge.com.cn' + i.get('href')
        resChapters = requests.get(chapter, headers=header)
        soup_chapter = BeautifulSoup(resChapters.text, 'html.parser')
        texts = soup_chapter.find_all('div', id='content')
        # 筛选出文字信息
        content = texts[0].text.replace('\xa0' * 4, '\n')
        path = 'F://zhe_tian'  + '//' + i.string + ".txt"
        with codecs.open(path, 'a', encoding='utf8') as f:
            f.write(content)
            print('第：' + str(k) + '章写入成功！')
    except Exception as e:
        print(e)
    k = k + 1
```

拿到每一章的链接后，需要获得这一章中储存的内容，获取后在存储到对应的文件中。注意写输出，可以看出当前爬取进度。


# 第一个版本

综上代码合并一下就是，但是有些比较混乱，后面有优化。

```python
import requests
import codecs
from bs4 import BeautifulSoup

# 遮天的网址
url = 'https://www.biquge.com.cn/book/7562/'

# 浏览器的请求头，模拟浏览器访问这个网站。
header = {'User-Agent': 'Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.132 Mobile Safari/537.36'}
res = requests.get(url, headers=header)
soup = BeautifulSoup(res.text , 'html.parser')

# 核心，检索到需要的内容
maxPages = soup.find('div', id='list').find_all('a')

# 产看当前进度
k = 1
for i in maxPages:
    print('爬到了第：'+ str(k) +' 章')
    try:
        chapter = 'https://www.biquge.com.cn' + i.get('href')
        resChapters = requests.get(chapter, headers=header)
        soup_chapter = BeautifulSoup(resChapters.text, 'html.parser')
        texts = soup_chapter.find_all('div', id='content')
        # 筛选出文字信息
        content = texts[0].text.replace('\xa0' * 4, '\n')
        path = 'F://zhe_tian'  + '//' + i.string + ".txt"
        with codecs.open(path, 'a', encoding='utf8') as f:
            f.write(content)
            print('第：' + str(k) + '章写入成功！')
    except Exception as e:
        print(e)
    k = k + 1
```


# 第二个版本

将功能写成函数。

- 获取章节链接
- 章节爬取
- 写入文件

```python
import requests
import codecs
from bs4 import BeautifulSoup

# 遮天的网址
url = 'https://www.biquge.com.cn/book/7562/'

# 浏览器的请求头，模拟浏览器访问这个网站。
header = {'User-Agent': 'Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.132 Mobile Safari/537.36'}

res = requests.get(url, headers=header)
soup = BeautifulSoup(res.text , 'html.parser')
maxPages = soup.find('div', id='list').find_all('a')

# 三个参数分别是文件路径，章节内容，编码方式
def writePath(path , content ,code):
    with codecs.open(path , 'a' , encoding=code) as f :
        f.write(content);

# 获取章节内容，第一个参数是章节的链接
def downChapter(chapter):
    resChapters = requests.get(chapter, headers=header)
    soup_chapter = BeautifulSoup(resChapters.text, 'html.parser')
    texts = soup_chapter.find_all('div', id='content')
    content = texts[0].text.replace('\xa0' * 4, '\n')
    return content

def main():
    k = 1
    for i in maxPages:
        print('爬到了第：'+ str(k) +' 章')
        try:
            chapter = 'https://www.biquge.com.cn' + i.get('href')
            content = downChapter(chapter)
            path = 'F://zhe_tian'  + '//' + i.string + ".txt"
            writePath(path,content,'utf8')
            print('第：' + str(k) + '章写入成功！')
        except Exception as e:
            print(e)
        k = k + 1

if __name__ == '__main__':
    main()
```


# 第三个版本
之前的版本就已经可以爬取了。但是如果访问的过于频繁很容易被网站给禁掉，我测试的时候爬取到200章左右就卡住不动了。

这个版本主要引入的一下几个功能。
- 增加 header，防止被禁
- 降低访问频率，防止被禁

```python
import random
import time

import requests
import codecs
from bs4 import BeautifulSoup

zt_headers = [
    "Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36",
    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/35.0.1916.153 Safari/537.36",
    "Mozilla/5.0 (Windows NT 6.1; WOW64; rv:30.0) Gecko/20100101 Firefox/30.0",
    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_2) AppleWebKit/537.75.14 (KHTML, like Gecko) Version/7.0.3 Safari/537.75.14",
    "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; Win64; x64; Trident/6.0)",
    'Mozilla/5.0 (Windows; U; Windows NT 5.1; it; rv:1.8.1.11) Gecko/20071127 Firefox/2.0.0.11',
    'Opera/9.25 (Windows NT 5.1; U; en)',
    'Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.1.4322; .NET CLR 2.0.50727)',
    'Mozilla/5.0 (compatible; Konqueror/3.5; Linux) KHTML/3.5.5 (like Gecko) (Kubuntu)',
    'Mozilla/5.0 (X11; U; Linux i686; en-US; rv:1.8.0.12) Gecko/20070731 Ubuntu/dapper-security Firefox/1.5.0.12',
    'Lynx/2.8.5rel.1 libwww-FM/2.14 SSL-MM/1.4.1 GNUTLS/1.2.9',
    "Mozilla/5.0 (X11; Linux i686) AppleWebKit/535.7 (KHTML, like Gecko) Ubuntu/11.04 Chromium/16.0.912.77 Chrome/16.0.912.77 Safari/535.7",
    "Mozilla/5.0 (X11; Ubuntu; Linux i686; rv:10.0) Gecko/20100101 Firefox/10.0",
    'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.99 Safari/537.36'
]
# 遮天的网址
url = 'https://www.biquge.com.cn/book/7562/'

# 浏览器的请求头，模拟浏览器访问这个网站。
header = {'User-Agent': random.choice(zt_headers)}

res = requests.get(url, headers=header)
soup = BeautifulSoup(res.text , 'html.parser')
maxPages = soup.find('div', id='list').find_all('a')

# 三个参数分别是文件路径，章节内容，编码方式
def writePath(path , content ,code):
    with codecs.open(path , 'a' , encoding=code) as f :
        f.write(content);

# 获取章节内容，第一个参数是章节的链接
def downChapter(chapter):
    resChapters = requests.get(chapter, headers=header)
    soup_chapter = BeautifulSoup(resChapters.text, 'html.parser')
    texts = soup_chapter.find_all('div', id='content')
    content = texts[0].text.replace('\xa0' * 4, '\n')
    return content

def main():
    k = 1
    for i in maxPages:
        # 程序随机暂停 1 - 3 秒
        time.sleep(random.randint(1,3))
        print('爬到了第：'+ str(k) +' 章')
        try:
            chapter = 'https://www.biquge.com.cn' + i.get('href')
            content = downChapter(chapter)
            path = 'F://zhe_tian'  + '//' + i.string + ".txt"
            writePath(path,content,'utf8')
            print('第：' + str(k) + '章写入成功！')
        except Exception as e:
            print(e)
        k = k + 1

if __name__ == '__main__':
    main()
```

这个版本的修改效果很明显，如果没有修改需要一个多小时爬取到二百章，但是加过后花了大概十几分钟就到了二百章，前者看似快但会被服务器针对，欲速则不达。