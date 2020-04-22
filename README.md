# Reptile

[toc]

## 概述

爬虫，应该成为网络爬虫，也叫网页蜘蛛、网络机器人、网络蚂蚁等。

搜索引擎，就是网络爬虫的应用者。

为什么到了今天，反而这个词汇被频繁的提起呢？有搜索引擎不就够了吗？

实际上，大数据时代的到来，所有企业都希望通过海量数据发现其中的价值。

所以，需要爬取对特定网站，特定类别的数据，而搜索引擎不能提供这样的功能，因此，需要自己开发爬虫解决。

### 爬虫分类

#### 通用爬虫

常见就是搜索引擎，无差别的搜集数据、存储，提取关键字，构建索引库，给用户提供搜索接口。

爬取一般流程

1. 初始一批URL，将这些URL放到爬取队列
2. 从队列取出这些URL，通过DNS解析IP，对IP对应的站点下载HTML页面，保存到本地服务器中，爬取完的URL放到已爬取队列。
3. 分析这些网页内容，找出网页里面的其他关心的URL链接，继续执行第2步，知道爬取条件结束。

搜索引擎如何获取一个新网站的URL

- 新网站主动提交给搜索引擎
- 通过其他网站页面中设置的外链
- 搜索引擎和DNS服务商合作，获取最新收录的网站

#### 聚焦爬虫

有针对性的编写特定领域数据的爬取程序，针对某些类别数据采集的爬虫，是面向主题的爬虫

### Robots协议

制定一个 robots.txt 文件，告诉爬虫引擎什么可以爬取。

淘宝[https://www.taobao.com/robots.txt](https://www.taobao.com/robots.txt)

```html
User-agent: Baiduspider
Disallow: /

User-agent: baiduspider
Disallow: /
```

马蜂窝[http://www.mafengwo.cn/robots.txt](http://www.mafengwo.cn/robots.txt)

```html
User-agent: Baiduspider
Disallow: /music/
Disallow: /booking/discount_booking.php
Disallow: /secrect/
Disallow: /rank/
Disallow: /hotel/s.php
Disallow: /order_center/
Disallow: /hotel_zx/order/detail.php
Disallow: /sales/order/
```

```html
User-agent: sogou spider
Disallow: /music/
Disallow: /booking/discount_booking.php
Disallow: /secrect/
Disallow: /rank/
Disallow: /hotel/s.php
Disallow: /order_center/
Disallow: /hotel_zx/order/detail.php
Disallow: /sales/order/
```

```html
User-agent: Googlebot
Disallow: /music/
Disallow: /booking/discount_booking.php
Disallow: /secrect/
Disallow: /rank/
Disallow: /hotel/s.php
Disallow: /order_center/
Disallow: /hotel_zx/order/detail.php
Disallow: /sales/order/

User-agent: Bingbot
Disallow: /music/
Disallow: /booking/discount_booking.php
Disallow: /secrect/
Disallow: /rank/
Disallow: /hotel/s.php
Disallow: /order_center/
Disallow: /hotel_zx/order/detail.php
Disallow: /sales/order/

User-agent: *
Disallow: /
Disallow: /poi/detail.php

Sitemap: http://www.mafengwo.cn/sitemapIndex.xml
```

其他爬虫，不允许爬取

User-agent: *
Disallow: /

这是一个君子协定，非强制性“爬亦有道”

这个协定为了让搜索引擎更有效率搜索自己内容，提供了如Sitemap这样的文件。

这个文件禁止抓取的往往又是可能我们感兴趣的内容，它反而泄露了这些地址。

------

## HTTP请求和响应

其实爬取网页就是通过HTTP协议访问网页，不过通过浏览器访问往往是人的行为，把这种行为变成使用程序来访问。

### urllib 包

urllib是标准库，它是一个工具包模块，包含下面模块来处理url：

- urllib.request 用于打开和读写 url
- urllib.error 包含了由 urllib.request 引起的一场
- urllib.parse 用于解析 url
- urllib。robotparser 分析 robots.txt 文件

Python2中提供了urllib和urllib2.urllib提供较为底层的接口，urllib2对urllib进行了进一步封装。Python3中将urllib合并到了urllib2中，并只提供了标准库urllib包。

#### urllib.request 模块

模块定义了在基本和摘要身份验证、重定向、cookies 等应用中打开Url（主要是HTTP）的函数和类。

**urlopen方法**

```python
urlopen(url, data = None)
```

url 是链接地址字符串，或请求对象。

data 提交的数据，如果 data 位None发起 GET 请求，否则发起 Post 请求。见 `urllib.request.#_get_method`

返回 http.client.HTTPResponse 类的响应对象，这是一个类文件对象。

```python
from urllib.request import urlopen


#打开一个 url 返回一个响应对象，类文件对象
#下面的链接访问后会有跳转
response = urlopen('http://www.bing.com')#GET方法
print(response.closed)#Flase
with response :
    print(1, type(response))#http.client.HTTPResponse类对象
    print(2, response.status,response.reason)#状态
    print(3, response.geturl())#返回真正的URL
    print(4, response.info())# headers
    print(5, response.read())# 读取返回的内容
print(response.closed)#True
```

运行结果部分截图如下：

![image-20200422102323183](/Users/xiahaitao/Library/Application Support/typora-user-images/image-20200422102323183.png)

上例，通过 urllib.request.urlopen 方法，发起一个 HTTP 的 GET 请求，WEB 服务器返回了网页内容。响应的数据被封装到类文件对象中，可以通过 read 方法、readline 方法、 readline 方法获取数据，status 和 reason 属性表示返回的状态码，info 方法返回头信息，等等。

**User-Agent问题**

上例的代码非常精简，即可以获得王赞的响应数据。urlopen 方法只能传递 url 和 data 这样的数据，不能构造 HTTP 的请求，例如 useragent。

源码中构造的 useragent 如下：

```Python
# urllib.request.OpenerDirector
class OpenerDirector:
  def __init__(self):
    client_version = "Python-urllib/%s"%__version__
    self.addheaders = [('User-agent',client_version)]
```

当前显示位`Python-urllib/3.6`

有些网站是反爬虫的，所以要把爬虫伪装成浏览器。随便打开一个浏览器，复制浏览器的 UA 值，用来伪装。

#### Request 类

`Request(url, data=None, headers={})`

初始化方法，构造一个请求对象。可添加一个 header 的字典。data 参数决定是 GET 还是 POST 请求。

`add_header(key, val)`为 header 中增加一个键值对。

```python
from urllib.request import Request, urlopen
import random

#打开一个url返回一个Request请求对象
#url = 'https://movie.douban.com/'# 注意尾部的斜杠一定要有
url = 'http://www.bing.com/'

ua_list = ["Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_4) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.1 Safari/605.1.15","Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.113 Safari/537.36"]
ua = random.choice(ua_list)#pick one
# ua需要加到请求头中
request = Request(url)
request.add_header('User-Ageny',random.choice(ua_list))
print(type(request))

response = urlopen(request,timeout=20)# request对象或者url都可以
print(type(response))

with response:
    print(1,response.status,response.getcode(),response.reason) # 状态，getcode本质上就是返回status
    print(2,response.geturl())# 返回数据的url.如果重定向，这个url和原始url不一样
    # 例如原始url是http://www.bing.com/,返回http://cn.bing.com/
    print(3, response.info()) # 返回响应头headers
    print(4, response.read()) # 读取返回内容

print(5,request.get_header('User-agent'))
print(6, 'user-agent'.capitalize())
```

