# 1. CSS3

### CSS3 现状

* 在CSS2的基础上新增（扩展）样式
* 移动端支持优于PC端
* 不断改进中
* 应用相对广泛
* IE9以下几乎不支持

# 2. CSS3 选择器

**类选择器、属性选择器、伪类选择器的权重为10**

### 2.1 属性选择器

| 选择符        | 简介                                      |
| :------------ | :---------------------------------------- |
| [E="att"] | 选择具有att属性的E元素                    |
| [att="val"] | 选择具有att属性并且属性值等于val的E元素   |
| [att^="val"] | 匹配具有att属性，且值以val**开头**的E元素 |
| [att$="val"] | 匹配具有att属性，且值以val**结尾**的E元素 |
| [att*="val"] | 匹配具有att属性，且值**包含**val的E元素   |

### 2.2 结构伪类选择器

| 选择符           | 简介                          |
| :--------------- | :---------------------------- |
| E:first-child    | 匹配父元素中的第一个子元素E   |
| E:last-child     | 匹配父元素中的最后一个子元素E |
| E:nth-child(n)   | 匹配父元素中的第n个子元素E    |
| E:first-of-type  | 指定类型E的第一个             |
| E:last-of-type   | 指定类型E的最后一个           |
| E:nth-of-type(n) | 指定类型E的第n个              |

E:nth-child(n) 和E:nth-of-type(n)这里的n有两个特殊值：even和odd

even表示偶数，odd表示奇数

点击这里 ](code/2-CSS3/08-nth-child.html)查看详细的例子。

#### 2.2.1 nth-child(n)和nth-of-type(n)

* n可以是数字、关键字和公式
* n是数字就是选择第几个
* 常见的关键字是even(偶数)、odd(奇数)
* 常见的公式如下（如果n是公式，则从0开始计算）
* 如果公式超出了元素个数则会被忽略

公式示例：

| 公式 | 取值                             |
| :--- | :------------------------------- |
| 2n   | 偶数                             |
| 2n+1 | 奇数                             |
| 5n   | 5  10  15  ...                   |
| n+5  | 从第5个开始（包含第5个）直到最后 |
| -n+5 | 前5个（包含第5个）               |

### 2.3 伪元素选择器

| 选择符   | 简介                     |
| :------- | :----------------------- |
| ::before | 在元素内部的前面插入内容 |
| ::after  | 在元素内部的后面插入内容 |

在伪元素选择器中插入内容需要属性content:"值"

![image-20210219144641476](resource/image/伪类选择器.png)

**注意：**

* before 和 after 必须有 content 属性
* before 在内容的前面，after 在内容的后面
* before 和 after 创建一个元素，但是属于行内元素
* 因为在 dom 中看不到刚才创建的元素，所以我们称之为**伪元素**
* 伪元素和标签选择器一样，权重都是1

#### 2.3.1 引入第三方图标库的注意事项

以iconfont为例，我们在官网下载完之后会有一个demo.html

里面会提供图片的字符实体编码，例如：`&#xe957;`

我们在使用::before或::after的content里面要放十六进制的代码，而字符实体编码中x后面的字符为十六进制，例如`&#xe957;` 在伪元素中该这么写

```css
div::before{
	content:"\e957"
	font-family:"iconfont"
}
```

# 3. CSS3 2D转换

转换（transform）是 CSS3 中具有颠覆性的特征之一，可以实现元素的位移、旋转、缩放等效果。

转换（transform）可以简单理解为变形。

* 移动：translate
* 旋转：rotate
* 缩放：scale

### 3.1 二维坐标系

2D转换是改变标签在二维平面上的位置和形状的一种技术。二维坐标系分为X轴（水平）和Y轴（垂直）

### 3.2 2D 转换之移动 translate

2D移动是2D转换种的一种功能，可以改变元素在页面中的位置，类似于**定位**。

##### 语法：

| 语法                       | 简介                                   |
| :------------------------- | :------------------------------------- |
| transform: translate(x, y) | x代表在x轴移动位置，y代表在y轴移动位置 |
| transform: translateX(n);  | 表示在x轴移动                          |
| transform: translateY(n);  | 表示在y轴移动                          |

**但是translateX和translateY不能同时用。**

具体写法是这样的：

```css
div {
    transform: translate(100px, 100px);
    transform: translateX(200px);
    transform: translateY(200px);
}
```

**一定要记得在后面加上单位px！！！**

##### 重点：

* translate 定义了2D转换中的移动，沿着X轴和Y轴移动元素的位置
* translate 最大的优点：**不会影响其他元素的位置**
* translate 中的百分比单位是相对于自身元素的，例如：`translate(50%,50%)`
* 对行内标签没有效果

解释一下在translate中写百分比的效果，例如50%只是基于本身的宽高的50%进行的数值进行移动的。

### 3.3 2D转换之旋转 rotate

##### 语法：

```css
transform:rorate(度数)
```

##### 重点：

* rotate 里面跟度数，单位是 deg ，比如 rotate(45deg)
* 角度为正时，顺时针；角度为负时，逆时针
* 默认旋转的中心是元素的中心点

### 3.4 2D 转换中心点 transform-origin

我们可以设置元素转换的中心点

##### 语法：

```css
transform-origin: x y;
```

##### 重点：

* 注意后面的参数 x 和 y 要隔开
* x y 默认转换的中心点是元素的中心点（50% 50%）
* 还可以给 x y 设置像素或者方位名词（top bottom lef right center）

### 3.5 2D 转换之缩放 scale

只要给元素添加这个属性就可以控制元素的放大和缩小。

##### 语法：

```css
transform:scale(x,y)
```

##### 注意：

* 注意其中的 x 和 y 要用逗号隔开
* transform:scale(1,1)：宽和高都放大1倍，相当于没有放大
* transform:scale(2,2)：宽和高都放大2倍
* transform:scale(2)：只写一个参数，那么第二个参数就和第一个参数一样。
* transform:scale(0.5,0.5)：缩小
* scale 缩放最大的优势：可以设置转换中心点缩放，默认是元素中心点进行缩放的
* scale 缩放不影响其他元素

### 3.6 2D 转换综合写法

##### 注意：

* 同时使用多个转换，其格式为：`transform: translate() rotate() scale()...`
* 其顺序会影响转换的效果。（例如：先旋转会改变坐标轴方向）
* **当我们同时有位移和其他转换的时候，记得把位移放到最前**

# 4. CSS3 动画

动画（animation）是 CSS3 中具有颠覆性的特征之一。可通过设置多个节点来精确控制一个或一组动画，常用来实现复杂的动画效果。
相比较过渡，动画可以实现更多变化、更多控制、连续自动播放等效果。

### 4.1 动画的基本使用

制作动画分为两步：

1. 先定义动画
2. 再使用（调用）动画

#### 4.1.1 用 keyframes 定义动画（类似定义类选择器）

```css
@keyframes 动画名称{
    0% {
        width: 100px;
    }
    100% {
        width: 200px;
    }
}
```

##### 动画序列：

* 0% 是动画的**开始**，100%是动画的**完成**。这样的规则就是动画序列。
* 在 **@keyframes** 中规定某项 CSS 样式，就能创建由当前样式逐渐变为新样式的动画。
* 动画是使元素从一种样式逐渐变为另一种样式的效果，可以改变任意多的样式任意多的**次数**。
* 用百分比来规定变化发生的时间，或用关键词“from”和“to”，等于 0% 和 100% 。

#### 4.1.2 元素使用动画

```css
div {
    /* 动画名称 */
    animation-name: move;
    /* 动画持续时间 */
    animation-duration: 1s;
}
```

* `animation-name` ：调用动画的名称
* `animation-duration`：动画的持续时间，单位是秒（s）或毫秒（ms）

### 4.2 动画常见的属性

| 属性                      | 描述                                                         |
| :------------------------ | :----------------------------------------------------------- |
| @keyframes                | 规定动画。                                                   |
| animation                 | 所有动画属性的简写属性，除了 animation-play-state 属性。     |
| animation-name            | 规定 @keyframes 动画的名称。                                 |
| animation-duration        | 规定动画完成一个周期所花费的秒或毫秒。默认是 0。             |
| animation-timing-function | 规定动画的速度曲线。默认是 "ease"。                          |
| animation-delay           | 规定动画何时开始。默认是 0。                                 |
| animation-iteration-count | 规定动画被播放的次数。默认是 1。infinite（无限循环）         |
| animation-direction       | 规定动画是否在下一周期逆向地播放。默认是 "normal"。alternate（逆向播放） |
| animation-play-state      | 规定动画是否正在运行或暂停。默认是 "running"。paused（停止） |
| animation-fill-mode       | 规定对象动画结束后的状态。保持当前位置：forwards，回到起始状态：backwards。 |

### 4.3 动画简写属性

animation: 动画名称 持续时间 运动曲线 何时开始 播放次数 是否反方向 动画结束的状态

**请严格按照这个顺序去写，否则效果会错乱**

```css
animation:move 1s linear 2s infinite alternate
```

* 简写属性里面不包含 animation-play-state
* 暂停动画：animation-play-state:puased; 经常配合鼠标经过等一同使用
* 想要动画走回来而不是直接重置位置：animation-direction:alternate;
* 动画结束后，停在原地：animation-fill-mode:forwards;

### 4.4 速度曲线

animation-timing-function：规定动画的速度曲线，默认是“ease”

| 值          | 描述                                         |
| ----------- | -------------------------------------------- |
| linear      | 动画从头到尾的速度都是一样的，匀速           |
| ease        | 默认。动画从低速开始，然后加快，在结束时变慢 |
| ease-in     | 动画从低速开始                               |
| ease-out    | 动画以低速结束                               |
| ease-in-out | 动画以低速开始和结束                         |
| steps()     | 指定了间隔数量（步长）                       |

# 5. CSS3 3D转换

3D 转换常用的是 3D位移 和 3D旋转

**主要知识点：**

* 3D位移：translate3d(x,y,z)
* 3D旋转：rotate3d(x,y,z)
* 透视：perspective
* 3D呈现：transform-style

### 5.1 三维坐标系

三维坐标就是指立体空间，立体空间由3个轴组成。

* X 轴：水平向右。**注意：x 右边是正值，左边是负值**
* Y 轴：垂直向下。**注意：y 下面是正值，上面是负值**
* Z 轴：垂直屏幕。**注意：z 往外是正值，往里是负值**

### 5.2 3D位移 translate3d

##### 语法：

* `transform: translateX(100px)`：仅仅在 X轴 上移动
* `transform: translateY(100px)`：仅仅在 Y轴 上移动
* `transform: translateZ(100px)`：仅仅在 Z轴 上移动（注意：translateZ一般用**px**单位）
* `transform: translate3d(x,y,z)`：在 x y z 轴上移动

### 5.3 透视 perspective

 在2D平面上产生近大远小视觉立体，但是效果只是2维的

* 如果想要在网页产生3D效果需要透视（理解成3D物体投影在2D平面内）
* 模拟人类的视觉位置，可认为安排一只眼睛去看
* 透视，我们也称为视距：视距就是人眼到屏幕的距离
* 透视的值越大，元素越大；透视的值越小，元素越小。
* 透视的单位是像素（px）
* 透视写在被观察元素的**父元素**上面

### 5.4 3D旋转 rotate3d

3D旋转可以让元素在三维沿着X轴、Y轴、Z轴或者自定义轴进行旋转

##### 语法：

* `transform: rotateX(45deg)`：沿着 X轴 正方向旋转45°
* `transform: rotateY(45deg)`：沿着 Y轴 正方向旋转45°
* `transform: rotateZ(45deg)`：沿着 Z轴 正方向旋转45°
* `transform: rotate3d(x,y,z,deg)`：沿着 自定义轴 旋转，deg为角度

对于元素旋转的方向的判断，可以依靠一个左手准则。

* 左手的大拇指指向 X轴/Y轴/Z轴 的正方向
* 其余的手指弯曲的方向就是该元素沿着 X轴/Y轴/Z轴 旋转的方向。

transform: rotate3d(x,y,z,deg)中的 x,y,z 代表旋转轴的矢量。

```css
/* 沿着 x,y相交 轴旋转 */
transform: rotate3d(1, 1, 0, 45deg);
/* 沿着 y,z相交 轴旋转 */
transform: rotate3d(0, 1, 1, 45deg);
```

### 5.5 3D呈现 transform-style

##### 重点：

* 控制子元素是否打开三维立体
* `transform-style: flat;` 子元素**不开启**3d立体，默认是这个属性。
* `transform-style: preserve-3d;` 子元素**开启**立体空间
* 属性给父元素，但影响的是子元素
* 这个属性很重要，后面必用

# 6. H5C3 综合案例

[效果图](code/2-CSS3/36-H5C3综合案例.html)

### 6.1 创建骨架

```html
<section>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
</section>
```

### 6.2 优化样式

```css
body {
    perspective: 1000px;
}

section {
    width: 250px;
    height: 140px;
    margin: 300px auto;
    position: relative;
    transform-style: preserve-3d;
    animation: rotate linear 10s infinite;
    background: url(images/horse.jpg);
}

section:hover {
    /* 鼠标经过暂停动画 */
    animation-play-state: paused;
}

section div {
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    background: url(images/dog.jpg);
}

section div:nth-child(1) {
    transform: translateZ(300px);
}

section div:nth-child(2) {
    transform: rotateY(60deg) translateZ(300px);
}

section div:nth-child(3) {
    transform: rotateY(120deg) translateZ(300px);
}

section div:nth-child(4) {
    transform: rotateY(180deg) translateZ(300px);
}

section div:nth-child(5) {
    transform: rotateY(240deg) translateZ(300px);
}

section div:nth-child(6) {
    transform: rotateY(300deg) translateZ(300px);
}

@keyframes rotate {
    100% {
        transform: rotateY(360deg);
    }
}
```

# 7. 浏览器私有前缀

浏览器私有前缀是为了兼容老版本的写法，比较新版本的浏览器无需添加

### 7.1 私有前缀

* `-moz-`：代表 firefox 浏览器私有属性
* `-ms-`：代表 ie 浏览器私有属性
* `-webkit-`：代表 safari、chrome私有属性
* `-o-`：代表 Opera 私有属性

### 7.2 提倡的写法

```css
-moz-border-redius: 10px;
-webkit-border-redius: 10px;
-o-border-redius: 10px;
border-redius: 10px;
```

可以先写各浏览器的私有前缀，再写一个不带前缀的属性。