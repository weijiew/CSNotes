# Lab 3：爬取电影评论 

这次要爬取无间道这部电影的[影评](https://movie.douban.com/subject/1307914/comments?status=P)。

# 无间道影评

以下是一个雏形，大概五百多条。后续会进行优化，打算用 keras 来做个二分类的模型。

```python
import random
import os
import requests
import codecs

from bs4 import BeautifulSoup

headers = [
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
header = {'User-Agent': random.choice(headers)}
url = 'https://movie.douban.com/subject/1307914/comments?start=0&limit=20&sort=new_score&status=P'

# 一共 480 个评论，每一页 20 个评论
for i in range(20 , 481, 20):
    res = requests.get(url ,headers = header)
    urls = 'https://movie.douban.com/subject/1307914/comments?start=' + str(i) + '&limit=20&sort=new_score&status=P'
    soup = BeautifulSoup(res.text , 'html.parser')
    comments = soup.find_all('div',class_ = 'comment-item')
    for i in comments:
        comment = i.find('span' , class_='short')
        print(comment.text)
```
