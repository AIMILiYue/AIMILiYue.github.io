## 原生ajax

### 1. 基本语法

```js
var xml = new XMLHttpRequest()  // 得到xml对象
xml.onreadystatechange = function(){}  // 监听响应
xml.open()  // 建立连接
xml.send()  // 发送请求

// 或者可以在这使用onload监听响应
xml.onload = function(){} 
```



### 2. 基本语法详解

#### 2.1 open()方法

> open()方法的作用是用来建立连接，支持传入三个参数：①请求方式 ②接口地址 ③同步/异步

示例：

```js
xml.open('get','/getusers',false)
```

!> open()的第三个参数：true为异步(默认)，false为同步(不推荐)



#### 2.2 send()方法

> send()方法的作用是用来发送请求，支持传入参数：null/对象

示例：

```js
xml.send(null)
// 或者
xml.sned("name=tom&age=18")  // post传参，需要设置HTTP头，详见3.2章节
```

!> 当请求方式为post并传参时，需要在send()中传参;如为get，则在open接口地址后传参



#### 2.3 onload

> onload为客户端得到服务器端响应后触发的回调函数，可以在该回调函数中处理响应及后续行为

示例：

```js
xml.onload = function(){
    var res = xml.response
    if(res.code === 200){
        window.location.href = './list.html'
    }else{
        alert(res.message)
    }
}
```

!> onload只能监听readyState 等于4时的状态，其他状态值下无法触发onload,若想监听readyState值变化，可以使用onreadystatechange



#### 2.4 onreadystatechange

> onreadystatechange和onload使用方法和作用极其类似，都可监听响应。唯一不同的是，onload无法监听readyState值变化。而每次readyState值变化都会触发onreadystatechange，当 readyState 为 3 时，它也可能调用多次



### 3. 进阶

#### 3.1 关于readyState

?> readyState是ajax的状态值，不同的状态代表着ajax进行到了不同的阶段，共有5个值：0,1,2,3,4

| 状态值 |     名称      |                             描述                             |
| :----: | :-----------: | :----------------------------------------------------------: |
|   0    | Uninitialized | 初始化状态。XMLHttpRequest 对象已创建或已被 abort() 方法重置。 |
|   1    |     Open      | open() 方法已调用，但是 send() 方法未调用。请求还没有被发送。 |
|   2    |     Sent      | Send() 方法已调用，HTTP 请求已发送到 Web 服务器。未接收到响应。 |
|   3    |   Receiving   |      所有响应头部都已经接收到。响应体开始接收但未完成。      |
|   4    |    Loaded     |                   HTTP 响应已经完全接收。                    |

#### 3.2 关于get/post传参

3.2.1 get传参，直接在接口地址后传参，例如：

```js
xml.open('get','getusers?name=tom&age=18')
```

3.2.2 post，需要在send()中传参，并在send()前设置HTTP 头，例如：

```js
xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
xml.send("name=tom&age=18")
```



#### 3.3 关于同步/异步

W3C手册中有这么一句话，如图：

![W3C手册截图](../_media/ajax-w3c.png)

虽然官方说不推荐使用 async=false同步，但是对于一些小型的请求，也是可以的

但会导致一个问题：如果服务器繁忙或缓慢，应用程序会挂起或停止。

原因：JavaScript 会等到服务器响应就绪才继续执行。

所以，不推荐使用同步



!> 当使用 async=false 时，请不要编写 onreadystatechange 函数 ，把代码放到 send() 后面即可

```js
xml.open("GET","/getusers",false)
xml.send()
document.getElementById("box").innerHTML = xml.responseText
```



#### 3.4 关于响应

##### 3.4.1 接收数据

?> 如果想要获取服务器返回的响应，使用responseText 或 responseXML 属性

```js
var xml = new XMLHttpRequest()
xml.onreadystatechange = function(){
    if(xml.readyState == 4 && xml.status==200){
       document.getElementById("box").innerHTML = xml.responseText
    }
}
xml.open('get','/getusers?id=1')
xml.send(null)
```



##### 3.4.2 判断响应

!> 如果使用的是onreadystatechange，请判断readyState和status，否则，将会多次触发该函数导致未知错误





## JQ中的ajax