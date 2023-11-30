---
title: Fidder抓包
date: 2023-11-28 17:21:21
tags: ['测试','抓包',Fidder]
---

### Fidder

Fiddler是位于客户端和服务器端的HTTP代理，也是目前最常用的http抓包工具之一 。 它能够记录客户端和服务器之间的所有 HTTP请求，可以针对特定的HTTP请求，分析请求数据、设置断点、调试web应用、修改请求的数据，甚至可以修改服务器返回的数据<!-- more -->

注意：使用Fiddler的话，需要先设置浏览器的代理地址，才可以抓取到浏览器的数据包。**而很方便的是在你启动该工具后，它就已经自动帮你设置好了浏览器的代理了**，当关闭后，它又将浏览器代理还原了。当然如果发现没有自动设置浏览器代理的话，那就得自己动手去浏览器进行设置代理操作了。（可自行百度每个浏览器是如何设置代理的）

### Fidder设置

####　https设置

首先点击Tools--->Options,之后在出现的页面选择HTTSPS，将其全部勾选。如下图所示。

<img src="https://oss-hl.oss-cn-chengdu.aliyuncs.com/img/image-20231128144508010.png" alt="image-20231128144508010" style="zoom: 67%;" />

之后，浏览器访问http://localhost:8888，点击download the FidderRoot certificate，安装对应证书。之后打开下载的内容，一直next即可。再次进入上图页面，点击Actions，选择reset all certificate，，先删除所有证书，再安装你下载的证书（按照提示来即可）点击yes，点击是，之后ok关闭即可。完成上述步骤就可以进行https请求的抓取了。

####　远程连接设置

首先点击Tools--->Options,然后点击Connections，勾选Allow remote computers to connect即可。

<img src="https://oss-hl.oss-cn-chengdu.aliyuncs.com/img/image-20231128150943183.png" alt="image-20231128150943183" style="zoom: 67%;" />

同时，此界面还可以更改fidder的默认端口8888。

### Fidder请求

1）各图标含义

<img src="https://oss-hl.oss-cn-chengdu.aliyuncs.com/img/image-20231128151748732.png" alt="image-20231128151748732" style="zoom:67%;" />

2）工具栏

![image-20231128152053892](https://oss-hl.oss-cn-chengdu.aliyuncs.com/img/image-20231128152053892.png)

从左到右表示的意义分别是：

- WinConfig：windows 使用了一种叫做“AppContainer”的隔离技术，使得一些流量无法正常捕获，在 fiddler中点击 WinConfig 按钮可以解除这个诅咒，这个与菜单栏 Tools→Win8 Loopback Exemptions 功能是一致的
- 气泡：给请求添加备注说明。选中一条请求，点击气泡，即可添加对应的备注说明
- Replay：对请求进行回放。选择一条请求，点击replay，即再次发送该请求
- X：删除某些请求会话，可下拉选择。常用remove all
- Go：debug断点调试时，继续执行该请求
- Stream：流模式，实时通信模式，有请求就有返回。一般用不到
- Decode：将请求的东西解压出来，方便查阅
- Keep all Sessions：保存所有会话。注意，保存越多，占用内存越大。fidder默认保存所有，可下拉选择更改
- Any Process：过滤请求。将其按住拖到edge浏览器上即可屏蔽该浏览器发出的请求。只能屏蔽对应的进程，应用发出的会话请求
- Find：查找会话，使用黄色标记该会话。
- Save：保存选择的会话。保存为 .saz文件
- 照相机/截图：保存截图，5秒后将截图保存下来
- 计时器：左击开始计时，再左击停止。右击清0
- Browse：快速启动浏览器，可下拉选择
- Clear cache：清除缓存
- TextWizard：编/解码字符串
- Tearoff：将请求和响应单独独立为一个新窗口，方便查看
- MSDN：搜索功能
- ？：fidder在线帮助网站

3）选项卡

![image-20231128154847828](https://oss-hl.oss-cn-chengdu.aliyuncs.com/img/image-20231128154847828.png)

下面将只介绍一些常用的选项卡。

####　Inspectors 

![image-20231128155446293](https://oss-hl.oss-cn-chengdu.aliyuncs.com/img/image-20231128155446293.png)

![image-20231128155505246](https://oss-hl.oss-cn-chengdu.aliyuncs.com/img/image-20231128155505246.png)

包含请求和响应的一些信息。主要是对这些信息的多种格式展示。主要查看请求头，响应头重要信息。

#### Filters

<img src="https://oss-hl.oss-cn-chengdu.aliyuncs.com/img/image-20231128155952719.png" alt="image-20231128155952719" style="zoom:67%;" />

设置完成后，点击Actions执行定义的过滤规则。

1）Hosts过滤

```text
No Zone Filter：不设置过滤；指定只显示内网（Intranet）或互联网（Internet）的内容(不常用)
Show only Intranet Hosts：指定只显示内网（Intranet）的内容
Show only Internet Hosts：指定只显示互联网（Internet）的内容
```

```
No Host Filter：不过滤
Hide the following Hosts ： 隐藏文本框中的相关主机请求
Show only the following Hosts ：显示文本框中相关的主机请求（多个用分号分开）
Flag the following Hosts ：标记（高亮）显示文本框中的主机请求
```

2）Client Process过滤

```
Show only traffic from：你可以指定只捕获哪个Windows进程中的请求；
Show only Internet Explorer traffic：只显示IE发出的请求；
Hide traffic from Service Host：隐藏来自Host发出的请求；
```

3）RequestHeader过滤

```
show only if URL contains: 只显示URL包含的，多个时空格分开
Hide if URL contains: 隐藏URL包含的，多个时空格分开
Flag requests with headers: 加粗显示HTTP请求头包含指定的HTTP请求头的类型名称
Delete request headers: 删除HTTP请求头包含指定的HTTP请求头的类型名称
Set request header : 创建一个指定名称和值的HTTP请求头,或更新HTTP请求头为指定值。
Break request on POST: POST请求设置断点
Break request on GET with query string: GET方法且URL中包含查询条件的请求设置断点（URL中包含参数params）
Break on XMLHttpRequest: 通过 XMLHttpRequest对象发送的请求设置断点。通过查找请求头中是否含有X-Request-With 和X-Download-Initiator
Break response on Content-Type: 响应头Content-Type 中包含了指定的文本设置断点
```

5）Response Status Code过滤

```
Hide success(2XX): 隐藏状态码在200至299的响应
Hide non-2xx: 隐藏非200至299的响应
Hide Authentication demands(401,407): 隐藏状态码为401,407的响应.需要用户进一步确认证书的请求
Hide redirects(300,301,302,303,307): 藏状态码为300,301,302,303,307重定向的响应
Hide Not Modified(304):藏状态码为304的响应.缓存实体有效返回304
```

6）Response Type and Size过滤

7）Response Headers过滤

```
Flag responses that set cookies: 粗体显示和响应头包含Set-Cookie的响应
Flag responses with headers: 粗体显示指定HTTP响应头。同Flag requests with headers
Delete responses headers: 删除特定的HTTP响应头。只是从响应头中删除，不删除session
Set response header；创建更新响应头。同Set Request header 用法一样
```

#### Composer

Composer选项卡支持手动构建和发请求；也可以在session列表中拖拽Session放到Composer中，会直接把该session的请求复制到用户界面；

<img src="https://oss-hl.oss-cn-chengdu.aliyuncs.com/img/image-20231128163002061.png" alt="image-20231128163002061" style="zoom:67%;" />

上面填写对应的请求路径，请求头信息等。下面填写请求体中对应的内容。即请求的参数

```
	Options标签：
Inspect Session: 执行请求后，会自动打开Inspectors选项卡；
Fix Content-Length header:控制Composer是否会自动添加或修改Content-Length请求头，表示请求体的大小；
Follow Redirects:是否会自动使用响应的Location头；
Automatically Authenticate: 是否会自动响应服务器的认证需求；
Tear off 按钮：使Composer选项卡以独立的窗口悬浮；
```

### Fidder应用

#### 错误排查

根据对应的请求头，响应头信息，判断是前端错误还是后端错误。若请求有问题，则前端错误，若响应的数据有问题，则后端服务错误，若响应数据没问题，则前端渲染错误。（同时还要结合数据库数据进行判断）

#### Mock测试---查看页面布局

**断点改数据：**

![image-20231128170848085](https://oss-hl.oss-cn-chengdu.aliyuncs.com/img/image-20231128170848085.png)

点击第一下是请求前断点，可以修改对应的请求数据；点击第二下是响应后断点，可以修改对应的响应数据，点击第三下是取消断点。断点设置之后点击Run to completion执行断点后的操作，观察结果。

**改数据库：**优点，可以同时测多种环境下页面显示问题，如web页面，app页面，h5等。缺点：信息千人前面，每个人的推荐信息是不同的。

**mock测试：**先save对应的响应体数据，然后修改保存。后点击AutoResponder选项卡，拖动对应请求到其中，而后选择find a file替换响应结果，而后再次发送该请求去查看结果即可。如下图所示

<img src="https://oss-hl.oss-cn-chengdu.aliyuncs.com/img/image-20231128165733545.png" alt="image-20231128165733545" style="zoom:67%;" />

#### 弱网测试

Rules->Performance -> Simulate Modem Speeds

<img src="https://oss-hl.oss-cn-chengdu.aliyuncs.com/img/image-20231128172259066.png" alt="image-20231128172259066" style="zoom:67%;" />

同时，可以点击Rules—>Cutomize Rules，即

<img src="https://oss-hl.oss-cn-chengdu.aliyuncs.com/img/image-20231128172415152.png" alt="image-20231128172415152" style="zoom:67%;" />

进去弱网设置代码里，找到m_SimulateModem，修改oSession["request-trickle-delay"] = "300";（表示上传1kb需要300ms）和oSession["response-trickle-delay"] = "150";，自定义弱网的网络条件。

#### app抓包

首先打开fidder的远程连接，见上。其次，进行手机上的设置，保证手机网络和fidder所在电脑网络处于同一局域网下。然后设置手机的代理网络为fidder的服务地址。

此外，手机上也需要下载安装fidder安全证书。访问http://电脑IP:8888，一路下载安装，按照提示干就完了。（不要干坏事哇）



