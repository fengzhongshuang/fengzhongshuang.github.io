---
title:  "Python爬虫实例之美空Model"
summary:  爬虫实例
categories: [Python]
tags: [Sample]
---

昨日不知抽了什么邪风，就想用 Python 写个爬虫，原本是想爬芥末堆网的，然而，最终还是选择了美女如云的美空网，呵呵，你懂得。。。所以，今天就记录下这拙劣的案例吧。

虽然以前也写过弱智的爬虫，但是真的是特别弱智，所以有很多问题并不能详细解释，甚至都无法确认这样写是否合理，无论如何先写出来吧，以后回头看的时候再做补充，到时也可以默默的对自己说上句SB了。。。

实例中主要使用了 **pyquery、requests、urllib、json、os** 这样几个库，使用 requests 库请求数据，pyquery 库做筛选，urllib 获取图片信息，json 将数据转成 json 格式以便后续存文件，os 负责写文件，创建目录。然后就没有什么了，直接上代码了。

```Python
#! /usr/bin/env python
# -*- coding:utf-8 -*-

from pyquery import PyQuery as pq
import requests
import json
import urllib
import os

user_agent = r'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_8_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/28.0.1500.95 Safari/537.36'

models = dict()

def process_list():
    raw_url = r'http://www.moko.cc/channels/post/23/'
    for i in range(1, 21):
        url = raw_url + str(i) + '.html'
        ret = requests.get(url=url, headers={'user-agent': user_agent, 'referer': url})
        query = pq(ret.text)
        items = query('ul.small-post')
        for item in items:
            cover = pq(item).find('div.cover').attr('cover-text')
            img = pq(item).find('a img').attr('src2')
            detail = 'http://www.moko.cc' + pq(item).find('a').attr('href')
            # print img
            # save_photo(cover, img.strip('/'))
            #     save_brief(name, json.dumps(mm))
            process_detail(detail)

def process_detail(url):
    ret = requests.get(url=url, headers={'user-agent': user_agent, 'referer': url})
    query = pq(ret.text)
    name = query('#workNickName').text()
    logo = query('#imgUserLogo').attr('src')
    if not os.path.exists(name):
        os.mkdir(name)
    save_photo(name + '/' + name, logo)
    if name not in models:
        models[name] = logo
        items = query('p.picBox')
        for i in range(items.size()):
            item = items[i]
            img = pq(item).find('img').attr('src2')
            title = pq(item).find('img').attr('title')
            save_photo(name + '/' + name + str(i+1), img)

def save_photo(file_name, url):
    data = urllib.urlopen(url)
    fp = open(file_name + '.jpg', 'wb')
    fp.write(data.read())
    fp.close()

def save_brief(file_name, contents):
    fp = open(file_name + '.txt', 'w+')
    fp.write(contents)
    fp.close()

if __name__ == '__main__':
    process_list()
```

过程中主要遇到三个问题：
* 第一个是 **user-agent** 的设置，主要是为了告诉网站的服务器，这次请求是来自于浏览器的，如果不设置可能会无法获取数据。
* 第二个就是 **Referer** 的设置，这是用来绕过防盗链设置的，没有这个header的属性的话，就无法获取实质的内容了，因此这两个设置灰常的重要。
* 第三个问题是 **字符编码** 的问题，使用 requests 发送请求时基本不需要处理字符编码的问题，将获取的数据传给 pyquery 即可，如果直接使用 pyquery 来请求数据的话就存在字符编码的问题。

至于 [requests](http://www.python-requests.org/en/master/)、[pyquery](http://pyquery.readthedocs.org) 库的使用，可以参考官方文档。实例代码可以从[这里](https://github.com/fengzhongshuang/python_demos/blob/master/spider-meikong.py)获取。
