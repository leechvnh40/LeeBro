## React 的理解和特性

### 是什么

React，用于构建用户界面的 JavaScript 库，只提供了 UI 层面的解决方案

遵循组件设计模式、声明式编程范式和函数式编程概念，以使前端应用程序更高效

使用虚拟 `DOM` 来有效地操作 `DOM`，遵循从高阶组件到低阶组件的单向数据流

帮助我们将界面成了各个独立的小块，每一个块就是组件，这些组件之间可以组合、嵌套，构成整体页面

`react` 类组件使用一个名为 `render()` 的方法或者函数组件`return`，接收输入的数据并返回需要展示的内容

```jsx
class HelloMessage extends React.Component {
  render() {
    return <div>Hello {this.props.name}</div>;
  }
}

ReactDOM.render(
  <HelloMessage name="Taylor" />,
  document.getElementById("hello-example")
);
```

上述这种类似 `XML` 形式就是 `JSX`，最终会被 `babel` 编译为合法的 `JS` 语句调用

被传入的数据可在组件中通过 `this.props` 在 `render()` 访问。

### 特性

`React` 特性有很多，如：

- JSX 语法
- 单向数据绑定
- 虚拟 DOM
- 声明式编程
- Component

### 优势

通过上面的初步了解，可以感受到 `React` 存在的优势：

- 高效灵活
- 声明式的设计，简单使用
- 组件式开发，提高代码复用率
- 单向响应的数据流会比双向绑定的更安全，速度更快

## 声明式编程

### 什么是声明式编程

声明式编程是一种**编程范式**，它关注的是你**要做什么**，而不是**如何做**。它表达逻辑而不显式地定义步骤。这意味着我们需要根据逻辑的计算来声明要显示的组件。它没有描述控制流步骤。声明式编程的例子有HTML、SQL等。

如下，数组中的每个元素都乘以 `2`，我们使用声明式`map`函数，让编译器来完成其余的工作，而使用命令式，需要编写所有的流程步骤。

```js
const numbers = [1,2,3,4,5];

// 声明式
const doubleWithDec = numbers.map(number => number * 2);

console.log(doubleWithDec)

// 命令式
const doubleWithImp = [];
for(let i=0; i<numbers.length; i++) {
    const numberdouble = numbers[i] * 2;
    doubleWithImp.push(numberdouble)
}

console.log(doubleWithImp)
```

### 声明式编程 vs 命令式编程

声明式编程的编写方式描述了应该做什么，而命令式编程描述了如何做。在声明式编程中，让编译器决定如何做事情。命令式编程关注计算机执行的步骤，即一步一步告诉计算机先做什么再做什么。

### 函数式编程

函数式编程是声明式编程的一部分。javascript中的函数是第一类公民，这意味着函数是数据，你可以像保存变量一样在应用程序中保存、检索和传递这些函数。

函数式编程有些核心的概念，如下：

- 不可变性(Immutability)
- 纯函数(Pure Functions)
- 数据转换(Data Transformations)
- 高阶函数 (Higher-Order Functions)
- 递归
- 组合

#### 不可变性(Immutability)

不可变性意味着不可改变。 在函数式编程中，你无法更改数据，也不能更改。 如果要改变或更改数据，则必须复制数据副本来更改。

例如，这是一个**student**对象和`changeName`函数，如果要更改学生的名称，则需要先复制 student 对象，然后返回新对象。

在javascript中，函数参数是对实际数据的引用，你不应该使用 **student.firstName =“testing11”**，这会改变实际的`student` 对象，应该使用**Object.assign**复制对象并返回新对象。

```
let student = {
    firstName: "testing",
    lastName: "testing",
    marks: 500
}

function changeName(student) {
    // student.firstName = "testing11" //should not do it
    let copiedStudent = Object.assign({}, student);
    copiedStudent.firstName = "testing11";
    return copiedStudent;
}

console.log(changeName(student));

console.log(student);
复制代码
```

#### 纯函数

纯函数是始终接受一个或多个参数并计算参数并返回数据或函数的函数。 它没有副作用，例如设置全局状态，更改应用程序状态，它总是将参数视为不可变数据。

我想使用 **appendAddress** 的函数向`student`对象添加一个地址。 如果使用非纯函数，它没有参数，直接更改 `student` 对象来更改全局状态。

使用纯函数，它接受参数，基于参数计算，返回一个新对象而不修改参数。

```
let student = {
    firstName: "testing",
    lastName: "testing",
    marks: 500
}

// 非纯函数
function appendAddress() {
    student.address = {streetNumber:"0000", streetName: "first", city:"somecity"};
}

console.log(appendAddress());

// 纯函数
function appendAddress(student) {
    let copystudent = Object.assign({}, student);
    copystudent.address = {streetNumber:"0000", streetName: "first", city:"somecity"};
    return copystudent;
}

console.log(appendAddress(student));

console.log(student);
复制代码
```

#### 数据转换

我们讲了很多关于不可变性的内容，如果数据是不可变的，我们如何改变数据。如上所述，我们总是生成原始数据的转换副本，而不是直接更改原始数据。

再介绍一些 javascript内置函数，当然还有很多其他的函数，这里有一些例子。所有这些函数都不改变现有的数据，而是返回新的数组或对象。

```
let cities = ["irving", "lowell", "houston"];

// we can get the comma separated list
console.log(cities.join(','))
// irving,lowell,houston

// if we want to get cities start with i
const citiesI = cities.filter(city => city[0] === "i");
console.log(citiesI)
// [ 'irving' ]

// if we want to capitalize all the cities
const citiesC = cities.map(city => city.toUpperCase());
console.log(citiesC)
// [ 'IRVING', 'LOWELL', 'HOUSTON' ]
复制代码
```

#### 高阶函数

高阶函数是将函数作为参数或返回函数的函数，或者有时它们都有。 这些高阶函数可以操纵其他函数。

`Array.map，Array.filter和Array.reduce`是高阶函数，因为它们将函数作为参数。

```
const numbers = [10,20,40,50,60,70,80]

const out1 = numbers.map(num => num * 100);
console.log(out1);
// [ 1000, 2000, 4000, 5000, 6000, 7000, 8000 ]

const out2 = numbers.filter(num => num > 50);
console.log(out2);
// [ 60, 70, 80 ]

const out3 = numbers.reduce((out,num) => out + num);
console.log(out3);
// 330
复制代码
```

下面是另一个名为`isPersonOld`的高阶函数示例，该函数接受另外两个函数，分别是 `message`和`isYoung` 。

```
const isYoung = age => age < 25;

const message = msg => "He is "+ msg;

function isPersonOld(age, isYoung, message) {
    const returnMessage = isYoung(age)?message("young"):message("old");
    return returnMessage;
}

// passing functions as an arguments
console.log(isPersonOld(13,isYoung,message))
// He is young
复制代码
```

#### 递归

递归是一种函数在满足一定条件之前调用自身的技术。只要可能，最好使用递归而不是循环。你必须注意这一点，浏览器不能处理太多递归和抛出错误。

下面是一个演示递归的例子，在这个递归中，打印一个类似于楼梯的名称。我们也可以使用`for`循环，但只要可能，我们更喜欢递归。

```
function printMyName(name, count) {
    if(count <= name.length) {
        console.log(name.substring(0,count));
        printMyName(name, ++count);
    }
}

console.log(printMyName("Bhargav", 1));

/*
B
Bh
Bha
Bhar
Bharg
Bharga
Bhargav
*/

// withotu recursion
var name = "Bhargav"
var output = "";
for(let i=0; i<name.length; i++) {
    output = output + name[i];
    console.log(output);
}

复制代码
```

#### 组合

在React中，我们将功能划分为小型可重用的纯函数，我们必须将所有这些可重用的函数放在一起，最终使其成为产品。 将所有较小的函数组合成更大的函数，最终，得到一个应用程序，这称为**组合**。

实现组合有许多不同方法。 我们从Javascript中了解到的一种常见方法是链接。 链接是一种使用**点**表示法调用前一个函数的返回值的函数的方法。

这是一个例子。 我们有一个`name`，如果`firstName`和`lastName`大于5个单词的大写字母，刚返回，并且打印名称的名称和长度。

```
const name = "Bhargav Bachina";

const output = name.split(" ")
    .filter(name => name.length > 5)
    .map(val => {
    val = val.toUpperCase();
    console.log("Name:::::"+val);
    console.log("Count::::"+val.length);
    return val;
});

console.log(output)
/*
Name:::::BHARGAV
Count::::7
Name:::::BACHINA
Count::::7
[ 'BHARGAV', 'BACHINA' ]
*/
复制代码
```

在React中，我们使用了不同于链接的方法，因为如果有30个这样的函数，就很难进行链接。这里的目的是将所有更简单的函数组合起来生成一个更高阶的函数。

```
const name = compose(
    splitmyName,
    countEachName,
    comvertUpperCase,
    returnName
)

console.log(name);
```

## 在React中遍历的方法有哪些？

**（1）遍历数组：map && forEach**

```javascript
import React from 'react';

class App extends React.Component {
  render() {
    let arr = ['a', 'b', 'c', 'd'];
    return (
      <ul>
        {
          arr.map((item, index) => {
            return <li key={index}>{item}</li>
          })
        }
      </ul>
    )
  }
}

class App extends React.Component {
  render() {
    let arr = ['a', 'b', 'c', 'd'];
    return (
      <ul>
        {
          arr.forEach((item, index) => {
            return <li key={index}>{item}</li>
          })
        }
      </ul>
    )
  }
}
复制代码
```

**（2）遍历对象：map && for in**

```javascript
class App extends React.Component {
  render() {
    let obj = {
      a: 1,
      b: 2,
      c: 3
    }
    return (
      <ul>
        {
          (() => {
            let domArr = [];
            for(const key in obj) {
              if(obj.hasOwnProperty(key)) {
                const value = obj[key]
                domArr.push(<li key={key}>{value}</li>)
              }
            }
            return domArr;
          })()
        }
      </ul>
    )
  }
}

// Object.entries() 把对象转换成数组
class App extends React.Component {
  render() {
    let obj = {
      a: 1,
      b: 2,
      c: 3
    }
    return (
      <ul>
        {
          Object.entries(obj).map(([key, value], index) => {   // item是一个数组，把item解构，写法是[key, value]
            return <li key={key}>{value}</li>
          }) 
        }
      </ul>
    )
  }
}
```

## Real DOM 和 Virtual DOM

**Real DOM**，真实 `DOM`，意思为文档对象模型，是一个结构化文本的抽象，在页面渲染出的每一个结点都是一个真实 `DOM` 结构，如下：

![img](https://static.vue-js.com/fc7ba8d0-d302-11eb-85f6-6fac77c0c9b3.png)

**Virtual Dom**，本质上是以 `JavaScript` **对象**形式存在的对 `DOM` 的描述

创建虚拟 `DOM` 目的就是为了更好将虚拟的节点渲染到页面视图中，虚拟 `DOM` 对象的节点与真实 `DOM` 的属性一一照应

在 `React` 中，`JSX` 是其一大特性，可以让你在 `JS` 中通过使用 `XML` 的方式去直接声明界面的 `DOM` 结构

```jsx
// 创建 h1 标签，右边千万不能加引号
const vDom = <h1>Hello World</h1>; 
// 找到 <div id="root"></div> 节点
const root = document.getElementById("root"); 
// 把创建的 h1 标签渲染到 root 节点上
ReactDOM.render(vDom, root); 
```

上述中，`ReactDOM.render()` 用于将你创建好的虚拟 `DOM` 节点插入到某个真实节点上，并渲染到页面上

`JSX` 实际是一种语法糖，在使用过程中会被 `babel` 进行编译转化成 `JS` 代码，上述 `VDOM` 转化为如下：

```jsx
const vDom = React.createElement(
  'h1'，
  { className: 'hClass', id: 'hId' },
  'hello world'
)
```

可以看到，`JSX` 就是为了简化直接调用 `React.createElement()` 方法：

- 第一个参数是标签名，例如 h1、span、table...
- 第二个参数是个对象，里面存着标签的一些属性，例如 id、class 等
- 第三个参数是节点中的文本

通过 `console.log(VDOM)`，则能够得到虚拟 `VDOM` 消息

![img](https://static.vue-js.com/1716b9a0-d303-11eb-ab90-d9ae814b240d.png)

所以可以得到，`JSX` 通过 `babel` 的方式转化成 `React.createElement` 执行，返回值是一个对象，也就是虚拟 `DOM`

### 区别

两者的区别如下：

- 虚拟 DOM 不会进行排版与重绘操作，而真实 DOM 会频繁重排与重绘
- 虚拟 DOM 的总损耗是“虚拟 DOM 增删改+真实 DOM 差异增删改+排版与重绘”，真实 DOM 的总损耗是“真实 DOM 完全增删改+排版与重绘”

拿[以前文章 (opens new window)](https://mp.weixin.qq.com/s?__biz=MzU1OTgxNDQ1Nw==&mid=2247484516&idx=1&sn=965a4ce32bf93adb9ed112922c5cb8f5&chksm=fc10c632cb674f2484fdf914d76fba55afcefca3b5adcbe6cf4b0c7fd36e29d0292e8cefceb5&scene=178&cur_album_id=1711105826272116736#rd)举过的例子：

传统的原生 `api` 或 `jQuery` 去操作 `DOM` 时，浏览器会从构建 `DOM` 树开始从头到尾执行一遍流程

当你在一次操作时，需要更新 10 个 `DOM` 节点，浏览器没这么智能，收到第一个更新 `DOM` 请求后，并不知道后续还有 9 次更新操作，因此会马上执行流程，最终执行 10 次流程

而通过 `VNode`，同样更新 10 个 `DOM` 节点，虚拟 `DOM` 不会立即操作 `DOM`，而是将这 10 次更新的 `diff` 内容保存到本地的一个 `js` 对象中，最终将这个 `js` 对象一次性 `attach` 到 `DOM` 树上，避免大量的无谓计算

### 优缺点

真实 `DOM` 的优势：

- 易用

缺点：

- 效率低，解析速度慢，内存占用量过高
- 性能差：频繁操作真实 DOM，**易于导致重绘与回流**

使用虚拟 `DOM` 的优势如下：

- 简单方便：如果使用手动操作真实 `DOM` 来完成页面，繁琐又容易出错，在大规模应用下维护起来也很困难
- 性能方面：使用 Virtual DOM，能够有效避免真实 DOM 数频繁更新，减少多次引起重绘与回流，提高性能
- 跨平台：React 借助虚拟 DOM，带来了跨平台的能力，一套代码多端运行

缺点：

- 在一些性能要求极高的应用中虚拟 DOM 无法进行针对性的极致优化
- 首次渲染大量 DOM 时，由于多了一层虚拟 DOM 的计算，速度比正常稍慢

### Virtual DOM 的工作原理

Virtual DOM 是一个轻量级的 JavaScript 对象，它最初只是 real DOM 的副本。它是一个节点树，它将元素、它们的属性和内容作为对象及其属性。 React 的渲染函数从 React 组件中创建一个节点树。然后它响应数据模型中的变化来更新该树，该变化是由用户或系统完成的各种动作引起的。

Virtual DOM 工作过程有三个简单的步骤。

1. 每当底层数据发生改变时，整个 UI 都将在 Virtual DOM 描述中重新渲染。
2. 然后计算之前 DOM 表示与新表示的之间的差异。
3. 完成计算后，将只用实际更改的内容更新 real DOM。

## React diff的原理

跟`Vue`一致，`React`通过引入`Virtual DOM`的概念，极大地避免无效的`Dom`操作，使我们的页面的构建效率提到了极大的提升

而`diff`算法就是更高效地通过对比**新旧**`Virtual DOM`来找出真正的`Dom`变化之处

### 原理

`react`中`diff`算法主要遵循三个层级的策略：

- tree层级
- conponent 层级
- element 层级

#### tree层级

`DOM`节点跨层级的操作不做优化，只会对相同层级的节点进行比较

![img](https://static.vue-js.com/ae71d1c0-ec91-11eb-85f6-6fac77c0c9b3.png)

只有删除、创建操作，没有移动操作，如下图：

![img](https://static.vue-js.com/b85f2bb0-ec91-11eb-ab90-d9ae814b240d.png)

`react`发现新树中，R节点下没有了A，那么直接删除A，在D节点下创建A以及下属节点

上述操作中，只有删除和创建操作

#### conponent层级

如果是同一个类的组件，则会继续往下`diff`运算，如果不是一个类的组件，那么直接删除这个组件下的所有子节点，创建新的

![img](https://static.vue-js.com/c1fcdf00-ec91-11eb-ab90-d9ae814b240d.png)

当`component D`换成了`component G` 后，即使两者的结构非常类似，也会将`D`删除再重新创建`G`

#### element层级

对于比较同一层级的节点们，每个节点在对应的层级用唯一的`key`作为标识

提供了 3 种节点操作，分别为 `INSERT_MARKUP`(插入)、`MOVE_EXISTING` (移动)和 `REMOVE_NODE` (删除)

如下场景：

![img](https://static.vue-js.com/cae1c9a0-ec91-11eb-ab90-d9ae814b240d.png)

通过`key`可以准确地发现新旧集合中的节点都是相同的节点，因此无需进行节点删除和创建，只需要将旧集合中节点的位置进行移动，更新为新集合中节点的位置

流程如下表：

![img](https://static.vue-js.com/d34c5420-ec91-11eb-85f6-6fac77c0c9b3.png)

- index： 新集合的遍历下标。
- oldIndex：当前节点在老集合中的下标
- maxIndex：在新集合访问过的节点中，其在老集合的最大下标

如果当前节点在新集合中的位置比老集合中的位置靠前的话，是不会影响后续节点操作的，这里这时候被动字节不用动

操作过程中只比较oldIndex和maxIndex，规则如下：

- 当oldIndex>maxIndex时，将oldIndex的值赋值给maxIndex
- 当oldIndex=maxIndex时，不操作
- 当oldIndex<maxIndex时，将当前节点移动到index的位置

`diff`过程如下：

- 节点B：此时 maxIndex=0，oldIndex=1；满足 maxIndex< oldIndex，因此B节点不动，此时maxIndex= Math.max(oldIndex, maxIndex)，就是1
- 节点A：此时maxIndex=1，oldIndex=0；不满足maxIndex< oldIndex，因此A节点进行移动操作，此时maxIndex= Math.max(oldIndex, maxIndex)，还是1
- 节点D：此时maxIndex=1, oldIndex=3；满足maxIndex< oldIndex，因此D节点不动，此时maxIndex= Math.max(oldIndex, maxIndex)，就是3
- 节点C：此时maxIndex=3，oldIndex=2；不满足maxIndex< oldIndex，因此C节点进行移动操作，当前已经比较完了

当ABCD节点比较完成后，`diff`过程还没完，还会整体遍历老集合中节点，看有没有没用到的节点，有的话，就删除

### 注意事项

对于简单列表渲染而言，不使用`key`比使用`key`的性能，例如：

将一个[1,2,3,4,5]，渲染成如下的样子：

```html
<div>1</div>
<div>2</div>
<div>3</div>
<div>4</div>
<div>5</div>
```

后续更改成[1,3,2,5,4]，使用`key`与不使用`key`作用如下：

```html
1.加key
<div key='1'>1</div>             <div key='1'>1</div>     
<div key='2'>2</div>             <div key='3'>3</div>  
<div key='3'>3</div>  ========>  <div key='2'>2</div>  
<div key='4'>4</div>             <div key='5'>5</div>  
<div key='5'>5</div>             <div key='4'>4</div>  
操作：节点2移动至下标为2的位置，节点4移动至下标为4的位置。

2.不加key
<div>1</div>             <div>1</div>     
<div>2</div>             <div>3</div>  
<div>3</div>  ========>  <div>2</div>  
<div>4</div>             <div>5</div>  
<div>5</div>             <div>4</div>  
操作：修改第1个到第5个节点的innerText
```

如果我们对这个集合进行增删的操作改成[1,3,2,5,6]

```html
1.加key
<div key='1'>1</div>             <div key='1'>1</div>     
<div key='2'>2</div>             <div key='3'>3</div>  
<div key='3'>3</div>  ========>  <div key='2'>2</div>  
<div key='4'>4</div>             <div key='5'>5</div>  
<div key='5'>5</div>             <div key='6'>6</div>  
操作：节点2移动至下标为2的位置，新增节点6至下标为4的位置，删除节点4。

2.不加key
<div>1</div>             <div>1</div>     
<div>2</div>             <div>3</div>  
<div>3</div>  ========>  <div>2</div>  
<div>4</div>             <div>5</div>  
<div>5</div>             <div>6</div> 
操作：修改第1个到第5个节点的innerText
```

由于`dom`节点的移动操作开销是比较昂贵的，没有`key`的情况下要比有`key`的性能更好

### keys 的作用

Keys 是 React 用于追踪哪些列表中元素被修改、被添加或者被移除的辅助标识。

在 React 中渲染集合时，向每个重复的元素添加关键字对于帮助React跟踪元素与数据之间的关联非常重要。key 应该是唯一ID，最好是 UUID 或收集项中的其他唯一字符串：

```javascript
<ul>
  {todos.map((todo) =>
    <li key={todo.id}>
      {todo.text}
    </li>
  )};
</ul>
复制代码
```

在集合中添加和删除项目时，不使用键或将索引用作键会导致奇怪的行为。

## React Jsx转换成真实DOM过程

`react`通过将组件编写的`JSX`映射到屏幕，以及组件中的状态发生了变化之后 `React`会将这些「变化」更新到屏幕上

在前面文章了解中，`JSX`通过`babel`最终转化成`React.createElement`这种形式，例如：

```jsx
<div>
  < img src="avatar.png" className="profile" />
  <Hello />
</div>
```

会被`bebel`转化成如下：

```jsx
React.createElement(
  "div",
  null,
  React.createElement("img", {
    src: "avatar.png",
    className: "profile"
  }),
  React.createElement(Hello, null)
);
```

在转化过程中，`babel`在编译时会判断 JSX 中组件的首字母：

- 当首字母为小写时，其被认定为原生 `DOM` 标签，`createElement` 的第一个变量被编译为字符串
- 当首字母为大写时，其被认定为自定义组件，createElement 的第一个变量被编译为对象

最终都会通过`RenderDOM.render(...)`方法进行挂载，如下：

```jsx
ReactDOM.render(<App />,  document.getElementById("root"));
```



### 过程

在`react`中，节点大致可以分成四个类别：

- 原生标签节点
- 文本节点
- 函数组件
- 类组件

如下所示：

```jsx
class ClassComponent extends Component {
  static defaultProps = {
    color: "pink"
  };
  render() {
    return (
      <div className="border">
        <h3>ClassComponent</h3>
        <p className={this.props.color}>{this.props.name}</p >
      </div>
    );
  }
}

function FunctionComponent(props) {
  return (
    <div className="border">
      FunctionComponent
      <p>{props.name}</p >
    </div>
  );
}

const jsx = (
  <div className="border">
    <p>xx</p >
    < a href=" ">xxx</ a>
    <FunctionComponent name="函数组件" />
    <ClassComponent name="类组件" color="red" />
  </div>
);
```

这些类别最终都会被转化成`React.createElement`这种形式

`React.createElement`其被调用时会传⼊标签类型`type`，标签属性`props`及若干子元素`children`，作用是生成一个虚拟`Dom`对象，如下所示：

```js
function createElement(type, config, ...children) {
    if (config) {
        delete config.__self;
        delete config.__source;
    }
    // ! 源码中做了详细处理，⽐如过滤掉key、ref等
    const props = {
        ...config,
        children: children.map(child =>
   typeof child === "object" ? child : createTextNode(child)
  )
    };
    return {
        type,
        props
    };
}
function createTextNode(text) {
    return {
        type: TEXT,
        props: {
            children: [],
            nodeValue: text
        }
    };
}
export default {
    createElement
};
```

`createElement`会根据传入的节点信息进行一个判断：

- 如果是原生标签节点， type 是字符串，如div、span
- 如果是文本节点， type就没有，这里是 TEXT
- 如果是函数组件，type 是函数名
- 如果是类组件，type 是类名

虚拟`DOM`会通过`ReactDOM.render`进行渲染成真实`DOM`，使用方法如下：

```jsx
ReactDOM.render(element, container[, callback])
```



当首次调用时，容器节点里的所有 `DOM` 元素都会被替换，后续的调用则会使用 `React` 的 `diff`算法进行高效的更新

如果提供了可选的回调函数`callback`，该回调将在组件被渲染或更新之后被执行

`render`大致实现方法如下：

```js
function render(vnode, container) {
    console.log("vnode", vnode); // 虚拟DOM对象
    // vnode _> node
    const node = createNode(vnode, container);
    container.appendChild(node);
}

// 创建真实DOM节点
function createNode(vnode, parentNode) {
    let node = null;
    const {type, props} = vnode;
    if (type === TEXT) {
        node = document.createTextNode("");
    } else if (typeof type === "string") {
        node = document.createElement(type);
    } else if (typeof type === "function") {
        node = type.isReactComponent
            ? updateClassComponent(vnode, parentNode)
        : updateFunctionComponent(vnode, parentNode);
    } else {
        node = document.createDocumentFragment();
    }
    reconcileChildren(props.children, node);
    updateNode(node, props);
    return node;
}

// 遍历下子vnode，然后把子vnode->真实DOM节点，再插入父node中
function reconcileChildren(children, node) {
    for (let i = 0; i < children.length; i++) {
        let child = children[i];
        if (Array.isArray(child)) {
            for (let j = 0; j < child.length; j++) {
                render(child[j], node);
            }
        } else {
            render(child, node);
        }
    }
}
function updateNode(node, nextVal) {
    Object.keys(nextVal)
        .filter(k => k !== "children")
        .forEach(k => {
        if (k.slice(0, 2) === "on") {
            let eventName = k.slice(2).toLocaleLowerCase();
            node.addEventListener(eventName, nextVal[k]);
        } else {
            node[k] = nextVal[k];
        }
    });
}

// 返回真实dom节点
// 执行函数
function updateFunctionComponent(vnode, parentNode) {
    const {type, props} = vnode;
    let vvnode = type(props);
    const node = createNode(vvnode, parentNode);
    return node;
}

// 返回真实dom节点
// 先实例化，再执行render函数
function updateClassComponent(vnode, parentNode) {
    const {type, props} = vnode;
    let cmp = new type(props);
    const vvnode = cmp.render();
    const node = createNode(vvnode, parentNode);
    return node;
}
export default {
    render
};
```

### 总结

在`react`源码中，虚拟`Dom`转化成真实`Dom`整体流程如下图所示：

![img](https://static.vue-js.com/28824fa0-f00a-11eb-ab90-d9ae814b240d.png)

其渲染流程如下所示：

- 使用React.createElement或JSX编写React组件，实际上所有的 JSX 代码最后都会转换成React.createElement(...) ，Babel帮助我们完成了这个转换的过程。
- createElement函数对key和ref等特殊的props进行处理，并获取defaultProps对默认props进行赋值，并且对传入的孩子节点进行处理，最终构造成一个虚拟DOM对象
- ReactDOM.render将生成好的虚拟DOM渲染到指定容器上，其中采用了批处理、事务等机制并且对特定浏览器进行了性能优化，最终转换为真实DOM

## React 生命周期

跟`Vue`一样，`React`整个组件生命周期包括从创建、初始化数据、编译模板、挂载Dom→渲染、更新→渲染、卸载等一系列过程

### 流程

这里主要讲述`react16.4`之后的生命周期，可以分成三个阶段：

- 创建阶段
- 更新阶段
- 卸载阶段

#### 创建阶段

创建阶段主要分成了以下几个生命周期方法：

- constructor
- getDerivedStateFromProps
- render
- componentDidMount

#### constructor

实例过程中自动调用的方法，在方法内部通过`super`关键字获取来自父组件的`props`

在该方法中，通常的操作为初始化`state`状态或者在`this`上挂载方法

#### getDerivedStateFromProps

该方法是新增的生命周期方法，是一个静态的方法，因此不能访问到组件的实例

执行时机：组件创建和更新阶段，不论是`props`变化还是`state`变化，也会调用

在每次`render`方法前调用，第一个参数为即将更新的`props`，第二个参数为上一个状态的`state`，可以比较`props` 和 `state`来加一些限制条件，防止无用的state更新

该方法需要返回一个新的对象作为新的`state`或者返回`null`表示`state`状态不需要更新

#### render

类组件必须实现的方法，用于渲染`DOM`结构，可以访问组件`state`与`prop`属性

注意： 不要在 `render` 里面 `setState`, 否则会触发死循环导致内存崩溃

#### componentDidMount

组件挂载到真实`DOM`节点后执行，其在`render`方法之后执行

此方法多用于执行一些数据获取，事件监听等操作

### 更新阶段

该阶段的函数主要为如下方法：

- getDerivedStateFromProps
- shouldComponentUpdate
- render
- getSnapshotBeforeUpdate
- componentDidUpdate

#### getDerivedStateFromProps

该方法介绍同上

#### shouldComponentUpdate

用于告知组件本身基于当前的`props`和`state`是否需要重新渲染组件，默认情况返回`true`

执行时机：到新的props或者state时都会调用，通过返回true或者false告知组件更新与否

一般情况，不建议在该周期方法中进行深层比较，会影响效率

同时也不能调用`setState`，否则会导致无限循环调用更新

#### render

介绍如上

#### getSnapshotBeforeUpdate

该周期函数在`render`后执行，执行之时`DOM`元素还没有被更新

该方法返回的一个`Snapshot`值，作为`componentDidUpdate`第三个参数传入

```jsx
getSnapshotBeforeUpdate(prevProps, prevState) {
    console.log('#enter getSnapshotBeforeUpdate');
    return 'foo';
}

componentDidUpdate(prevProps, prevState, snapshot) {
    console.log('#enter componentDidUpdate snapshot = ', snapshot);
}
```

此方法的目的在于获取组件更新前的一些信息，比如组件的滚动位置之类的，在组件更新后可以根据这些信息恢复一些UI视觉上的状态

#### componentDidUpdate

执行时机：组件更新结束后触发

在该方法中，可以根据前后的`props`和`state`的变化做相应的操作，如获取数据，修改`DOM`样式等

### 卸载阶段

#### componentWillUnmount

此方法用于组件卸载前，清理一些注册是监听事件，或者取消订阅的网络请求等

一旦一个组件实例被卸载，其不会被再次挂载，而只可能是被重新创建

### 总结

新版生命周期整体流程如下图所示：

![img](https://static.vue-js.com/66c999c0-d373-11eb-85f6-6fac77c0c9b3.png)

旧的生命周期流程图如下：

![img](https://static.vue-js.com/d379e420-d374-11eb-ab90-d9ae814b240d.png)

通过两个图的对比，可以发现新版的生命周期减少了以下三种方法：

- componentWillMount
- componentWillReceiveProps
- componentWillUpdate

其实这三个方法仍然存在，只是在前者加上了`UNSAFE_`前缀，如`UNSAFE_componentWillMount`，并不像字面意思那样表示不安全，而是表示这些生命周期的代码可能在未来的 `react`版本可能废除

同时也新增了两个生命周期函数：

- getDerivedStateFromProps
- getSnapshotBeforeUpdate

### React的请求应该放在哪个生命周期中?

React的异步请求到底应该放在哪个生命周期里,有人认为在`componentWillMount`中可以提前进行异步请求,避免白屏,其实这个观点是有问题的.

由于JavaScript中异步事件的性质，当您启动API调用时，浏览器会在此期间返回执行其他工作。当React渲染一个组件时，它不会等待componentWillMount它完成任何事情 - React继续前进并继续render,没有办法“暂停”渲染以等待数据到达。

而且在`componentWillMount`请求会有一系列潜在的问题,首先,在服务器渲染时,如果在 componentWillMount 里获取数据，fetch data会执行两次，一次在服务端一次在客户端，这造成了多余的请求,其次,在React 16进行React Fiber重写后,`componentWillMount`可能在一次渲染中多次调用.

目前官方推荐的异步请求是在`componentDidmount`中进行.

如果有特殊需求需要提前请求,也可以在特殊情况下在`constructor`中请求:

> react 17之后`componentWillMount`会被废弃,仅仅保留`UNSAFE_componentWillMount`

## React事件机制

React基于浏览器的事件机制自身实现了一套事件机制，包括事件注册、事件的合成、事件冒泡、事件派发等

在React中这套事件机制被称之为合成事件

**合成事件(SyntheticEvent)**

合成事件是 React模拟原生 DOM事件所有能力的一个事件对象，即浏览器原生事件的跨浏览器包装器

根据 W3C规范来定义合成事件，兼容所有浏览器，拥有与浏览器原生事件相同的接口

如果想要获得原生DOM事件，可以通过e.nativeEvent属性获取

```
const handleClick = (e) => console.log(e.nativeEvent);; 
const button = <button onClick={handleClick}>按钮</button> 
```

从上面可以看到React事件和原生事件也非常的相似，但也有一定的区别：

- 事件名称命名方式不同

```
// 原生事件绑定方式 
<button onclick="handleClick()">按钮命名</button> 
       
// React 合成事件绑定方式 
const button = <button onClick={handleClick}>按钮命名</button> 
```

- 事件处理函数书写不同

```
// 原生事件 事件处理函数写法 
<button onclick="handleClick()">按钮命名</button> 
       
// React 合成事件 事件处理函数写法 
const button = <button onClick={handleClick}>按钮命名</button> 
```

虽然onclick看似绑定到DOM元素上，但实际并不会把事件代理函数直接绑定到真实的节点上，而是把所有的事件绑定到结构的最外层，使用一个统一的事件去监听

这个事件监听器上维持了一个映射来保存所有组件内部的事件监听和处理函数。当组件挂载或卸载时，只是在这个统一的事件监听器上插入或删除一些对象

当事件发生时，首先被这个统一的事件监听器处理，然后在映射里找到真正的事件处理函数并调用。这样做简化了事件处理和回收机制，效率也有很大提升

### 执行顺序

关于React合成事件与原生事件执行顺序，可以看看下面一个例子：

```
import  React  from 'react'; 
class App extends React.Component{ 
 
  constructor(props) { 
    super(props); 
    this.parentRef = React.createRef(); 
    this.childRef = React.createRef(); 
  } 
  componentDidMount() { 
    console.log("React componentDidMount！"); 
    this.parentRef.current?.addEventListener("click", () => { 
      console.log("原生事件：父元素 DOM 事件监听！"); 
    }); 
    this.childRef.current?.addEventListener("click", () => { 
      console.log("原生事件：子元素 DOM 事件监听！"); 
    }); 
    document.addEventListener("click", (e) => { 
      console.log("原生事件：document DOM 事件监听！"); 
    }); 
  } 
  parentClickFun = () => { 
    console.log("React 事件：父元素事件监听！"); 
  }; 
  childClickFun = () => { 
    console.log("React 事件：子元素事件监听！"); 
  }; 
  render() { 
    return ( 
      <div ref={this.parentRef} onClick={this.parentClickFun}> 
        <div ref={this.childRef} onClick={this.childClickFun}> 
          分析事件执行顺序 
        </div> 
      </div> 
    ); 
  } 
} 
export default App; 
```

输出顺序为：

复制

```
原生事件：子元素 DOM 事件监听！  
原生事件：父元素 DOM 事件监听！  
React 事件：子元素事件监听！  
React 事件：父元素事件监听！  
原生事件：document DOM 事件监听！  
1.2.3.4.5.
```

可以得出以下结论：

- React 所有事件都挂载在 document 对象上
- 当真实 DOM 元素触发事件，会冒泡到 document 对象后，再处理 React 事件
- 所以会先执行原生事件，然后处理 React 事件
- 最后真正执行 document 上挂载的事件

对应过程如图所示：

[![img](https://s3.51cto.com/oss/202106/30/55439577dc4407a159c44719e1c8038c.jpg)](https://s3.51cto.com/oss/202106/30/55439577dc4407a159c44719e1c8038c.jpg)

[![img](https://s2.51cto.com/oss/202106/30/7bff46537e944e01939ec33d7f5bdb44.jpg)](https://s2.51cto.com/oss/202106/30/7bff46537e944e01939ec33d7f5bdb44.jpg)

所以想要阻止不同时间段的冒泡行为，对应使用不同的方法，对应如下：

- 阻止合成事件间的冒泡，用e.stopPropagation()
- 阻止合成事件与最外层 document 上的事件间的冒泡，用e.nativeEvent.stopImmediatePropagation()
- 阻止合成事件与最外层document上的原生事件上的冒泡，通过判断e.target来避免

复制

```
document.body.addEventListener('click', e => {    
    if (e.target && e.target.matches('div.code')) {   
        return;     
    }     
    this.setState({   active: false,    });   });  
} 
```

### 总结

React事件机制总结如下：

- React 上注册的事件最终会绑定在document这个 DOM 上，而不是 React 组件对应的 DOM(减少内存开销就是因为所有的事件都绑定在 document 上，其他节点没有绑定事件)
- React 自身实现了一套事件冒泡机制，所以这也就是为什么我们 event.stopPropagation()无效的原因。
- React 通过队列的形式，从触发的组件向父组件回溯，然后调用他们 JSX 中定义的 callback
- React 有一套自己的合成事件 SyntheticEvent

### React17事件机制

React v17 中，React 不会再将事件处理添加到 `document` 上，而是将事件处理添加到渲染 React 树的根 DOM 容器中：

```js
const rootNode = document.getElementById('root');
ReactDOM.render(<App />, rootNode);
复制代
```

在 React 16 及之前版本中，React 会对大多数事件进行 `document.addEventListener()` 操作。React v17 开始会通过调用 `rootNode.addEventListener()` 来代替。

react17会在rootContainer上监听所有事件的冒泡和捕获阶段，不能冒泡的事件会直接在对应的dom节点上直接绑定

dom节点上不会绑定事件监听器（除了几种特殊情况）

代码element中的事件回调函数保存在fiber节点的props中

监听到事件后，事件会在rootContainer上处理捕获和冒泡阶段（不能冒泡的事件在对应的dom节点上处理），经过函数调用，最后会调用`dispatchEventsForPlugins`将native事件处理成合成事件，并会从触发节点的fiber节点开始向上搜寻监听相同事件的fiber节点（不能冒泡的事件不会向上搜索），将props中的回调函数放到一个数组里等待执行

最后根据是冒泡事件还是捕获事件，决定执行顺序



## 受控元素和非受控元素

### 受控组件

受控组件，简单来讲，就是受我们控制的组件，组件的状态全程响应外部数据

举个简单的例子：

```js
class TestComponent extends React.Component {
  constructor (props) {
    super(props);
    this.state = { username: 'lindaidai' };
  }
  render () {
    return <input name="username" value={this.state.username} />
  }
}
```

这时候当我们在输入框输入内容的时候，会发现输入的内容并无法显示出来，也就是`input`标签是一个可读的状态

这是因为`value`被`this.state.username`所控制住。当用户输入新的内容时，`this.state.username`并不会自动更新，这样的话`input`内的内容也就不会变了

如果想要解除被控制，可以为`input`标签设置`onChange`事件，输入的时候触发事件函数，在函数内部实现`state`的更新，从而导致`input`框的内容页发现改变

因此，受控组件我们一般需要初始状态和一个状态更新事件函数

### 非受控组件

非受控组件，简单来讲，就是不受我们控制的组件

一般情况是在初始化的时候接受外部数据，然后自己在内部存储其自身状态

当需要时，可以使用`ref` 查询 `DOM`并查找其当前值，如下：

```jsx
import React, { Component } from 'react';

export class UnControll extends Component {
  constructor (props) {
    super(props);
    this.inputRef = React.createRef();
  }
  handleSubmit = (e) => {
    console.log('我们可以获得input内的值为', this.inputRef.current.value);
    e.preventDefault();
  }
  render () {
    return (
      <form onSubmit={e => this.handleSubmit(e)}>
        <input defaultValue="lindaidai" ref={this.inputRef} />
        <input type="submit" value="提交" />
      </form>
    )
  }
}
```

### 异同和使用场景

 1、**受控组件** 

受控组件依赖于状态 

受控组件的修改会实时映射到状态值上，此时可以对输入的内容进行校验 

受控组件只有继承React.Component才会有状态 

受控组件必须要在表单上使用onChange事件来绑定对应的事件 

2、**非受控组件** 

非受控组件不受状态的控制 

非受控组件获取数据就是相当于操作DOM 

非受控组件可以很容易和第三方组件结合，更容易同时集成 React 和非 React 代码

**总的来说：**

- 共同点，都是指表单元素，或者表单组件
- 不同点，被react的state控制，就是受控组件。不会state控制，就是非受控。
- 受控组件的实现方式，就是设置state，使用事件调用setstate，更新数据和视图。
- 非受控组件，避开state，使用ref等等方式，更新数据和视图。 

**选择受控组件还是非受控组件** 1、受控组件使用场景：一般用在需要动态设置其初始值的情况。例如：某些form表单信息编辑时，input表单元素需要初始显示[服务器](https://cloud.tencent.com/product/cvm?from=10680)返回的某个值然后进行编辑。 2、非受控组件使用场景：一般用于无任何动态初始值信息的情况。例如：form表单创建信息时，input表单元素都没有初始值，需要用户输入的情况。

## React高阶组件HOC

高阶函数（Higher-order function），至少满足下列一个条件的函数

- 接受一个或多个函数作为输入
- 输出一个函数

在`React`中，高阶组件即接受一个或多个组件作为参数并且返回一个组件，本质也就是一个函数，并不是一个组件

```jsx
const EnhancedComponent = highOrderComponent(WrappedComponent);
```

上述代码中，该函数接受一个组件`WrappedComponent`作为参数，返回加工过的新组件`EnhancedComponent`

高阶组件的这种实现方式，本质上是一个装饰者设计模式

### 如何编写

最基本的高阶组件的编写模板如下：

```jsx
import React, { Component } from 'react';

export default (WrappedComponent) => {
  return class EnhancedComponent extends Component {
    // do something
    render() {
      return <WrappedComponent />;
    }
  }
}
```

通过对传入的原始组件 `WrappedComponent` 做一些你想要的操作（比如操作 props，提取 state，给原始组件包裹其他元素等），从而加工出想要的组件 `EnhancedComponent`

把通用的逻辑放在高阶组件中，对组件实现一致的处理，从而实现代码的复用

所以，高阶组件的主要功能是封装并分离组件的通用逻辑，让通用逻辑在组件间更好地被复用

但在使用高阶组件的同时，一般遵循一些约定，如下：

- props 保持一致
- 你不能在函数式（无状态）组件上使用 ref 属性，因为它没有实例
- 不要以任何方式改变原始组件 WrappedComponent
- 透传不相关 props 属性给被包裹的组件 WrappedComponent
- 不要再 render() 方法中使用高阶组件
- 使用 compose 组合高阶组件
- 包装显示名字以便于调试

这里需要注意的是，高阶组件可以传递所有的`props`，但是不能传递`ref`

如果向一个高阶组件添加`refe`引用，那么`ref` 指向的是最外层容器组件实例的，而不是被包裹的组件，如果需要传递`refs`的话，则使用`React.forwardRef`，如下：

```jsx
function withLogging(WrappedComponent) {
    class Enhance extends WrappedComponent {
        componentWillReceiveProps() {
            console.log('Current props', this.props);
            console.log('Next props', nextProps);
        }
        render() {
            const {forwardedRef, ...rest} = this.props;
            // 把 forwardedRef 赋值给 ref
            return <WrappedComponent {...rest} ref={forwardedRef} />;
        }
    };

    // React.forwardRef 方法会传入 props 和 ref 两个参数给其回调函数
    // 所以这边的 ref 是由 React.forwardRef 提供的
    function forwardRef(props, ref) {
        return <Enhance {...props} forwardRef={ref} />
    }

    return React.forwardRef(forwardRef);
}
const EnhancedComponent = withLogging(SomeComponent);
```

### 应用场景

通过上面的了解，高阶组件能够提高代码的复用性和灵活性，在实际应用中，常常用于与核心业务无关但又在多个模块使用的功能，如权限控制、日志记录、数据校验、异常处理、统计上报等

举个例子，存在一个组件，需要从缓存中获取数据，然后渲染。一般情况，我们会如下编写：

```jsx
import React, { Component } from 'react'

class MyComponent extends Component {

  componentWillMount() {
      let data = localStorage.getItem('data');
      this.setState({data});
  }
  
  render() {
    return <div>{this.state.data}</div>
  }
}
```

上述代码当然可以实现该功能，但是如果还有其他组件也有类似功能的时候，每个组件都需要重复写`componentWillMount`中的代码，这明显是冗杂的

下面就可以通过高价组件来进行改写，如下：

```jsx
import React, { Component } from 'react'

function withPersistentData(WrappedComponent) {
  return class extends Component {
    componentWillMount() {
      let data = localStorage.getItem('data');
        this.setState({data});
    }
    
    render() {
      // 通过{...this.props} 把传递给当前组件的属性继续传递给被包装的组件WrappedComponent
      return <WrappedComponent data={this.state.data} {...this.props} />
    }
  }
}

class MyComponent2 extends Component {  
  render() {
    return <div>{this.props.data}</div>
  }
}

const MyComponentWithPersistentData = withPersistentData(MyComponent2)
```

再比如组件渲染性能监控，如下：

```jsx
class Home extends React.Component {
    render() {
        return (<h1>Hello World.</h1>);
    }
}
function withTiming(WrappedComponent) {
    return class extends WrappedComponent {
        constructor(props) {
            super(props);
            this.start = 0;
            this.end = 0;
        }
        componentWillMount() {
            super.componentWillMount && super.componentWillMount();
            this.start = Date.now();
        }
        componentDidMount() {
            super.componentDidMount && super.componentDidMount();
            this.end = Date.now();
            console.log(`${WrappedComponent.name} 组件渲染时间为 ${this.end - this.start} ms`);
        }
        render() {
            return super.render();
        }
    };
}

export default withTiming(Home);
```

## React Hooks

`Hook` 是 React 16.8 的新增特性。它可以让你在不编写 `class` 的情况下使用 `state` 以及其他的 `React` 特性

至于为什么引入`hook`，官方给出的动机是解决长时间使用和维护`react`过程中常遇到的问题，例如：

- 难以重用和共享组件中的与状态相关的逻辑
- 逻辑复杂的组件难以开发与维护，当我们的组件需要处理多个互不相关的 local state 时，每个生命周期函数中可能会包含着各种互不相关的逻辑在里面
- 类组件中的this增加学习成本，类组件在基于现有工具的优化上存在些许问题
- 由于业务变动，函数组件不得不改为类组件等等

在以前，函数组件也被称为无状态的组件，只负责渲染的一些工作

因此，现在的函数组件也可以是有状态的组件，内部也可以维护自身的状态以及做一些逻辑方面的处理

### 有哪些

上面讲到，`Hooks`让我们的函数组件拥有了类组件的特性，例如组件内的状态、生命周期

最常见的`hooks`有如下：

- useState
- useEffect
- 其他

#### useState

首先给出一个例子，如下：

```js
import React, { useState } from 'react';

function Example() {
  // 声明一个叫 "count" 的 state 变量
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p >
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

在函数组件中通过`useState`实现函数内部维护`state`，参数为`state`默认的值，返回值是一个数组，第一个值为当前的`state`，第二个值为更新`state`的函数

该函数组件等价于的类组件如下：

```js
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p >
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

从上述两种代码分析，可以看出两者区别：

- state声明方式：在函数组件中通过 useState 直接获取，类组件通过constructor 构造函数中设置
- state读取方式：在函数组件中直接使用变量，类组件通过`this.state.count`的方式获取
- state更新方式：在函数组件中通过 setCount 更新，类组件通过this.setState()

总的来讲，useState 使用起来更为简洁，减少了`this`指向不明确的情况

#### useEffect

`useEffect`可以让我们在函数组件中进行一些带有副作用的操作

同样给出一个计时器示例：

```js
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  componentDidMount() {
    document.title = `You clicked ${this.state.count} times`;
  }
  componentDidUpdate() {
    document.title = `You clicked ${this.state.count} times`;
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p >
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

从上面可以看见，组件在加载和更新阶段都执行同样操作

而如果使用`useEffect`后，则能够将相同的逻辑抽离出来，这是类组件不具备的方法

对应的`useEffect`示例如下：

```jsx
import React, { useState, useEffect } from 'react';
function Example() {
  const [count, setCount] = useState(0);
 
  useEffect(() => {    document.title = `You clicked ${count} times`;  });
  return (
    <div>
      <p>You clicked {count} times</p >
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

`useEffect`第一个参数接受一个回调函数，默认情况下，`useEffect`会在第一次渲染和更新之后都会执行，相当于在`componentDidMount`和`componentDidUpdate`两个生命周期函数中执行回调

如果某些特定值在两次重渲染之间没有发生变化，你可以跳过对 effect 的调用，这时候只需要传入第二个参数，如下：

```js
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // 仅在 count 更改时更新
```

上述传入第二个参数后，如果 `count` 的值是 `5`，而且我们的组件重渲染的时候 `count` 还是等于 `5`，React 将对前一次渲染的 `[5]` 和后一次渲染的 `[5]` 进行比较，如果是相等则跳过`effects`执行

回调函数中可以返回一个清除函数，这是`effect`可选的清除机制，相当于类组件中`componentwillUnmount`生命周期函数，可做一些清除副作用的操作，如下：

```jsx
useEffect(() => {
    function handleStatusChange(status) {
        setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
        ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
});
```

所以， `useEffect`相当于`componentDidMount`，`componentDidUpdate` 和 `componentWillUnmount` 这三个生命周期函数的组合

#### 其它 hooks

在组件通信过程中可以使用`useContext`，`refs`学习中我们也用到了`useRef`获取`DOM`结构......

还有很多额外的`hooks`，如：

- useReducer
- useCallback
- useMemo
- useRef

### 解决什么

通过对上面的初步认识，可以看到`hooks`能够更容易解决状态相关的重用的问题：

- 每调用useHook一次都会生成一份独立的状态
- 通过自定义hook能够更好的封装我们的功能

编写`hooks`为函数式编程，每个功能都包裹在函数中，整体风格更清爽，更优雅

```
hooks`的出现，使函数组件的功能得到了扩充，拥有了类组件相似的功能，在我们日常使用中，使用`hooks`能够解决大多数问题，并且还拥有代码复用机制，因此优先考虑`hooks
```

## react中引入css

组件式开发选择合适的`css`解决方案尤为重要

通常会遵循以下规则：

- 可以编写局部css，不会随意污染其他组件内的原生；
- 可以编写动态的css，可以获取当前组件的一些状态，根据状态的变化生成不同的css样式；
- 支持所有的css特性：伪类、动画、媒体查询等；
- 编写起来简洁方便、最好符合一贯的css风格特点

在这一方面，`vue`使用`css`起来更为简洁：

- 通过 style 标签编写样式
- scoped 属性决定编写的样式是否局部有效
- lang 属性设置预处理器
- 内联样式风格的方式来根据最新状态设置和改变css

而在`react`中，引入`CSS`就不如`Vue`方便简洁，其引入`css`的方式有很多种，各有利弊

### 方式

常见的`CSS`引入方式有以下：

- 在组件内直接使用
- 组件中引入 .css 文件
- 组件中引入 .module.css 文件
- CSS in JS

#### 在组件内直接使用

直接在组件中书写`css`样式，通过`style`属性直接引入，如下：

```js
import React, { Component } from "react";

const div1 = {
  width: "300px",
  margin: "30px auto",
  backgroundColor: "#44014C",  //驼峰法
  minHeight: "200px",
  boxSizing: "border-box"
};

class Test extends Component {
  constructor(props, context) {
    super(props);
  }
 
  render() {
    return (
     <div>
       <div style={div1}>123</div>
       <div style={{backgroundColor:"red"}}>
     </div>
    );
  }
}

export default Test;
```

上面可以看到，`css`属性需要转换成驼峰写法

这种方式优点：

- 内联样式, 样式之间不会有冲突
- 可以动态获取当前state中的状态

缺点：

- 写法上都需要使用驼峰标识
- 某些样式没有提示
- 大量的样式, 代码混乱
- 某些样式无法编写(比如伪类/伪元素)

#### 组件中引入css文件

将`css`单独写在一个`css`文件中，然后在组件中直接引入

`App.css`文件：

```css
.title {
  color: red;
  font-size: 20px;
}

.desc {
  color: green;
  text-decoration: underline;
}
```

组件中引入：

```js
import React, { PureComponent } from 'react';

import Home from './Home';

import './App.css';

export default class App extends PureComponent {
  render() {
    return (
      <div className="app">
        <h2 className="title">我是App的标题</h2>
        <p className="desc">我是App中的一段文字描述</p >
        <Home/>
      </div>
    )
  }
}
```

这种方式存在不好的地方在于样式是全局生效，样式之间会互相影响

#### 组件中引入 .module.css 文件

将`css`文件作为一个模块引入，这个模块中的所有`css`，只作用于当前组件。不会影响当前组件的后代组件

这种方式是`webpack`特工的方案，只需要配置`webpack`配置文件中`modules:true`即可

```jsx
import React, { PureComponent } from 'react';

import Home from './Home';

import './App.module.css';

export default class App extends PureComponent {
  render() {
    return (
      <div className="app">
        <h2 className="title">我是App的标题</h2>
        <p className="desc">我是App中的一段文字描述</p >
        <Home/>
      </div>
    )
  }
}
```

这种方式能够解决局部作用域问题，但也有一定的缺陷：

- 引用的类名，不能使用连接符(.xxx-xx)，在 JavaScript 中是不识别的
- 所有的 className 都必须使用 {style.className} 的形式来编写
- 不方便动态来修改某些样式，依然需要使用内联样式的方式；

#### CSS in JS

CSS-in-JS， 是指一种模式，其中`CSS`由 `JavaScript`生成而不是在外部文件中定义

此功能并不是 React 的一部分，而是由第三方库提供，例如：

- styled-components
- emotion
- glamorous

下面主要看看`styled-components`的基本使用

本质是通过函数的调用，最终创建出一个组件：

- 这个组件会被自动添加上一个不重复的class
- styled-components会给该class添加相关的样式

基本使用如下：

创建一个`style.js`文件用于存放样式组件：

```js
export const SelfLink = styled.div`
  height: 50px;
  border: 1px solid red;
  color: yellow;
`;

export const SelfButton = styled.div`
  height: 150px;
  width: 150px;
  color: ${props => props.color};
  background-image: url(${props => props.src});
  background-size: 150px 150px;
`;
```

引入样式组件也很简单：

```jsx
import React, { Component } from "react";

import { SelfLink, SelfButton } from "./style";

class Test extends Component {
  constructor(props, context) {
    super(props);
  }  
 
  render() {
    return (
     <div>
       <SelfLink title="People's Republic of China">app.js</SelfLink>
       <SelfButton color="palevioletred" style={{ color: "pink" }} src={fist}>
          SelfButton
        </SelfButton>
     </div>
    );
  }
}

export default Test;
```

### 区别

通过上面四种样式的引入，可以看到：

- 在组件内直接使用`css`该方式编写方便，容易能够根据状态修改样式属性，但是大量的演示编写容易导致代码混乱
- 组件中引入 .css 文件符合我们日常的编写习惯，但是作用域是全局的，样式之间会层叠
- 引入.module.css 文件能够解决局部作用域问题，但是不方便动态修改样式，需要使用内联的方式进行样式的编写
- 通过css in js 这种方法，可以满足大部分场景的应用，可以类似于预处理器一样样式嵌套、定义、修改状态等

至于使用`react`用哪种方案引入`css`，并没有一个绝对的答案，可以根据各自情况选择合适的方案

## React组件之间通信的方法

- 父组件 => 子组件：
  1. Props
  2. Instance Methods
- 子组件 => 父组件：
  1. Callback Functions
  2. Event Bubbling
- 兄弟组件之间：
  1. Parent Component
- 不太相关的组件之间：
  1. Context
  2. Portals
  3. Global Variables
  4. Observer Pattern
  5. Redux等

### 1. Props

这是最常见的react组件之间传递信息的方法了吧，父组件通过props把数据传给子组件，子组件通过`this.props`去使用相应的数据

```javascript
const Child = ({ name }) => {
    <div>{name}</div>
}

class Parent extends React.Component {
    constructor(props) {
        super(props)
        this.state = {
            name: 'zach'
        }
    }
    render() {
        return (
            <Child name={this.state.name} />
        )
    }
}
```

### 2. Instance Methods

第二种父组件向子组件传递信息的方式有些同学可能会比较陌生，但这种方式非常有用，请务必掌握。原理就是：父组件可以通过使用`refs`来直接调用子组件实例的方法，看下面的例子：

```scala
class Child extends React.Component {
  myFunc() {
    return "hello"
  }
}

class Parent extends React.Component {
  componentDidMount() {
    var x = this.foo.myFunc()   // x is now 'hello'
  }
  render() {
    return (
      <Child
        ref={foo => {
          this.foo = foo
        }}
      />
    )
  }
}
```

大致的过程：

1. 首先子组件有一个方法myFunc
2. 父组件给子组件传递一个ref属性，并且采用[callback-refs](https://link.segmentfault.com/?enc=77FUMGM07WEpZN6Id3vP%2Fw%3D%3D.%2B8eJJRRL%2BrZOJzK978KWI5F7NkV5GQJJ4uXmelPlWePk5V7dbG0B9vAZxWu0GdVU%2BA4K3vM9DWUUhuBQufEggg%3D%3D)的形式。这个callback函数接收react组件实例/原生dom元素作为它的参数。当父组件挂载时，react会去执行这个ref回调函数，并将子组件实例作为参数传给回调函数，然后我们把子组件实例赋值给`this.foo`。
3. 最后我们在父组件当中就可以使用`this.foo`来调用子组件的方法咯

### 3. Callback Functions

讲完了父组件给子组件传递信息的两种方式，我们再来讲子组件给父组件传递信息的方法。回调函数这个方法也是react最常见的一种方式，子组件通过调用父组件传来的回调函数，从而将数据传给父组件。

```javascript
const Child = ({ onClick }) => {
    <div onClick={() => onClick('zach')}>Click Me</div>
}

class Parent extends React.Component {
    handleClick = (data) => {
        console.log("Parent received value from child: " + data)
    }
    render() {
        return (
            <Child onClick={this.handleClick} />
        )
    }
}
```

### 4. Event Bubbling

这种方法其实跟react本身没有关系，我们利用的是原生dom元素的事件冒泡机制。

```javascript
class Parent extends React.Component {
  render() {
    return (
      <div onClick={this.handleClick}>
         <Child />
      </div>
    );
  }
  handleClick = () => {
    console.log('clicked')
  }
}
function Child {
  return (
    <button>Click</button>
  );    
}
```

巧妙的利用下事件冒泡机制，我们就可以很方便的在父组件的元素上接收到来自子组件元素的点击事件

### 5. Parent Component

讲完了父子组件间的通信，再来看非父子组件之间的通信方法。一般来说，两个非父子组件想要通信，首先我们可以看看它们是否是兄弟组件，即它们是否在同一个父组件下。如果不是的话，考虑下用一个组件把它们包裹起来从而变成兄弟组件是否合适。这样一来，它们就可以通过父组件作为中间层来实现数据互通了。

```scala
class Parent extends React.Component {
  constructor(props) {
    super(props)
    this.state = {count: 0}
  }
  setCount = () => {
    this.setState({count: this.state.count + 1})
  }
  render() {
    return (
      <div>
        <SiblingA
          count={this.state.count}
        />
        <SiblingB
          onClick={this.setCount}
        />
      </div>
    );
  }

}
```

### 6. Context

通常一个前端应用会有一些"全局"性质的数据，比如当前登陆的用户信息、ui主题、用户选择的语言等等。这些全局数据，很多组件可能都会用到，当组件层级很深时，用我们之前的方法，就得通过props一层一层传递下去，这显然太麻烦了，看下面的示例：

```scala
class App extends React.Component {
  render() {
    return <Toolbar theme="dark" />;
  }
}

function Toolbar(props) {
  return (
    <div>
      <ThemedButton theme={props.theme} />
    </div>
  );
}

class ThemedButton extends React.Component {
  render() {
    return <Button theme={this.props.theme} />;
  }
}
```

上面的例子，为了让我们的Button元素拿到主题色，我们必须把theme作为props，从App传到Toolbar，再从Toolbar传到ThemedButton，最后Button从父组件ThemedButton的props里终于拿到了主题theme。假如我们不同组件里都有用到Button，就得把theme向这个例子一样到处层层传递，麻烦至极。

因此react为我们提供了一个新api：Context，我们用Context改写下上例

```scala
const ThemeContext = React.createContext('light');

class App extends React.Component {
  render() {
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}

function Toolbar() {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

class ThemedButton extends React.Component {
  static contextType = ThemeContext;
  render() {
    return <Button theme={this.context} />;
  }
}
```

简单的解析一下：

1. `React.createContext`创建了一个Context对象，假如某个组件订阅了这个对象，当react去渲染这个组件时，会从离这个组件最近的一个`Provider`组件中读取当前的context值

2. `Context.Provider`: 每一个Context对象都有一个`Provider`属性，这个属性是一个react组件。在Provider组件以内的所有组件都可以通过它订阅context值的变动。具体来说，Provider组件有一个叫`value`的prop传递给所有内部组件，每当`value`的值发生变化时，Provider内部的组件都会根据新value值重新渲染

3. 那内部的组件该怎么使用这个context对象里的东西呢？
   a. 假如内部组件是用class声明的有状态组件：我们可以把Context对象赋值给这个类的属性`contextType`，如上面所示的ThemedButton组件

   ```scala
       class ThemedButton extends React.Component {
         static contextType = ThemeContext;
         render() {
           const value = this.context
           return <Button theme={value} />;
         }
       }
   ```

   b. 假如内部组件是用function创建的无状态组件：我们可以使用`Context.Consumer`，这也是Context对象直接提供给我们的组件，这个组件接受一个函数作为自己的child，这个函数的入参就是context的value，并返回一个react组件。可以将上面的ThemedButton改写下：

   ```javascript
       function ThemedButton {
           return (
               <ThemeContext.Consumer>
                   {value => <Button theme={value} />}
               </ThemeContext.Consumer>
           )
       }
   ```

最后提一句，context对于解决react组件层级很深的props传递很有效，但也不应该被滥用。只有像theme、language等这种全局属性（很多组件都有可能依赖它们）时，才考虑用context。如果只是单纯为了解决层级很深的props传递，可以直接用[component composition](https://link.segmentfault.com/?enc=wg8Ou%2Fw6VDJT2gq%2BVscIFQ%3D%3D.38Hf5gmnFjAXzIAbcFyPgCTQKmHk%2BbcOSYq30kUpHc1U3XZ6mEQ7GLuE4BFPzcWLze6YFrjXwoJdidp4NfHPoA%3D%3D)

### 7. Portals

Portals也是react提供的新特性，虽然它并不是用来解决组件通信问题的，但因为它也涉及到了组件通信的问题，所以我也把它列在我们的十种方法里面。

Portals的主要应用场景是：当两个组件在react项目中是父子组件的关系，但在HTML DOM里并不想是父子元素的关系。

举个例子，有一个父组件Parent，它里面包含了一个子组件Tooltip，虽然在react层级上它们是父子关系，但我们希望子组件Tooltip渲染的元素在DOM中直接挂载在body节点里，而不是挂载在父组件的元素里。这样就可以避免父组件的一些样式（如`overflow:hidden`、`z-index`、`position`等）导致子组件无法渲染成我们想要的样式。

如下图所示，父组件是这个红色框的范围，并且设置了`overflow:hidden`，这时候我们的Tooltip元素超出了红色框的范围就被截断了。
![image](https://segmentfault.com/img/bVbLaWt)

怎么用portals解决呢？

首先，修改html文件，给portals增加一个节点

```xml
<html>
    <body>
        <div id="react-root"></div>
        <div id="portal-root"></div>
    </body>
</html>
```

然后我们创建一个可复用的portal容器，这里使用了react hooks的语法，看不懂的先过去看下我另外一篇讲解react hooks的文章：[30分钟精通React今年最劲爆的新特性——React Hooks](https://segmentfault.com/a/1190000016950339)

```javascript
import { useEffect } from "react";
import { createPortal } from "react-dom";

const Portal = ({children}) => {
  const mount = document.getElementById("portal-root");
  const el = document.createElement("div");

  useEffect(() => {
    mount.appendChild(el);
    return () => mount.removeChild(el);
  }, [el, mount]);

  return createPortal(children, el)
};

export default Portal;
```

最后在父组件中使用我们的portal容器组件，并将Tooltip作为children传给portal容器组件

```javascript
const Parent = () => {
  const [coords, setCoords] = useState({});

  return <div style={{overflow: "hidden"}}>
      <Button>
        Hover me
      </Button>
      <Portal>
        <Tooltip coords={coords}>
          Awesome content that is never cut off by its parent container!
         </Tooltip>
      </Portal>
  </div>
}
```

这样就ok啦，虽然父组件仍然是`overflow: hidden`，但我们的Tooltip再也不会被截断了，因为它直接超脱了，它渲染到body节点下的`<div id="portal-root"></div>`里去了。

总结下适用的场景: Tooltip、Modal、Popup、Dropdown等等

### 8. Global Variables

哈哈，这也不失为一个可行的办法啊。当然你最好别用这种方法。

```scala
class ComponentA extends React.Component {
    handleClick = () => window.a = 'test'
    ...
}
class ComponentB extends React.Component {
    render() {
        return <div>{window.a}</div>
    }
}
```

### 9. Observer Pattern

观察者模式是软件设计模式里很常见的一种，它提供了一个订阅模型，假如一个对象订阅了某个事件，当那个事件发生的时候，这个对象将收到通知。

这种模式对于我们前端开发者来说是最不陌生的了，因为我们经常会给某些元素添加绑定事件，会写很多的event handlers，比如给某个元素添加一个点击的响应事件`elm.addEventListener('click', handleClickEvent)`，每当elm元素被点击时，这个点击事件会通知elm元素，然后我们的回调函数handleClickEvent会被执行。这个过程其实就是一个观察者模式的实现过程。

那这种模式跟我们讨论的react组件通信有什么关系呢？当我们有两个完全不相关的组件想要通信时，就可以利用这种模式，其中一个组件负责订阅某个消息，而另一个元素则负责发送这个消息。javascript提供了现成的api来发送自定义事件: `CustomEvent`，我们可以直接利用起来。

首先，在ComponentA中，我们负责接受这个自定义事件：

```javascript
class ComponentA extends React.Component {
    componentDidMount() {
        document.addEventListener('myEvent', this.handleEvent)
    }
    componentWillUnmount() {
        document.removeEventListener('myEvent', this.handleEvent)
    }
    
    handleEvent = (e) => {
        console.log(e.detail.log)  //i'm zach
    }
}
```

然后，ComponentB中，负责在合适的时候发送该自定义事件：

```scala
class ComponentB extends React.Component {
    sendEvent = () => {
        document.dispatchEvent(new CustomEvent('myEvent', {
          detail: {
             log: "i'm zach"
          }
        }))
    }
    
    render() {
        return <button onClick={this.sendEvent}>Send</button>
    }
}
```

这样我们就用观察者模式实现了两个不相关组件之间的通信。当然现在的实现有个小问题，我们的事件都绑定在了document上，这样实现起来方便，但很容易导致一些冲突的出现，所以我们可以小小的改良下，独立一个小模块EventBus专门这件事：

```reasonml
class EventBus {
    constructor() {
        this.bus = document.createElement('fakeelement');
    }

    addEventListener(event, callback) {
        this.bus.addEventListener(event, callback);
    }

    removeEventListener(event, callback) {
        this.bus.removeEventListener(event, callback);
    }

    dispatchEvent(event, detail = {}){
        this.bus.dispatchEvent(new CustomEvent(event, { detail }));
    }
}

export default new EventBus
```

然后我们就可以愉快的使用它了，这样就避免了把所有事件都绑定在document上的问题：

```scala
import EventBus from './EventBus'
class ComponentA extends React.Component {
    componentDidMount() {
        EventBus.addEventListener('myEvent', this.handleEvent)
    }
    componentWillUnmount() {
        EventBus.removeEventListener('myEvent', this.handleEvent)
    }
    
    handleEvent = (e) => {
        console.log(e.detail.log)  //i'm zach
    }
}
class ComponentB extends React.Component {
    sendEvent = () => {
        EventBus.dispatchEvent('myEvent', {log: "i'm zach"}))
    }
    
    render() {
        return <button onClick={this.sendEvent}>Send</button>
    }
}
```

最后我们也可以不依赖浏览器提供的api，手动实现一个观察者模式，或者叫pub/sub，或者就叫EventBus。

```javascript
function EventBus() {
  const subscriptions = {};
  this.subscribe = (eventType, callback) => {
    const id = Symbol('id');
    if (!subscriptions[eventType]) subscriptions[eventType] = {};
    subscriptions[eventType][id] = callback;
    return {
      unsubscribe: function unsubscribe() {
        delete subscriptions[eventType][id];
        if (Object.getOwnPropertySymbols(subscriptions[eventType]).length === 0) {
          delete subscriptions[eventType];
        }
      },
    };
  };

  this.publish = (eventType, arg) => {
    if (!subscriptions[eventType]) return;

    Object.getOwnPropertySymbols(subscriptions[eventType])
      .forEach(key => subscriptions[eventType][key](arg));
  };
}
export default EventBus;
```

### 10. Redux等

最后终于来到了大家喜闻乐见的Redux等状态管理库，当大家的项目比较大，前面讲的9种方法已经不能很好满足项目需求时，才考虑下使用redux这种状态管理库。这里就先不展开讲解redux了...否则我花这么大力气讲解前面9种方法的意义是什么？？？

### 总结

十种方法，每种方法都有对应的适合它的场景，大家在设计自己的组件前，一定要好好考虑清楚采用哪种方式来解决通信问题。

文初提到的那个小问题，最后我采用方案9，因为既然是小迭代项目，又是改别人的代码，当然最好避免对别人的代码进行太大幅度的改造。而pub/sub这种方式就挺小巧精致的，既不需要对别人的代码结构进行大改动，又可以满足产品需求。



## 类组件和函数组件有何不同？

### 类组件

类组件，顾名思义，也就是通过使用ES6类的编写形式去编写组件，该类必须继承React.Component

如果想要访问父组件传递过来的参数，可通过this.props的方式去访问

在组件中必须实现render方法，在return中返回React对象，如下：

复制

```js
class Welcome extends React.Component { 
  constructor(props) { 
    super(props) 
  } 
  render() { 
    return <h1>Hello, {this.props.name}</h1> 
  } 
} 
```

### 函数组件

函数组件，顾名思义，就是通过函数编写的形式去实现一个React组件，是React中定义组件最简单的方式

复制

```js
function Welcome(props) { 
  return <h1>Hello, {props.name}</h1>; 
} 
```

函数第一个参数为props用于接收父组件传递过来的参数

### 区别

针对两种React组件，其区别主要分成以下几大方向：

- 编写形式
- 状态管理
- 生命周期
- 调用方式
- 获取渲染的值

**编写形式**

两者最明显的区别在于编写形式的不同，同一种功能的实现可以分别对应类组件和函数组件的编写形式

函数组件：

```js
function Welcome(props) { 
  return <h1>Hello, {props.name}</h1>; 
} 
```

类组件：

```js
cass Welcome extends React.Component { 
  constructor(props) { 
    super(props) 
  } 
  render() { 
    return <h1>Hello, {this.props.name}</h1> 
  } 
} 
```

**状态管理**

在hooks出来之前，函数组件就是无状态组件，不能保管组件的状态，不像类组件中调用setState

如果想要管理state状态，可以使用useState，如下：

复制

```js
const FunctionalComponent = () => { 
    const [count, setCount] = React.useState(0); 
 
    return ( 
        <div> 
            <p>count: {count}</p> 
            <button onClick={() => setCount(count + 1)}>Click</button> 
        </div> 
    ); 
}; 
```

在使用hooks情况下，一般如果函数组件调用state，则需要创建一个类组件或者state提升到你的父组件中，然后通过props对象传递到子组件

**生命周期**

在函数组件中，并不存在生命周期，这是因为这些生命周期钩子都来自于继承的React.Component

所以，如果用到生命周期，就只能使用类组件

但是函数组件使用useEffect也能够完成替代生命周期的作用，这里给出一个简单的例子：

```js
const FunctionalComponent = () => { 
    useEffect(() => { 
        console.log("Hello"); 
    }, []); 
    return <h1>Hello, World</h1>; 
}; 
```

上述简单的例子对应类组件中的componentDidMount生命周期

如果在useEffect回调函数中return一个函数，则return函数会在组件卸载的时候执行，正如componentWillUnmount

```js
const FunctionalComponent = () => { 
 React.useEffect(() => { 
   return () => { 
     console.log("Bye"); 
   }; 
 }, []); 
 return <h1>Bye, World</h1>; 
}; 
```

**调用方式**

如果是一个函数组件，调用则是执行函数即可：

```js
// 你的代码  
function SayHi() {  
    return <p>Hello, React</p>  
}  
// React内部  
const result = SayHi(props) // » <p>Hello, React</p> 
```

如果是一个类组件，则需要将组件进行实例化，然后调用实例对象的render方法：

```js
// 你的代码  
class SayHi extends React.Component {  
    render() {  
        return <p>Hello, React</p>  
    }  
}  
// React内部  
const instance = new SayHi(props) // » SayHi {}  
const result = instance.render() // » <p>Hello, React</p> 
```

**获取渲染的值**

首先给出一个示例

函数组件对应如下：

```
function ProfilePage(props) { 
  const showMessage = () => { 
    alert('Followed ' + props.user); 
  } 
 
  const handleClick = () => { 
    setTimeout(showMessage, 3000); 
  } 
 
  return ( 
    <button onClick={handleClick}>Follow</button> 
  ) 
} 
```

类组件对应如下：

```
class ProfilePage extends React.Component { 
  showMessage() { 
    alert('Followed ' + this.props.user); 
  } 
 
  handleClick() { 
    setTimeout(this.showMessage.bind(this), 3000); 
  } 
 
  render() { 
    return <button onClick={this.handleClick.bind(this)}>Follow</button> 
  } 
} 
```

两者看起来实现功能是一致的，但是在类组件中，输出this.props.user，Props在 React中是不可变的所以它永远不会改变，但是 this 总是可变的，以便您可以在 render 和生命周期函数中读取新版本

因此，如果我们的组件在请求运行时更新。this.props 将会改变。showMessage方法从“最新”的 props 中读取 user

而函数组件，本身就不存在this，props并不发生改变，因此同样是点击，alert的内容仍旧是之前的内容

### 小结

两种组件都有各自的优缺点

函数组件语法更短、更简单，这使得它更容易开发、理解和测试

而类组件也会因大量使用 this而让人感到困惑



## state 和 props 的区别

### state

一个组件的显示形态可以由数据状态和外部参数所决定，而数据状态就是 `state`，一般在 `constructor` 中初始化

当需要修改里面的值的状态需要通过调用 `setState` 来改变，从而达到更新组件内部数据的作用，并且重新调用组件 `render` 方法，如下面的例子：

```jsx
class Button extends React.Component {
  constructor() {
    super();
    this.state = {
      count: 0,
    };
  }

  updateCount() {
    this.setState((prevState, props) => {
      return { count: prevState.count + 1 };
    });
  }

  render() {
    return (
      <button onClick={() => this.updateCount()}>
        Clicked {this.state.count} times
      </button>
    );
  }
}
```

`setState` 还可以接受第二个参数，它是一个函数，会在 `setState` 调用完成并且组件开始重新渲染时被调用，可以用来监听渲染是否完成

```js
this.setState(
  {
    name: "JS每日一题",
  },
  () => console.log("setState finished")
);
```

### props

`React` 的核心思想就是组件化思想，页面会被切分成一些独立的、可复用的组件

组件从概念上看就是一个函数，可以接受一个参数作为输入值，这个参数就是 `props`，所以可以把 `props` 理解为从外部传入组件内部的数据

`react` 具有单向数据流的特性，所以他的主要作用是从父组件向子组件中传递数据

`props` 除了可以传字符串，数字，还可以传递对象，数组甚至是回调函数，如下：

```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello {this.props.name}</h1>;
  }
}

const element = <Welcome name="Sara" onNameChanged={this.handleName} />;
```

上述 `name` 属性与 `onNameChanged` 方法都能在子组件的 `props` 变量中访问

在子组件中，`props` 在内部不可变的，如果想要改变它看，只能通过外部组件传入新的 `props` 来重新渲染子组件，否则子组件的 `props` 和展示形式不会改变

### 区别

相同点：

- 两者都是 JavaScript 对象
- 两者都是用于保存信息
- props 和 state 都能触发渲染更新

区别：

- props 是外部传递给组件的，而 state 是在组件内被组件自己管理的，一般在 constructor 中初始化
- props 在组件内部是不可修改的，但 state 在组件内部可以进行修改
- state 是多变的、可以修改

## React中的setState执行机制(为什么要用setState)

一个组件的显示形态可以由数据状态和外部参数所决定，而数据状态就是`state`

当需要修改里面的值的状态需要通过调用`setState`来改变，从而达到更新组件内部数据的作用

如下例子：

```jsx
import React, { Component } from 'react'

export default class App extends Component {
    constructor(props) {
        super(props);

        this.state = {
            message: "Hello World"
        }
    }

    render() {
        return (
            <div>
                <h2>{this.state.message}</h2>
                <button onClick={e => this.changeText()}>面试官系列</button>
            </div>
        )
    }

    changeText() {
        this.setState({
            message: "JS每日一题"
        })
    }
}
```

通过点击按钮触发`onclick`事件，执行`this.setState`方法更新`state`状态，然后重新执行`render`函数，从而导致页面的视图更新

如果直接修改`state`的状态，如下：

```jsx
changeText() {
    this.state.message = "你好啊,李银河";
}
```

我们会发现页面并不会有任何反应，但是`state`的状态是已经发生了改变

这是因为`React`并不像`vue2`中调用`Object.defineProperty`数据响应式或者`Vue3`调用`Proxy`监听数据的变化

**（那为什么必须通过setState呢？）**

**必须通过`setState`方法来告知`react`组件`state`已经发生了改变**

**关于`state`方法的定义是从`React.Component`中继承**，定义的源码如下：

```js
Component.prototype.setState = function(partialState, callback) {
  invariant(
    typeof partialState === 'object' ||
      typeof partialState === 'function' ||
      partialState == null,
    'setState(...): takes an object of state variables to update or a ' +
      'function which returns an object of state variables.',
  );
  this.updater.enqueueSetState(this, partialState, callback, 'setState');
};
```

从上面可以看到`setState`第一个参数可以是一个对象，或者是一个函数，而第二个参数是一个回调函数，用于可以实时的获取到更新之后的数据

### 更新类型

在使用`setState`更新数据的时候，`setState`的更新类型分成：

- 异步更新
- 同步更新

#### 异步更新

先举出一个例子：

```jsx
changeText() {
  this.setState({
    message: "你好啊"
  })
  console.log(this.state.message); // Hello World
}
```

从上面可以看到，最终打印结果为`Hello world`，并不能在执行完`setState`之后立马拿到最新的`state`的结果

如果想要立刻获取更新后的值，在第二个参数的回调中更新后会执行

```jsx
changeText() {
  this.setState({
    message: "你好啊"
  }, () => {
    console.log(this.state.message); // 你好啊
  });
}
```

#### 同步更新

同样先给出一个在`setTimeout`中更新的例子：

```jsx
changeText() {
  setTimeout(() => {
    this.setState({
      message: "你好啊
    });
    console.log(this.state.message); // 你好啊
  }, 0);
}
```

上面的例子中，可以看到更新是同步

再来举一个原生`DOM`事件的例子：

```jsx
componentDidMount() {
  const btnEl = document.getElementById("btn");
  btnEl.addEventListener('click', () => {
    this.setState({
      message: "你好啊,李银河"
    });
    console.log(this.state.message); // 你好啊,李银河
  })
}
```

### 小结

- **在组件生命周期或React合成事件中，setState是异步**
- **在setTimeout或者原生dom事件中，setState是同步**

### 批量更新

同样先给出一个例子：

```jsx
handleClick = () => {
    this.setState({
        count: this.state.count + 1,
    })
    console.log(this.state.count) // 1

    this.setState({
        count: this.state.count + 1,
    })
    console.log(this.state.count) // 1

    this.setState({
        count: this.state.count + 1,
    })
    console.log(this.state.count) // 1
}
```

点击按钮触发事件，打印的都是 1，页面显示 `count` 的值为 2

对同一个值进行多次 `setState`， `setState` 的批量更新策略会对其进行覆盖，取最后一次的执行结果

上述的例子，实际等价于如下：

```js
Object.assign(
  previousState,
  {index: state.count+ 1},
  {index: state.count+ 1},
  ...
)
```

由于后面的数据会覆盖前面的更改，所以最终只加了一次

如果是下一个`state`依赖前一个`state`的话，推荐给`setState`一个参数传入一个`function`，如下：

```jsx
onClick = () => {
    this.setState((prevState, props) => {
      return {count: prevState.count + 1};
    });
    this.setState((prevState, props) => {
      return {count: prevState.count + 1};
    });
}
```

而在`setTimeout`或者原生`dom`事件中，由于是同步的操作，所以**并不会进行覆盖现象**

### 调用 setState 之后发生了什么

在代码中调用setState函数之后，React 会将传入的参数对象与组件当前的状态合并，然后触发所谓的调和过程（Reconciliation）。经过调和过程，React 会以相对高效的方式根据新的状态构建 React 元素树并且着手重新渲染整个UI界面。在 React 得到元素树之后，React 会自动计算出新的树与老树的节点差异，然后根据差异对界面进行最小化重渲染。在差异计算算法中，React 能够相对精确地知道哪些位置发生了改变以及应该如何改变，这就保证了按需更新，而不是全部重新渲染。

##  React 性能优化的手段

`React`凭借`virtual DOM`和`diff`算法拥有高效的性能，但是某些情况下，性能明显可以进一步提高

在前面文章中，我们了解到类组件通过调用`setState`方法， 就会导致`render`，父组件一旦发生`render`渲染，子组件一定也会执行`render`渲染

当我们想要更新一个子组件的时候，如下图绿色部分：

![img](https://static.vue-js.com/b41f6f30-f270-11eb-ab90-d9ae814b240d.png)

理想状态只调用该路径下的组件`render`：

![img](https://static.vue-js.com/bc0f2460-f270-11eb-85f6-6fac77c0c9b3.png)

但是`react`的默认做法是调用所有组件的`render`，再对生成的虚拟`DOM`进行对比（黄色部分），如不变则不进行更新

![img](https://static.vue-js.com/c2f0c4f0-f270-11eb-85f6-6fac77c0c9b3.png)

从上图可见，黄色部分`diff`算法对比是明显的性能浪费的情况

### 避免不必要的`render`

从上面可以看到，父组件渲染导致子组件渲染，子组件并没有发生任何改变，这时候就可以从避免无谓的渲染，具体实现的方式有如下：

- shouldComponentUpdate
- PureComponent
- React.memo

#### shouldComponentUpdate

通过`shouldComponentUpdate`生命周期函数来比对 `state`和 `props`，确定是否要重新渲染

默认情况下返回`true`表示重新渲染，如果不希望组件重新渲染，返回 `false` 即可

#### PureComponent

跟`shouldComponentUpdate`原理基本一致，通过对 `props` 和 `state`的浅比较结果来实现 `shouldComponentUpdate`，源码大致如下：

```
if (this._compositeType === CompositeTypes.PureClass) {
    shouldUpdate = !shallowEqual(prevProps, nextProps) || ! shallowEqual(inst.state, nextState);
}
```

`shallowEqual`对应方法大致如下：

```
const hasOwnProperty = Object.prototype.hasOwnProperty;

/**
 * is 方法来判断两个值是否是相等的值，为何这么写可以移步 MDN 的文档
 * https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/is
 */
function is(x: mixed, y: mixed): boolean {
  if (x === y) {
    return x !== 0 || y !== 0 || 1 / x === 1 / y;
  } else {
    return x !== x && y !== y;
  }
}

function shallowEqual(objA: mixed, objB: mixed): boolean {
  // 首先对基本类型进行比较
  if (is(objA, objB)) {
    return true;
  }

  if (typeof objA !== 'object' || objA === null ||
      typeof objB !== 'object' || objB === null) {
    return false;
  }

  const keysA = Object.keys(objA);
  const keysB = Object.keys(objB);

  // 长度不相等直接返回false
  if (keysA.length !== keysB.length) {
    return false;
  }

  // key相等的情况下，再去循环比较
  for (let i = 0; i < keysA.length; i++) {
    if (
      !hasOwnProperty.call(objB, keysA[i]) ||
      !is(objA[keysA[i]], objB[keysA[i]])
    ) {
      return false;
    }
  }

  return true;
}
```

当对象包含复杂的数据结构时，对象深层的数据已改变却没有触发 `render`

注意：在`react`中，是不建议使用深层次结构的数据

#### React.memo

`React.memo`用来缓存组件的渲染，避免不必要的更新，其实也是一个高阶组件，与 `PureComponent` 十分类似。但不同的是， `React.memo` 只能用于函数组件

```
import { memo } from 'react';

function Button(props) {
  // Component code
}

export default memo(Button);
```

如果需要深层次比较，这时候可以给`memo`第二个参数传递比较函数

```
function arePropsEqual(prevProps, nextProps) {
  // your code
  return prevProps === nextProps;
}

export default memo(Button, arePropsEqual);
```

### 此外

 常见性能优化常见的手段有如下：

- 避免使用内联函数
- 使用 React Fragments 避免额外标记
- 使用 Immutable
- 懒加载组件
- 事件绑定方式
- 服务端渲染

#### 避免使用内联函数

如果我们使用内联函数，则每次调用`render`函数时都会创建一个新的函数实例，如下：

```jsx
import React from "react";

export default class InlineFunctionComponent extends React.Component {
  render() {
    return (
      <div>
        <h1>Welcome Guest</h1>
        <input type="button" onClick={(e) => { this.setState({inputValue: e.target.value}) }} value="Click For Inline Function" />
      </div>
    )
  }
}
```

我们应该在组件内部创建一个函数，并将事件绑定到该函数本身。这样每次调用 `render` 时就不会创建单独的函数实例，如下：

```jsx
import React from "react";

export default class InlineFunctionComponent extends React.Component {
  
  setNewStateData = (event) => {
    this.setState({
      inputValue: e.target.value
    })
  }
  
  render() {
    return (
      <div>
        <h1>Welcome Guest</h1>
        <input type="button" onClick={this.setNewStateData} value="Click For Inline Function" />
      </div>
    )
  }
}
```

#### 使用 React Fragments 避免额外标记

用户创建新组件时，每个组件应具有单个父标签。父级不能有两个标签，所以顶部要有一个公共标签，所以我们经常在组件顶部添加额外标签`div`

这个额外标签除了充当父标签之外，并没有其他作用，这时候则可以使用`fragement`

其不会向组件引入任何额外标记，但它可以作为父级标签的作用，如下所示：

```jsx
export default class NestedRoutingComponent extends React.Component {
    render() {
        return (
            <>
                <h1>This is the Header Component</h1>
                <h2>Welcome To Demo Page</h2>
            </>
        )
    }
}
```

### 事件绑定方式

在[事件绑定方式 (opens new window)](https://mp.weixin.qq.com/s/VfQ34ZEPXUXsimzMaJ_41A)中，我们了解到四种事假绑定的方式

从性能方面考虑，在`render`方法中使用`bind`和`render`方法中使用箭头函数这两种形式在每次组件`render`的时候都会生成新的方法实例，性能欠缺

而`constructor`中`bind`事件与定义阶段使用箭头函数绑定这两种形式只会生成一个方法实例，性能方面会有所改善

#### 使用 Immutable

在[理解Immutable中 (opens new window)](https://mp.weixin.qq.com/s/laYJ_KNa8M5JNBnIolMDAA)，我们了解到使用 `Immutable`可以给 `React` 应用带来性能的优化，主要体现在减少渲染的次数

在做`react`性能优化的时候，为了避免重复渲染，我们会在`shouldComponentUpdate()`中做对比，当返回`true`执行`render`方法

`Immutable`通过`is`方法则可以完成对比，而无需像一样通过深度比较的方式比较

#### 懒加载组件

从工程方面考虑，`webpack`存在代码拆分能力，可以为应用创建多个包，并在运行时动态加载，减少初始包的大小

而在`react`中使用到了`Suspense`和 `lazy`组件实现代码拆分功能，基本使用如下：

```jsx
const johanComponent = React.lazy(() => import(/* webpackChunkName: "johanComponent" */ './myAwesome.component'));
 
export const johanAsyncComponent = props => (
  <React.Suspense fallback={<Spinner />}>
    <johanComponent {...props} />
  </React.Suspense>
);
```

#### 服务端渲染

采用服务端渲染端方式，可以使用户更快的看到渲染完成的页面

服务端渲染，需要起一个`node`服务，可以使用`express`、`koa`等，调用`react`的`renderToString`方法，将根组件渲染成字符串，再输出到响应中

例如：

```js
import { renderToString } from "react-dom/server";
import MyPage from "./MyPage";
app.get("/", (req, res) => {
  res.write("<!DOCTYPE html><html><head><title>My Page</title></head><body>");
  res.write("<div id='content'>");  
  res.write(renderToString(<MyPage/>));
  res.write("</div></body></html>");
  res.end();
});
```

客户端使用render方法来生成HTML

```jsx
import ReactDOM from 'react-dom';
import MyPage from "./MyPage";
ReactDOM.render(<MyPage />, document.getElementById('app'));
```

#### 其他

除此之外，还存在的优化手段有组件拆分、合理使用`hooks`等性能优化手段..