# 目标：

* 能够说出什么是BOM
* 能够知道浏览器的顶级对象window
* 能够写出页面加载事件以及注意事项
* 能够写出两种定时器函数并说出区别
* 能够说出JS执行机制
* 能够使用location对象完成页面之间的跳转
* 能够知道navigator对象涉及的属性
* 能够使用history提供的方法实现页面刷新

# 1. BOM概述

### 1.1 什么是BOM

BOM（Browser Object Model）即浏览器对象模型，它提供了独立于内容而与浏览器窗口进行交互的对象，其核心对象是window

BOM由一系列相关的对象构成，每个对象都提供了很多方法和属性

BOM缺乏标准，JS语法的标准化组织是ECMA，DOM的标准化组织是W3C，BOM最初是Netscape（网景）浏览器标准的一部分。

DOM：

* 文档对象模型
* 把「`文档`」看做一个「`对象`」
* 顶级对象是`document`
* 主要作用是操作页面元素
* W3C规范

BOM：

* 浏览器对象模型
* 把「`浏览器`」看做一个「`对象`」
* 顶级对象是`window`
* 主要作用是浏览器窗口交互
* 规范性较差

### 1.2 BOM的构成

BOM比DOM更大，它包含DOM。

window：

* document
* location
* navigation
* screen
* history

window对象是浏览器的顶级对象，它具有双重角色：

1. 它是JS访问浏览器的`一个接口`
2. 它是`一个全局对象`。定义在全局作用域下的变量、函数都会成为window对象的属性和方法

在调用的时候可以省略window

**注意**：为什么声明变量时最好不要用name，因为window有一个属性就是name 

# 2. window对象的常见事件

### 2.1 窗口加载事件

```javascript
window.onload = function(){}
或者：
window.addEventListener("load",function(){});
```

`load`窗口加载事件，只有当页面加载完毕（包括图片，脚本文件，css）后才会调用执行函数

```javascript
document.addEventListener("DOMContentLoaded",function(){})
```

`DOMContentLoaded`事件触发时，仅是当DOM加载完成，不包括图片，CSS等内容 IE9以上才支持。

如果页面的图片较多，使用`onload`那么就会影响交互准备完毕的时间，此时使用`DOMContentLoaded`更加合适。

### 2.2 调整窗口大小事件

```javascript
window.onresize = function(){}
window.addEventListener("resize",function(){})
```

`resize`是调整窗口大小加载事件，当触发时就会调用函数

**注意**：

1. 只要窗口发生像素变化，就会触发这个事件
2. 我们经常利用这个事件完成响应式布局。`window.innerWidth`：当前屏幕宽度

```javascript
window.addEventListener("resize", function() {
    // 我们让这个div在屏幕小于900px时隐藏起来
    var div = document.querySelector("div");
    // window.innerWidth 屏幕宽度
    if (window.innerWidth < 900) {
        div.style.display = "none";
    } else {
        div.style.display = "block";
    }
});
```

# 3. 定时器

### 3.1 两种定时器

`window`给我们提供了两种定时器方法

* `setTimeout()`
* `setInterval()`

### 3.2 setTimeout() 定时器

```javascript
window.setTimeout(调用函数,[延迟的毫秒数]);
```

`setTimeout()`用于设置一个定时器，该定时器在定时器到期时执行函数

* 在使用时`window`可以省略
* 注意时间是毫秒数，默认是0
* 调用函数可以写匿名函数，还可以写函数名，还有一种写法"函数名()"不提倡
* 页面中可能有很多定时器，我们经常给定时器赋值一个标识符

`setTimeout()`调用的函数我们也称为`回调函数(callback)`。普通的函数是按照代码顺序去调用的，而这个函数，需要`等待时间`，时间到了才会调用函数，因此叫做回调函数。

之前的`element.onclick = function(){}`或者`element.addEventListener("click",fn)`像这种函数其实都是`回调函数`

### 3.3 停止 setTimeout() 定时器

```javascript
window.clearTimeout(timeoutID)
```

`clearTimeout()`方法取消之前通过`setTimeout()`设定的定时器。

* `window`可省略
* 里面的参数是定时器的标识符

### 3.4 setInterval() 定时器

```javascript
window.setInterval(回调函数,[间隔的毫秒数]);
```

`setInterval()`方法重复调用一个函数，每隔一段时间，就调用一次回调函数

* `window`可省略
* `setTimeout`只调用一次回调函数，`setInterval`会重复调用回调函数

### 3.5 停止 setInterval() 定时器

```javascript
window.clearInterval(intervalID)
```

`clearInterval()`方法取消之前`setInterval()	`建立的定时器

* window可以省略

### 3.6 this

`this`的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定`this`到底指向谁，一般情况下`this`的最终指向的是那个调用它的对象

# 4. JS执行机制

### 4.1 JS是单线程

JavaScript语言的一大特点就是单线程，也就是说，同一时间只能做一件事。这是因为JS这门脚本语言诞生的使命所致的——JavaScript是为了处理页面中用户的交互，以及操作DOM而诞生的。比如我们对某个DOM元素进行添加和删除操作，不能同时进行，应该先进行添加，再进行删除。

### 4.2 同步和异步

为了解决等待的问题，利用多核CPU的计算能力，H5提出`Web Worker`标准，允许JS脚本创建多个线程，因此，JS出现了`同步`和`异步`

##### 同步

前一个任务结束后才会执行下一个任务，任务的执行顺序和任务的排列顺序是一致的。

##### 异步

如果一个任务耗费时间较长，可以在执行这个任务的同时，执行下个任务

### 4.3 同步和异步的执行顺序

##### 同步任务

同步任务都在主线程上执行，形成一个`执行栈`

##### 异步任务

JS的异步是通过`回调函数`实现的，一般而言：异步任务有以下三种类型：

1. 普通事件，如：click、resize等
2. 资源加载，如：load、error等
3. 定时器，如：setInterval、setTimeout等

所以在JS中，所有的`回调函数的执行`放入到异步任务中。

### 4.4 JS执行机制

1. 先执行执行栈中的`同步任务`
2. 异步任务（`回调函数`）放入到任务队列中
3. 一旦执行栈中的所有`同步任务`执行完毕，系统就会按次序读取`任务队列`中的异步任务，于是被读取的异步任务结束等待状态，进入`执行栈`开始执行

![JS执行机制](/resource/js_yibu.png)

4. 如果遇到多个异步任务，那么就按照进入任务队列`异步任务`的顺序依次执行

![事件循环](resource/event_loop.jpg)

5. 如果遇到点击事件、鼠标移动事件等等这种情况，那么此时如果鼠标不点击，那么异步队列就是空的状态，鼠标点击，回调函数进行异步队列；鼠标再一点击，回调函数又进入异步队列......像这种情况，由于主程序不断重复的`获取任务`、`执行任务`、`再获取任务`、`再执行任务`这种机制叫做`事件循环`（`event loop`）。

# 5. location对象

### 5.1 什么是location对象

`window`对象给我们提供了一个`location`属性用于`获取或设置窗体的URL`，并且可以`解析URL`。因为这个属性返回的是一个对象，所以我们也把这个属性叫做`location对象`。

### 5.2 URL

统一资源定位符（Uniform Resource Locator,URL）是互联网上标准资源的地址。互联网上的每个文件都有唯一的URL，它包含的信息指出文件的位置以及浏览器如何处理它。

URL的语法格式为：

```
protocol://host[:port]/path/[?query]#fragment
http://www.baidu.com/index.html?wd='123'#link
```

| 组成     | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| protocol | 通信协议 常用的http、ftp、mailto等                           |
| host     | 主机（域名） www.baidu.com                                   |
| port     | 端口号，可选，省略时使用方案的默认端口，如http默认端口80     |
| path     | 路径，由零或者多个/符号隔开的字符串，一般用于表示主机的一个目录或者文件地址 |
| query    | 参数 以键值对的方式通过&符号隔开                             |
| fragment | 片段 #后面的内容 常见于链接锚点                              |

### 5.3 location 对象的属性

| 属性                | 返回值                           |
| ------------------- | -------------------------------- |
| location.`href`     | 获取或者设置整个URL              |
| location.`host`     | 返回主机（域名）                 |
| location.`port`     | 返回端口号，如果未写返回空字符串 |
| location.`pathname` | 返回路径                         |
| location.`search`   | 返回参数                         |
| location.`hash`     | 返回片段 #后面的内容             |

重点记住：`href`和`search`

### 5.4 location 对象的方法

| 方法                 | 返回值                                                       |
| -------------------- | ------------------------------------------------------------ |
| location.`assign()`  | 跟`href`一样，可以跳转页面（也称为重定向页面）               |
| location.`replace()` | 替换当前页面，因为不记录历史，所以不能后退页面               |
| location.`reload()`  | 重新加载页面，相当于刷新按钮或者F5。如果参数是`true`，那么就相当于`ctrl+F5`强制刷新 |

# 6. navigator对象

`navigator`对象包含有关浏览器的信息，它有很多属性，我们最常用的是`userAgent`，该属性可以返回由客户机发送服务器的`user-agent	`的值

下面代码可以判断客户端的类型，返回不同的结果

```javascript
if ((navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i))) {
    window.location.href = "https://m.baidu.com"; //手机
}
```

# 7. history对象

`window`对象给我们提供了一个`history`对象，与`浏览器历史记录`进行交互，该对象包含用户（在浏览器窗口中）访问过的URL。

| 方法                | 作用                                                         |
| ------------------- | ------------------------------------------------------------ |
| history.`back()`    | 后退                                                         |
| history.`forward()` | 前进                                                         |
| history.`go(参数)`  | 前进后退，参数是`1`表示`前进`1个页面，如果是`-1`，那么是`后退`1个页面 |

这个对象不经常使用，但是会在一些`OA`系统上能看到