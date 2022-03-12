---
layout: post
title: 火币Python_API SDK V2使用指南1
categories:
- blog
---

# 1.前言
#### 发生了什么让我要把一个“到处是教程”的量化SDK教程发出来呢？
首先为了搞币圈的量化交易，我们肯定要先去这个原网址https://huobiapi.github.io/docs/spot/v1/cn 看看他们都在干啥。于是我点进去了，嗷，有基于REST的也有基于websocket的，这可难不倒我，毕竟我是金融科班出身，怎么可能不懂这些后端网络通信部分嘛，用python爬虫包装一下就好了！世上无难事只要肯放弃，于是我点开了他封装好的PYTHON SDK （v2）网址：https://github.com/HuobiRDCenter/huobi_Python   
嗷，写的真详细——算了，硬着头皮上吧。翻译他总比拿着postman还要对时间戳和加密的测试接口好。  
网上的信息全是已经停止维护的v1网址。还有，目测只要波动性起来币圈大大的可以赚，那有什么人再发这教程呢？都闷头赚钱去了。

# 2.python sdk结构：
#### 以下源于他的github readme：  
  
1.

| 客户端作用 | 客户端名称        | 私人/公开 | API 协议类型       |
| ------------- | ------------- | ------- | ------------------ |
| 通用       | GenericClient | Public  | Rest               |
| 市场行情        | MarketClient  | Public  | Rest, WebSocket    |
| 账户      | AccountClient | Private | Rest, WebSocket v2 |
| 钱包        | WalletClient  | Private | Rest               |
| 交易        | TradeClient   | Private | Rest, WebSocket v2 |
| 杠杆       | MarginClient  | Private | Rest               |
| ETF           | ETFClient     | Private | Rest      |    

其中属于私人Private的接口/api必须通过私人密钥的传递来获取数据，详细自己看文档吧。
关于协议类型，自然是前文所述分为REST和websocket。  

> REST，即Representational State Transfer的缩写，是目前较为流行的基于HTTP的一种通信机制，每一个URL代表一种资源。交易或资产提币等一次性操作，建议开发者使用REST API进行操作  


> WebSocket是HTML5一种新的协议（Protocol）。它实现了客户端与服务器全双工通信，通过一次简单的握手就可以建立客户端和服务器连接，服务器可以根据业务规则主动推送信息给客户端。市场行情和买卖深度等信息，建议开发者使用WebSocket API进行获取。

其中，每个客户端中应用于其中四种类型，属于一次性连接的是其中带有很明显的get，post开头的REST协议API方法和req开头的websocket方法，带有sub（Subscription）为连续性websocket链接方法。

另外sdk文件描述为：  

- **performance**: 这是内部性能测试，咋用也没人告诉我啊
- **tests**: 这是内部功能测试，咋用也没人告诉我啊
- **example**: ！所有的例子都写在这里了——但没有任何的注释，你只能猜测和靠ide跳转过去
- **huobi**: SDK文件夹，cd到文件目录下运行（pip install .）
  - **connection**: 负责管理远程服务器连接
  - **constant**: 常量配置（但我配了发现没什么用，比如改到.pro为.be(你懂的)）
  - **exception**: 包装的异常选项
  - **model**: 服务器返回的数据模型解释——这难道还要我看吗？不是文档要说清楚噗噗噗！！
  - **service**: 这就是pythonsdk所封装的各类v1，v2的api内容了，要是造轮子抄这里就可以了
  - **utils**: 实用程序类，包括签名、 json 解析器、日志记录等。
  - **client**: 外部接口层，包括上面那个表格所提的所有Client
    - **举个例子你要引用以下的包和初始化**：
```python
from huobi.client.account import * #账户相关
from huobi.client.algo import * #订单相关
from huobi.client.generic import * #基础的一堆东西不知道干啥，比如时间戳
from huobi.client.margin import * #杠杆相关！杠杆毁一生，数字货币除外
from huobi.client.market import * #市场查询
from huobi.client.subuser import * #用户相关
from huobi.client.trade import * #交易相关
from huobi.client.wallet import * #钱包相关
```

### 在引用之前：
1.正确安装该安装的环境什么的我就不说了，下载他的那个火币源码，精确点说是folk它的源码然后拉取，在setup.py那个目录中开命令行，然后
```python
pip install .
```

然后随便开一个他的例子就可以用了，哦对了，那个key部分你需要自己按照第一段的那个文档里去申请，然后输进去——很简单不用教了。

目前遇到的bug，希望大佬解决一下：
https://github.com/HuobiRDCenter/huobi_Python/issues/80 这个issue

下一篇就直接来说必要的几个方法然后搭一个能跑起来的策略吧！