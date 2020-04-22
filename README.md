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



