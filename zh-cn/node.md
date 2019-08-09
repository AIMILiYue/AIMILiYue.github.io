## 1. 前言

### 1.1 node是用来做什么的

个人理解，node可以用来搭建微服务器，为需要者提供接口，类似于我们传统的后端开发（php/java等），也可单独运行一个小型项目。

### 1.2 node中的内置模块

既然node属于类后端开发，要想搭建一个服务器或者提供数据接口，那肯定有很多的依赖需求，例如：读写文件、处理路径、url地址处理等，需要依赖一些模块才可完成.

那node中存在哪些模块呢？

?> 请前往第二章节开始学习。



## 2. 开始学习

### 2.1 准备工作

在学习之前，要确保系统中安装了node，可以去官网下载安装

下载地址：[node官方下载地址](http://nodejs.cn/download/ )

?> 安装完成后，可以再cmd命令行中执行node -v，如果有版本号，证明安装成功；如果没有，请确保node安装目录已经添加到计算机path环境变量中



### 2.2 node中常见内置模块

常见内置模块有：

> Web模块---http模块：用来创建Web服务器

> 文件系统模块---fs模块：进行文件操作

> URL模块：处理浏览器端请求（GET/POST等）

> 工具模块：OS模块、Path模块、Net模块、DNS模块、Domain模块

以上为常用模块，当然，内置模块远不止这些，如有需要，可以参考[Node.js官方API文档](http://nodejs.cn/api/ )

接下来，将要学习列出的模块。



### 2.3 HTTP模块

#### 2.3.1 使用http模块搭建服务器

``` js
// 1.导入模块
const http = require('http')

// 2.创建服务器
const server = http.createServer()

// 3. 设置端口号，启动服务
server.listen(5678,function(){   
    console.log('node服务已启动，请在浏览器输入http://127.0.0.1:5678访问')
})

// 4. 监听浏览器端请求
server.on('request', function (req, res) {
  	console.log(req)
})
```

#### 2.3.2 http模块解读 

在监听浏览器端请求的回调函数中，`req代表请求`，`res代表响应`。

如果有关于对请求的处理（判断请求方式、处理请求参数、获取请求url等），一定要使用req，否则，使用res

例如：如果请求地址为：<http://127.0.0.1:5678/index?id=1> 

```js
const http = require('http')
const server = http.createServer()
server.listen(5678,function(){   
    console.log('node服务已启动，请在浏览器输入http://127.0.0.1:5678访问')
})
server.on('request', function (req, res) {
  	// req.url  ---  /index?id=1 获取的是端口号之后的url
    // req.method  ---  GET
    // req.headers --- 请求头信息
})
```

#### 2.3.3 如何接收get请求的参数 

在http模块中，想要接收get请求的参数，需要借助`url模块`对`req.url`进行处理

```js
const http = require('http')
const url = require('url')   //  引入url模块
const server = http.createServer()
server.listen(5678,function(){   
    console.log('node服务已启动，请在浏览器输入http://127.0.0.1:5678访问')
})
server.on('request', function (req, res) {
    if(req.method == 'GET'){
    	let params = url.parse(req.url,true)
        console.log(params) 
        /* 结果: 
        Url {
 			 protocol: null,
              slashes: null,
              auth: null,
              host: null,
              port: null,
              hostname: null,
              hash: null,
              search: '?id=1',
              query: { id: '1' },
              pathname: '/index',
              path: '/index?id=1',
              href: '/index?id=1' 
        } */
    }
})
```

?> 解读：url.parse()支持两个参数：`url`：需要处理的url；`true/false`:true可以将url中的参数处理为对象形式，false则处理为字符串形式

接着，我们就可以通过`params.query.id`和`params.pathname`来获取参数和不带参数的地址了

!> 注：以上使用`url.parse()`为旧版本遗留的 API ，除此之外，还可使用 WHATWG标准的新 API ，详见下文2.4章节

#### 2.3.4 如何接收POST请求参数

```js
const http = require('http')
const server = http.createServer()
server.listen(5678,function(){   
    console.log('node服务已启动，请在浏览器输入http://127.0.0.1:5678访问')
})
server.on('request', function (req, res) {
    if(req.method == 'POST'){
    	var str = ''
        req.on('data',(chunk)=>{
            str += chunk
        })
        req.on('end',()=>{
            console.log(str)
            // 结果：  id=1
        })
    }
})
```



### 2.4 URL模块

#### 2.4.1 遗留API

在URL模块中，之前经常使用的处理url的API为`url.parse()`,下面看详解。

```js
const http = require('http')
const url = require('url')   //  引入url模块
const server = http.createServer()
server.listen(5678,function(){   
    console.log('node服务已启动，请在浏览器输入http://127.0.0.1:5678访问')
})
server.on('request', function (req, res) {
    if(req.method == 'GET'){
    	let params = url.parse(req.url,true)
        console.log(params) 
        /* 结果: 
        Url {
 			 protocol: null,
              slashes: null,
              auth: null,
              host: null,
              port: null,
              hostname: null,
              hash: null,
              search: '?id=1',
              query: { id: '1' },
              pathname: '/index',
              path: '/index?id=1',
              href: '/index?id=1' 
        } */
    }
})
```

上面代码中，使用了`url.parse()`来处理了get请求的url，目的是为了获得get中的参数。

?> `url.parse()`需要的参数： ① 需要处理的url； ②true/false

上述代码得到的结果中可以看出，结果为一个URL对象，该对象中包含很多属性。

?> 如果`url.parse()`的第二个参数为true，则会把URL对象中的`query`属性转为对象的形式；如果参数为false，则转为字符串处理



#### 2.4.2 最新使用方法

!> 遗留的 API 还没有被废弃，保留是为了兼容已存在的应用程序。 新的应用程序应使用 WHATWG 的 API。 

##### 2.1.2.1 基本使用

```js
const { URL } = require('url')
let url = 'https://www.example.org/a/b/c?id=1&name=tom&age=18'
const myUrl = new URL(url)
```

##### 2.4.2.2 使用详解

我们使用 `new URL()`来解析和处理url，并得到一个`myUrl`对象。

在实例化(new)时，需要传入两个参数：①要解析的url（必须） ②要解析的基本url【可选】

!> 实例化URL时传入的url须为一个完成的url，如果不完整，则使用第二个参数。

例如：

```js
const { URL } = require('url')
const myUrl = new URL('/foo', 'https://example.org/')
// 结果：https://example.org/foo
```

?> myUrl对象中包含多个属性，如：myUrl.hash、myUrl.host、myUrl.hostname、myUrl.href、myUrl.origin、myUrl.pathname、myUrl.port、myUrl.search、myUrl.searchParams等

+ **myUrl.hash**

  获取及设置URL的分段(hash)部分。 

  ```js
  const { URL } = require('url')
  const myUrl = new URL('https://example.org/foo#bar')
  console.log(myUrl.hash)  // 输出 #bar
  
  myUrl.hash = 'baz'
  console.log(myUrl.href)  // 输出 https://example.org/foo#baz
  ```

  



+ **myUrl.host**

  获取及设置URL的主机(host)部分。

  ```js
  const { URL } = require('url')
  const myUrl = new URL('https://example.org:81/foo')
  console.log(myUrl.host) // 输出 example.org:81
  
  myUrl.host = 'example.com:82'
  console.log(myUrl.href)  // 输出 https://example.com:82/foo
  ```

+ **myUrl.href**

  获取及设置序列化的URL 

  ```js
  const { URL } = require('url')
  const myUrl = new URL('https://example.org/foo')
  console.log(myUrl.href)  // 输出 https://example.org/foo
  
  myUrl.href = 'https://example.com/bar'
  console.log(myUrl.href)   // 输出 https://example.com/bar
  ```

+ **myUrl.origin**

  获取只读序列化的URL origin部分 

  ```js
  const { URL } = require('url')
  const myUrl = new URL('https://example.org/foo/bar?baz')
  console.log(myUrl.origin)   // 输出 https://example.org
  ```

  `特殊情况：`

  ```js
  const { URL } = require('url')
  const idnURL = new URL('https://你好你好')
  console.log(idnURL.origin)  // 输出 https://xn--6qqa088eba
  ```

  



