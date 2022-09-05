# DOCTYPE

`Doctype`是HTML5的**文档声明**，通过它可以告诉浏览器，**使用哪一个HTML版本标准解析文档**。在浏览器发展的过程中，HTML出现过很多版本，不同的版本之间格式书写上略有差异。如果没有事先告诉浏览器，那么浏览器就不知道**文档解析标准**是什么？此时，大部分浏览器将开启最大兼容模式来解析网页，我们一般称为`怪异模式`，这不仅会降低解析效率，而且会在解析过程中产生一些难以预知的`bug`，所以文档声明是必须的。

# Quirks怪癖模式和Standards标准模式

页面如果写了DTD，就意味着这个页面采用对CSS支持更好的布局，而如果没有，则采用兼容之前的布局方式，这就是Quirks模式，有时候也叫怪癖模式、诡异模式、怪异模式。

区别：总体会有布局、样式解析、脚本执行三个方面区别，这里列举一些比较常见的区别：

- `盒模型`：在W3C标准中，如果设置一个元素的宽度和高度，指的是元素内容的宽度和高度，然而在Quirks模式下，IE的宽度和高度还包含了padding和border
- `设置行内元素的高宽`：在Standards模式下，给行内元素设置width和height都不会生效，而在Quriks模式下会生效
- `用margin：0 auto设置水平居中`:在Standards模式下，设置margin：0 auto；可以使元素水平居中，但是在Quriks模式下失效
- `设置百分比高度`:在Standards模式下，元素的高度是由包含的内容决定的，如果父元素没有设置百分比的高度，子元素设置百分比的高度是无效的

# 严格模式

严格模式的概念，是从`ECMAScript5`引入的，通过严格模式，可以在函数内部选择进行较为严格的全局或局部的**错误条件检测**。

使用严格模式的好处**是可以提早知道代码中存在的错误**，及时捕获一些可能导致编程错误的 `ECMAScript` 行为。

## 使用

1、如果是在**全局作用域**中（函数外部）给出`"use strict"`，则整个代码都将使用严格模式。（如果把带有`"use strict"`的代码放到其他文件的全局中，则该文件中的 `JavaScript` 代码也将处于严格模式下。）写在<script>顶部

2、也可以只在**函数**中打开严格模式，就像下面这样：

```csharp
function xiaozhima(){ 
  "use strict";
  //其他代码
}
```

## 变量

在严格模式下，什么时候创建变量以及怎么创建变量都是有限制的。

- **不允许意外创建全局变量**
- **不允许delete删除变量**
- 对变量名也有限制

不能使用 `implements`、`interface`、`let`、`package`、`private`、`protected`、`public`、`static` 和 `yield` 等作为变量名。

## 对象

1、在下列情形下操作对象的属性会导致错误：

- 为只读属性赋值会抛出 `TypeError`；
- 对不可配置的（nonconfigurable）的属性使用 `delete` 操作符会抛出 `TypeError`；
- 为不可扩展的（nonextensible）的对象添加属性会抛出 `TypeError`。

2、在使用对象字面量时，属性名必须唯一。例如：

```csharp
//重名属性
//非严格模式：没有错误，以第二个属性为准
//严格模式：抛出语法错误
var person = {
  name: "Nicholas", 
  name: "Greg"
};
```

## 函数

1、**命名函数的参数必须唯一**

2、函数体中的`arguments`

- 在非严格模式下，修改参数也会改变`arguments[0]`的值，
- 但在严格模式下，`arguments[0]`的值仍然是传入的值。

与变量类似，严格模式对函数名也做出了限制，不允许用 `implements`、`interface`、`let`、`package`、`private`、`protected`、`public`、`static` 和 `yield`等作为函数名。

3、`if` 语句中声明函数会导致语法错误

## eval()

饱受诟病的 `eval()`函数在严格模式下也得到了提升。最大的变化就是它在包含上下文中不再创建变量或函数。

## eval 与 arguments

严格模式已经明确禁止使用 `eval` 和 `arguments` 作为标识符，也不允许读写它们的值。

## this

- 在非严格模式下使用函数的 `apply()`或 `call()`方法时，`null` 或 `undefined` 值会被转换为全局对象。
- 在严格模式下，函数的 `this` 值始终是指定的值，无论指定的是什么值。例如：

```csharp
//访问属性
//非严格模式：访问全局属性
//严格模式：抛出错误，因为 this 的值为 null 

var color = "red";
function displayColor(){ 
  alert(this.color);
}

displayColor.call(null);
```

以上代码向`displayColor.call()`中传入了`null`，

如果在是非严格模式下，这意味着函数的`this`值是全局对象。结果就是弹出对话框显示`"red"`。

而在严格模式下，这个函数的 `this` 的值是 `null`，因此在访问 `null` 的属性时就会抛出错误。

## 其他

1、抛弃了 `with` 语句

2、去掉了 `JavaScript` 中的八进制字面量

# html语义化

1. **概念**：

   HTML5的语义化指的是**合理正确的使用语义化的标签来创建页面结构**。【正确的标签做正确的事】

2. **语义化标签**：

   header nav main article section aside footer

3. **语义化的优点**:

   - 在`没CSS样式的情况下，页面整体也会呈现很好的结构效果`
   - `代码结构清晰`，易于阅读，
   - `利于开发和维护` 方便其他设备解析（如屏幕阅读器）根据语义渲染网页。
   - `有利于搜索引擎优化（SEO）`，搜索引擎爬虫会根据不同的标签来赋予不同的权重

# meta标签

meta标签一般放在整个`html`页面的`head`部分，在`MDN`中对他这样定义：

> meta是**文档级元数据元素**，用来表示那些不能由其它 HTML 元相关元素（`<base>`、`<link>`, `<script>`、`<style>`或 `<title>`）之一表示的任何元数据。

说白了就是为了**传达信息**。

先看看`meta` 元素定义的元数据的类型：

- 如果设置了 `name`属性，`meta` 元素提供的是文档级别的**元数据**，应用于整个页面。
- 如果设置了 `http-equiv`属性，`meta` 元素则是**编译指令**，提供的信息与**类似命名的 HTTP 头部相同**。
- 如果设置了 `charset`属性，`meta` 元素是一个字符集声明，告诉文档使用哪种字符编码。
- 如果设置了 `itemprop` 属性，`meta` 元素提供用户定义的元数据。

**SEO搜索引擎优化**

```html
<!-- 关键词，填写网页关键词，优化SEO的重要标签 -->
<meta name="keywords" content="请输入网页关键字，例如：程序员;写代码;高薪;加班严重"/>
<!-- 声明优先使用的浏览器，例如下面是优先使用的是edge和chrome -->
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"/>
<!-- 网页概述，优化SEO的重要标签 -->
<meta name="description" content="请输入网页概述，例如：知识社区，前端技术"/>
```

- 自动刷新
- 跳转页面
- 定义语言

## name属性

`name`和`content`一起使用，前者表示要表示的元数据的`名称`，后者是元数据的`值`。

**author**

用来表示网页的作者的名字，例如某个组织或者机构。

```html
<meta name="author" content="aaa@mail.abc.com">
```

**description**

是一段简短而精确的、对页面内容的**描述**。以头条和taobao的`description`标签为例：

![img](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/49f4385dc1ed48feaa3fc5e0b46acf83~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp?)

![img](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/825ce35f4abd4c06afa40e66013579bf~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp?)

**keywords**

与页面内容相关的关键词，使用逗号分隔。某些搜索引擎在遇到这些关键字时，会用这些关键字对文档进行分类。 还是以头条和taobao为例

![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0e7392738a9648d18a959b3cdb63cef5~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp?)

![img](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/78198feb62944b84a2b464b9bec5df6c~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp?)

**viewpoint**

为 viewport（视口）的初始大小提供指示。目前仅用于移动设备。

可能你也发现了，我们在`vscode`中自动生成`html`的代码片段时，会自动生成：

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

`width`用来设置 viewport 的宽度为设备宽度;

`initial-scale`为设备宽度与 viewport 大小之间的缩放比例。

![img](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1be03506cff042e89f244a225a6bcb05~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp?)

**robots**

表示爬虫对此页面的处理行为，或者说，应当遵守的规则，是用来做搜索引擎抓取的。

它的`content`可以为：

1. `all`:搜索引擎将索引此网页，并继续通过此网页的链接索引文件将被检索
2. `none`:搜索引擎讲忽略此网页
3. `index`:搜索引擎索引此网页
4. `follow`:搜索引擎继续通过此网页的链接索引搜索其它的网页

**renderer**

用来指定双核浏览器的渲染方式，比如360浏览器，我们可以通过这个设置来指定360浏览器的渲染方式

```html
<meta name="renderer" content="webkit"> //默认webkit内核
<meta name="renderer" content="ie-comp"> //默认IE兼容模式
<meta name="renderer" content="ie-stand"> //默认IE标准模式
```

## http-equiv

`http-equiv`也是和`content`一起使用，前者表示要表示的元数据的`名称`，后者是元数据的`值`。

`http-equiv` 所有允许的值都是特定 HTTP 头部的名称，

**X-UA-Compatible**

我们最常见的`http-equiv`值可能就是`X-UA-Compatible`了，它常常长这个样子：

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0a51d78fca0e470d871cdad5aac6d331~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp?)

**指定浏览器渲染方式**

`IE=edge`告诉浏览器，以当前浏览器支持的最新版本来渲染，IE9就以IE9版本来渲染。

`chrome=1`告诉浏览器，如果当前IE浏览器安装了`Google Chrome Frame`插件，就以chrome内核来渲染页面。

像上图这种两者都存在的情况：如果有chrome插件，就以chrome内核渲染，如果没有，就以当前浏览器支持的最高版本渲染。

另外，这个属性支持的范围是`IE8-IE11`

你可能注意到了，如果在我们的`http`头部中也设置了这个属性，并且和`meta`中设置的有冲突，那么哪一个优先呢？ 答案是：开发者偏好（`meta`元素）优先于Web服务器设置（HTTP头）。

**content-type**

用来声明**文档类型和字符集**

![img](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e79c0e0e369749cc862755af01bfd01b~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp?)

**x-dns-prefetch-control**

一般来说，HTML页面中的a标签会自动启用DNS提前解析来提升网站性能，但是在使用https协议的网站中失效了，我们可以设置：

![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/29822f4066704d098e91860a98d2cae2~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp?)

来打开dns对a标签的提前解析

**Refresh**

设置自动刷新或者跳转新页面，其中`content`第一个数字代表 5 秒后自动刷新：

```xml
<meta http-equiv="Refresh" content="5;URL=www.baidu.com" />
```

**Set-Cookie**

设置 Cookie ，如果网页过期，这个网页的cookies的也会被删除：

```xml
<meta http-equiv="Set-Cookie" content="cookie value=xxx;expires=2020 05:28:00 GMT+0800 (CST) path=/" />
```

**Content-Language**

设置页面的语言的：

```xml
<meta http-equiv="Content-Language" content="zh-cn" />
```

**cache-control、Pragma、Expires**

和缓存相关的设置，但是遗憾的是这些往往不生效，我们一般都通过`http headers`来设置缓存策略

# head 标签里有什么？

每一个 HTML 文档中，都有一个不可或缺的标签：`<head>` ，它作为一个容器，主要包含了用于描述 HTML 文档自身信息（元数据）的标签，这些标签一般不会在页面中被显示出来，大多情况下是给浏览器和搜索引擎看的。

**可以用在 `<head>` 里面的标签有： `<title>` , `<base>` , `<link>` , `<style>` , `<meta>` , `<script>` , `<noscript>` 。**

#### `<base>`

给页面上所有相对 URL 的提供一个基础。一份文档中只能有一个 `<base>` 标签。

目前我只观察到「淘宝网」使用了这个标签。

#### `<link>`

规定外部资源与当前文档的关系，常于链接样式表，如下所示：

```xml
<link rel="stylesheet" href="xxx.css" type="text/css">
```

当然还有很多其他的作用：

1. 比如用于 SEO，主要给搜索引擎看的：

```xml
<link rel="canonical" href="...">
```

在网站中常有多个 url 指向同一个页面的情况，上述标签告知搜索引擎页面的主 url 是什么，以便搜索引擎保留主要页面而去除其他重复页面。

1. 提供 rss 订阅的：

```xml
<link rel="alternate" type="application/rss+xml" title="RSS" href="...">
```

上述标签除搜索引擎可以看懂以外，也能被很多浏览器插件识别。

1. 表示页面 icon 的：

```xml
<link rel="icon" href="https://xxx.png">
```

多数浏览器会读取这个 link 的资源并展示在页面上。

1. 对页面提供预处理的：

```xml
<link rel="dns-prefetch" href="//xxx.com">
```

提前对一个域名做 dns 查询。强制对域名进行预读取在有的情况下很有用,。

> 比如, 在网站的主页上，强制在整个网站上对频繁引用的域名做预解析处理，即使它们不在主页本身上使用。虽然主页的性能可能不受影响，但是会提高站点整体性能。

# title 与 h1 的区别、b 与 strong 的区别、i 与 em 的区别？

- title 属性表示网页的标题，h1 元素则表示层次明确的页面内容标题，对页面信息的抓取也有很大的影响
- strong 是标明重点内容，有语气加强的含义，使用阅读设备阅读网络时：strong会重读，而b是展示强调内容
- i 是italic(斜体)的简写，是早期的斜体元素，表示内容展示为斜体，而 em 是emphasize（强调）的简写，表示强调的文本



# 前端页面三层构成

构成：`结构层`、`表示层`、`行为层`

1. 结构层（structural layer）

   结构层类似于盖房子需要打地基以及房子的悬梁框架，它是由**HTML超文本标记语言**来创建的，也就是页面中的各种标签，在结构层中保存了用户可以看到的所有内容，比如说：一段文字、一张图片、一段视频等等

2. 表示层（presentation layer）

   表示层是由**CSS**负责创建，它的作用是如何显示有关内容，学名：`层叠样式表`，也就相当于装修房子，看你要什么风格的，田园的、中式的、地中海的，总之CSS都能办妥

3. 行为层（behaviorlayer）

   行为层表示网页内容跟用户之间产生交互性，简单来说就是用户操作了网页，网页给用户一个反馈，这是**JavaScript**和`DOM`主宰的领域

# iframe的作用以及优缺点

iframe也称作**嵌入式框架**，嵌入式框架和框架网页类似，它可以把一个网页的框架和内容嵌入到现有的网页中。

优点：

- 可以用来处理加载缓慢的内容，比如：广告

缺点：

- iframe会**阻塞主页面的Onload事件**
- iframe和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载。但是可以通过JS动态给ifame添加src属性值来解决这个问题，当然也可以解决iframe会阻塞主页面的Onload事件的问题
- 会产生很多页面，不易管理
- 浏览器的**后退按钮没有作用**
- 无法被一些搜索引擎识别

#  img上 title 与 alt

- alt：全称`alternate`，切换的意思，如果无法显示图像，浏览器将显示alt指定的内容
- title：当鼠标移动到元素上时显示title的内容

区别：

一般当鼠标滑动到元素身上的时候显示`title`，而`alt`是img标签特有的属性，是图片内容的等价描述，用于图片无法加载时显示，这样用户还能看到关于丢失了什么东西的一些信息，相对来说比较友好。

# 对于Web标准以及W3C的理解

`Web标准`简单来说可以分为结构、表现、行为。其中结构是由HTML各种标签组成，简单来说就是body里面写入标签是为了页面的结构。表现指的是CSS层叠样式表，通过CSS可以让我们的页面结构标签更具美感。行为指的是页面和用户具有一定的交互，这部分主要由JS组成

`W3C`，全称：world wide web consortium是一个制定各种标准的非盈利性组织，也叫万维网联盟，标准包括HTML、CSS、ECMAScript等等，web标准的制定有很多好处，比如说：

- 可以统一开发流程，统一使用标准化开发工具（VSCode、WebStorm、Sublime），方便多人协作
- 学习成本降低，只需要学习标准就行，否则就要学习各个浏览器厂商标准
- 跨平台，方便迁移都不同设备
- 降低代码维护成本

- 

# 什么是微格式？在前端构建中应该考虑微格式吗？

所谓的微格式是建立在已有的、被广泛采用的标准基础之上的一组简单的、开放的数据格式。

具体表现是把语义嵌入到HTML中，以便有助于分离式开发，并通过制定一些简单的约定，来兼顾HTML文档的人机可读性，相当于对web网页进行语义注解。

采用微格式的web页面，在HTML文档中给一些标签增加一些属性，这些属性对信息的语义结构进行注解，有助于处理HTML文档的软件，更好的理解HTML文档。当爬取web内容时，能够更为准确地识别内容块的语义，微格式可以对网站进行SEO优化。

# HTML5为什么只需要写`<!DOCTYPE html>`?

为什么HTML5只需要写一段：

```html
<!DOCTYPE html>
```

而HTML4却需要写很长的一段

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```

其实主要是因为HTML5不基于SGML，所以不需要引用DTD。在HTML4中，`<!DOCTYPE>`声明引用DTD，因为HTML4基于SGML。DTD规定了标记语言的规则，这样浏览器才能正确的呈现内容。

# ⚝HTML5新特性

HTML5主要是关于图像、位置、存储、多任务等功能的增加：

- **语义化标签**，如：article、footer、header、nav等
- **视频video、音频audio**
- **画布**canvas
- 表单控件，calemdar、date、time、email
- 地理
- 本地离线存储**webStorage**，localStorage长期存储数据，浏览器关闭后数据不丢失，sessionStorage的数据在浏览器关闭后自动删除
- 拖拽释放

移除的元素：

- 纯表现的元素：`basefont、font、s、strike、tt、u、big、center`
- 对可选用性产生负面影响的元素：`frame、frameset、noframes`

# HTML5：音频和视频

## Audio 音频

HTML `<audio>` 元素用于在文档中嵌入音频内容，可以包含一个或多个音频资源。有两种添加音频资源的方式：

- `src` 属性，添加了嵌入媒体的路径，比如 `<audio src="audio.mp3">`

- <source>元素，指定多个媒体资源，用于以不同浏览器支持的多种格式提供相同的媒体内容：

  ```html
  <audio src="source/audio.mp3" autoplay controls loop preload></audio>
  
  <!-- 或者用如下方式：  -->
  
  <audio  autoplay controls loop preload>
      <source src="source/audio.mp3" type="">
      <source src="source/audio02.wav" type="">
  </audio>
  ```

对应属性：
1、autoplay 自动播放
2、controls 显示播放器
3、loop 循环播放
4、preload 预加载
5、muted 静音

source标签的作用是提供多个媒体文件地址，如果一个地址的文件不兼容，就使用下一个地址。

## Video 视频

HTML `<video>` 元素用于在文档中嵌入媒体播放器，用于支持文档内的视频播放。同样支持多个视频源（由多个 `<source>` 元素提供），浏览器将会使用它所支持的第一个源。

```html
<video>
  <source src="myVideo.mp4" type="video/mp4" />
  <source src="myVideo.webm" type="video/webm" />
  <!-- 针对浏览器不支持此元素时候的降级处理。👇 -->
  <p>Your browser doesn't support HTML5 video.</p>
</video>
```

### 与 audio 相同的属性

- `autoplay`自动播放
- `controls`显示播放器
- `loop`循环播放
- `muted`静音
- `currentTime`
- `duration` 只读
- `crossorigin`
- `preload`
- `src`

### 其他的属性

- `height` / `width`，设置视频播放器的高度 / 宽度。
- `buffered`，这个属性可以读取到哪段时间范围内的媒体被缓存了。
- `playsinline`，布尔属性，表示在元素的播放区域内。请注意，没有此属性并不意味着视频始终是全屏播放的。
- `played`，指明了视频已经播放的所有范围。
- `poster`，指定了视频正在下载时显示的图像，直到用户点击播放按钮。



# ⚝CSS3新特性

## 新增选择器

详见“选择器部分”

## 新样式

#### 边框

`css3`新增了三个边框属性，分别是：

- border-radius：创建圆角边框
- box-shadow：为元素添加阴影
- border-image：使用图片来绘制边框

#### 背景

新增了几个关于背景的属性，分别是

- background-clip：确定背景画区
- background-origin：对齐设置
- background-size：调整背景图片的大小
- background-break：元素可以被分成几个独立的盒子

#### 文字

- word-wrap： normal：使用浏览器默认的换行，break-all：允许在单词内换行
- text-overflow： clip：修剪文本； ellipsis：显示省略符号来代表被修剪的文本
- text-shadow ：文本应用阴影
- text-decoration： 文字的更深层次的渲染 text-fill-color: 设置文字内部填充颜色；text-stroke-color: 设置文字边界填充颜色；text-stroke-width: 设置文字边界宽度

#### 颜色

> css3`新增了新的颜色表示方式`rgba`与`hsla

- rgba分为两部分，rgb为颜色值，a为透明度
- hala分为四部分，h为色相，s为饱和度，l为亮度，a为透明度

## transition 过渡

·`transition`属性可以被指定为一个或多个`CSS`属性的过渡效果，多个属性之间用逗号进行分隔，必须规定两项内容：

- 过度效果
- 持续时间

**详见下面**

## transform 转换

`transform`属性允许你旋转，缩放，倾斜或平移给定元素

```
transform-origin`：转换元素的位置（围绕那个点进行转换），默认值为`(x,y,z):(50%,50%,0)
```

使用方式：

- transform: translate(120px, 50%)：位移
- transform: scale(2, 0.5)：缩放
- transform: rotate(0.5turn)：旋转
- transform: skew(30deg, 20deg)：倾斜

**详见下面**

## animation 动画

**详见下面**

## 渐变

颜色渐变是指在两个颜色之间平稳的过渡，`css3`渐变包括

- linear-gradient：线性渐变

> background-image: linear-gradient(direction, color-stop1, color-stop2, ...);

- radial-gradient：径向渐变

> linear-gradient(0deg, red, green);

## flex弹性布局

**详见下面**

## 媒体查询

**详见下面**

# @import和link引入样式的区别

### 1.从属关系区别

`@import`是 CSS 提供的语法规则，**只有导入样式表**的作用；`link`是HTML提供的标签，不仅可以加载 CSS 文件，还可以定义 RSS、rel 连接属性等。rel为relationship表示为两者的关系

> ​    link（链接式）语法为：
>
> ​    <link rel="stylesheet" href="style.css" type="text/css"/>
>
> ​    @import（导入式）语法为：
>
> ​        <style type="text/css">
>
> ​    @import url("style.css");
>
> ​    </style>

### 2.加载顺序区别

加载页面时，`link`标签引入的 CSS 被同时加载；`@import`引入的 CSS 将在页面加载完毕后被加载。

### 3.兼容性区别

`@import`是 CSS2.1 才有的语法，故只可在 IE5+ 才能识别；`link`标签作为 HTML 元素，不存在兼容性问题。

### 4.DOM可控性区别

可以通过 JS 操作 DOM ，插入`link`标签来改变样式；由于DOM方法是基于文档的，无法使用`@import`的方式插入样式。

 **link 标签的加载机制是由浏览器决定的，不同的浏览器可能有不同的表现**。**link标签引入css资源时在火狐浏览器中是异步加载的，在谷歌浏览器中是同步加载的。**

link是**异步**操作 遇到link不会开辟新的HTTP线程去获取资源 GUI继续渲染页面；(这句话并不正确，要想link为异步，应该用以下设置)

```html
<link rel="preload" href="mystyles.css" as="style" onload="this.rel='stylesheet'">
```

@import 是**同步**的 GUI渲染页面的时候遇到@import会等它获取新的样式回来后继续渲染

# src和href的区别

href是Hypertext Reference的简写，表示**超文本引用**，指向**网络资源所在位置**。

常见场景:

```javascript
<a href="http://www.baidu.com"></a> 
<link type="text/css" rel="stylesheet" href="common.css">
```

src是source的简写，目的是要把文件下载到html页面中去。

常见场景:

```javascript
<img src="img/girl.jpg"> 
<iframe src="top.html"> 
<script src="show.js">
```

### 作用结果

1. href 用于在当前文档和引用资源之间确立联系
2. src 用于替换当前内容

### 浏览器解析方式

1. 当浏览器**遇到href会并行下载资源并且不会停止对当前文档的处理**。(同时也是为什么建议使用 link 方式加载 CSS，而不是使用 @import 方式)
2. 当浏览器解析到src ，**会暂停其他资源的下载和处理，直到将该资源加载或执行完毕**。(这也是script标签为什么放在底部而不是头部的原因)



# 块级元素和内联元素

这道题就是会问你块级元素有哪些和内联元素有哪些及特点，以及之间怎么相互转换

## 块级元素

常见块级元素

```html
<div>、<p>、<h1>-<h6>、<ol>、<ul>、<dl>、<table>、<address>、<blockquote> 、<form>
```

- 块级元素会**独自占据一行**，可以**声明宽和高**
- display:block可以把元素转换为块级元素

## 行内元素

常见行内元素

```html
<a>、<span>、<br>、<i>、<em>、<strong>、<label>、<q>、<var>、<cite>、<code>
```

- 行内元素会出现在一行，不会起新行。**不能直接声明宽高**，如果要声明宽高需要把元素转换为行内或者行内块元素。
- display:inline把元素转换为行内元素

## 行内块元素

常见行内块元素

```html
<img>、<input>
```

- **不自动换行，可以声明宽高**
- display: inline-block将元素转换为行内块元素

# 选择器及优先级

### 基本选择器

| 选择器  | 含义           | 作用                                         | CSS  |
| ------- | -------------- | -------------------------------------------- | ---- |
| .class  | 类选择器       | 匹配 class 包含(不是等于)特定类的元素        | 1    |
| #id     | id选择器       | 匹配特定 id 的元素                           | 1    |
| *       | 通用元素选择器 | 匹配页面任何元素（这也就决定了我们很少使用） | 2    |
| element | 元素选择器     | 选择HTML元素                                 | 1    |

### 组合选择器

| 选择器 | 含义                         | 作用                                                     | CSS  |
| ------ | ---------------------------- | -------------------------------------------------------- | ---- |
| E,F    | 并集选择器                   | 同时匹配元素E或元素F                                     | 1    |
| EF     | 交集选择器                   | 相交的部分为所选择的标签                                 | 1    |
| E F    | 后代选择器                   | 匹配E元素所有的后代（不只是子元素、子元素向下递归）元素F | 1    |
| E>F    | 子元素选择器                 | 匹配E元素的所有直接子元素                                | 2    |
| E+F    | 直接相邻选择器               | 匹配E元素之后的相邻的同级元素F                           | 2    |
| E~F    | 普通相邻选择器（弟弟选择器） | 匹配E元素之后的同级元素F（无论直接相邻与否）             | 3    |

### 属性选择器

| 选择器                | 示例            | 示例说明                                    | CSS  |
| --------------------- | --------------- | ------------------------------------------- | :--: |
| [attribute]           | [target]        | 选择所有带有target属性元素                  |  2   |
| [attribute=value]     | [target=-blank] | 选择所有使用target="-blank"的元素           |  2   |
| [attribute~=value]    | [title~=flower] | 选择标题属性**包含**单词"flower"的所有元素  |  2   |
| [attribute^=language] | [lang ^= en]    | 选择一个lang属性的**起始值**="EN"的所有元素 |  3   |
| [attribute$=language] | [lang $= en]    | 选择一个lang属性的**结尾值**="EN"的所有元素 |  3   |
| [attribute*=language] | [lang *= en]    | 选择一个lang属性的**包含**"EN"的所有元素    |  3   |

### CSS3新增伪类选择器

### 伪类选择器

| 选择器                 | 示例             | 示例说明                                    | CSS  |
| ---------------------- | ---------------- | ------------------------------------------- | ---- |
| :link                  | a:link           | 选择所有未访问链接                          | 1    |
| :visited               | a:visited        | 选择所有访问过的链接                        | 1    |
| **:hover**             | a:hover          | 把鼠标放在链接上的状态                      | 1    |
| :active                | a:active         | 选择正在活动链接                            | 1    |
| :focus                 | input:focus      | 选择元素输入后具有焦点                      | 2    |
| **:first-child**       | p:first-child    | 选择每个p元素是其父级的**第一个子级**。     | 2    |
| **:first-of-type**     | p:first-of-type  | 选择每个p元素是其父级的**第一个p元素**      | 3    |
| **:last-child**        | p:last-child     | 选择每个p元素是其父级的**最后一个子级**。   | 3    |
| **:last-of-type**      | p:last-of-type   | 选择每个p元素是其父级的**最后一个p元素**    | 3    |
| **:only-child**        | p:only-child     | 选择每个p元素是其父级的**唯一子元素**       | 3    |
| **:only-of-type**      | p:only-of-type   | 选择每个p元素是其父级的**唯一p元素**        | 3    |
| **:nth-child(an+b)**   | p:nth-child(2)   | 选择每个p元素是其父级的第二个子元素         | 3    |
| **:nth-of-type(an+b)** | p:nth-of-type(2) | 选择每个p元素是其父级的第二个p元素          | 3    |
| :root                  | :root            | 选择文档的根元素                            | 3    |
| :empty                 | p:empty          | 选择每个没有任何子级的p元素（包括文本节点） | 3    |
| :enabled               | input:enabled    | 选择每一个已启用的输入元素                  | 3    |
| :disabled              | input:disabled   | 选择每一个禁用的输入元素                    | 3    |
| :chenked               | input:checked    | 选择每个选中的输入元素                      | 3    |
| **:not(selector)**     | :not(p)          | 选择每个并非p元素的元素                     | 3    |
| :out-of-range          | :out-of-range    | 匹配值在指定区间之外的input元素             | 3    |
| :in-range              | :in-range        | 匹配值在指定区间之内的input元素             | 3    |
| :read-write            | :read-write      | 用于匹配可读及可写的元素                    | 3    |
| :read-only             | :read-only       | 用于匹配设置 "readonly"（只读） 属性的元素  | 3    |
| :optional              | :optional        | 用于匹配可选的输入元素                      | 3    |

**:first-child** 匹配的是某父元素的第一个子元素，可以说是**结构上的第一个子元素**。

**:first-of-type** 匹配的是某父元素下相同类型子元素中的第一个，比如 p:first-of-type，就是**指所有类型为p的子元素中的第一个**。这里不再限制是第一个子元素了，只要是该类型元素的第一个就行了。

### 伪元素选择器

| 选择器                       | 作用                     | 说明                                                         | CSS  |
| ---------------------------- | ------------------------ | ------------------------------------------------------------ | ---- |
| ::before/:before             | 在被选元素前插入内容。   | 需要使用 content 属性来指定要插入的内容。被插入的内容实际上不在文档树中。 | 2    |
| ::after/:after               | 在选被元素后插入内容     | 其用法和特性与:before相似。                                  | 2    |
| ::first-letter/:first-letter | 匹配元素中文本的首字母。 | 被修饰的首字母不在文档树中。                                 | 1    |
| ::first-line/:first-line     | 匹配元素中第一行的文本。 | 这个伪元素只能用在块元素中，不能用在内联元素中。             | 1    |

### 伪类和伪元素详解/伪元素和dom的关系

**为什么引入伪类，伪元素**

css 引入伪类和伪元素概念是为了格式化文档树以外的信息。

也就是说，伪类和伪元素是用来修饰**不在文档树中的部分**，比如，一句话中的第一个字母，或者是列表中的第一个元素

**什么是 伪类**

伪类用于当已有元素处于的某个状态时，为其添加对应的样式，这个状态是根据用户行为而动态变化的。虽然它和普通的 `css` 类相似，**可以为已有的元素添加样式，但是它只有处于 dom树无法描述的状态下才能为元素添加样式，所以将其称为伪类**

伪类是一个冒号（`:`）后跟伪类的名字构成的，有时候名字后面还会有一个放在括号里的值(`:nth-child`)

**什么是 伪元素**

伪元素是一种**虚拟的元素**，`CSS`把它当成普通`HTML`元素看待。之所以叫伪元素，就因为它们在`文档树`**或DOM中并不实际存在，js无法直接操作**。换句话说，我们`不会在HTML`中包含伪元素，只会通过`CSS`来创建伪元素。也就是伪元素的内容**只存在于CSS渲染树中**，并不存在于真实的DOM中。

### 选择器优先级

#### 优先级计算规则

> 内联 > ID选择器 > 类选择器 > 标签选择器。

到具体的计算层⾯，优先级是由 A 、B、C、D 的值来决定的，其中它们的值计算规则如下：

- 如果存在**内联样式**，那么 A = 1, 否则 A = 0
- B的值等于 **ID选择器**出现的次数
- C的值等于 **类选择器** 和 **属性选择器** 和 **伪类** 出现的总次数
- D 的值等于 **标签选择器** 和 **伪元素** 出现的总次数

这里举个例子：

```css
#nav-global > ul > li > a.nav-link
```

套用上面的算法，依次求出 `A` `B` `C` `D` 的值：

- 因为没有内联样式 ，所以 A = 0
- ID选择器总共出现了1次， B = 1
- 类选择器出现了1次， 属性选择器出现了0次，伪类选择器出现0次，所以 C = (1 + 0 + 0) = 1
- 标签选择器出现了3次， 伪元素出现了0次，所以 D = (3 + 0) = 3

上面算出的`A` 、 `B`、`C`、`D` 可以简记作：`(0, 1, 1, 3)`

知道了优先级是如何计算之后，就来看看比较规则：

- 从左往右依次进行比较 ，较大者优先级更高
- 如果相等，则继续往右移动一位进行比较
- 如果4位全部相等，则后面的会覆盖前面的

经过上面的优先级计算规则，我们知道**内联样式的优先级最高，如果外部样式需要覆盖内联样式，就需要使用!important**

即在声明键值对后面加上!important

#### 超越!important的情况

一些情况，我们可以超越 `!important`,

- html:

```
<div class="box" style="background: #f00; width: 300px!important;">我的宽度是多少呢？？<div>
```

- css:

```
.box {
	max-width: 100px;
}
```

这时候 `.box` 的宽度只有 `100px` , 而不是 `300px`, 可见，`max-width` 可以超越 `width!important`!但是，这实际上不是优先级的问题，因为优先级是比较相同属性的，而 `max-width` 和 `width` 是两个不同的属性。



# CSS哪些属性(不)可以继承

## 属性的是否默认继承

初始值是指当属性没有指定值时的默认值，如这些语句的值都是默认值：`background-color: transparent`、`left: auto` 、`float: none`、`width: auto` 等。

css 的继承很简单，分为默认继承的和默认不继承的，但所有属性都可以通过设置 `inherit` 实现继承。

当没有指定值时，默认继承的属性取父元素的同属性的计算值（相当于设置了 `inherit` ），默认不继承的属性取属性的初始值（时相当于设置了 `initial` ）。

## 默认继承的 ("Inherited: Yes") 的属性：

- 所有元素默认继承：visibility、cursor
- 文本属性默认继承：letter-spacing、word-spacing、white-space、line-height、color、font、 font-family、font-size、font-style、font-variant、font-weight、text-indent、text-align、text-shadow、text-transform、direction
- 列表元素默认继承：list-style、list-style-type、list-style-position、list-style-image
- 表格元素默认继承：border-collapse

## 默认不继承的("Inherited: No") 的属性：

- 所有元素默认不继承：all、display、overflow、contain
- 文本属性默认不继承：vertical-align、text-decoration、text-overflow
- 盒子属性默认不继承：width、height、padding、margin、border、min-width、min-height、max-width、max-height
- 背景属性默认不继承：background、background-color、background-image、background-repeat、background-position、background-attachment
- 定位属性默认不继承：float、clear、position、top、right、bottom、left、z-index
- 内容属性默认不继承：content、counter-reset、counter-increment
- 轮廓属性默认不继承：outline-style、outline-width、outline-color、outline
- 页面属性默认不继承：size、page-break-before、page-break-after
- 声音属性默认不继承：pause-before、pause-after、pause、cue-before、cue-after、cue、play-during

## 四个通用属性值

css 为控制继承提供了四个特殊的通用属性值，**每个 css 属性都能使用这些值**。

- inherit

设置该属性会使子元素属性和父元素相同。实际上，就是“开启继承”。

- initial

将属性的初始值应用于元素。实际上，就是重置为css规范定义中的默认值（不是浏览器定义的样式表中的样式）。

- unset

将属性重置为自然值，也就是如果属性是自然继承的那么就是 `inherit` ，否则和 `initial` 一样。

- revert

表示使用样式表中定义的元素属性的默认值。若用户定义样式表中显式设置，则按此设置；否则，按照浏览器定义样式表中的样式设置；否则，等价于 unset 。

**position中也可使用这些属性值，见“定位”**



# CSS 三大特性

### 层叠性

所谓层叠性是指多种CSS样式的叠加。

是浏览器处理冲突的一个能力,如果一个属性通过两个相同选择器设置到同一个元素上，那么这个时候一个属性就会将另一个属性层叠掉

比如先给某个标签指定了内部文字颜色为红色，接着又指定了颜色为蓝色，此时出现一个标签指定了相同样式不同值的情况，这就是样式冲突。

一般情况下，如果出现样式冲突，则会按照CSS书写的顺序，以最后的样式为准。

1. 样式冲突，遵循的原则是`就近原则`。 那个样式离着结构近，就执行那个样式。
2. 样式`不冲突，不会层叠`

### 继承性

在**css**中，继承是指的是给父元素设置一些属性，后代元素会自动拥有这些属性

关于继承属性，可以分成：

- 字体系列属性
  - font:组合字体
  - font-family:规定元素的字体系列
  - font-weight:设置字体的粗细
  - font-size:设置字体的尺寸
  - font-style:定义字体的风格
  - font-variant:偏大或偏小的字体
- 文字系列属性
  - text-indent：文本缩进
  - text-align：文本水平
  - line-height：行高
  - word-spacing：增加或减少单词间的空白
  - letter-spacing：增加或减少字符间的空白
  - text-transform：控制文本大小写
  - direction：规定文本的书写方向
  - color：文本颜色
- 元素可见性
  - visibility
- 表格布局属性
  - caption-side：定位表格标题位置
  - border-collapse：合并表格边框
  - border-spacing：设置相邻单元格的边框间的距离
  - empty-cells：单元格的边框的出现与消失
  - table-layout：表格的宽度由什么决定
- 列表属性
  - list-style-type：文字前面的小点点样式
  - list-style-position：小点点位置
  - list-style：以上的属性可通过这属性集合
- 引用
  - quotes：设置嵌套引用的引号类型
- 光标属性
  - cursor：箭头可以变成需要的形状

> 继承中比较特殊的几点：

- a 标签的字体颜色不能被继承
- h1-h6标签字体的大下也是不能被继承的

> 无继承的属性

- display
- 文本属性：vertical-align、text-decoration
- 盒子模型的属性：宽度、高度、内外边距、边框等
- 背景属性：背景图片、颜色、位置等
- 定位属性：浮动、清除浮动、定位position等
- 生成内容属性：content、counter-reset、counter-increment
- 轮廓样式属性：outline-style、outline-width、outline-color、outline
- 页面样式属性：size、page-break-before、page-break-after

### 优先级

总结优先级：

1. 使用了 !important声明的规则。
2. 内嵌在 HTML 元素的 style属性里面的声明。
3. 使用了 ID 选择器的规则。
4. 使用了类选择器、属性选择器、伪元素和伪类选择器的规则。
5. 使用了元素选择器的规则。
6. 只包含一个通用选择器的规则。
7. 同一类选择器则遵循就近原则。



# 盒模型/margin塌陷

### 定义

CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：`外边距（margin）`、`边框（border）`、`内边距（padding）`、`实际内容（content）`四个属性。

盒模型各部分说明：

- Margin(外边距) ：清除边框外的区域，外边距是透明的（可以为负值）。
- Border(边框) ：围绕在内边距和内容外的边框。
- Padding(内边距) ：清除内容周围的区域，内边距是透明的（不允许负值）。
- Content(内容) ：盒子的内容，显示文本和图像。

### 盒模型分类

盒模型分为两类: IE盒模型（怪异盒模型）和W3C标准盒模型。 两者的区别在于:

> IE盒模型的width/height = content + border + padding
> 标准盒模型的width/height = content

#### CSS如何设置这两种模型

标准：`box-sizing: content-box;` ( 浏览器默认设置 )
IE： `box-sizing: border-box;`

css的盒模型由content(内容)、padding(内边距)、border(边框)、margin(外边距)组成。**但盒子的大小由content+padding+border这几部分决定**，**把margin算进去的那是盒子占据的位置，而不是盒子的大小！**

我们可以试着给上面的粉色方块设置box-sizing属性为**border-box**发现，会发现：无论我们怎么改border和padding盒子大小始终是定义的width和height.

### JS如何获取盒模型对应的宽和高

```js
1``) dom.style.width/height【只能取到内联元素】
2``) dom.currentStyle.width/height【只有IE支持】
3``) document.getComputedStyle(dom,null).width/height 取到的是最终渲染后的宽和高，如果有设置宽高，则不论哪种盒模型获取到的都是设置的宽高，和currentStyle相同，但是兼容性更好，IE9 以上支持。
4``) dom.getBoundingClientRect().width/height 得到渲染后的宽和高，大多浏览器支持。IE9以上支持。
5``) dom.offsetWidth/offsetHeight【常用】包括高度（宽度）、内边距和边框，不包括外边距。最常用，兼容性最好。
```

### 根据盒模型解释边距重叠(边距塌陷)

边距重叠是指两个【**垂直**】 【**相邻**】的**块级元素**，当上下两个边距相遇时，其外边距会产生重叠现象，且重叠后的外边距，等于其中较大者。（水平方向不会发生）

**注意**：只有普通文档流中**块框的垂直外边距才会发生外边距合并**，行内框、浮动框或绝对定位之间的外边距不会合并。

#### 原因

根据[W3C文档](https://link.juejin.cn/?target=https%3A%2F%2Fwww.w3.org%2FTR%2FCSS22%2Fbox.html%23collapsing-margins)的说明，当符合以下条件时，就会触发外边距重合

- 都是普通流中的元素且属于同一个 BFC
- 没有被 padding、border、clear 或非空内容隔开
- 两个或两个以上垂直方向的「相邻元素」

> 相邻元素包括父子元素和兄弟元素

**『重叠后的margin计算』**

- 1、margin都是正值时取较大的margin值
- 2、margin都是负值时取绝对值较大的，然后负向位移。
- 3、margin有正有负，从负值中选绝对值最大的，从正值中选取绝对值最大的，然后相加

#### 边距重叠详解及解决方案

**1、嵌套块（父子）元素垂直外边距的合并**

对于两个嵌套关系的块元素，如果父元素没有`padding-top`及`border`，则父元素的`margin-top`会与子元素的`margin-top`发生合并，合并后的外边距为两者中的较大者，即使父元素的上外边距为0，也会发生合并。

![](pics/父子外边距塌陷.png)

**『解决办法』**

1、为**父元素**定义1px的**border**-top或**padding**-top。

2、触发bfc(块级格式上下文),改变父级的渲染规则

改变父级的渲染规则有以下四种方法,给父级盒子添加 **OFDP**

```scss
(1)position:absolute/fixed

(2)display:inline-block;

(3)float:left/right;

(4)overflow:hidden;
```

这四种方法都能触发bfc,但是使用的时候都会带来不同的麻烦,具体使用中还需根据具体情况选择没有影响的来解决margin塌陷

3、父元素加前置内容（::before）生成。



**2、相邻块（兄弟）元素垂直外边距的合并（外边距塌陷）**

当上下相邻的两个块元素相遇时，如果

- 上面的元素有下外边距margin-bottom，
- 下面的元素有上外边距margin-top，

则他们之间的垂直间距**不是margin-bottom与margin-top之和**，而是**两者中的较大者**。

![](pics/兄弟元素塌陷.png)

**『解决办法』**

1）为了达到想要的间距，最好在设置margin-top/bottom值时统一设置上或下；

2）用BFC解决，1、给第二个元素加上为BFC的父级元素；2、给两个元素都加上为BFC的父元素



# BFC

**BFC(Block Formatting Context)**：**块级格式化上下文**。
一个**独立**的**块级渲染区域**，该区域拥有一套**渲染规格**来约束**块级盒子**的**布局**，且与区域外部无关。

### BFC的布局规则（原理/渲染规则）

（1）内部的 `Box` 会在**垂直方向**，从顶部开始一个接着一个地放置；
（2）`Box` 垂直方向的距离由 `margin` (外边距)决定，属于同一个 `BFC` 的两个相邻 `Box` 的 `margin` 会发生重叠；
（3）每个元素的 `margin Box` 的左边， 与包含块 `border Box` 的左边相接触，（对于从左到右的格式化，否则相反）。即使存在浮动也是如此；
（4）BFC 在页面上是一个**隔离的独立容器**，**里外元素互不影响**。文字环绕效果，设置 `float`；
（5）BFC 的区域不会与 `float Box` 重叠（**清浮动**）;
（6）计算 `BFC` 的**高度时，浮动元素也参与计算**。

### 创建出BFC的情况（即脱离文档流）（OFDP）

1. 根元素，即 HTML 元素（最大的一个 `BFC`）
2. 浮动（ `float 的值不为 none` ）
3. 绝对定位元素（ `position 的值为 absolute 或 fixed` ）
4. 行内块（ `display 为 inline-block` ）
5. 表格单元（ `display 为 table、table-cell、table-caption、inline-block 等 HTML 表格相关的属性` )
6. 弹性盒（ `display 为 flex 或 inline-flex` ）
7. 网格布局（ `display 为 grid 或 inline-grid` ）
8. `overflow`的值不为`visible`的块元素；

> 并不是任意一个元素都可以被当做BFC，只有当这个元素满足以上任意一个条件的时候，这个元素才会被当做一个BFC

### BFC作用（使用场景）

1、**防止margin塌陷**，分属于不同的 `BFC` 时，可以阻止 `margin` 重叠，**垂直方向margin塌陷问题**

2、自适应两（三）栏布局（避免多列布局由于宽度计算四舍五入而自动换行）
3、避免元素被浮动元素覆盖
4、可以让**父元素**的高度包含子浮动元素，**清除内部浮动**,**防止塌陷**（原理：触发父 `div` 的 `BFC` 属性，使下面的子 `div` 都处在父 `div`的同一个 `BFC` 区域之内）

# 浮动

浮动会**使当前元素脱离文档流**，浮动元素碰到包含**它的边框或者浮动元素的边框停留**。会影响页面布局，可以通过清除浮动来解决。同时浮动**会造成父元素高度塌陷**，影响和父元素同级的Dom布局，可以触发父元素的BFC解决。

加了浮动之后的元素,会具有很多特性,需要我们掌握的.

1．浮动元素**会脱离标准流**(脱标)

2．浮动的元素**会一行内显示并且元素顶部对齐**

3．浮动的元素会具有**行内块元素的特性**

### 浮动的影响

`float`不会影响前面已经渲染好的文档，而会影响在其后面将要渲染的文档。

> 第一个影响：影响了后面元素的布局。

> 第二个影响：影响了父容器的高度，正常父元素的高度是自适应的，高度为其包含的内容总高度，而内部元素的浮动造成了父容器高度塌陷。（脱离标准流）

> 第三个影响：父容器高度塌陷了，将会影响和父元素同级的文档布局。

要解决这三个影响，需要从两个方向思考：

> 第一个方向：解决父元素给其同级的元素造成的影响，比喻成解决**外部矛盾**。

> 第二个方向：解决父级元素内部的浮动元素对其同级元素的影响，比喻成解决**内部矛盾**。

![](pics/清除浮动.png)

如图所示，绿粉为内部影响，黑蓝为外部影响。

### 解决外部矛盾（解决父元素给其同级的元素造成的影响）

### 1、触发bfc

触发方式：可以给父级元素设置`overflow:auto`，hidden或者scroll都行

### 2、clear原理

> clear属性用于在块级元素中清除浮动效果,其值可以设置成：
>
> clear：both 清除左右两边的的浮动
> clear：left 清除左边的浮动
> clear： right 清除右边浮动
> clear： none 左右都有浮动效果

设置了clear属性的元素，相应的会在两侧按照没有浮动元素的样式排下去，所以不会受到浮动元素的影响，从而实现清除浮动

**1、after伪元素清除浮动**

```css
    .clearfix:after {
        display: block;
        overflow: hidden;
        content: '';
        clear: both;
        height: 0;
    }
//给父元素添加这个class即可
```

**本质**上就是给**父元素**再加一个**块级子容器**，当然这个也就是父元素的最后一个块级子容器了。同时给这个**块级子容器设置clear属性来清除其浮动**，这样这个子容器就能排列在**浮动元素的后面**，同时也把父元素的高度撑起来了。那么父元素的同级元素也能正常排列了。所以这个子容器不能有高度和内容，不然会影响父元素的布局。

**2、额外标签法**

在浮动**父元素**结束标签之前插入**清除浮动的块级元素**。假设空块级元素为`<div class='emptyClear'></div>`。我们还需要为其设置清楚浮动的CSS属性

```css
.emptyClear{
    clear:both;
}
```

> 在父级元素末尾添加的元素必须是一个**块级元素**，否则无法撑起父级元素高度。

**3、双伪元素清除浮动**

```css
.clearfix:before,
.clearfix:after {
    content: "";
    display: table;
}
.clearfix:after {
    clear: both;
}
//给父元素添加这个class即可
```

原理是给父元素添加第一个子容器和最后一个子容器。

**三种方法的原理一样，都是添加块级子容器来清除浮动。**

解决**外部矛盾**的效果如图：

![](pics/清除浮动1.png)

### 解决内部矛盾

解决内部矛盾的原理和解决外部矛盾的第二种方式的原理是一样的，通过给**被浮动影响的第一个元素**进行清除浮动，就可以使后面的元素也不会受到浮动影响了。

```css
  .z4 {
    background: green;
    clear:both;/*加了这个*/
  }
```

**给内部元素设置`clear:both;`清除浮动后，会直接解决内部矛盾和外部矛盾。**

![](pics/清除浮动2.png)



# 定位

**文档流**

文档流就是HTML文档内所有元素按照一定规律排列并显示的形式。

CSS文档流大致可以分为3种：标准流，浮动流，定位流。

### ⚝属性

| **值**   | **描述**                                                     | **归属文档流** |
| -------- | :----------------------------------------------------------- | -------------- |
| static   | **默认值**，**静态定位**，**没有定位**，元素出现在**正常的流**中（忽略 top, bottom, left, right 或者 z-index 声明）。 | 常规文档流     |
| relative | 参照**自身**在常规流中的位置通过top，right，bottom，left这4个定位**偏移属性**进行偏移时**不会影响常规流中的任何元素** | 常规文档流     |
| absolute | 生成**绝对定位**的元素，相对于 static 定位以外的第一个父元素进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。  偏移属性参照的是**离自身最近的定位祖先元素**，如果没有定位的祖先元素，则一直回溯到body元素。**盒子的偏移位置不影响常规流中的任何元素，其margin不与其他任何margin折叠** | 脱离文档流     |
| fixed    | 生成**绝对定位**的元素，相对于**浏览器窗口**进行定位。 元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。  与absolute一致，但偏移定位是以窗口为参考。当出现滚动条时，对象不会随着滚动 | 脱离文档流     |
| inherit  | 规定应该从父元素继承 position 属性的值。                     |                |
| sticky   | **粘性定位**position: sticky; 的元素根据用户的滚动位置进行定位。 粘性元素根据滚动位置在相对（relative）和固定（fixed）之间切换。起先它会被**相对定位**，直到在视口中遇到给定的偏移位置为止 - 然后将其“粘贴”在适当的位置（比如 position:fixed）。 | 常规文档流     |
| initial  | 将属性的初始值应用于元素。实际上，就是重置为css规范定义中的默认值 |                |
| unset    | 将属性重置为自然值，也就是**不设置**，也就是如果属性是自然继承的那么就是 `inherit` ，否则和 `initial` 一样。 |                |
| revert   | 表示使用样式表中定义的元素属性的默认值。**若用户定义样式表中显式设置，则按此设置**；否则，按照浏览器定义样式表中的样式设置；否则，等价于 unset 。 |                |

**只要不是float和绝对定位方式布局，都在文档流里**

**inherit和后三种是通用属性，通常用于继承中**

### absolute详解

加了absolute定位后后面是文字的话，文字会被图片遮盖了，这点和**float**不同。float是欺骗父元素，让其父元素误以为其高度塌陷了，但**float元素本身仍处于文档流**中，文字会环绕着float元素，不会被遮蔽。

但absolute其实已经不能算是欺骗父元素了，而是出现了**层级**关系。从父元素的视点看，设成absolute的图片已经完全消失不见了，因此从最左边开始显示文字。而absolute的层级高，所以图片遮盖了文字。

**子绝父相**的方式来设置定位

当绝对定位和固定定位参照物都是浏览器窗口时的区别： 当出现滚动条时，固定定位的元素不会跟随滚动条滚动，绝对定位会跟随滚动条滚动。

### 堆叠顺序z-index

在使用定位布局时，可能会出现盒子重叠的情况。

加了定位的盒子，默认`后来者居上`， 后面的盒子会压住前面的盒子。

应用 `z-index` 层叠等级属性可以**调整盒子的堆叠顺序**。如下图所示：

`z-index` 的特性如下：

1. 属性值：正整数、负整数或 0，**默认值是 0**，数值越大，盒子越靠上；（w3c默认auto，不参与成绩比较，IE6/7默认0，参与比较）
2. 如果属性值相同，则**按照书写顺序，后来居上**；
3. 数字后面不能加单位。

注意：`z-index` 只能应用于**相对定位**、**绝对定位**和**固定定位**的元素，其他标准流、浮动和静态定位无效。

如果所有节点都定义了 position:relative. z-index 为 0 的节点与没有定义 z-index 在同一层级内没有高低之分; 但 z-index 大于等于 1 的节点会遮盖没有定义 z-index 的节点; z-index 的值为负数的节点将被没有定义 z-index 的节点覆盖.

### 层级树规则

**同时将 position 设为 relative, absolute 或者 fixed, 并且 z-index 经过整数赋值的节点**, 会被放置到一个与 DOM 不一样的**层级树**里面, 并且在层级树中**通过对比 z-index 决定显示的层级**. 上面的例子如果用层级树来表示的话, 应该如下图所示.

![img](https://images0.cnblogs.com/blog2015/686913/201507/042203210887312.jpg)

图中虽然 A-1 (`z-index:0`) 的值比 B-1 (`z-index:1`) 小, 但因为在层级树里 A (`z-index:2`) 和 B-1 在一个层级, 而 A 的值比 B-1 大, 根据从父规则, A-1 显示在 B-1 前面.这就是**从父规则**，如果父亲节点也在层级树中，比较只看父节点的z-index，都为0的话看顺序，而不是看子节点的z-index





### 减少重绘和回流的开销

例如将元素隐藏，你或许会用display:none。

其实我更推荐的是absolute控制隐藏和显示。方法当然相当简单，如absolute+ top:-9999em，或absolute + visibility:hidden。

优点是absolute由于层级的关系，隐藏和显示只会重绘，但不会回流（其实我对absolute不会导致回流这一观点是持怀疑态度的，说不会回流显得过于武断，应该是absolute的回流开销小于正常DOM流中回流的开销。但我战斗力不够，尚未找到靠谱合理的检测回流的方法）。而用display:none会导致render tree重绘和回流。

另外，考虑到重绘和回流的开销，可以将动画效果放到absolute元素中，避免浏览器将render tree回流。



# ⚝css设置盒子高度总是宽度的一半

1

```css
width:100%;
padding :25% 0
```

2

```css
width: 100%;
height: 50vw;
background-color: #e0e0e0;
```

3

```js
$(window).load(function(){
    $('.element').height($('.element').width() / 2);
    $(window).resize(function(){
        $('.element').height($('.element').width() / 2);
    });
});
```



# 水平垂直居中

## 水平居中

### 行内元素

```css
.parent {
    text-align: center;
}
```

### 块级元素

#### 块级元素一般居中方法

通过把固定宽度块级元素的`margin-left`和`margin-right`设成auto，就可以使块级元素水平居中。

```css
.son {
    margin: 0 auto;
}
```

#### 子元素含 float

```css
.parent{
    width:fit-content;
    margin:0 auto;
}

.son {
    float: left;
}
```

#### 子元素 inline-block

```css
.parent {
    text-align: center;
}
.son {
    display: inline-block;
}
```

#### table

```css
.parent {
    display: table-cell;
    text-align: center;
}
.son {
    display: inline-block;
}
```

#### Flex 弹性盒子

```css
.parent {
    display: flex;
    justify-content: center;
}
```

#### grid盒子

```css
.parent {
    display: grid;
}
.son {
    justify-self: center;
}
```

#### ⚝绝对定位

1）transform

```css
.son {
    position: absolute;
    left: 50%;
    transform: translate(-50%, 0);
}
```

2）left: 50%

```css
.son {
    position: absolute;
    width: 宽度;
    left: 50%;
    margin-left: -0.5*宽度
}
```

3）left/right: 0

```css
.son {
    position: absolute;
    width: 宽度;
    left: 0;
    right: 0;
    margin: 0 auto;
}
```

## 垂直居中

### 行内元素

```css
.parent {
    height: 高度;
}

.son {
    line-height: 高度;
}
```

**注：① 子元素 line-height 值为父元素 height 值。② 单行文本。**

### 块级元素

#### 行内块级元素伪元素法

即在父容器内放一个100%高度的伪元素，让文本和伪元素垂直对齐，从而达到垂直居中的目的。

```css
.parent::after, .son{
    display:inline-block;
    vertical-align:middle;
}
.parent::after{
    content:'';
    height:100%;
}
```

适应 IE7。

#### table

```css
.parent {
    display: table-cell;
    vertical-align: middle;
}
.son {
    display: inline-block;
}
```



#### Flex 弹性盒子

```css
.parent {
    display: flex;
    align-items: center;
}
```

#### grid盒子

```css
.parent {
    display: grid;
}
.son {
    align-self: center;
}
```

#### ⚝绝对定位

1）transform

```css
.son {
    position: absolute;
    top: 50%;
    transform: translate( 0, -50%);
}
```

**优点**

- 代码少。

**缺点**

- IE8不支持, 属性需要追加浏览器厂商前缀, 可能干扰其他 transform 效果, 某些情形下会出现文本或元素边界渲染模糊的现象。

2）top: 50%

```css
.son {
    position: absolute;
    top: 50%;
    height: 高度;
    margin-top: -0.5高度;
}
```

**优点**

- 适用于所有浏览器。

**缺点**

- 父元素空间不够时, 子元素可能不可见(当浏览器窗口缩小时,滚动条不出现时).如果子元素设置了overflow:auto, 则高度不够时, 会出现滚动条。

3）top/bottom: 0;

```css
.son {
    position: absolute;
    top: 0;
    bottom: 0;
    margin: auto 0;
}
```

**优点**

- 简单。

**缺点**

- 没有足够空间时, 子元素会被截断, 但不会有滚动条

![](pics/css居中.png)



# 两栏布局

一般两栏布局指的是左边一栏宽度固定，右边一栏宽度自适应，两栏布局的具体实现：

- 利用浮动，将左边元素宽度设置为200px，并且设置向左浮动。将右边元素的margin-left设置为200px，宽度设置为auto（默认为auto，撑满整个父元素）。
- 利用浮动，左侧元素设置固定大小，并左浮动，右侧元素设置overflow: hidden; 这样右边就触发了BFC，BFC的区域不会与浮动元素发生重叠，所以两侧就不会发生重叠。
- 利用flex布局，将左边元素设置为固定宽度200px，将右边的元素设置为flex:1。
- 利用绝对定位，将父级元素设置为相对定位。左边元素设置为absolute定位，并且宽度设置为200px。将右边元素的margin-left的值设置为200px。
- 利用绝对定位，将父级元素设置为相对定位。左边元素宽度设置为200px，右边元素设置为绝对定位，左边定位为200px，其余方向定位为0。

# flexbox（弹性盒布局模型）

`Flexible Box` 简称 `flex`，意为”弹性布局”，可以简便、完整、响应式地实现各种页面布局

采用Flex布局的元素，称为`flex`容器`container`

**它的所有子元素自动成为容器成员**，称为flex**项目item**

**任何一个容器都可以指定为 Flex 布局**,行内元素也可以使用 Flex 布局,Webkit 内核的浏览器，必须加上`-webkit`前缀。

**注意，设为 Flex 布局以后，子元素的`float`、`clear`和`vertical-align`属性将失效。**

![img](https://static.vue-js.com/fbc5f590-9837-11eb-ab90-d9ae814b240d.png)

容器中默认存在**两条轴，主轴和交叉轴**，呈90度关系。**项目默认沿主轴排列**，通过`flex-direction`来决定主轴的方向

每根轴都有起点和终点，这对于元素的对齐非常重要

关于`flex`常用的属性，我们可以划分为**父容器属性**和**容器成员属性**

### 父容器属性

- flex-direction
- flex-wrap
- flex-flow
- justify-content
- align-items
- align-content

#### **flex-direction**

决定主轴的方向(即项目的排列方向)

```css
.container {   
    flex-direction: row | row-reverse | column | column-reverse;  
} 
```

属性对应如下：

- row（默认值）：主轴为水平方向，起点在左端
- row-reverse：主轴为水平方向，起点在右端
- column：主轴为垂直方向，起点在上沿。
- column-reverse：主轴为垂直方向，起点在下沿

如下图所示：

![img](https://static.vue-js.com/0c9abc70-9838-11eb-ab90-d9ae814b240d.png)

#### **flex-wrap**

弹性元素永远沿主轴排列，那么如果主轴排不下，通过`flex-wrap`决定容器内项目**是否可换行**

```css
.container {  
    flex-wrap: nowrap | wrap | wrap-reverse;
}  
```

属性对应如下：

- nowrap（默认值）：不换行
- wrap：换行，第一行在下方
- wrap-reverse：换行，第一行在上方

默认情况是不换行，但这里也不会任由元素直接溢出容器，会涉及到元素的弹性伸缩

#### **flex-flow**

是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`

```css
.box {
  flex-flow: <flex-direction> || <flex-wrap>;
}
```

#### **justify-content**

定义了项目在**主轴上的对齐方式**

```css
.box {
    justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

属性对应如下：

- flex-start（默认值）：左对齐
- flex-end：右对齐
- center：居中
- space-between：两端对齐，项目之间的间隔都相等
- space-around：两个项目两侧间隔相等

效果图如下：

![img](https://static.vue-js.com/2d5ca950-9838-11eb-85f6-6fac77c0c9b3.png)

#### align-items

定义项目在交叉轴上如何对齐

```css
.box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}
```

属性对应如下：

- flex-start：交叉轴的起点对齐
- flex-end：交叉轴的终点对齐
- center：交叉轴的中点对齐
- baseline: 项目的第一行文字的基线对齐
- stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度

#### align-content

定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用

```css
.box {
    align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

属性对应如吓：

- flex-start：与交叉轴的起点对齐
- flex-end：与交叉轴的终点对齐
- center：与交叉轴的中点对齐
- space-between：与交叉轴两端对齐，轴线之间的间隔平均分布
- space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍
- stretch（默认值）：轴线占满整个交叉轴

效果图如下：

![img](https://static.vue-js.com/39bcb0f0-9838-11eb-ab90-d9ae814b240d.png)

### 子容器属性

**容器成员属性**如下：

- `order`
- `flex-grow`
- `flex-shrink`
- `flex-basis`
- `flex`
- `align-self`

#### order

定义项目的排列顺序。数值越小，排列越靠前，默认为0

```css
.item {
    order: <integer>;
}
```

#### flex-grow

上面讲到当容器设为`flex-wrap: nowrap;`不换行的时候，容器宽度**有不够分**的情况，弹性元素会根据`flex-grow`来决定

定义项目的**放大比例**（容器宽度>元素总宽度时如何伸展）

**默认为0，即如果存在剩余空间，也不放大**

```css
.item {
    flex-grow: <number>;
}
```

如果所有项目的`flex-grow`属性都为1，则它们将等分剩余空间（如果有的话）

![img](https://static.vue-js.com/48c8c5c0-9838-11eb-ab90-d9ae814b240d.png)

如果一个项目的`flex-grow`属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍

![img](https://static.vue-js.com/5b822b20-9838-11eb-ab90-d9ae814b240d.png)

弹性容器的宽度正好等于元素宽度总和，无多余宽度，此时无论`flex-grow`是什么值都不会生效

#### flex-shrink

定义了项目的**缩小比例**（容器宽度<元素总宽度时如何收缩），默认为1，**即如果空间不足**，该项目将缩小

```css
.item {
    flex-shrink: <number>; /* default 1 */
}
```

如果所有项目的`flex-shrink`属性都为1，当空间不足时，都将等比例缩小

如果一个项目的`flex-shrink`属性为0，其他项目都为1，则空间不足时，前者不缩小

![img](https://static.vue-js.com/658c5be0-9838-11eb-85f6-6fac77c0c9b3.png)

在容器宽度有剩余时，`flex-shrink`也是不会生效的

#### flex-basis

设置的是**元素在主轴上的初始尺寸**，所谓的初始尺寸就是元素在`flex-grow`和`flex-shrink`生效前的尺寸

浏览器根据这个属性，计算主轴是否有多余空间，默认值为`auto`，即项目的本来大小，如设置了`width`则元素尺寸由`width/height`决定（主轴方向），没有设置则由内容决定

```css
.item {
   flex-basis: <length> | auto; /* default auto */
}
```

**当设置为0的是，会根据内容撑开**

它可以设为跟`width`或`height`属性一样的值（比如350px），则项目将占据固定空间

#### flex

`flex`属性是`flex-grow`, `flex-shrink` 和 `flex-basis`的简写，默认值为`0 1 auto`，也是比较难懂的一个复合属性

```css
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```

一些属性有：

- flex: 1 = flex: 1 1 0%
- flex: 2 = flex: 2 1 0%
- flex: auto = flex: 1 1 auto
- flex: none = flex: 0 0 auto，常用于固定尺寸不伸缩

`flex:1` 和 `flex:auto` 的区别，可以归结于`flex-basis:0`和`flex-basis:auto`的区别

当设置为0时（绝对弹性元素），此时相当于告诉`flex-grow`和`flex-shrink`在伸缩的时候不需要考虑我的尺寸

当设置为`auto`时（相对弹性元素），此时则需要在伸缩时将元素尺寸纳入考虑

注意：建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值

#### align-self

允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性

默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`

```css
.item {
    align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

效果图如下：

![img](https://static.vue-js.com/6f8304a0-9838-11eb-85f6-6fac77c0c9b3.png)

# flex布局经典场景

## 水平垂直居中

```css
.box{
    display: flex;
    width: 100%;
    height: 500px;
    background: #CCCCCC;
    justify-content: center;   //主轴居中
    align-items: center; //交叉轴居中    
   }
   
   .box div{
    width: 50px;
    height: 50px;
    background: red;
    border:2px solid #007AFF;
  
   }
 <div class="box">
  <div class="div1">你大爷</div>
 </div>
```

## 每行的项目数固定并自动换行的列表项

```css
ul{
  display:flex;
  flex-wrap:wrap;
}
li{
  list-style:none;
  flex:0 0 25%;
  background:#ddd;
  height:100px;
  border:1px solid red;
}

```

## 两列布局

### 左列定宽右列自适应

html代码:

```xml
<body>
<div id="parent">
    <div id="left">左列定宽</div>
    <div id="right">右列自适应</div>
</div>
</body>

```

css代码:

```css
#parent{
    width: 100%;
    height: 500px;
    display: flex;
}
#left {
    width: 100px;
    background-color: #f00;
}
#right {
    flex: 1; /*均分了父元素剩余空间*/
    background-color: #0f0;
}
```

### 一列不定，一列自适应

html代码:

```xml
<body>
<div id="parent">
    <div id="left">左列不定宽</div>
    <div id="right">右列自适应</div>
</div>
</body>
```

css代码:

```css
#parent{
    display: flex;
}
#left { /*不设宽度*/
    margin-right: 10px;
    height: 500px;
    background-color: #f00;
}
#right {
    height: 500px;
    background-color: #0f0;
    flex: 1;  /*均分#parent剩余的部分*/
}
```

## 三列布局

左右定宽中间自适应

```xml
<body>
<div id="parent">
    <div id="left">左列定宽</div>
    <div id="center">中间自适应</div>
    <div id="right">右列定宽</div>
</div>
</body>

复制代码
```

css代码:

```css
#parent {
    height: 500px;
    display: flex;
}
#left {
    width: 100px;
    background-color: #f00;
}
#center {
    flex: 1;  /*均分#parent剩余的部分*/
    background-color: #eeff2b;
}
#right {
    width: 200px;
    background-color: #0f0;
}
```

## 平均分配空间的栅格布局

  各大UI里栅格布局基本是必备的布局之一，平均分配布局又是栅格布局里最常用的布局，利用flex实现平均分配的栅格布局，关键之处就是利用它的自动收缩空间。

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/5/29/163ac70674fc706e~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)

图4

html如下：

```ini
<div class="row">
         <div class="column">column1</div>
         <div  class="column">colum22</div>
         <div  class="column">colum322</div>
</div>

```

css如下：

```css
.row{
  display:flex;
  flex-wrap:wrap;
  border:1px solid black;
}
.column{
  list-style:none;
  background:#ddd;
  flex:1;               == flex: 1 1 0%
  height:100px; 
  border:1px solid red;
}
```



## 上-中-下布局

```css
* {
    margin: 0;
    padding: 0;
}
body {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
}
header {
    /*min-height: 50px;*/
    flex: none;
    background-color: cadetblue;
}
article {
    flex: auto;
    /*flex-basis: auto;*/
}
footer {
    /*min-height: 50px;*/
    flex: none;
    background-color: aqua;
}
```

```html
<body>
    <header>HEADER</header>
    <article>CONTENT</article>
    <footer>FOOTER</footer>
</body>
```

![demo 1 - Sticky Footer](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2017/11/28/160014efc9500d24~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)demo 1 - Sticky Footer

## Fixed-Width Sidebar

在上-中-下布局的基础上，加了左侧定宽 sidebar。

```css
        * {
            margin: 0;
            padding: 0;
        }
        body {
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }
        header {

            background-color: cadetblue;
        }
        footer {

            background-color: aqua;
        }
        .content {
            flex: auto;
            display: flex;
        }
        .content aside {
            background-color: blanchedalmond;
            width: 900px;
        }
        .content article {
            flex: auto;
        }
```

```html
<body>
<header>HEADER</header>
<div class="content">
    <aside>ASIDE</aside>
    <article>CONTENT</article>
</div>
<footer>FOOTER</footer>
</body>
```



![demo 2 - Fixed-Width Sidebar](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2017/11/28/160014fd16c275f0~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)demo 2 - Fixed-Width Sidebar

## Sidebar

左边是定宽 sidebar，右边是上-中-下布局。

![demo 3](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2017/11/28/160014ff75039d5c~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)demo 3

```css
<body>
  <aside>ASIDE</aside>
  <div class="content">
    <header>HEADER</header>
    <article>CONTENT</article>
    <footer>FOOTER</footer>
  </div>
</body>复制代码
body {
  min-height: 100vh;
  display: flex;
}
aside {
  flex: none;
}
.content {
  flex: auto;
  display: flex;
  flex-direction: column;
}
.content article {
  flex: auto;
}
.content header {
  background-color: cadetblue;
}
.content footer {
  background-color: aqua;
}
```

## Sticky Header

![demo 4 - Sticky Header](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2017/11/28/160015022ab639ae~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)demo 4 - Sticky Header



```css
<body>
  <header>HEADER</header>
  <article>CONTENT</article>
  <footer>FOOTER</footer>
</body>复制代码
body {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  padding-top: 60px;
}
header {
  height: 60px;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  padding: 0;
}
article {
  flex: auto;
  height: 1000px;
}
```

## 圣杯布局

![](pics/圣杯布局.png)

```css
        body {
            display: flex;
            flex-direction: column;
            min-height: 100vh;
            font-weight: bolder;
        }

        header,
        footer {
            height: 150px;
            background-color: #666;
            display: flex;
            justify-content: center;
            align-items: center;
            flex: none;
        }

        .content {
            flex: 1; /* 高度自适应 */
            display: flex;
        }

        nav,
        aside {
            background-color: #eb6f43;
            flex: 0 1 200px;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        main {
            display: flex;
            justify-content: center;
            align-items: center;
            flex: 1; /* 宽度自适应 */
            background-color: #d6d6d6;
        }

        nav {
            order: -1; /* 调整顺序在main前面 */
        }
```

```html
<body>
<header>
    header
</header>
<div class="content">
    <main>main</main>
    <nav>nav</nav>
    <aside>aside</aside>
</div>
<footer>
    footer
</footer>
</body>
```



# grid网格布局

`Grid` 布局即网格布局，是一个**二维**的布局方式，由纵横相交的**两组网格线**形成的框架性布局结构，能够同时处理行与列

擅长将一个页面划分为几个主要区域，以及定义这些区域的大小、位置、层次等关系

![img](https://static.vue-js.com/59680a40-9a94-11eb-85f6-6fac77c0c9b3.png)

这与之前讲到的**`flex`一维布局**不相同

设置`display:grid/inline-grid`的元素就是网格布局容器，这样就能出发浏览器渲染引擎的网格布局算法

```css
<div class="container">
    <div class="item item-1">
        <p class="sub-item"></p >
 	</div>
    <div class="item item-2"></div>
    <div class="item item-3"></div>
</div> 
```

上述代码实例中，`.container`元素就是网格布局容器，`.item`元素就是网格的项目，由于**网格元素只能是容器的顶层子元素**，所以`p`元素并不是网格元素

这里提一下，网格线概念，有助于下面对`grid-column`系列属性的理解

网格线，即划分网格的线，如下图所示：

![img](https://static.vue-js.com/61be7080-9a94-11eb-ab90-d9ae814b240d.png)

上图是一个 2 x 3 的网格，共有3根水平网格线和4根垂直网格线

## 属性

同样，`Grid` 布局属性可以分为两大类：

- 容器属性，
- 项目属性

### 容器属性

**display 属性**

文章开头讲到，在元素上设置`display：grid` 或 `display：inline-grid` 来创建一个网格容器

- display：grid 则该容器是一个块级元素
- display: inline-grid 则容器元素为行内元素

**grid-template-columns 属性，grid-template-rows 属性**

`grid-template-columns` 属性设置列宽，`grid-template-rows` 属性设置行高

```css
.wrapper {
  display: grid;
  /*  声明了三列，宽度分别为 200px 200px 200px */
  grid-template-columns: 200px 200px 200px;
  grid-gap: 5px;
  /*  声明了两行，行高分别为 50px 50px  */
  grid-template-rows: 50px 50px;
}
```

以上表示固定列宽为 200px 200px 200px，行高为 50px 50px

上述代码可以看到重复写单元格宽高，通过使用`repeat()`函数，可以简写重复的值

- 第一个参数是重复的次数
- 第二个参数是重复的值

所以上述代码可以简写成

```css
.wrapper {
  display: grid;
  grid-template-columns: repeat(3,200px);
  grid-gap: 5px;
  grid-template-rows:repeat(2,50px);
}
```

除了上述的`repeact`关键字，还有：

- auto-fill：示自动填充，让一行（或者一列）中尽可能的容纳更多的单元格

> `grid-template-columns: repeat(auto-fill, 200px)` 表示列宽是 200 px，但列的数量是不固定的，只要浏览器能够容纳得下，就可以放置元素

- fr：片段，为了方便表示比例关系

> `grid-template-columns: 200px 1fr 2fr` 表示第一个列宽设置为 200px，后面剩余的宽度分为两部分，宽度分别为剩余宽度的 1/3 和 2/3

- minmax：产生一个长度范围，表示长度就在这个范围之中都可以应用到网格项目中。第一个参数就是最小值，第二个参数就是最大值

> ```
> minmax(100px, 1fr)`表示列宽不小于`100px`，不大于`1fr
> ```

- auto：由浏览器自己决定长度

> `grid-template-columns: 100px auto 100px` 表示第一第三列为 100px，中间由浏览器决定长度

**grid-row-gap 属性， grid-column-gap 属性， grid-gap 属性**

`grid-row-gap` 属性、`grid-column-gap` 属性分别设置行间距和列间距。`grid-gap` 属性是两者的简写形式

`grid-row-gap: 10px` 表示行间距是 10px

`grid-column-gap: 20px` 表示列间距是 20px

`grid-gap: 10px 20px` 等同上述两个属性

**grid-template-areas 属性**

用于定义区域，一个区域由一个或者多个单元格组成

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
  grid-template-areas: 'a b c'
                       'd e f'
                       'g h i';
}
```

上面代码先划分出9个单元格，然后将其定名为`a`到`i`的九个区域，分别对应这九个单元格。

多个单元格合并成一个区域的写法如下

```css
grid-template-areas: 'a a a'
                     'b b b'
                     'c c c';
```

上面代码将9个单元格分成`a`、`b`、`c`三个区域

如果某些区域不需要利用，则使用"点"（`.`）表示

**grid-auto-flow 属性**

划分网格以后，容器的子元素会按照顺序，自动放置在每一个网格。

顺序就是由`grid-auto-flow`决定，默认为行，代表"先行后列"，即先填满第一行，再开始放入第二行

![img](https://static.vue-js.com/70fb3240-9a94-11eb-ab90-d9ae814b240d.png)

当修改成`column`后，放置变为如下：

![img](https://static.vue-js.com/7c26ffa0-9a94-11eb-ab90-d9ae814b240d.png)

**justify-items 属性， align-items 属性， place-items 属性**

`justify-items` 属性设置单元格内容的水平位置（左中右），`align-items` 属性设置单元格的垂直位置（上中下）

两者属性的值完成相同

```css
.container {
  justify-items: start | end | center | stretch;
  align-items: start | end | center | stretch;
}
```

属性对应如下：

- start：对齐单元格的起始边缘
- end：对齐单元格的结束边缘
- center：单元格内部居中
- stretch：拉伸，占满单元格的整个宽度（默认值）

`place-items`属性是`align-items`属性和`justify-items`属性的合并简写形式

**justify-content 属性， align-content 属性， place-content 属性**

`justify-content`属性是整个内容区域在容器里面的水平位置（左中右），`align-content`属性是整个内容区域的垂直位置（上中下）

```css
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
}
```

两个属性的写法完全相同，都可以取下面这些值：

- start - 对齐容器的起始边框
- end - 对齐容器的结束边框
- center - 容器内部居中

![img](https://static.vue-js.com/9d1ec990-9a94-11eb-ab90-d9ae814b240d.png)

- space-around - 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与容器边框的间隔大一倍
- space-between - 项目与项目的间隔相等，项目与容器边框之间没有间隔
- space-evenly - 项目与项目的间隔相等，项目与容器边框之间也是同样长度的间隔
- stretch - 项目大小没有指定时，拉伸占据整个网格容器

![img](https://static.vue-js.com/a620b210-9a94-11eb-85f6-6fac77c0c9b3.png)

**grid-auto-columns 属性和 grid-auto-rows 属性**

有时候，一些项目的指定位置，在现有网格的外部，就会产生显示网格和隐式网格

比如网格只有3列，但是某一个项目指定在第5行。这时，浏览器会自动生成多余的网格，以便放置项目。超出的部分就是隐式网格

而`grid-auto-rows`与`grid-auto-columns`就是专门用于指定隐式网格的宽高

### 项目属性

**grid-column-start 属性、grid-column-end 属性、grid-row-start 属性以及grid-row-end 属性**

指定网格项目所在的四个边框，分别定位在哪根网格线，从而指定项目的位置

- grid-column-start 属性：左边框所在的垂直网格线
- grid-column-end 属性：右边框所在的垂直网格线
- grid-row-start 属性：上边框所在的水平网格线
- grid-row-end 属性：下边框所在的水平网格线

举个例子：

```html
<style>
    #container{
        display: grid;
        grid-template-columns: 100px 100px 100px;
        grid-template-rows: 100px 100px 100px;
    }
    .item-1 {
        grid-column-start: 2;
        grid-column-end: 4;
    }
</style>

<div id="container">
    <div class="item item-1">1</div>
    <div class="item item-2">2</div>
    <div class="item item-3">3</div>
</div>
```

通过设置`grid-column`属性，指定1号项目的左边框是第二根垂直网格线，右边框是第四根垂直网格线

![img](https://static.vue-js.com/b7925530-9a94-11eb-ab90-d9ae814b240d.png)

**grid-area 属性**

`grid-area` 属性指定项目放在哪一个区域

```css
.item-1 {
  grid-area: e;
}
```

意思为将1号项目位于`e`区域

与上述讲到的`grid-template-areas`搭配使用

**justify-self 属性、align-self 属性以及 place-self 属性**

`justify-self`属性设置单元格内容的水平位置（左中右），跟`justify-items`属性的用法完全一致，但只作用于单个项目。

`align-self`属性设置单元格内容的垂直位置（上中下），跟`align-items`属性的用法完全一致，也是只作用于单个项目

```css
.item {
  justify-self: start | end | center | stretch;
  align-self: start | end | center | stretch;
}
```

这两个属性都可以取下面四个值。

- start：对齐单元格的起始边缘。
- end：对齐单元格的结束边缘。
- center：单元格内部居中。
- stretch：拉伸，占满单元格的整个宽度（默认值）

### 应用场景

文章开头就讲到，`Grid`是一个强大的布局，如一些常见的 CSS 布局，如居中，两列布局，三列布局等等是很容易实现的，在以前的文章中，也有使用`Grid`布局完成对应的功能

关于兼容性问题，结果如下：

![img](https://static.vue-js.com/c24a2b10-9a94-11eb-85f6-6fac77c0c9b3.png)

总体兼容性还不错，但在 IE 10 以下不支持

目前，`Grid`布局在手机端支持还不算太友好



# sprites精灵图

### css sprites

css sprite又叫css精灵/雪碧图，是把网页中的一些背景图整合到一张图片中，再通过background-position属性进行图片定位。

### 优点

1. **减少http请求**，提高页面加载速度，减轻服务器的负担
2. 风格更换方便，只需要修改一张图片就可以改变整个网页的风格
3. 命名简单，只需要给一张整合的图片命名即可

### 缺点

1. **定位繁琐**，需要精确计算图片大小和图片位置
2. **维护麻烦**，修改一个地方可能需要重新布局整张图片、样式
3. 宽屏、高分屏下的自适应页面，如果图片宽度不够，可能会出现背景断裂的现象



# css 画基本形状

### 画三角形

然后我们可以通过给任意三边的颜色设置为 `transparent` 即可分别实现任一方向的三角形。

通过设置某条边的宽度比其x它边宽，来调整三角形的高度。

```css
    <style>
        .triangle {
            width: 0;
            height: 0;
            border: 100px solid transparent;
            border-bottom: 200px solid #0ff;
        }
    </style>
```

### 画梯形

梯形也是基于 border 来绘制的，只不过绘制梯形时，宽高和border尺寸相同。

```css
    .trapezoid {
      width: 50px;
      height: 50px;
      background: transparent;
      border-top: 50px solid #f00;
      border-bottom: 50px solid transparent;
      border-left: 50px solid transparent;
      border-right: 50px solid transparent;
    }
```

### 画扇形

原理：左上角是圆角，其余三个角都是直角：左上角的值为宽和高一样的值，其他三个角的值不变（等于0）。 `border-radius`四个值的顺序是：左上角，右上角，右下角，左下角。

```css
    <style>
        .sector1 {
            border-radius:100px 0 0;
            width: 100px;
            height: 100px;
            background: #00f;
        }

    </style>
```

### 画圆

```css
    <style>
        .sector1 {
            border-radius:50%;
            width: 100px;
            height: 100px;
            background: #00f;
        }

    </style>
```

### 画椭圆

椭圆依旧依赖 `border-radius` 属性，很多人应该都没注意过，border-radius 其实可以设置水平半径和垂直半径两个值 ，具体用法为 `border-radius: 水平半径 / 垂直半径;`.

```css
<style>
        .oval {
            width: 100px;
            height: 50px;
            background: #ff0;
            border-radius: 50px / 25px;
        }
</style>
<div class="oval"></div>
```



# ⚝WEB动画

## ⚝主要实现方式

通常我们在前端实现动画效果的几种主要实现方式如下：

> - JavaScript：通过定时器（setTimeout 和 setIterval）来间隔来改变元素样式，或者使用requestAnimationFrame；
> - CSS3：transition 和 animation；
> - HTML5：使用HTML5提供的绘图方式（canvas、svg、webgl）；（JS调用实现）

## requestAnimationFrame

`requestAnimationFrame`是浏览器用于定时循环操作的一个接口，类似于`setTimeout`，主要用途是**按帧对网页进行重绘**。

设置这个API的目的是**为了让各种网页动画效果（DOM动画、Canvas动画、SVG动画、WebGL动画）能够有一个统一的刷新机制，从而节省系统资源，提高系统性能，改善视觉效果**。代码中使用这个API，就是告诉浏览器希望执行一个动画，让浏览器在下一个动画帧安排一次网页重绘。

`requestAnimationFrame`使用一个回调函数作为参数，这个回调函数会在浏览器重绘之前调用，由于功效只是一次性的，所以想实现连续的动效，需要递归调用

cancelAnimationFrame方法用于取消重绘

**使用requestAnimationFrameAPI的优势如下：**

- 会把每一帧中的所有DOM操作集中起来，在一次重绘或回流中就完成，并且重绘或回流的时间间隔紧紧跟随显示器的刷新频率（60 Hz或者75 Hz）；
- 在隐藏或不可见的元素中，将不会进行重绘或回流，这当然就意味着更少的的cpu，gpu和内存使用量；

## Transition

CSS 中的 transition 属性允许块级元素中的属性在指定的时间内平滑的改变，简单看下其语法规则：

```php
transition: property duration timing-function delay;
```

**看下面详解**

## Animation

类似的CSS还提供了一个Animation属性，不过区别于Transition，Animation作用于元素本身而不是样式属性，可以使用关键帧的概念，应该说可以实现更自由的动画效果。

```php
animation: name duration timing-function delay iteration-count direction;
```

**看下面详解**

## Canvas

`<canvas>`是**HTML5新增的元素，作为页面图形绘制的容器，可用于通过使用JavaScript中的脚本来绘制图形**。例如，**它可以用于绘制形状，绘制路径、绘制文本、变换、制作照片，创建动画，甚至可以进行实时视频处理或渲染**，Canvas具有如下特点：

- 依赖分辨率，基于位图；
- 不支持事件处理器；
- 弱的文本渲染能力；
- 能够以 .png 或 .jpg 格式保存结果图像；
- 最适合图像密集型的游戏，其中的许多对象会被频繁重绘； 
- 大多数 Canvas 绘图 API 都没有定义在 <canvas> 元素本身上，而是定义在通过画布的`getContext()`方法获得的一个“绘图环境”对象上。Canvas API也使用了路径的表示法，**路径由一系列的方法调用来定义，比如调用 `beginPath()` 和 `arc()绘制弧线`和lineTo()和moveTo() 方法。一旦定义了路径**，其他的方法，如 `fill()`，都是对此路径操作。

#### **渲染上下文**

**getContext()** ，通过它我们可以获得渲染上下文和绘画功能，getContext方法是有一个接收参数，它是绘图上下文的类型，参数有：

- 2d：建立一个二维渲染上下文。
- webgl（或 experimental-webgl）： 创建一个 WebGLRenderingContext 三维渲染上下文对象。实现WebGL 版本1
- webgl2（或 experimental-webgl2）：创建一个 WebGL2RenderingContext 三维渲染上下文对象。实现 WebGL 版本2
- bitmaprenderer：创建一个只提供将canvas内容替换为指定ImageBitmap功能的ImageBitmapRenderingContext。

#### 绘制形状

**直线**  ： moveTo(x, y) lineTo(x, y) stroke()

**三角形**： 三条直线拼

**矩形**： strokeRect(x, y, width, height) 绘制一个矩形的边框  fillRect(x, y, width, height) 绘制一个填充的矩形

**圆弧和圆**： arc(x, y, radius, startAngle, endAngle, anticlockwise)。x和Y为圆心的坐标，radius为半径，startAngle为圆弧或圆的开始位置，endAngle为圆弧或圆的结束位置，anticlockwise是绘制的方向

**椭圆** ： ellipse(x, y, radiusX, radiusY, rotation, startAngle, endAngle, anticlockwise)

**贝塞尔曲线**： 一般用来绘制复杂有规律的图形quadraticCurveTo(cp1x, cp1y, x, y)，其中cp1x和cp1y为一个控制点，x和y为结束点。

#### 绘制文本

提供了两种方法来渲染文本，一种是描边一种是填充。

**strokeText（描边）**

语法：ctx.strokeText(text, x, y, maxWidth)

**fillText（填充）**

语法：ctx.fillText(text, x, y, maxWidth)

**文本样式** font textAlign(对齐方式) direction textBaseline

**阴影** shadowOffsetX、shadowOffsetY延伸距离 shadowBlur模糊程度 shadowColor

#### 绘制图片

**drawImage**他的用法有三种，是根据不同的传参实现不同的功能。

drawImage(image, dx, dy)：只有单纯的绘制功能，可以绘制图片、视频和别的Canvas对象等。

drawImage(image, dx, dy, dWidth, dHeight)：在绘制的基础上增加了两个参数，这两个参数能控制绘制元素的大小，整体实现一个缩放的效果。

drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)：在缩放的基础上又增加了四个参数，整体也是在缩放的基础上增加了裁剪的功能。

#### 变形

- 移动：translate(x, y) ，x 是左右偏移量，y 是上下偏移量。
- 旋转：rotate(angle)，angle是旋转的角度，它是顺时针旋转，以弧度为单位的值。
- 缩放：scale(x, y)，x 为水平缩放的值，y 为垂直缩放得值。x和y的值小于1则为缩小，大于1则为放大。默认值为 1
- transform(a, b, c, d, e, f)方法能将当前的变形矩阵乘上一个基于自身参数的矩阵；
- setTransform(a, b, c, d, e, f)方法会将当前变形矩阵重置为单位矩阵，然后用相同的参数调用 transform 方法
- resetTransform()方法为重置当前变形为单位矩阵。效果等同于调用 setTransform(1, 0, 0, 1, 0, 0)
- 状态的保存和恢复 ：save() 和 restore() 方法是用来保存和恢复 canvas 状态的，方法不需要参数。可以理解为就是对canvas 状态的快照进行保存和恢复。

#### canvas动画

那么为了实现动画，我们需要一些可以定时执行重绘的方法。

- setInterval(function, delay) ：定时器，当设定好间隔时间后，function 会定期执行。
- setTimeout(function, delay)：延时器，在设定好的时间之后执行函数
- requestAnimationFrame(callback)：告诉浏览器你希望执行一个动画，并在重绘之前，请求浏览器执行一个特定的函数来更新动画。



## WebGL

WebGL（全写Web Graphics Library）是一种3D绘图标准，这种**绘图技术标准允许把JavaScript和OpenGL ES 2.0结合在一起。**

通过增加OpenGL ES 2.0的一个JavaScript绑定，WebGL可以为HTML5 Canvas提供**硬件3D加速渲染**，这样Web开发人员就可以借助系统显卡来在浏览器里更流畅地展示3D场景和模型了，还能创建复杂的**导航和数据视觉化**。

## SVG

SVG是英文Scalable Vector Graphics的缩写，意为可缩放矢量图形，用来定义用于网络的基于矢量的图形，其使用 XML 格式定义图像，并且具有如下特点：

> - 不依赖分辨率，基于矢量图；
> - 支持事件处理器；
> - 最适合带有大型渲染区域的应用程序（比如谷歌地图）；
> - 复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）；
> - 不适合游戏应用；

来看一个简单的示例，用SVG画了一个圆：

```xml
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <rect x="50" y="20" rx="20" ry="20" width="150" height="150"
  style="fill:red;stroke:black;stroke-width:5;opacity:0.5"/>
</svg>
```

SVG 代码以 `<svg>` 元素开始，包括开启标签 `<svg>` 和关闭标签 `</svg>` 。这是根元素。**width** 和 **height** 属性可设置此 SVG 文档的宽度和高度。**version** 属性可定义所使用的 SVG 版本，**xmlns** 属性可定义 SVG 命名空间。

<path>标签 **path 标签**，最重要的属性是 d 属性，它是一组指令和参数的集合

- d 属性里的那些指令

| 指令  | 参数                                                    | 含义                                                         |
| ----- | ------------------------------------------------------- | ------------------------------------------------------------ |
| **M** | **x y**                                                 | **将画笔移动到点(x,y)**                                      |
| **L** | **x y**                                                 | **画笔从当前的点绘制线段到点(x,y)**                          |
| **H** | **x**                                                   | **画笔从当前的点绘制水平线段到点(x,y0)，y0 表示绘制前画笔所在 y 轴坐标，也就是 y 轴不变** |
| **V** | **y**                                                   | **画笔从当前的点绘制竖直线段到点(x0,y)，x0 表示绘制前画笔所在 x 轴坐标，也就是 x 轴不变** |
| **A** | **rx ry x-axis-rotation large-arc-flag sweep-flag x y** | **画笔从当前的点绘制一段圆弧到点(x,y)**                      |
| C     | x1 y1, x2 y2, x y                                       | 画笔从当前的点绘制一段三次贝塞尔曲线到点(x,y)                |
| S     | x2 y2, x y                                              | 特殊版本的三次贝塞尔曲线(省略第一个控制点)                   |
| Q     | x1 y1, x y                                              | 绘制二次贝塞尔曲线到点(x,y)                                  |
| T     | x y                                                     | 特殊版本的二次贝塞尔曲线(省略控制点)                         |
| **Z** | **无参数**                                              | **绘制闭合图形，如果 d 属性不指定Z命令，则绘制线段，而不是封闭图形** |

**基本图形**这块相对比较好理解，我们直接一张表总结下，不做过多赘述

| 图形   | 标签         | 模板                                                         | 含义                                                         |
| ------ | ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 矩形   | < rect >     | `<rect x="60" y="10" rx="10" ry="10" width="30" height="30"/>` | x:起点横坐标，y:起点纵坐标，rx:倒角x轴方向半径，ry:倒角x轴方向半径，width:宽度，height:高度 |
| 圆形   | < circle >   | `<circle cx="100" cy="100" r="50" fill="#fff"></circle>`     | cx:圆心横坐标，cy:圆心纵坐标，r:半径                         |
| 椭圆   | < ellipse >  | `<ellipse cx="75" cy="75" rx="20" ry="5"/>`                  | cx:椭圆心横坐标，cy:椭圆心纵坐标，rx:椭圆x轴方向半径，ry:椭圆y轴方向半径 |
| 直线   | < line >     | `<line x1="10" x2="50" y1="110" y2="150"/>`                  | x1，y1:起点，x2，y2:终点                                     |
| 折线   | < polyline > | `<polyline points="60 110, 65 120, 70 115, 75 130, 80 125, 85 140, 90 135, 95 150, 100 145"/>` | 每两个点以空格配对为一个坐标点，逗号隔开形成坐标集合。连成折线。 |
| 多边形 | < polygon >  | `<polygon points="50 160, 55 180, 70 180, 60 190, 65 205, 50 195, 35 205, 40 190, 30 180, 45 180"/>` | 类似折线，不同的是，最后一个点会自动                         |

**symbol标签**

相当于是一个元件，放在我们的工具箱里，放一份就可以无限引用，就像下面这样：

```html
<svg class="svg-sprite">[工具箱]
	<symbol id="icon-wave_add" viewBox="0 0 76 76"><path d="M38 0a4 4 0 014 4v30h30a4 4 0 110 8H41.999L42 72a4 4 0 11-8 0l-.001-30H4a4 4 0 110-8h30V4a4 4 0 014-4z" fill="currentColor" fill-rule="evenodd" opacity="none"></path></symbol>
</svg>
```



下面主要是介绍SVG中的几个用于动画的元素，它们分别是：

> **<animate>**：通常放置到一个SVG图像元素里面，用来定义这个图像元素的某个属性的动画变化过程；
> **<animateMotion>**：元素也是放置一个图像元素里面，它可以引用一个事先定义好的动画路径，让图像元素按路径定义的方式运动；
> **<animateTransform>**：元素对图形的运动和变换有更多的控制，它可以指定图形的变换、缩放、旋转和扭曲等；
> **<mpath>**：元素的用法在上面的例子里出现过，它是一个辅助元素，通过它，`<animateMotion>`等元素可以引用一个外部的定义的`<path>`。让图像元素按这个`<path>`轨迹运动；



## ⚝css3动画与js动画的区别

### JS动画

缺点：

(1)JavaScript在浏览器的**主线程中运行**，而主线程中还有其它需要运行的JavaScript脚本、样式计算、布局、绘制任务等,**对其干扰导致线程可能出现阻塞**，从而造成丢帧的情况。

(2)代码的**复杂度高于CSS动画**

(3)**因为js动画是逐帧动画，会增加制作负担，资源占有量也比较大**。

优点：

(1)JavaScript**动画控制能力很强**, 可以在动画播放过程中对动画进行控制：开始、暂停、回放、终止、取消都是可以做到的。

(2)**动画效果比css3动画丰富,**有些动画效果，比如曲线运动,冲击闪烁,视差滚动效果，只有JavaScript动画才能完成

(3)CSS3有兼容性问题，而**JS大多时候没有兼容性问题**

### CSS动画

缺点：

(1)**运行过程控制较弱,无法附加事件绑定回调函数**。CSS动画只能暂停,不能在动画中寻找一个特定的时间点，不能在半路反转动画，不能变换时间尺度，不能在特定的位置添加回调函数或是绑定回放事件,无进度报告。

(2)**代码冗长**。想用 CSS 实现稍微复杂一点动画,最后CSS代码都会变得非常笨重。

优点：

(1)**浏览器可以对动画进行优化**，会比js使用更少的占用cpu资源，但是效果足够流畅。。

(2)因为CSS动画是**补间动画**，只需确定第一帧和最后一帧的关键位置即可，两个关键帧之间帧的内容由Composite线程自动生成，不需要人为处理。当然也可以多次添加[关键帧](https://www.zhihu.com/search?q=关键帧&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A1627856428})的位置。**代码相对简单**,性能调优方向固定。

(3)对于帧速表现不好的低版本浏览器，CSS3可以做到自然降级，而JS则需要撰写额外代码

### CSS动画流畅原因

渲染线程分为main thread(主线程)和compositor thread(合成器线程)。

如果CSS动画只是改变`transform`和`opacity`，这时整个CSS动画得以在compositor thread完成（而JS动画则会在main thread执行，然后触发compositor进行下一步操作）。

在JS执行一些昂贵的任务时，main thread繁忙，CSS动画由于使用了compositor thread可以保持流畅。

### 结论

如果动画只**是简单的状态切换**，不需要中间过程控制，在这种情况下，**css动画是优选方案**。它可以让你将动画逻辑放在样式文件里面，而不会让你的页面充斥 Javascript 库。然而如果你在设计很复杂的富客户端界面或者在开发一个有着**复杂UI状态**的 APP。那么你应该**使用js动画**，这样你的动画可以保持高效，并且你的工作流也更可控。所以，在实现一些小的交互动效的时候，就多考虑考虑CSS动画。对于一些复杂控制的动画，使用javascript比较可靠。



# 动画的优化

#### 1、使用gpu加速

动画的性能优化的方向，**减少元素的重绘与回流**。

这其中，如何减少页面的回流与重绘呢，这里就会运用到我们常说的**GPU 加速**。

**GPU 加速的本质其实是减少浏览器渲染页面每一帧过程中的 reflow 和 repaint，其根本，就是让需要进行动画的元素，生成自己的 GraphicsLayer。**

浏览器渲染一个页面时，它使用了许多没有暴露给开发者的中间表现形式，其中最重要的结构便是层(layer)。

在 Chrome 中，存在有不同类型的层： RenderLayer(负责 DOM 子树)，GraphicsLayer(负责 RenderLayer 的子树)。

GraphicsLayer的作用就是 将一个层的内容在作为纹理上传到 GPU 前先**绘制(paint)进一个位图**中。如果内容不会改变，那么就没有必要重绘(repaint)层。

**而当元素生成了自己的 GraphicsLayer 之后，在动画过程中，Chrome 并不会始终重绘整个层，它会尝试智能地去重绘 DOM 中失效的部分，也就是发生动画的部分**，在 Composite 之前，页面是处于一种分层状态，借助 GPU，浏览器仅仅在每一帧对生成了自己独立 GraphicsLayer 元素层进行重绘，如此，大大的降低了整个页面重排重绘的开销，**提升了页面渲染的效率。**

因此，CSS 动画（Web 动画同理）优化的第一条准则就是**让需要动画的元素生成了自己独立的 GraphicsLayer，强制开始 GPU 加速**，而我们需要知道是，GPU 加速的本质是利用让元素生成了自己独立的 GraphicsLayer，降低了页面在渲染过程中重绘重排的开销。

除了上述准则之外，还有一些提升 CSS 动画性能的建议：

#### 2、requestAnimationFrame代替setTimeout和setInterval

**为什么`setTimeout`和`setInterval`不好？**
由于js是单线程执行，所以为了防止某个任务执行时间过长而导致进程阻塞，js中存在异步队列的概念，对于如`setTimeout`和`ajax`请求都是把进程放到了异步队列中，当主进程为空时才执行异步队列中的任务。所以 `setTimeout`和`setInterval`无**法保证回调函数的执行时机，可能会在一帧之内执行多次导致多次页面渲染，浪费CPU资源甚至产生卡顿，或者是在一帧即将结束时执行导致重新渲染，出现掉帧的情况。**
**`requestAnimationFrame`是怎么优化的？**

- CPU节能，当页面被隐藏或最小化时，暂停渲染。
- 函数节流，其循环间隔是由屏幕刷新频率决定的，保证回调函数在屏幕的每一次刷新间隔中只执行一次。

#### **3、使用 will-change 提高页面滚动、动画等渲染性能**

`will-change` ：CSS3 will-change属于web标准属性,**提前通知浏览器元素将要做什么动画，让浏览器提前准备合适的优化设置**

```js
will-change: auto      //表示没有特别指定哪些属性会变化，需要浏览器自己去猜，
will-change: scroll-position  //表示开发者希望在不久后改变滚动条的位置或者使之产生动画
will-change: contents     //表示开发者希望在不久后改变元素内容中的某些东西，或者使它们产生动画
will-change: transform        // Example of <custom-ident> 
will-change: opacity          // Example of <custom-ident>
will-change: left, top        // Example of two <animateable-feature>

will-change: unset
will-change: initial
will-change: inherit
```

#### **4、减少使用耗性能样式**

不同样式在消耗性能方面是不同的，改变一些属性的开销比改变其他属性要多，因此更可能使动画卡顿。

例如，与改变元素的文本颜色相比，改变元素的 **box-shadow(添加阴影)** 将需要开销大很多的绘图操作。从渲染角度来讲十分耗性能，原因就是与其他样式相比，它们的绘制代码执行时间过长。这就是说，如果一个耗性能严重的样式经常需要重绘，那么你就会遇到性能问题。

```js
box-shadow: *h-shadow v-shadow blur spread color* inset;//水平阴影的位置 垂直阴影的位置 模糊距离 大小 颜色 从外层改变内侧阴影
```

类似的还有 CSS 3D 变换、`mix-blend-mode`、`filter`，这些样式相比其他一些简单的操作，会更加的消耗性能。我们应该尽可能的在动画过程中**降低其使用的频率或者寻找替代方案。**

当然，没有不变的事情，在今天性能很差的样式，可能明天就被优化，并且浏览器之间也存在差异。

因此**关键在于，我们需要针对每一起卡顿的例子，借助开发工具来分辨出性能瓶颈所在，然后设法减少浏览器的工作量**。学会 Chrome 开发者工具的 Performance 面板及其他渲染相关的面板非常重要

#### 5、用flexbox布局替代老的布局模型

常用的经典布局方案有基于浮动的布局、基于绝对定位的布局，flexbox布局相较而言更加高效。在能用flexbox布局的项目中，尽量用flexbox布局

# 动画animation

CSS 动画用于实现元素从一个 CSS 样式配置转换到另一个 CSS 样式配置。

动画包括两个部分: **描述动画的样式规则**和用于**指定动画开始、结束以及中间点样式的关键帧**。

```CSS
div {
    animation: change 3s;
}

@keyframes change {
    0% {
        color: #f00;
    }
    100% {
        color: #000;
    }
}
```

1. `animation: move 1s` 部分就是动画的第一部分，用于描述动画的各个规则;
2. `@keyframes move {}` 部分就是动画的第二部分，用于指定动画开始、结束以及中间点样式的关键帧;

一个 CSS 动画一定要由上述两部分组成。

## CSS 动画的语法

接下来，我们简单看看 CSS 动画的语法。

创建动画序列，需要使用 animation 属性或其子属性，该属性允许配置**动画时间、时长以及其他动画细节**，但该属性不能配置动画的实际表现，动画的实际表现是由 @keyframes 规则实现。

animation 的子属性有：

- animation-name：指定由 @keyframes 描述的关键帧**名称**。
- animation-duration：设置动画一个周期的**时长**。
- animation-delay：设置**延时**，即从元素加载完成之后到动画序列开始执行的这段时间。
- animation-direction：设置动画在每次**运行完**后是反向运行还是重新回到开始位置重复运行。
- animation-iteration-count：设置动画**重复次数**， 可以指定 infinite 无限次重复动画
- animation-play-state：允许**暂停和恢复**动画。
- animation-timing-function：设置**动画速度**， 即通过建立加速度曲线，设置动画在关键帧之间是如何变化。
- animation-fill-mode：指定动画执行前后如何为目标元素应用样式
- @keyframes 规则，当然，一个动画想要运行，还应该包括 @keyframes 规则，在内部设定动画关键帧

其中，对于一个动画：

- **必须项**：`animation-name`、`animation-duration` 和 `@keyframes`规则
- **非必须项**：`animation-delay`、`animation-direction`、`animation-iteration-count`、`animation-play-state`、`animation-timing-function`、`animation-fill-mode`，当然不是说它们不重要，只是不设置时，它们都有默认值

**@keyframes:**

```
@keyframes animationname {keyframes-selector {css-styles;}}
```

| 值                 | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| animationname      | 必需, 定义动画的名称                                         |
| keyframes-selector | 必需, 定义动画的多个中间态 合法值: 0-100% from(和0%相同) to(和100%相同) |
| css-styles         | 必需的, 一个或多个合法的CSS样式属性                          |



## animation-delay 详解

`animation-delay` 就比较有意思了，它可以设置动画延时，即从元素加载完成之后到动画序列开始执行的这段时间。

**animation-delay 可以为负值**

关于 `animation-delay`，最有意思的技巧在于，它可以是负数。也就是说，虽然属性名是**动画延迟时间**，但是运用了负数之后，动画可以**提前进行**。

## animation-timing-function 缓动函数

缓动函数在动画中非常重要，它定义了动画在每一动画周期中执行的节奏。

缓动主要分为两类：

1. cubic-bezier-timing-function 三次贝塞尔曲线缓动函数
2. step-timing-function 步骤缓动函数（这个翻译是我自己翻的，可能有点奇怪）

## animation-play-state

接下来，我们讲讲 `animation-play-state`，顾名思义，它可以控制动画的状态 -- 运行或者暂停。类似于视频播放器的开始和暂停。是 CSS 动画中有限的控制动画状态的手段之一。

它的取值只有两个（默认为 running）：

```CSS
{
    animation-play-state: paused | running;
}
```



# 动画animation和过渡transition和2D转换 transform

## **transform转换**

`transform`主要用于给元素做变换,主要由以下几种变换,`rotate`(旋转),`scale`(缩放),`skew`(扭曲),`translate`(移动)和`matrix`(矩阵变换).

**语法:**

```css
transform: none | transform-functions
```

`none`表示不做变换, `transform-functions`表示变化函数,可以使一个变换函数或者多个变换函数的组合.

```css
.transform-multi:hover {
  transform: scale(1.2) rotate(30deg);
}
```

包含四个常用的功能：

- translate：位移
- scale：缩放
- rotate：旋转
- skew：倾斜

一般配合`transition`过度使用

## **transition过渡**

用来定义某个css属性或者多个css属性的变化的过渡效果.

**属性定义及使用说明:**

| 值                         | 描述                                                         |
| -------------------------- | ------------------------------------------------------------ |
| transition-property        | 指定CSS属性的name，transition效果: 大小,位置,扭曲等          |
| transition-duration        | 规定完成过渡效果需要花费的时间（以秒或毫秒计）。 默认值是 0，意味着不会有效果。 |
| transition-timing-function | 指定transition效果的转速曲线                                 |
| transition-delay           | 动画效果的延迟触发时间                                       |

1. 需要具体数值，不能用block,none等
2. transition**需用事件触发**【比如加个hover伪类】，不能在网页加载时自动发生
3. 一次性，不能重复发生，除非**一再触发**
4. **只有两个状态：开始和结束状态**
5. 一条transition规则只能定义一个属性
6. 并不是所有的属性都能使用过渡的，如`display:none display:block`

animation就是为了解决以上问题的

## transition和transform结合例子

transition和transform结合实现动画过渡

**html:**

```css
<h2>利用transition和transform结合实现动画过渡</h2>
<div class="boll1" id="boll1"></div>
```

**css:**

```css
.boll1 {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: #03A9F4;
  transform: translateY(0px);
  margin-bottom: 300px;
  transition: all cubic-bezier(0.42,0,0.58,1) 2.0s 1.0s;
  cursor: pointer
}
.boll1:hover {
  transform: translateY(200px);
}
```

**效果图:**

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/12/8/16ee5455e807b1c5~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)



日常的菜单交互,鼠标移进放大,小球下落等交互都可以使用transition实现,transition一般只有两个状态,始态和终态.一般只有两个状态的单次动画交互使用transition.

## animation和transition区别

- transform**本身是没有过渡**效果的,它只是对元素做大小,旋转,倾斜等各种变换,通过和transition或者animation相结合,可以让这一变换过程具有动画的效果,它通常只有一个到达态,中间态的过渡可以通过和**transition或者animation相结合实现**,也可以通过js设置定时器来实现.
- transition一般是定义单个或多个css属性发生变化时的过渡动画,比如width,opacity等.当定义的css属性发生变化的时候才会执行过渡动画,animation动画一旦定义,就会在页面加载完成后自动执行.
- transition定义的动画触发一次**执行一次**,想要再次执行想要再次触发.animation定义的动画可以**指定播放次数**或者无限循环播放. transition: 需要用户操作,执行次数固定.
- transition定义的动画**只有两个状态**,开始态和结束态,animation可以定义**多个动画中间态,**且可以控制多个复杂动画的有序执行.



# JS CSS 实现返回顶部

## 锚点法CSS回到顶部

原理是利用a标签 href="#"**回到顶部**，设置**样式scroll-behavior: smooth**来设置**平滑移动**

html：

```html
<body>
  <a class="back" href="#"></a><!--添加再这里就行了-->
  <article>
  ...很多内容
  </article>
</body>
```

css

```css
html,body{
  scroll-behavior: smooth;
}
.back{
  position: sticky;
  float: right;
  top: -110px;
  margin-top: -50px;
  border-radius: 50%;
  background: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 320 512'%3E %3Cpath fill='%23ffffff'") center no-repeat dodgerblue;
  background-size: 50%;
  width: 50px;
  height: 50px;
  transform: translateY(calc(100vh + 50px));
}
```

## JS 回到顶部

1、scrollTop

scrollTop属性表示被隐藏在内容区域上方的像素数。元素未滚动时，scrollTop的值为0，如果元素被垂直滚动了，scrollTop的值大于0，且表示元素上方不可见内容的像素宽度

由于**scrollTop**是可写的，可以利用scrollTop来实现回到顶部的功能。

```html
<body style="height:2000px;">
    <button id="test" style="position:fixed;right:0;bottom:0">回到顶部</button>
    <script>
        test.onclick = function(){
            document.body.scrollTop = document.documentElement.scrollTop = 0;
        }
    </script>
</body>
```

2、scrollTo()

scrollTo(x,y)方法滚动当前window中显示的文档，让文档中由坐标x和y指定的点位于显示区域的左上角

```html
<body style="height:2000px;">
    <button id="test" style="position:fixed;right:0;bottom:0">回到顶部</button>
    <script>
        test.onclick = function(){
            scrollTo(0,0);
        }
    </script>
</body>
```

**js也可以设置scroll-behavior: smooth达到平滑滚动的效果！**

# CSS单位



## 单位

在`css`单位中，可以分为长度单位、绝对单位，如下表所指示

| CSS单位      |                                        |
| ------------ | -------------------------------------- |
| 相对长度单位 | em、ex、ch、rem、vw、vh、vmin、vmax、% |
| 绝对长度单位 | cm、mm、in、px、pt、pc                 |

这里我们主要讲述px、em、rem、vh、vw

## px

px，表示**像素**，所谓像素就是呈现在我们显示器上的一个个小点，每个像素点都是大小等同的，所以像素为计量单位被分在了**绝对长度单位**中

有些人会把`px`认为是相对长度，原因在于在移动端中存在设备像素比，`px`实际显示的大小是不确定的

这里之所以认为`px`为绝对单位，在于`px`的大小和元素的其他属性无关。

## **百分比%**

常见 **相对长度单位**，相对于**父元素**的尺寸的取值，实际使用中，如果父元素是一个非稳定的取值，可能会导致父元素被撑开，而实际值取决于其祖先元素中最近的一个拥有稳定取值的元素。整数取值，并不适用于解决自适应问题。

## em

em是**相对长度单位**。**相对于当前对象内文本的字体尺寸**。如当前对行内文本的字体尺寸未被人为设置，则**相对于浏览器的默认字体尺寸**（`1em = 16px`）

为了简化 `font-size` 的换算，我们需要在`css`中的 `body` 选择器中声明`font-size`= `62.5%`，这就使 em 值变为 `16px*62.5% = 10px`

这样 `12px = 1.2em`, `10px = 1em`, 也就是说只需要将你的原来的`px` 数值除以 10，然后换上 `em`作为单位就行了

特点：

- em 的值并不是固定的
- em 会继承父级元素的字体大小
- em 是相对长度单位。相对于当前对象内文本的字体尺寸。如当前对行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸
- 任意浏览器的默认字体高都是 16px

##  rem

rem，**相对单位**，相对的只是**HTML根元素**`font-size`的值

同理，如果想要简化`font-size`的转化，我们可以在根元素`html`中加入`font-size: 62.5%`

```css
html {font-size: 62.5%;  } /*  公式16px*62.5%=10px  */ 
```

这样页面中1rem=10px、1.2rem=12px、1.4rem=14px、1.6rem=16px;使得视觉、使用、书写都得到了极大的帮助

特点：

- rem单位可谓集相对大小和绝对大小的优点于一身
- 和em不同的是rem总是相对于根元素，而不像em一样使用级联的方式来计算尺寸

## vh、vw

vw ，就是根据**窗口的宽度**，分成100等份，100vw就表示满宽，50vw就表示一半宽。（vw 始终是针对窗口的宽），同理，`vh`则为**窗口的高度**

这里的窗口分成几种情况：

- 在桌面端，指的是浏览器的**可视区域**
- 移动端指的就是布局视口

像`vw`、`vh`，比较容易混淆的一个单位是`%`，不过百分比宽泛的讲是相对于父元素：

- 对于普通定位元素就是我们理解的父元素
- 对于position: absolute;的元素是相对于已定位的父元素
- 对于position: fixed;的元素是相对于 ViewPort（可视窗口）

## 总结

**px**：绝对单位，页面按精确像素展示

**em**：相对单位，基准点为父节点字体的大小，如果自身定义了`font-size`按自身来计算，整个页面内`1em`不是一个固定的值

**rem**：相对单位，可理解为`root em`, 相对根节点`html`的字体大小来计算

**vh、vw**：主要用于**页面视口大小布局**，在页面布局上更加方便简单



# overflow、flow、display、position有哪些值

## CSS Overflow

CSS overflow 属性可以控制内容溢出元素框时在对应的元素区间内添加滚动条。

overflow属性有以下值：

| 值      | 描述                                                     |
| :------ | :------------------------------------------------------- |
| visible | 默认值。内容不会被修剪，会呈现在元素框之外。             |
| hidden  | 内容会被修剪，并且其余内容是不可见的。                   |
| scroll  | 内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。 |
| auto    | 如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。 |
| inherit | 规定应该从父元素继承 overflow 属性的值。                 |

**注意:**overflow 属性只工作于指定高度的块元素上。

## CSS float

float属性指定一个盒子（元素）是否应该浮动。

| 值      | 描述                                                 |
| :------ | :--------------------------------------------------- |
| left    | 元素向左浮动。                                       |
| right   | 元素向右浮动。                                       |
| none    | 默认值。元素不浮动，并会显示在其在文本中出现的位置。 |
| inherit | 规定应该从父元素继承 float 属性的值。                |

## CSS display

display 属性规定元素应该生成的框的类型。

| 值           | 描述                                                         |
| :----------- | :----------------------------------------------------------- |
| none         | 此元素不会被显示。                                           |
| block        | 此元素将显示为块级元素，此元素前后会带有换行符。             |
| inline       | 默认。此元素会被显示为内联元素，元素前后没有换行符。         |
| inline-block | 行内块元素。（CSS2.1 新增的值）                              |
| list-item    | 此元素会作为列表                                             |
| table        | 此元素会作为块级表格来显示（类似 <table>），表格前后带有换行符。 |
| inherit      | 规定应该从父元素继承 display 属性的值。                      |
| flex         | flex是一种弹性布局属性                                       |
| grid         | 栅格模型，支持度都很低                                       |

## CSS position

position属性指定一个元素（静态的，相对的，绝对或固定）的定位方法的类型。

| 值                                                           | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [absolute](https://www.runoob.com/css/css-positioning.html#position-absolute) | 生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。 |
| [fixed](https://www.runoob.com/css/css-positioning.html#position-fixed) | 生成固定定位的元素，相对于浏览器窗口进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。 |
| [relative](https://www.runoob.com/css/css-positioning.html#position-relative) | 生成相对定位的元素，相对于其正常位置进行定位。因此，"left:20" 会向元素的 LEFT 位置添加 20 像素。 |
| [static](https://www.runoob.com/css/css-positioning.html#position-static) | 默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明）。 |
| [sticky](https://www.runoob.com/css/css-positioning.html#position-sticky) | 粘性定位，该定位基于用户滚动的位置。它的行为就像 position:relative; 而当页面滚动超出目标区域时，它的表现就像 position:fixed;，它会固定在目标位置。**注意:** Internet Explorer, Edge 15 及更早 IE 版本不支持 sticky 定位。 Safari 需要使用 -webkit- prefix (查看以下实例)。 |
| inherit                                                      | 规定应该从父元素继承 position 属性的值。                     |
| initial                                                      | 设置该属性为默认值，详情查看 [CSS initial 关键字](https://www.runoob.com/cssref/css-initial.html)。 |

# 元素隐藏的方法

## display: none

```css
.hide-display {
  display: none;
}
```

- 真正意义上的隐藏，元素**不会占据任何空间，不会生成元素**，所以动态改变此属性时**会引起重排**。
- 用户无法与其进行直接的交互
- **通过DOM依然可以获取到该元素**
- 其子孙元素即使重新设置`display: block`也无法显示
- `transition`动画会失效

## visibility: hidden

```css
.hide-visibility {
  visibility: hidden;
}
```

- 隐藏元素的**依然占据着空间**，影响其他元素的布局，动态修改此属性**会引起重绘**。
- 不会响应任何用户交互
- 通过DOM依然可以获取该元素，**无法响应DOM事件**
- 其子孙元素可以通过重新设置`visibility: visible`来显示

## opacity: 0

```css
.hide-opacity {
  opacity: 0;
}
```

- 只是**视觉上的隐藏**，隐藏元素的依然占据着空间，影响其他元素的布局，
- 依然能够响应用户的交互
- 通过DOM依然可以获取该元素，可以响应DOM事件
- 其子孙元素即使重新设置了`opacity: 1`也无法显示

## hidden(H5)

HTML5新增的`hidden`属性，可以直接隐藏元素。

```css
<div hidden>
  我是被隐藏的元素
</div>
```

特点：

**跟`display`表现一致。**

## position

```css
.hide-position {
  position: absolute;
  top: -9999px;
  left: -9999px;
}
```

- 视觉上的隐藏，实际显示在屏幕的可视区之外，不会占据空间，不影响其他元素的布局
- 用户无法与其进行直接的交互
- 通过DOM依然可以获取到该元素
- 其子孙元素无法通过重新设置对应的属性来显示

## clip-path

通过裁剪元素来实现隐藏。

```css
.hide-clip {
  clip-path: polygon(0 0, 0 0, 0 0, 0 0);
}
```

- 视觉上的隐藏，隐藏元素的依然占据着空间，影响其他元素的布局
- 用户无法与其进行直接的交互
- 通过DOM依然可以获取到该元素
- 其子孙元素无法通过重新设置对应的属性来显示

## overflow

通过设置元素的宽高为`0`来隐藏元素。

```css
.hide-overflow {
  width: 0;
  height: 0;
  overflow: hidden;
}
```

必须加上`overflow: hidden`，否则其子孙元素依然可以显示，

- 视觉上的隐藏，隐藏元素的不会占据任何空间，不会影响其他元素的布局
- 用户无法与其进行直接的交互
- 通过DOM依然可以获取到该元素
- 其子孙元素无法通过重新设置对应的属性来显示

## transform

```css
.hide-transform {
  transform: translate(-9999px, -9999px);
}
```

- 视觉上的隐藏，隐藏元素的依然占据着空间，影响其他元素的布局
- 用户无法与其进行直接的交互
- 通过DOM依然可以获取到该元素
- 其子孙元素无法通过重新设置对应的属性来显示

## 完全隐藏

针对上面所列的8种方法，能够实现完全隐藏的只有下面这3种：

- display: none
- visibility: hidden
- HTML5新增的hidden属性

## 视觉上的隐藏

- opacity
- position
- transform
- clip-path
- overflow

## 语义上的隐藏

- aria-hidden="true"

## 其他区别

| 隐藏方式   | 占据原来的空间 | 直接跟用户交互 | 直接响应DOM事件 |
| ---------- | -------------- | -------------- | --------------- |
| opacity    | 是             | 是             | 是              |
| visibility | 是             | 否             | 否              |
| display    | 否             | 否             | 否              |
| hidden     | 否             | 否             | 否              |
| position   | 否             | 否             | 否              |
| clip-path  | 是             | 否             | 否              |
| overflow   | 否             | 否             | 否              |
| transform  | 是             | 否             | 否              |

# 媒体查询、自适应设计、响应式设计

### 前序

Media Query（媒体查询）是CSS3中的新增内容，用于定义**不同媒体类型在不同CSS属性时的样式表现**。

在以前，如果我们想同一个网站对不同设备（比如PC端，手机端，平板端等等）提供支持，一般性的做法是针对不同的设备额外实现一套页面，在web端判断出访问设备类型时再路由到不同的实现。

这种做法的弊端很明显，因为额外的实现，所以后续的更新及维护都比较繁琐且成本越来越高。那么我们有没有一种方法，就是只有一份实现但是可以根据不同的设备自动做展现上的调整。Media Query为这种思路的实现提供了支持。

[这里](http://runjs.cn/detail/zybewetf)是一个例子，当改变浏览器窗口的大小时，页面上文本的颜色将会发生变化。其实现原理就是使用Media Query。

### 媒体类型（Media Type）

所谓**媒体查询就是针对不同媒体类型在不同CSS属性时的样式表现**。它有两个要素，第一是针对不同媒体类型，第二是针对CSS属性。

```css
@media screen and (width: 888px) {
    p {
        color: gold;
    }
}
```

其中，

- `@media`是关键字（可以将其理解成css的一种语法糖，跟`@import`类似）
- `screen`，这个关键字就是我们所说的**媒体类型**（这里`screen`其实就是电脑屏幕）
- `width: 888px`，需要查询的CSS属性

所以上面的CSS Media Query代码要表达的意思就是：当页面在电脑屏幕上展现时，且屏幕的`width`（宽度）属性为888px时，设置所有的`p`标签元素的字体颜色为`gold`（土豪金，哈哈~）。

除了上面提到的`screen`，常见有**媒体类型**如下表,

| 媒体类型     | 备注                                     | 是否常用 |
| ------------ | ---------------------------------------- | -------- |
| `all`        | 匹配所有设备                             | **是**   |
| `braille`    | 盲文设备                                 | 否       |
| `embossed`   | 盲文打印                                 | 否       |
| `handheld`   | 手持设备                                 | 否       |
| `print`      | 打印模式                                 | **是**   |
| `projection` | 演示模式、幻灯片等                       | 否       |
| `screen`     | 电脑屏幕                                 | **是**   |
| `speech`     | 演讲                                     | 否       |
| `tty`        | 固定字母间距的网格的媒体，比如电传打字机 | 否       |
| `tv`         | 电视媒体                                 | 否       |

看到上面关于媒体类型的表格后，其实我们常用的也就`all`、`print`、`screen`这几种类型。其中`screen`要属于最常用的媒体类型了。

在具体使用`media type`时，我们还可以使用`not`或者`only`这两个关键字修饰媒体类型

### 媒体查询（Media Query）

说完了媒体类型，我们再来说一说媒体查询。其一般的语法如下，

```
@media screen and (width: 888px) {
    /* your css code */
}
```

媒体查询中**查询**两字的含义就体现在`screen`和`width: 888px`上（可能更加倾向后者）。换句话说，`screen`和`width: 888px`其实都是查询的条件，当有多个条件时，我们使用`and`将他们连起来。

上面的示例代码中，查询页面的`width`属性，当其宽度为888px时，将应用特别设置的样式。

不过Media Query所支持的CSS属性是有限的，与一般的CSS属性并不一致，而且会有一些特别的可选项。如下表，

| 可查询属性            | 属性值类型                 | 是否可用Max/Min前缀 | 描述                                                         | 常用   |
| --------------------- | -------------------------- | ------------------- | ------------------------------------------------------------ | ------ |
| `color`               | 整数                       | 是                  | 定义每一组输出设备的彩色原件个数。如果不是彩色设备，则等于0  | 否     |
| `color-index`         | 整数                       | 是                  | 定义在输出设备的彩色查询表中的条目数。如果没有使用彩色查询表，则等于0 | 否     |
| `width`               | 长度                       | 是                  | 定义输出设备中的页面可见区域宽度                             | **是** |
| `height`              | 长度                       | 是                  | 定义输出设备中的页面可见区域高度                             | **是** |
| `device-width`        | 长度                       | 是                  | 定义输出设备的屏幕可见宽度（设备本身的宽度）                 | **是** |
| `device-height`       | 长度                       | 是                  | 定义输出设备的屏幕可见高度（设备本身的高度）                 | **是** |
| `orientation`         | `portrait`/`landscape`     | 否                  | 定义`height`是否大于或等于`width`。值`portrait`代表是（**竖屏**），landscape代表否（**横屏**） | **是** |
| `aspect-ratio`        | 整数/整数                  | 是                  | 定义`width`与`height`的比率（**宽高比**）                    | **是** |
| `device-aspect-ratio` | 整数/整数                  | 是                  | 定义`device-width`与`device-height`的比率（**宽高比**）。如常见的显示器比率：4/3, 16/9, 16/10 | **是** |
| `monochrome`          | 整数                       | 是                  | 定义在一个单色框架缓冲区中每像素包含的单色原件个数。如果不是单色设备，则等于0 | 否     |
| `scan`                | `progressive`/`interlaced` | 否                  | 定义电视类设备的扫描工序                                     | 否     |
| `grid`                | 整数                       | 否                  | 用来查询输出设备是否使用栅格或点阵。只有`1`和`0`才是有效值，`1`代表是，`0`代表否 | 否     |
| `resolution`          | dpi/dpcm                   | 是                  | 定义设备的分辨率。如：96dpi, 300dpi, 118dpcm                 | 否     |

看过上面的表格之后，我们会发现其实Media Query就是为了不同设备而诞生的。就目前而言，我们常用的查询属性也就那么几个，大部分与宽高有关系（其实就是设置的展现区域大小）。

从上面的表格中，我们可以看出有部分的可查询属性还有添加`max-`或者`min-`前缀进行修饰。比如，

```
@media screen and (min-width: 961px) and (max-width: 1200px) {
    p {
        color: pink;
    }
}
```

上面代码的含义是指，当展现页面的宽度大于960px且小于1200px时，将`p`标签的字体颜色设置为粉色。

这个例子中，我们使用了`width`和`height`这两个可查询属性，而且还使用了`max`和`min`对齐进行修饰，分别表示**最小宽度**和**最大宽度**。

### 自适应网页设计

它的意思就是**可以自动识别屏幕宽度、并做出相应调整的网页设计**。

我们可以使用CSS3的Media Query做一些自适应的网页设计。比如，

```css
<head>
    <link rel="stylesheet" media="screen and (max-width: 480px)" href="tiny.css" />
    <link rel="stylesheet" media="screen and (min-width: 481px) and (max-width: 600px)" href="small.css" />
    <link rel="stylesheet" media="screen and (min-width: 601px) and (max-width: 960px)" href="middle.css" />
    <link rel="stylesheet" media="screen and (min-width: 961px)" href="pc.css" />
</head>
```

上述代码的含义很明确，查询屏幕的宽度属性，根据不同的宽度加载不同的css文件。

有时候我们为了减少css文件的解析，将所有的媒体查询代码写在同一个css文件中，在html中仅作一次加载。如下，

```css
<head>
    <link rel="stylesheet" href="style.css" />
</head>
```

```css
@media screen and (max-width: 480px) {
    /* your css code */
}
@media screen and (min-width: 481px) and (max-width: 600px) {
    /* your css code */
}
/* more css code */
```

这两种做法的效果是一致的。

### 自适应布局的局限性

这里额外说一点，所谓的自适应布局远远没有这么简单，并不是靠着在不同屏幕大小上对页面布局做一些调整就可以了。

它还将面临下面的几个问题，

- 不同屏幕尺寸下，图片（视频）等资源的展示如何处理
- 在较小的屏幕尺寸下，往往需要对一些元素进行隐藏，这势必会造成流量（带宽资源）的浪费
- 即使一套页面可以自适应好几种设备了，此时一旦有更新，需要同时维护各个设备相关的css代码并且要做好协调

### 响应式设计

响应式开发一套界面，通过检测视口分辨率，针对不同客户端在客户端做代码处理，来展现不同的布局和内容；

#### 实现方案

##### **1. 媒体查询**

`CSS3`媒体查询可以让我们针对不同的媒体类型定义不同的样式，当重置浏览器窗口大小的过程中，页面也会根据浏览器的宽度和高度重新渲染页面。

##### **2.百分比布局**

通过百分比单位，可以使得浏览器中组件的宽和高随着浏览器的高度的变化而变化，从而实现响应式的效果

##### **3.rem布局**

**rem响应式的布局思想：**

- 一般不要给元素设置具体的宽度，但是对于一些小图标可以设定具体宽度值
- 高度值可以设置固定值，设计稿有多大，我们就严格有多大
- 所有设置的固定值都用`rem`做单位（首先在HTML总设置一个基准值：`px`和`rem`的对应比例，然后在效果图上获取`px`值，布局的时候转化为`rem`值)
- js获取真实屏幕的宽度，让其除以设计稿的宽度，算出比例，把之前的基准值按照比例进行重新的设定，这样项目就可以在移动端自适应了

##### **rem布局的缺点：**

在响应式布局中，必须通过js来动态控制根元素`font-size`的大小，也就是说css样式和js代码有一定的耦合性，且必须将改变`font-size`的代码放在`css`样式之前

##### **4.视口单位**

`css3`中引入了一个新的单位`vw/vh`，与视图窗口有关，`vw`表示相对于视图窗口的宽度，`vh`表示相对于视图窗口高度，除了`vw`和`vh`外，还有`vmin`和`vmax`两个相关的单位。

##### **5.图片响应式**

这里的图片响应式包括两个方面，一个就是大小自适应，这样能够保证图片在不同的屏幕分辨率下出现压缩、拉伸的情况；一个就是根据不同的屏幕分辨率和设备像素比来尽可能选择高分辨率的图片，也就是当在小屏幕上不需要高清图或大图，这样我们用小图代替，就可以减少网络带宽了。

#### 响应式布局的成型方案

现在的css，UI框架等都已经考虑到了适配不同屏幕分辨率的问题，实际项目中我们可以直接使用这些新特性和框架来实现响应式布局。可以有以下选择方案：

- 利用上面的方法自己来实现，比如CSS3 Media Query,rem，vw等
- Flex弹性布局，兼容性较差
- Grid网格布局，兼容性较差
- Columns栅格系统，往往需要依赖某个UI库，如Bootstrap

### ⚝响应式设计与自适应设计的区别？

**响应式设计**：响应式**开发一套界面**，通过检测视口分辨率，针对不同客户端在客户端做代码处理，来展现不同的布局和内容。

**自适应设计**：自适应需要**开发多套界面**，通过检测视口分辨率，来判断当前访问的设备是 `PC` 端、平板还是手机，从而请求服务层，返回不同的页面。

#### 响应式设计与自适应设计如何选取？

- 页面不是太复杂的情况下，采用响应式布局的方式
- 页面中信息较多，布局较为复杂的情况，采用自适应布局的方式

#### 响应式设计与自适应设计的优缺点？

##### 响应式布局

**优点**：灵活性强，能够快捷解决多设备显示适用问题

**缺点**

- 效率较低，兼容各设备工作量大
- 代码较为累赘，加载时间可能会加长
- 在一定程度上改变了网站原有的布局结构

##### 自适应布局

**优点**

- 对网站复杂程度兼容更大
- 代码更高效
- 测试和运营都相对容易和精准

**缺点**：同一个网站需要为不同的设备开发不同的页面，增加的开发的成本

# ⚝移动端适配

## 设备独立像素DP

我们描述的像素都是`物理像素`，即设备上真实的物理单元。

设备独立像素的产生原因：

下面的黑色手机，它的分辨率是`640x940`，正好是白色手机的两倍。

理论上来讲，在白色手机上相同大小的图片和文字，在黑色手机上会被缩放一倍，因为它的分辨率提高了一倍。这样，岂不是后面出现更高分辨率的手机，页面元素会变得越来越小吗？

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/37001e1c48e14b6c8606024183a1151b~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp" alt="img" style="zoom: 40%;" />

不是的，黑色手机使用了视网膜屏幕的技术，那么显示结果应该是下面的情况，比如列表的宽度为`300`个像素，那么在一条水平线上，白色手机会用`300`个物理像素去渲染它，而黑色手机实际上会用`600`个物理像素去渲染它。

我们必须用一种单位来同时告诉不同分辨率的手机，它们在界面上显示元素的大小是多少，这个单位就是**设备独立像素**(`Device Independent Pixels`)简称`DIP`或`DP`。上面我们说，列表的宽度为`300`个像素，实际上我们可以说：列表的宽度为`300`个设备独立像素。

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a0dd7c1f817e4bbabe5590651ed97eb6~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp" alt="img" style="zoom:50%;" />

### 设备像素比dpr

设备像素比`device pixel ratio`简称`dpr`，即**物理像素和设备独立像素的比值**。

在`web`中，浏览器为我们提供了`window.devicePixelRatio`来帮助我们获取`dpr`。

在`css`中，可以使用媒体查询`min-device-pixel-ratio`，区分`dpr`：

```css
@media (-webkit-min-device-pixel-ratio: 2),(min-device-pixel-ratio: 2){ }
```

在`React Native`中，我们也可以使用`PixelRatio.get()`来获取`DPR`。

### 移动端开发

在`iOS`、`Android`和`React Native`开发中样式单位其实都使用的是**设备独立像素**。

`iOS`的尺寸单位为`pt`，`Android`的尺寸单位为`dp`，`React Native`中没有指定明确的单位，它们其实都是设备独立像素`dp`。

在使用`React Native`开发`App`时，`UI`给我们的原型图一般是基于`iphone6`的像素给定的。

为了适配所有机型，我们在写样式时需要把物理像素转换为设备独立像素：例如：如果给定一个元素的高度为`200px`(这里的`px`指物理像素，非`CSS`像素)，`iphone6`的设备像素比为`2`，我们给定的`height`应为`200px/2=100dp`。

### WEB端开发

在写`CSS`时，我们用到最多的单位是`px`，即`CSS像素`，当页面缩放比例为`100%`时，一个`CSS像素`等于一个设备独立像素。

但是`CSS像素`是很容易被改变的，当用户对浏览器进行了放大，`CSS像素`会被放大，这时一个`CSS像素`会跨越更多的物理像素。

`页面的缩放系数 = CSS像素 / 设备独立像素`。

## 视口

视口(`viewport`)代表当前可见的计算机图形区域。在`Web`浏览器术语中，通常与浏览器窗口相同，但不包括浏览器的`UI`， 菜单栏等——即指你正在浏览的文档的那一部分。

一般我们所说的视口共包括三种：**布局视口、视觉视口和理想视口**，它们在屏幕适配中起着非常重要的作用。

### 布局视口

布局视口(`layout viewport`)：当我们以百分比来指定一个元素的大小时，它的计算值是由这个元素的包含块计算而来的。当这个元素是最顶级的元素时，它就是基于布局视口来计算的。

所以，布局视口是网页布局的基准窗口，在`PC`浏览器上，布局视口就等于当前浏览器的窗口大小（不包括`borders` 、`margins`、滚动条）。

在移动端，布局视口被赋予一个默认值，大部分为`980px`，这保证`PC`的网页可以在手机浏览器上呈现，但是非常小，用户可以手动对网页进行放大。

我们可以通过调用`document.documentElement.clientWidth / clientHeight`来获取布局视口大小。

### 视觉视口

视觉视口(`visual viewport`)：用户通过屏幕真实看到的区域。

视觉视口默认等于当前浏览器的窗口大小（包括滚动条宽度）。

当用户对浏览器进行缩放时，不会改变布局视口的大小，所以页面布局是不变的，但是缩放会改变视觉视口的大小。

例如：用户将浏览器窗口放大了`200%`，这时浏览器窗口中的`CSS像素`会随着视觉视口的放大而放大，这时一个`CSS`像素会跨越更多的物理像素。

所以，布局视口会限制你的`CSS`布局而视觉视口决定用户具体能看到什么。

我们可以通过调用`window.innerWidth / innerHeight`来获取视觉视口大小。

### 理想视口

布局视口在移动端展示的效果并不是一个理想的效果，所以**理想视口(ideal viewport)**就诞生了：网站页面在移动端展示的理想大小。

### Meta viewport

<meta> 元素表示那些不能由其它HTML元相关元素之一表示的任何元数据信息，它可以告诉浏览器如何解析页面。

我们可以借助`<meta>`元素的`viewport`来帮助我们设置视口、缩放等，从而让移动端得到更好的展示效果。

```html
<meta name="viewport" content="width=device-width; initial-scale=1; maximum-scale=1; minimum-scale=1; user-scalable=no;
```

width：控制 viewport 的大小，可以指定的一个值，如果 600，或者特殊的值，如 device-width 为设备的宽度（单位为缩放为 100% 时的 CSS 的像素）。
height：和 width 相对应，指定高度。
initial-scale：初始缩放比例，也即是当页面第一次 load 的时候缩放比例。
maximum-scale：允许用户缩放到的最大比例。
minimum-scale：允许用户缩放到的最小比例。
user-scalable：用户是否可以手动缩放

### 移动端适配

为了在移动端让页面获得更好的显示效果，我们必须让布局视口、视觉视口都尽可能等于理想视口。

`device-width`就等于理想视口的宽度，所以设置`width=device-width`就相当于让布局视口等于理想视口。

由于`initial-scale = 理想视口宽度 / 视觉视口宽度`，所以我们设置`initial-scale=1;`就相当于让视觉视口等于理想视口。

这时，1个`CSS`像素就等于1个设备独立像素，而且我们也是基于理想视口来进行布局的，所以呈现出来的页面布局在各种设备上都能大致相似。

## 1px问题

为了适配各种屏幕，我们写代码时一般使用**设备独立像素**来对页面进行布局。

而在设备像素比大于`1`的屏幕上，我们写的`1px`实际上是被多个物理像素渲染，这就会**出现`1px`在有些屏幕上看起来很粗的现象。**

### 5.1 border-image

基于`media`查询判断不同的设备像素比给定不同的`border-image`：

```css
       .border_1px{
          border-bottom: 1px solid #000;
        }
        @media only screen and (-webkit-min-device-pixel-ratio:2){
            .border_1px{
                border-bottom: none;
                border-width: 0 0 1px 0;
                border-image: url(../img/1pxline.png) 0 0 2 0 stretch;
            }
        }
```

### 5.2 background-image

和`border-image`类似，准备一张符合条件的边框背景图，模拟在背景上。

```css
       .border_1px{
          border-bottom: 1px solid #000;
        }
        @media only screen and (-webkit-min-device-pixel-ratio:2){
            .border_1px{
                background: url(../img/1pxline.png) repeat-x left bottom;
                background-size: 100% 1px;
            }
        }
复制代码
```

上面两种都需要单独准备图片，而且圆角不是很好处理，但是可以应对大部分场景。

### 5.3 伪类 + transform

基于`media`查询判断不同的设备像素比对线条进行缩放：

```css
       .border_1px:before{
          content: '';
          position: absolute;
          top: 0;
          height: 1px;
          width: 100%;
          background-color: #000;
          transform-origin: 50% 0%;
        }
        @media only screen and (-webkit-min-device-pixel-ratio:2){
            .border_1px:before{
                transform: scaleY(0.5);
            }
        }
        @media only screen and (-webkit-min-device-pixel-ratio:3){
            .border_1px:before{
                transform: scaleY(0.33);
            }
        }
复制代码
```

这种方式可以满足各种场景，如果需要满足圆角，只需要给伪类也加上`border-radius`即可。

### 5.4 svg

上面我们`border-image`和`background-image`都可以模拟`1px`边框，但是使用的都是位图，还需要外部引入。

借助`PostCSS`的`postcss-write-svg`我们能直接使用`border-image`和`background-image`创建`svg`的`1px`边框：

```css
@svg border_1px { 
  height: 2px; 
  @rect { 
    fill: var(--color, black); 
    width: 100%; 
    height: 50%; 
    } 
  } 
.example { border: 1px solid transparent; border-image: svg(border_1px param(--color #00b1ff)) 2 2 stretch; }
```

编译后：

```css
.example { border: 1px solid transparent; border-image: url("data:image/svg+xml;charset=utf-8,%3Csvg xmlns='http://www.w3.org/2000/svg' height='2px'%3E%3Crect fill='%2300b1ff' width='100%25' height='50%25'/%3E%3C/svg%3E") 2 2 stretch; }
```

上面的方案是大漠在他的文章中推荐使用的，基本可以满足所有场景，而且不需要外部引入，这是我个人比较喜欢的一种方案。

### 5.5 设置viewport

通过设置缩放，让`CSS`像素等于真正的物理像素。

例如：当设备像素比为`3`时，我们将页面缩放`1/3`倍，这时`1px`等于一个真正的屏幕像素。

```js
    const scale = 1 / window.devicePixelRatio;
    const viewport = document.querySelector('meta[name="viewport"]');
    if (!viewport) {
        viewport = document.createElement('meta');
        viewport.setAttribute('name', 'viewport');
        window.document.head.appendChild(viewport);
    }
    viewport.setAttribute('content', 'width=device-width,user-scalable=no,initial-scale=' + scale + ',maximum-scale=' + scale + ',minimum-scale=' + scale);
复制代码
```

实际上，上面这种方案是早先`flexible`采用的方案。

当然，这样做是要付出代价的，这意味着你页面上所有的布局都要按照物理像素来写。这显然是不现实的，这时，我们可以借助`flexible`或`vw、vh`来帮助我们进行适配。

## ⚝移动端适配方案

### 媒体查询

通过 CSS 的 @media 媒体查询设置不同的 style。通过媒体查询，我们可以根据不同屏幕设置不同样式，这样就可以实现不同屏幕的适配。

### flexible方案

`flexible`方案是阿里早期开源的一个移动端适配解决方案，引用`flexible`后，**我们在页面上统一使用`rem`来布局。**

它的核心代码非常简单：

```js
// set 1rem = viewWidth / 10
function setRemUnit () {
    var rem = docEl.clientWidth / 10
    docEl.style.fontSize = rem + 'px'
}
setRemUnit();
```

`rem` 是相对于`html`节点的`font-size`来做计算的。

我们通过设置`document.documentElement.style.fontSize`就可以统一整个页面的布局标准。

上面的代码中，将`html`节点的`font-size`设置为页面`clientWidth`(布局视口)的`1/10`，即`1rem`就等于页面布局视口的`1/10`，这就意味着我们后面使用的`rem`都是按照页面比例来计算的。

这时，我们只需要将`UI`出的图转换为`rem`即可。

以`iPhone6`为例：布局视口为`375px`，则`1rem = 37.5px`，这时`UI`给定一个元素的宽为`75px`（设备独立像素），我们只需要将它设置为`75 / 37.5 = 2rem`。

当然，每个布局都要计算非常繁琐，我们可以借助`PostCSS`的`px2rem`插件来帮助我们完成这个过程。

下面的代码可以保证在页面大小变化时，布局可以自适应，当触发了`window`的`resize`和`pageShow`事件之后自动调整`html`的`fontSize`大小。

```js
  // reset rem unit on page resize
window.addEventListener('resize', setRemUnit)window.addEventListener('pageshow', function (e) {
    if (e.persisted) {
      setRemUnit()
    }
})
```

由于`viewport`单位得到众多浏览器的兼容，上面这种方案现在已经被官方弃用：

> lib-flexible这个过渡方案已经可以放弃使用，不管是现在的版本还是以前的版本，都存有一定的问题。建议大家开始使用viewport来替代此方案。

#### flexible规避“1px 问题”

设置 viewport 的 width 为 device-width，改变浏览器 viewport（布局视口和视觉视口）的默认宽度为理想视口宽度，从而使得用户可以在理想视口内看到完整的布局视口的内容。
等比设置 viewport 的 initial-scale、maximum-scale、minimum-scale 的值，从而实现 1 物理像素=1 css像素，以适配高倍屏的显示效果（就是在这个地方规避了大家熟知的“1px 问题”）

```javascript
var metaEL= doc.querySelector('meta[name="viewport"]');
var dpr = window.devicePixelRatio;
var scale = 1 / dpr
metaEl.setAttribute('content', 'width=device-width, initial-scale=' + scale + ', maximum-scale=' + scale + ', minimum-scale=' + scale + ', user-scalable=no'); 
```

#### ⚝流程

使用动态 rem 方案需要做的工作：

1. **meta 标签设置 viewport 宽度为屏幕宽度；**
2. 根据不同屏幕修改根元素 font-size 大小，一般设置为屏幕宽度的十分之一（可引入 [lib-flexible](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Famfe%2Flib-flexible) 库，或者自己写相应逻辑）；
3. 开发环境配置 [postcss-px2rem](https://link.juejin.cn/?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fpostcss-px2rem) 或者类似插件；
4. 根据设计稿写样式，元素宽高直接取设计稿宽高即可，单位为 px，插件会将其转换为 rem；
5. 段落文本也按照设计稿写，单位为px，不需要转换为 rem；

#### flexible的缺陷

- 由于其缩放的缘故，video 标签的视频频播放器的样式在不同 dpr 的设备上展示差异很大；
- 如果你去研究过 lib-flexible 的源码，那你一定知道 lib-flexible 对安卓手机的特殊处理，即：一律按 dpr = 1 处理；
- 不再兼容 @media 的响应式布局，因为 @media 语法中涉及到的尺寸查询语句，查询的尺寸依据是当前设备的物理像素，和 flexible 的布局理论（即针对不同 dpr 设备等比缩放视口的 scale 值，从而同时改变布局视口和视觉视口大小）相悖，因此响应式布局在“等比缩放视口大小”的情境下是无法正常工作的；

### vh、vw方案(viewport 适配方案)

`vh、vw`方案即将视觉视口宽度 `window.innerWidth `和视觉视口高度 `window.innerHeight` 等分为 100 份。

上面的`flexible`方案就是模仿这种方案，因为早些时候`vw`还没有得到很好的兼容。

- `vw(Viewport's width)`：`1vw`等于视觉视口的`1%`
- `vh(Viewport's height)` :` 1vh` 为视觉视口高度的`1%`
- `vmin` : `vw` 和 `vh` 中的较小值
- `vmax` : 选取 `vw` 和 `vh` 中的较大值

如果视觉视口为`375px`，那么`1vw = 3.75px`，这时`UI`给定一个元素的宽为`75px`（设备独立像素），我们只需要将它设置为`75 / 3.75 = 20vw`。

这里的比例关系我们也不用自己换算，我们可以使用`PostCSS`的 `postcss-px-to-viewport` 插件帮我们完成这个过程。写代码时，我们只需要根据`UI`给的设计图写`px`单位即可。

#### ⚝流程

1. **meta 标签设置 viewport 宽度为屏幕宽度；**
2. 开发环境配置 [postcss-px-to-viewport](https://link.juejin.cn/?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fpostcss-px-to-viewport) 或者类似插件；
3. 根据设计稿写样式，元素宽高直接取设计稿宽高即可，单位为 px，插件会将其转换为 vw；
4. 段落文本也按照设计稿写，单位为px，不需要转换为 vw；

当然，没有一种方案是十全十美的，`vw`同样有一定的缺陷：

- `px`转换成`vw`不一定能完全整除，因此有一定的像素差。
- 比如当容器使用`vw`，`margin`采用`px`时，很容易造成整体宽度超过`100vw`，从而影响布局效果。当然我们也是可以避免的，例如使用`padding`代替`margin`，结合`calc()`函数使用等等...

### 百分比布局

在 css 中，我们可以使用百分比来实现布局，但是需要特定宽度时，这个百分比的计算对开发者来说并不友好，且元素百分比参考的对象为父元素，元素嵌套较深时会有问题。

案例：设计稿为 750*1136，我们需要一个宽度为 200px 的盒子

```css
.box {
  /* w = 200 / (750/100) = 26.66667 */
  /* 可知，计算复杂，且会存在误差 */
  width: 26.6%;
}
```

## 横屏适配

很多视口我们要对横屏和竖屏显示不同的布局，所以我们需要检测在不同的场景下给定不同的样式：

### JavaScript检测横屏

`window.orientation`:获取屏幕旋转方向

```js
window.addEventListener("resize", ()=>{
    if (window.orientation === 180 || window.orientation === 0) { 
      // 正常方向或屏幕旋转180度
        console.log('竖屏');
    };
    if (window.orientation === 90 || window.orientation === -90 ){ 
       // 屏幕顺时钟旋转90度或屏幕逆时针旋转90度
        console.log('横屏');
    }  
}); 
```

### CSS检测横屏

```js
@media screen and (orientation: portrait) {
  /*竖屏...*/
} 
@media screen and (orientation: landscape) {
  /*横屏...*/
}
```

## 图片模糊问题

### 产生原因

我们平时使用的图片大多数都属于**位图（`png、jpg...`）**，位图由一个个像素点构成的，每个像素都具有特定的位置和颜色值：

位图的每个像素对应在屏幕上使用一个物理像素来渲染，才能达到最佳的显示效果。

而在`dpr > 1`的屏幕上，位图的一个像素可能由多个物理像素来渲染**，然而这些物理像素点并不能被准确的分配上对应位图像素的颜色，只能取近似值**，所以相同的图片在`dpr > 1`的屏幕上就会模糊

### 解决方案

为了保证图片质量，我们应该尽可能让一个屏幕像素来渲染一个图片像素，所以，针对不同`DPR`的屏幕，我们需要展示不同分辨率的图片。

如：在`dpr=2`的屏幕上展示两倍图`(@2x)`，在`dpr=3`的屏幕上展示三倍图`(@3x)`。

### media查询

使用`media`查询判断不同的设备像素比来显示不同精度的图片：

```css
       .avatar{
            background-image: url(conardLi_1x.png);
        }
        @media only screen and (-webkit-min-device-pixel-ratio:2){
            .avatar{
                background-image: url(conardLi_2x.png);
            }
        }
        @media only screen and (-webkit-min-device-pixel-ratio:3){
            .avatar{
                background-image: url(conardLi_3x.png);
            }
        }
```

> 只适用于背景图

### image-set

使用`image-set`：

```css
.avatar {
    background-image: -webkit-image-set( "conardLi_1x.png" 1x, "conardLi_2x.png" 2x );
}
```

> 只适用于背景图

### srcset

使用`img`标签的`srcset`属性，浏览器会自动根据像素密度匹配最佳显示图片：

```html
<img src="conardLi_1x.png"
     srcset=" conardLi_2x.png 2x, conardLi_3x.png 3x">

```

### JavaScript拼接图片url

使用`window.devicePixelRatio`获取设备像素比，遍历所有图片，替换图片地址：

```js
const dpr = window.devicePixelRatio;
const images =  document.querySelectorAll('img');
images.forEach((img)=>{
  img.src.replace(".", `@${dpr}x.`);
})
```

### 使用svg

`SVG `的全称是**可缩放矢量图**（`Scalable Vector Graphics`）。**不同于位图的基于像素**，`SVG` 则是属于对图像的形状描述，所以它本质上是文本文件，体积较小，且不管放大多少倍都不会失真。

除了我们手动在代码中绘制`svg`，我们还可以像使用位图一样使用`svg`图片：

```js
<img src="conardLi.svg">

<img src="data:image/svg+xml;base64,[data]">

.avatar {
  background: url(conardLi.svg);
}
```



# CSS提升页面性能

## 内联首屏关键CSS

将CSS直接内联到HTML文档中能使CSS更快速地下载。而使用外部CSS文件时，需要在HTML文档下载完成后才知道所要引用的CSS文件，然后才下载它们。所以说，**内联CSS能够使浏览器开始页面渲染的时间提前**，因为在HTML下载完成之后就能渲染了。

但是不能内敛较大的CSS文件，会使服务器和浏览器之间进行多次的往返，我们应当**只将渲染首屏内容所需的关键CSS内联到HTML中**。

不过内联CSS有一个缺点，**内联之后的CSS不会进行缓存**，每次都会重新下载。

## 异步加载CSS

CSS会阻塞渲染，在CSS文件请求、下载、解析完成之前，浏览器将不会渲染任何已处理的内容。有时，这种**阻塞是必须的，因为我们并不希望在所需的CSS加载之前，浏览器就开始渲染页面**。那么将首屏关键CSS内联后，剩余的CSS内容的阻塞渲染就不是必需的了，可以使用外部CSS，并且**异步加载**。

```HTML
<link rel="preload" href="mystyles.css" as="style" onload="this.rel='stylesheet'">
```

注意，`as`是必须的。忽略`as`属性，或者错误的`as`属性会使`preload`等同于`XHR`请求，浏览器不知道加载的是什么内容，因此此类资源加载优先级会非常低。`as`的可选值可以参考上述标准文档。

## 压缩

属性设置简写 比如设置一个元素的上下左右margin,可以直接简写margin: 10px 12px 23px 34px；

其实可以把0后面的单位去掉：margin: 0 10px;目的是减少最后打包代码的包的体积。

用构建工具，如webpack、gulp/grunt、rollup等也都支持CSS压缩功能。压缩后的文件能够明显减小，可以大大降低了浏览器的加载时间。

## 选择器的选择

**CSS选择器的匹配是从右向左进行的**，这一策略导致了不同种类的选择器之间的性能也存在差异

**Tips：为什么CSS选择器是从右向左匹配的？**

CSS中更多的选择器是不会匹配的，所以在考虑性能问题时，需要考虑的是如何在选择器不匹配时提升效率。从右向左匹配就是为了达成这一目的的，通过这一策略能够使得CSS选择器在不匹配的时候效率更高。这样想来，在匹配时多耗费一些性能也能够想的通了。

1. 保持简单，**不要使用嵌套过多**过于复杂的选择器。
2. **通配符和属性选择器效率最低**，需要匹配的元素最多，尽量避免使用。
3. 不要使用类选择器和ID选择器修饰元素标签，如`h3#markdown-content`，这样多此一举，还会降低效率。
4. 不要为了追求速度而放弃可读性与可维护性。

## 优化重排与重绘

**减少重排**

如下所示，有很多操作会触发**重排**，我们应该避免频繁触发这些操作。

1. 改变`font-size`和`font-family`
2. 改变元素的内外边距
3. 通过JS改变CSS类
4. 通过JS获取DOM元素的位置相关属性（如width/height/left等）
5. CSS伪类激活
6. 滚动滚动条或者改变窗口大小

**避免重绘**

## 不要使用@import

不建议使用@import主要有以下两点原因。

首先，使用@import引入CSS会影响浏览器的并行下载。使用@import引用的CSS文件只有在引用它的那个css文件被下载、解析之后，浏览器才会知道还有另外一个css需要下载，这时才去下载，然后下载后开始解析、构建render tree等一系列操作。这就导致浏览器无法并行下载所需的样式文件。

其次，多个@import会导致下载顺序紊乱。在IE中，@import会引发资源文件的下载顺序被打乱，即**排列在@import后面的js文件先于@import下载，并且打乱甚至破坏@import自身的并行下载**。

所以不要使用这一方法，使用link标签就行了。

## 图片处理

- 用css代替图片

比如，可以用css画三角形代替一些箭头图片

这样做的目的是减少http请求

- 用css雪碧图代替单个图片

这样做的目的是减少http请求节约带宽

## 封装公共样式

把长段相同样式提取出来作为公用样式使用，比如常用的清除浮动，单行超出显示省略号，多行超出省略号等等。



# ⚝CSS预处理器

CSS预处理器定义了一种新的语言，基本思想是，用一种专门的编程语言，**为CSS增加一些编程特性**，将CSS作为目标生成文件，开发者用这种语言进行CSS编码工作。换句话说，**CSS预处理器用一种专门的编程语言进行网页页面样式设计，然后再编译成正常的CSS文件，供项目使用。**

CSS与编译器现如今几乎成为开发CSS的标配，从以下几个方面提升了CSS开发的效率：

- 增强编程能力
- 增强可复用性
- 增强可维护性
- 更便于解决浏览器兼容性

不同的预编译器特性虽然有差异，但核心功能围绕的目标是相同的，比如：

- 嵌套
- 变量
- mixin/继承
- 运算
- 模块化

CSS预处理器技术已经相当成熟，现在也涌现了多种不同的CSS预处理器语言，包括Sass(Scss), Less, Stylus, Turbine, Swithch CSS, CSS Cacheer, DT CSS等。

Sass有两套语法规则：

- 使用缩进作为分隔符来区分代码块
- 和`CSS`一样采用大括号(`{}`)作为分隔符

后一种语法规则称为`SCSS`, 在`Sass3`之后的版本都支持这种语法规则。

[Less](https://link.juejin.cn/?target=https%3A%2F%2Flesscss.org%2F%23overview): 2009年开源，受Sass影响较大，但使用CSS语法。

[Stylus](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fstylus%2Fstylus): 2010年诞生，来自Node.js社区，主要用来给Node项目进行CSS预处理支持，在此社区内有一定人气，广泛意义上支持率不如Sass和Less。

Stylus同时支持缩进和CSS常规样式书写规则

## scss和less的区别


1. 编译环境不一样 Less是基于JavaScript，是在客户端处理的。Sass是基于Ruby的，是在服务器端处理的。

2. 变量符不一样，Less是@，而Scss是$。

3. 输出设置不一样，Less没有输出设置，Sass提供4种输出选项：nested, compact, compressed 和 expanded。

4. Sass支持条件语句，可以使用if{}else{},for{}循环等等。而Less不支持。

5. `Scss`引用的外部文件命名必须以_开头, 如下例所示:其中_test1.scss、_test2.scss、_test3.scss文件分别设置的h1 h2 h3。文件名如果以下划线_开头的话，Sass会认为该文件是一个引用文件，不会将其编译为css文件.

6. Sass和Less的工具库不同 Sass有工具库Compass, Less有UI组件库Bootstrap.

## sass/scss编译成css的原理

- 首先less 会转换为ast(抽象语法树）[语法](https://blog.csdn.net/Dear_Mr/article/details/72587908)
- 然后遍历转换后所有的节点
- 最后再形成css树



# CSS兼容性问题

#### 1. 浏览器CSS样式初始化

所有CSS开始前，先把marin和padding都设为0，以防不同浏览器的显示效果不一样。

```css
*{
    margin: 0;
    padding: 0;
}
```

可以用 Normalize.css库 

#### 2. 浏览器私有属性

在某个CSS的属性前添加一些前缀，比如`-webkit-`，`-moz-` ，`-ms-`，这些就是浏览器的私有属性。

为避免日后W3C公布标准时有所变更，会加入一个私有前缀，比如`-webkit-border-radius`，通过这种方式**来提前支持新属性**。等到日后W3C公布了标准，border-radius的标准写法确立之后，再让新版的浏览器支持border-radius这种写法。常用的前缀有：

- -moz代表firefox浏览器私有属性
- -ms代表IE浏览器私有属性
- -webkit代表chrome、safari私有属性
- -o代表opera私有属性

#### 3. CSS hack

**针对不同的浏览器或不同版本写特定的CSS样式，这种针对不同的浏览器/不同版本写相应的CSS code的过程，叫做CSS hack**

CSS hack的写法大致归纳为3种：**条件hack、属性级hack、选择符级hack。**

##### **条件hack**

- 语法：

```javascript
<!--[if <keywords>? IE <version>?]>
    代码块，可以是html，css，js
<![endif]-->
```

**keywords**

if后面跟的条件共包含6种选择方式：是否、大于、大于或等于、小于、小于或等于、非指定版本

**version**

IE浏览器版本，如6、7、8

##### 属性级hack

属性hack就是在CSS样式属性名前加上一些只有特定浏览器才能识别的hack前缀。

- 语法：

```javascript
    selector{<hack>?property:value<hack>?;}
```

- 取值：

*：选择IE6及以下。连接线（中划线）（-）亦可使用，为了避免与某些带中划线的属性混淆，所以使用下划线（*）更为合适。

*：选择IE7及以下。诸如：（+）与（#）之类的均可使用，不过业界对（*）的认知度更高

\9：选择IE6+

\0：选择IE8+和Opera15以下的浏览器

##### 选择符级hack

选择符级hack是针对一些页面表现不一致或者需要特殊对待的浏览器，在CSS选择器前加上一些只有某些特定浏览器才能识别的前缀进行hack。

- 语法：

```javascript
<hack> selector{ sRules }
```

- 取值： 常见的选择符级hack有

```javascript
*html *前缀只对IE6生效
*+html *+前缀只对IE7生效
@media screen\9{...}只对IE6/7生效
@media \0screen {body { background: red; }}只对IE8有效
@media \0screen\,screen\9{body { background: blue; }}只对IE6/7/8有效
@media screen\0 {body { background: green; }} 只对IE8/9/10有效
@media screen and (min-width:0\0) {body { background: gray; }} 只对IE9/10有效
@media screen and (-ms-high-contrast: active), (-ms-high-contrast: none) {body { background: orange; }} 只对IE10有效
```

#### 4. 自动化插件

[Autoprefixer](https://github.com/postcss/autoprefixer)是一款自动管理浏览器前缀的插件，它可以解析CSS文件并且添加浏览器前缀到CSS内容里，使用Can I Use（caniuse网站）的数据来决定哪些前缀是需要的。

把[Autoprefixer](https://github.com/postcss/autoprefixer)添加到资源构建工具（例如Grunt）后，可以完全忘记有关CSS前缀的东西，只需按照最新的W3C规范来正常书写CSS即可。
