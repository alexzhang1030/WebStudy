# 目标

* 能够写出常见的jQuery选择器
* 能够操作jQuery样式
* 能够写出常用的jQuery动画
* 能够操作jQuery属性
* 能够操作jQuery元素
* 能够操作jQuery元素尺寸、位置

# 1. jQuery选择器

### 1.1 jQuery基础选择器

原生JS获取元素的方式有很多，很杂，而且兼容性情况不一致，因此jQuery给我们做了封装，使获取元素统一标准。

```javascript
$("选择器") // 里面直接写CSS选择器即可，记得加引号
```

| 名称       | 用法              | 描述                  |
| ---------- | ----------------- | --------------------- |
| ID选择器   | `$("#id")`        | 获取指定ID的元素      |
| 全选选择器 | `$("*")`          | 匹配所有元素          |
| 类选择器   | `$(".class")`     | 获取同一类class的元素 |
| 标签选择器 | `$("div")`        | 获取同一类标签的元素  |
| 并集选择器 | `$("div,p,li")`   | 获取多个元素          |
| 交集选择器 | `$("li.current")` | 交集元素              |

### 1.2 jQuery层级选择器

| 名称       | 用法         | 描述                                       |
| ---------- | ------------ | ------------------------------------------ |
| 子代选择器 | `$("ul>li")` | 使用`>`只获取子级元素，不获取子级的子级    |
| 后代选择器 | `$("ul li")` | 使用`空格`获取所有的子元素，包括子级的子级 |

### 1.3 jQuery筛选选择器

| 语法       | 用法            | 描述                                  |
| ---------- | --------------- | ------------------------------------- |
| :first     | `$("li:first")` | 获取第一个li元素                      |
| :last      | `$("li:last")`  | 获取最后一个li元素                    |
| :eq(index) | `$("li:eq(2)")` | 获取第3个li元素，index是索引号从0开始 |
| :odd       | `$("li:odd")`   | 获取索引为奇数的li元素                |
| :even      | `$("li:even")`  | 获取索引为偶数的li元素                |

### 1.4 隐式迭代

**知识铺垫**

jQuery设置样式

```javascript
$("div").css("属性", "值")
```

遍历内部DOM元素（伪数组储存）的过程叫做`隐式迭代`。简单理解就是内部帮助我们迭代我们匹配的元素，不需要我们手动操作。

### 1.5 jQuery筛选方法（重点）

| 语法                 | 用法                         | 说明                                             |
| -------------------- | ---------------------------- | ------------------------------------------------ |
| `parent()`           | `$("li").parent()`           | 查找父级（返回最近一级）                         |
| `parents(selector)`  | `$("li").parents(".abc")`    | 查找指定的祖先元素                               |
| `children(selector)` | `$("ul").children("li")`     | 相当于`$("ul>li")`，只找子元素                   |
| `find(selector)`     | `$("ul").find("li")`         | 相当于`$("ul li")`，后代选择器                   |
| `siblings(selector)` | `$(".first").siblings("li")` | 查找兄弟节点，不包括本身                         |
| `nextAll([expr])`    | `$(".first").nextAll()`      | 查找当前元素后面的所有兄弟元素                   |
| `prevAll([expr])`    | `$(".last").prevAll()`       | 查找当前元素前面的所有节点                       |
| `hasClass(class)`    | `$("div").hasClass("nav")`   | 检查当前元素是否包含某个特定的类，如果有返回true |
| `eq(index)`          | `$("li").eq(2)`              | 相当于`$("li:eq(2)")`从0开始                     |
### 1.6 jQuery中的排他思想

```javascript
// jQuery隐式迭代让所有的button增加了点击事件
$("button").click(function() {
    // 1. 当前元素变化背景颜色
    $(this).css("background", "pink");
    // 2. 其余的兄弟去掉背景颜色
    $(this).siblings().css("background", "");
});
```

记住`siblings()`

### 1.7 jQuery的链式编程

链式编程可以减少代码量

```javascript
$("button").click(function() {
    $(this).siblings().css("color", "");
    $(this).css("color", "red");
});
```

以上代码是一个非常简单的排他思想案例，我们可以通过使用链式编程来简化代码

```javascript
$("button").click(function() {
    $(this).css("color", "red").siblings().css("color", "");
});
```

使用链式编程时要注意`在操作哪个元素`

# 2. jQuery样式操作

### 2.1 操作css方法

jQuery可以使用css方法来修改简单的元素样式，也可以操作类，修改多个样式。

##### 1. 参数只写属性名，则返回属性值（带单位）

```javascript
$(this).css("color");
```

##### 2. 参数有2个，则第1个是属性名，第2个是修改的属性值

属性名记得加`""`，属性值如果是是数字，不需要加引号，如果不是数字记得加单位，例如`px`、`rem`

```javascript
$(this).css("width", "300px")
$(this).css("height", 300)
```

##### 3. 参数可以是对象形式，方便设置多组样式

属性名和属性值记得用`:`分开，这里的属性名可以省略`""`，如果是复合属性，记得使用驼峰命名法，如果不是数字属性值记得加`""`。

```javascript
$("div").css({
    width: 300,
    height: 400,
    backgroundColor: "red",
});
```

### 2.2 设置类样式方法

作用相当于之前的`classList`，可以操作类样式，注意参数类名不要加点

##### 1. 添加类

```javascript
$("div").addClass("current");
```

##### 2. 删除类

```javascript
$("div").removeClass("current");
```

##### 3. 切换类

```javascript
$("div").toggleClass("current");
```

### 2.3 类操作和className的区别

原生JS中的`className`会覆盖元素的原class

jQuery里面的`类操作`只会对指定类进行操作，不会覆盖原来的类名

# 3. jQuery效果

jQuery给我们封装了很多`动画效果`，最为常见的如下：

| 效果       | 方法                                              |
| ---------- | ------------------------------------------------- |
| 显示隐藏   | `show()`、`hide()`、`toggle()`                    |
| 滑动       | `slideDown()`、`slideUp()`、`slideToggle()`       |
| 淡入淡出   | `fadeIn()`、`fadeOut()`、`fadeToggle`、`fadeTo()` |
| 自定义动画 | `animate()`                                       |

### 3.1 显示隐藏效果

一般情况下，我们不使用参数，直接显示/隐藏

##### 1. 显示语法规范

```javascript
show([speed, [easing], [fn]]);
```

##### 2. 显示参数

* 参数可以都省略，无动画直接显示

* `speed`：速度，有3个值`slow`、`normal`、`fast`或者直接写毫秒数，例如`1000`，表示1000毫秒。

* `easing`：指定切换效果，默认是`"swing"`，可选参数`"linear"`（匀速）
* `fn`：回调函数，在动画完成时执行函数，每个元素执行1次

##### 3. 隐藏语法规范

```javascript
hide([speed, [easing], [fn]]);
```

##### 4. 隐藏参数

* 参数可以都省略，无动画直接显示
* `speed`：速度，有3个值`slow`、`normal`、`fast`或者直接写毫秒数，例如`1000`，表示1000毫秒。
* `easing`：指定切换效果，默认是`"swing"`，可选参数`"linear"`（匀速）
* `fn`：回调函数，在动画完成时执行函数，每个元素执行1次

##### 5.切换语法规范

```javascript
toggle([speed, [easing], [fn]]);
```

##### 6. 切换参数

* 参数可以都省略，无动画直接显示

* `speed`：速度，有3个值`slow`、`normal`、`fast`或者直接写毫秒数，例如`1000`，表示1000毫秒。

* `easing`：指定切换效果，默认是`"swing"`，可选参数`"linear"`（匀速）
* `fn`：回调函数，在动画完成时执行函数，每个元素执行1次

### 3.2 滑动效果

##### 1. 下拉语法规范

```javascript
slideDown([speed, [easing], [fn]]);
```

##### 2. 下拉参数

* 参数可以都省略，无动画直接显示

* `speed`：速度，有3个值`slow`、`normal`、`fast`或者直接写毫秒数，例如`1000`，表示1000毫秒。

* `easing`：指定切换效果，默认是`"swing"`，可选参数`"linear"`（匀速）
* `fn`：回调函数，在动画完成时执行函数，每个元素执行1次

##### 3. 上拉语法规范

```javascript
slideUp([speed, [easing], [fn]]);
```

##### 4. 上拉参数

* 参数可以都省略，无动画直接显示
* `speed`：速度，有3个值`slow`、`normal`、`fast`或者直接写毫秒数，例如`1000`，表示1000毫秒。
* `easing`：指定切换效果，默认是`"swing"`，可选参数`"linear"`（匀速）
* `fn`：回调函数，在动画完成时执行函数，每个元素执行1次

##### 5. 切换语法规范

```javascript
slideToggle([speed, [easing], [fn]]);
```

##### 6. 切换参数

* 参数可以都省略，无动画直接显示

* `speed`：速度，有3个值`slow`、`normal`、`fast`或者直接写毫秒数，例如`1000`，表示1000毫秒。

* `easing`：指定切换效果，默认是`"swing"`，可选参数`"linear"`（匀速）
* `fn`：回调函数，在动画完成时执行函数，每个元素执行1次

### 3.3 事件切换

```javascript
hover([over,]out)
```

* `over`：鼠标移动到元素上需要触发的函数（相当于`mouseenter`）

* `out`：鼠标移出元素要触发的函数（相当于`mouseleave`）

* 如果在参数里面只写一个函数，那么鼠标经过和离开都会触发这个函数

### 3.4 动画队列及其停止排队方法

##### 1. 动画或效果队列

动画或效果一旦触发就会执行，如果多次触发，就会造成多个动画或效果派对执行

##### 2. 停止排队

```javascript
stop()
```

* `stop()`方法用于停止动画或效果
* `stop()`写在动画或效果的前面，相当于结束上一次动画

### 3.5 淡入淡出效果

##### 1. 淡入语法规范

```javascript
fadeIn([speed, [easing], [fn]]);
```

##### 2. 淡入参数

* 参数可以都省略，无动画直接显示

* `speed`：速度，有3个值`slow`、`normal`、`fast`或者直接写毫秒数，例如`1000`，表示1000毫秒。

* `easing`：指定切换效果，默认是`"swing"`，可选参数`"linear"`（匀速）
* `fn`：回调函数，在动画完成时执行函数，每个元素执行1次

##### 3. 淡出语法规范

```javascript
fadeOut([speed, [easing], [fn]]);
```

##### 4. 淡出参数

* 参数可以都省略，无动画直接显示
* `speed`：速度，有3个值`slow`、`normal`、`fast`或者直接写毫秒数，例如`1000`，表示1000毫秒。
* `easing`：指定切换效果，默认是`"swing"`，可选参数`"linear"`（匀速）
* `fn`：回调函数，在动画完成时执行函数，每个元素执行1次

##### 5. 切换语法规范

```javascript
fadeToggle([speed, [easing], [fn]]);
```

##### 6. 切换参数

* 参数可以都省略，无动画直接显示
* `speed`：速度，有3个值`slow`、`normal`、`fast`或者直接写毫秒数，例如`1000`，表示1000毫秒。
* `easing`：指定切换效果，默认是`"swing"`，可选参数`"linear"`（匀速）
* `fn`：回调函数，在动画完成时执行函数，每个元素执行1次

##### 7. 渐进调整到指定不透明度

```javascript
fadeTo([[speed], opacity, [easing], [fn]])
```

##### 8. 渐进参数

* `speed(必填)`：速度，有3个值`slow`、`normal`、`fast`或者直接写毫秒数，例如`1000`，表示1000毫秒。

* `opacity(必填)`：透明度，取值0-1
* `easing`：指定切换效果，默认是`"swing"`，可选参数`"linear"`（匀速）
* `fn`：回调函数，在动画完成时执行函数，每个元素执行1次

### 3.6 自定义动画

##### 1. 语法

```javascript
animate(params, [speed], [easing], [fn])
```

##### 2. 参数

* `params(必填)`：想要更改的样式属性，以对象形式传递，必填。属性名可以不带引号，如果是复合属性需要用到驼峰命名法，其他参数可以省略。
* `speed`：速度，有3个值`slow`、`normal`、`fast`或者直接写毫秒数，例如`1000`，表示1000毫秒。
* `easing`：指定切换效果，默认是`"swing"`，可选参数`"linear"`（匀速）
* `fn`：回调函数，在动画完成时执行函数，每个元素执行1次

# 4. jQuery属性操作

### 4.1 设置或获取元素固有属性值`prop()`

所谓元素的固有属性就是元素中自带的值，例如a标签里面的`href`，input标签里面的`type`

##### 1. 获取属性语法

```javascript
prop("属性")
```

##### 2. 设置属性语法

```javascript
prop("属性", "属性值")
```

### 4.2 设置或获取元素自定义属性值`attr()`

##### 1. 获取属性语法

```javascript
attr("属性")  // 类似原生getAttribute()
```

##### 2. 设置属性语法

```javascript
attr("属性", "属性值")  // 类似原生setAttribute()
```

该方法同样可以获取H5自定义属性值

### 4.3 数据缓存`data()`

`data()`方法可以在指定的元素上存取数据，但不会修改DOM元素结构。一旦页面刷新，之前存放的所有数据都将移除。

##### 1. 获取属性语法

```javascript
data("属性")
```

##### 2. 设置属性语法

```javascript
data("属性", "属性值")
```

该方法获取H5自定义属性的是偶，不需要写`data-`开头的，而且返回值是数字型。

# 5. jQuery内容文本值

主要针对于元素的`内容`和`表单的值`操作。

### 5.1 普通元素内容`html()`

相当于原生`innerHTML`

```javascript
html()       // 获取元素内容
html("内容")  // 设置元素内容
```

### 5.2 元素文本内容`text()`

相当于原生的`innerText`

```javascript
text()       // 获取元素文本内容
text("内容")  // 设置元素文本内容
```

### 5.3 表单值`val()`

相当于原生的`value`

```javascript
val()        // 获取表单值
val("内容")  // 设置表单值
```

### 5.4 购物车修改小计案例

* `parents(selector)`：可以获取到指定的祖先元素
* `toFixed(2)`：最终计算结果保留2位小数

# 6. jQuery元素操作

主要是`遍历`、`创建`、`添加`、`删除`元素操作

### 6.1 遍历元素

jQuery的隐式迭代是对同一类元素做了相同的操作，如果要给同一类元素做不同的操作，就需要用到遍历。

##### 语法1

```javascript
$("div").each(function(index, domEle){ xxx; })
```

* `each()`方法遍历匹配的每一个元素，主要用于DOM处理
* 回调函数有2个参数，`index`：每个元素的索引。`domEle`：迭代的每个元素，这是`DOM对象`，`不是jQuery对象`
* 如果想要变成jQuery对象需要`$(domEle)`

##### 语法2

```javascript
$.each(object, function(index, element){ xxx; })
```

* `$.each()`方法可用于遍历任何对象，主要用于数据处理。
* 里面的参数有2个，`index`是索引号，`element`是遍历内容

### 6.2 创建元素

##### 语法：

```javascript
$("<li></li>")
```

以上代码动态创建了一个`<li>`

### 6.3 添加元素

##### 1. 内部添加

```javascript
element.append("内容")
```

把内容放入匹配元素内部的`最后面`，类似于原生的`appendChild`

```javascript
element.prepend("内容")
```

把内容放入匹配元素内部的`最前面`，类似于原生的`insertBefore`

**注意**：若两者添加的内容相同，`append`和`prepend`不能同时存在，只会生效一个！

##### 2. 外部添加

```javascript
element.after("内容")
```

把内容放在匹配元素外部的`后面`

```javascript
element.before("内容")
```

把内容放在匹配元素外部的`前面`

### 6.4 删除元素

```javascript
element.remove()
```

删除匹配的元素`本身`

```javascript
element.empty()
```

删除匹配的元素集合中的`所有子节点`

```javascript
element.html("")
```

`清空`元素的内容

# 7. jQuery尺寸、位置操作

### 7.1 jQuery尺寸

| 语法                                   | 用法                                              |
| -------------------------------------- | ------------------------------------------------- |
| `width()`/`height()`                   | 获取匹配元素的宽或高，只算width/height            |
| `innerWidth()`/`innerHeight()`         | 获取匹配元素的宽或高，包括padding                 |
| `outerWidth()`/`outerHeight`           | 获取匹配元素的宽或高，包括padding、border         |
| `outerWidth(true)`/`outerHeight(true)` | 获取匹配元素的宽或高，包括padding、border、margin |

### 7.2 jQuery位置

位置主要有3个，`offset()`、`position()`、`scrollTop()`/`scrollLeft()`

##### 1. offset() 设置获取元素偏移

* `offset()`方法设置或返回被选元素相对于`文档`的偏移坐标，和父级没关系
* 该方法有两个属性`top`和`left`。`offset().left`获取左侧的偏移，`offset().top`获取上侧的偏移
* 如果需要修改，参数需要传入一个对象，里面有`top`和`left`

##### 2. position() 获取元素偏移

* `position()`方法用于返回被选元素相对于`带有定位的父级`的偏移坐标，如果父级都没有定位，则以文档为准。
* 该方法只能获取，无法设置

##### 3.scrollTop()/scrollLeft() 获取被卷去的头部/左侧

* `scrollTop()`和`scrollLeft()`分别获取被卷去的头部和左侧
* 可传入参数设置

