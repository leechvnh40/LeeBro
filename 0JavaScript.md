# js基本数据类型哪几个？引用类型有哪些？

### 基本数据类型：（原始值）

null, undefined, number, boolean, string, symbol(ES6)，bigint(ES10)；存储在栈中 

### 引用数据类型：（对象值）

引用类型统称为 object 类型，细分的话有：Object 类型、Array 类型、Date 类型、RegExp 类型、Function 类型，特殊的基本包装类型(String、Number、Boolean)以及单体内置对象(Global、Math)；存储在堆中 

JS的字符串是不能更改的，最大长度2^53 - 1

所有 JavaScript 数字均为 64 位，JavaScript 不是类型语言。与许多其他编程语言不同，JavaScript 不定义不同类型的数字，比如整数、短、长、浮点等等。

在JavaScript中，**数字不分为整数类型和浮点型类型，所有的数字都是由 浮点型类型**。JavaScript 采用 IEEE754 标准定义的 64 位浮点格式表示数字，它能表示最大值（Number.MAX_VALUE）为 ±1.7976931348623157e+308，最小值（Number.MIN_VALUE）为 ±5e-324。

**如果某个数值超过可以表示的范围，那么这个数值会自动转换为Infinitely**

*![](pics/64位Number.png)*

### 基本数据类型和基本包装类型

基本包装类型：Boolean, Number, String

```javascript
let str = 'test'
let str2 = str.substring(2)
console.log(str2) //st
```

上面 str 变量存储的值是一个字符串，'test' 字符串是基本数据类型 String 类型的值。
按照常理来说，字符串不是引用类型，所以它不应该有自己的方法，第二行代码应该报错才对。

字符串 'test' 确实不应该有自己的方法，但是在执行第二行代码时，后台会自动进行下面的步骤：
1.自动创建 String 类型的一个实例（和基本类型的值不同，这个实例就是一个基本包装类型的对象）
2.调用实例（对象）上指定的方法
3.销毁这个实例

用代码的方式解释就是如下：

```javascript
//用 String 构造函数创建一个实例，这个实例是一个对象
let str = new String('test')
//对象中有内置方法供开发人员调用
let str2 = str.substring()
str = null
```

虽然基本类型的值没有方法可以调用，但是后台临时创建的构造函数实例（也就是对象）上有内置方法可以让我们对值进行操作，因此这样我们就可以对字符串、数值、布尔值这三种基本数据类型的数据进行更多操作，这也是基本包装类型的主要用处：便于操作基本类型值。

基本包装类型的对象和引用类型的对象最大的一个区别是，对象的生存期不同，导致的一个结果就是，基本包装类型无法自定义自己的方法。

对于引用类型的数据，在执行流离开当前作用域之前都会保存在内存中，而对于自动创建的基本包装类型的对象，只存在于一行代码的执行瞬间，执行完毕就会立即被销毁。

# JS中基本数据类型和引用类型在内存上有什么区别？

![](pics/内存图.png)

1. **基本类型** 基本数据类型变量**保存在栈中**,它们的值直接存储在变量访问的位置。这是因为这些原始类型占据的空间是固定的，所以可将它们存储在较小的内存区域 – 栈中。这样存储便于迅速查寻变量的值。
2. **引用类型** javascript 的引用数据类型是**同时保存在栈内存和堆内存中**的对象。与其它语言的不同是，你不可以直接访问堆内存空间中的位置和操作堆内存空间。只能操作对象在栈内存中的引用地址。准确地说，引用类型的存储需要内存的栈区和堆区（堆区是指内存里的堆内存）共同完成，栈区内存保存变量标识符和指向堆内存中该对象的指针，也可以说是该对象在堆内存的地址。由于引用值的大小会改变，所以不能把它放在栈中，否则会降低变量查寻的速度。

### **复制值**

**基本类型** 在将一个保存着原始值的变量复制给另一个变量时，会将原始值的副本赋值给新变量，此后这两个变量是完全独立的，它们只是拥有相同的 value 而已。

很显然，a 不全等 b

**引用类型** 在将一个保存着对象内存地址的变量复制给另一个变量时，会把这个内存地址赋值给新变量，也就是说这两个变量都指向了堆内存中的同一个对象，它们中任何一个作出的改变都会反映在另一个身上。（这里要理解的一点就是，复制对象时并不会在堆内存中新生成一个一模一样的对象，只是多了一个保存指向这个对象指针的变量罢了）。多了一个指针

结果然显然，a 全等 b，因为它们的指针指向同一个堆内存

### **传递值**

JS 高级程序设计—> 4.1.3 中提到: “**ECMAScript 中所有函数的参数都是按值传递的**” 



# 浅拷贝与深拷贝

### 赋值和深/浅拷贝的区别

- 当我们把一个对象赋值给一个新的变量时，**赋的其实是该对象的在栈中的地址，而不是堆中的数据**。也就是两个对象指向的是同一个存储空间，无论哪个对象发生改变，其实都是改变的存储空间的内容，因此，两个对象是联动的。

  JS 分为原始类型和引用类型，**对于原始类型的拷贝，并没有深浅拷贝的区别，我们讨论的深浅拷贝都只针对引用类型**。

  - 浅拷贝和深拷贝都复制了值和地址，都是为了解决引用类型赋值后互相影响的问题。
  - 但是浅拷贝**只进行一层复制**，深层次的引用类型还是共享内存地址，原对象和拷贝对象还是会互相影响。
  - 深拷贝就是**无限层级拷贝**，深拷贝后的原对象不会和拷贝对象互相影响。

  

### 浅拷贝的实现方式

#### 1.Object.assign()

Object.assign() 方法可以把任意多个的源对象自身的**可枚举属性拷贝给目标对象**，然后返回目标对象。

```js
let obj1 = { person: {name: "kobe", age: 41},sports:'basketball' };
let obj2 = Object.assign({}, obj1);
obj2.person.name = "wade";
obj2.sports = 'football'
console.log(obj1); // { person: { name: 'wade', age: 41 }, sports: 'basketball' }
```

#### 2.函数库lodash的_.clone方法

该函数库也有提供_.clone用来做 Shallow Copy,后面我们会再介绍利用这个库实现深拷贝。

```js
var _ = require('lodash');
var obj1 = {
    a: 1,
    b: { f: { g: 1 } },
    c: [1, 2, 3]
};
var obj2 = _.clone(obj1);
console.log(obj1.b.f === obj2.b.f);// true
```

#### 3.展开运算符...

展开运算符是一个 es6 / es2015特性，它提供了一种非常方便的方式来执行浅拷贝，这与 Object.assign ()的功能相同。

```js
let obj1 = { name: 'Kobe', address:{x:100,y:100}}
let obj2= {... obj1}
obj1.address.x = 200;
obj1.name = 'wade'
console.log('obj2',obj2) // obj2 { name: 'Kobe', address: { x: 200, y: 100 } }
```

#### 4.Array.prototype.concat()

```js
let arr = [1, 3, {
    username: 'kobe'
    }];
let arr2 = arr.concat();    
arr2[2].username = 'wade';
console.log(arr); //[ 1, 3, { username: 'wade' } ]
```

#### 5.Array.prototype.slice()

```js
let arr = [1, 3, {
    username: ' kobe'
    }];
let arr3 = arr.slice();
arr3[2].username = 'wade'
console.log(arr); // [ 1, 3, { username: 'wade' } ]
```

#### 6.使用for in循环

对象循环我们使用 **for in** 循环，但**for in** 循环会遍历到对象的继承属性，我们只需要它的私有属性，所以可以加一个判断方法：**hasOwnProperty** 保留对象私有属性。

```javascript
let obj2 = {};
for(let i in obj) {
    if(!obj.hasOwnProperty(i)) break; // 这里使用 continue 也可以
    obj2[i] = obj[i];
}
```

#### 7.数组静态方法 Array.from

```js
const arr = ['lin', 'is', 'handsome']
const newArr = Array.from(arr)

arr[2] = 'rich' // 改变原来的数组

console.log(newArr) // ['lin', 'is', 'handsome']

console.log(arr == newArr) // false 两者指向不同地址
```

### 深拷贝的实现方式

#### 1.JSON.parse(JSON.stringify())

```js
let arr = [1, 3, {
    username: ' kobe'
}];
let arr4 = JSON.parse(JSON.stringify(arr));
arr4[2].username = 'duncan'; 
console.log(arr, arr4)
```

*![](pics/深拷贝1.png)*

这也是利用JSON.stringify将对象转成JSON字符串，再用JSON.parse把字符串解析成对象，一去一来，新的对象产生了，而且对象会开辟新的栈，实现深拷贝。

**这种方法虽然可以实现数组或对象深拷贝,但不能处理函数和正则**，因为这两者基于JSON.stringify和JSON.parse处理后，得到的正则就不再是正则（变为空对象），得到的函数就不再是函数（变为null）了。

还有**循环引用**和**递归爆栈**的问题。

比如下面的例子：

```js
let arr = [1, 3, {
    username: ' kobe'
},function(){}];
let arr4 = JSON.parse(JSON.stringify(arr));
arr4[2].username = 'duncan'; 
console.log(arr, arr4)
```

*![](pics/深拷贝2.png)*



#### 2.函数库lodash的_.cloneDeep方法

该函数库也有提供_.cloneDeep用来做 Deep Copy

```
var _ = require('lodash');
var obj1 = {
    a: 1,
    b: { f: { g: 1 } },
    c: [1, 2, 3]
};
var obj2 = _.cloneDeep(obj1);
console.log(obj1.b.f === obj2.b.f);// false
```

#### 3.jQuery.extend()方法

jquery 有提供一個`$.extend`可以用来做 Deep Copy

```
$.extend(deepCopy, target, object1, [objectN])//第一个参数为true,就是深拷贝
复制代码
var $ = require('jquery');
var obj1 = {
    a: 1,
    b: { f: { g: 1 } },
    c: [1, 2, 3]
};
var obj2 = $.extend(true, {}, obj1);
console.log(obj1.b.f === obj2.b.f); // false
```

#### 4.手写递归方法

递归方法实现深度克隆原理：**遍历对象、数组直到里边都是基本数据类型，然后再去复制，就是深度拷贝**。

有种特殊情况需注意就是对象存在**循环引用**的情况，即对象的属性直接的引用了自身的情况，解决循环引用问题，我们可以额外开辟一个存储空间，来存储当前对象和拷贝对象的对应关系，当需要拷贝当前对象时，先去存储空间中找，有没有拷贝过这个对象，如果有的话直接返回，如果没有的话继续拷贝，这样就巧妙化解的循环引用的问题。关于这块如有疑惑，请仔细阅读`ConardLi大佬`[如何写出一个惊艳面试官的深拷贝?](https://link.juejin.cn?target=https%3A%2F%2Fsegmentfault.com%2Fa%2F1190000020255831)这篇文章。

```js
function deepClone(obj, hash = new WeakMap()) {
  if (obj === null) return obj; // 如果是null或者undefined我就不进行拷贝操作
  if (obj instanceof Date) return new Date(obj);
  if (obj instanceof RegExp) return new RegExp(obj);
  // 可能是对象或者普通的值  如果是函数的话是不需要深拷贝
  if (typeof obj !== "object") return obj;
  // 是对象的话就要进行深拷贝
  if (hash.get(obj)) return hash.get(obj);
  let cloneObj = new obj.constructor();
  // 找到的是所属类原型上的constructor,而原型上的 constructor指向的是当前类本身
  hash.set(obj, cloneObj);
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      // 实现一个递归拷贝
      cloneObj[key] = deepClone(obj[key], hash);
    }
  }
  return cloneObj;
}
let obj = { name: 1, address: { x: 100 } };
obj.o = obj; // 对象存在循环引用的情况
let d = deepClone(obj);
obj.address.x = 200;
console.log(d);
```

#### 5.structuredClone()

```js
const obj = {
  person: {
    name: 'lin'
  }
}

const newObj = structuredClone(obj) // 
obj.person.name = 'xxx' // 改变原来的对象

console.log('原来的对象', obj)
console.log('新的对象', newObj)

console.log('更深层的对象指向同一地址', obj.person == newObj.person)
```

结构化克隆解决了该`JSON.stringify()`技术的许多（尽管不是全部）缺点。结构化克隆可以处理循环依赖，支持许多内置数据类型，并且更健壮且速度更快。

但是，它仍然有一些限制：

- **原型**：如果你使用`structuredClone()`类实例，你将获得一个普通对象作为返回值，因为结构化克隆会丢弃对象的原型链。
- **函数**：如果你的对象包含函数，它们将被悄悄丢弃。
- **不可克隆**：有些值不是结构化可克隆的，尤其是`Error`、 `DOM 节点` 和 `Function`。尝试这样做将引发 `DataCloneError` 异常。
- **属性描述符**：setter和getter(以及类似元数据的功能)不会被复制。例如，如果使用属性描述符将对象标记为只读，则复制后的对象中是可读写(默认配置)。
- **RegExp**：RegExp对象的lastIndex字段不会保留。



# 为什么0.1+0.2 != 0.3 

在小数点运算时，JavaScript将隐式的采取**IEEE754二进制浮点运算**。而不是我们想象中的十进制运算。而十进制和二进制转换时，0.1 和 0.2 在转换成二进制后会无限循环，超过 64 位的位数会被截掉，出现了精度的损失。

解决方法1：

那么应该怎样来解决0.1+0.2等于0.3呢? 最好的方法是**设置一个误差范围值**，通常称为”机器精度“，而对于Javascript来说，这个值通常是2^-52,而在ES6中，已经为我们提供了这样一个属性：**Number.EPSILON**，而这个值正等于2^-52。这个值非常非常小，在底层计算机已经帮我们运算好，并且无限接近0，但不等于0。这个时候我们只要判断Math.abs((0.1+0.2)-0.3)小于`Number.EPSILON`，在这个误差的范围内就可以判定0.1+0.2===0.3为true。

```js
function numbersequal(a,b){ 
    return Math.abs(a-b)<Number.EPSILON;
} 
var a=0.1+0.2， b=0.3;
console.log(numbersequal(a,b)); //true
```

解决方法2：

把需要计算的数字乘以10的n次方，让数值都变为整数，计算完后再除以10的n次方，这样就不会出现浮点数精度丢失问题

```markdown
> (0.1 * 10 + 0.2 * 10) / 10  // 0.3
```

解决方法3：

引入第三方库Math.js、decimal.js、big.js



# null和undefined的区别

**null**表示“没有对象”，null值是一个空对象指针，既该处不应该有值。

  （1）作为函数的参数，表示该函数的参数不是对象。

  （2）作为对象原型链的终点。

**undefind**表示“缺少值”，就是此处应该有一个值，但是没有定义。

   （1）变量被声明了，但是没有赋值时，就等undefined。

   （2）调用函数时，应该提供方的参数没有提供，该参数等于undefined。

   （3）对象没有赋值的属性，该属性的值为undefined。

   （4）函数没有返回值时，默认返回undefined。



# *隐式转换（valueOf和toString）*

### *三种隐式转换类型*

*涉及隐式转换最多的两个运算符 + 和 ==。*

**+运算符即可数字相加，也可以字符串相加。所以转换时很麻烦。== 不同于===，故也存在隐式转换。- * / 这些运算符只会针对number类型，故转换的结果只能是转换成number类型。**

*隐式转换中主要涉及到三种转换：*

*1、将值转为原始值，ToPrimitive()。*

*2、将值转为数字，ToNumber()。*

*3、将值转为字符串，ToString()。*

### *通过ToPrimitive将值转换为原始值*

*js引擎内部的抽象操作ToPrimitive有着这样的签名：*

*ToPrimitive(input, PreferredType?)*

*input是要转换的值，PreferredType是可选参数，可以是Number或String类型。他只是一个转换标志，转化后的结果并不一定是这个参数所值的类型，但是转换结果一定是一个原始值（或者报错）。*

***如果PreferredType被标记为Number，则会进行下面的操作流程来转换输入的值。***

```js
1、如果输入的值已经是一个原始值，则直接返回它
2、否则，如果输入的值是一个对象，则调用该对象的valueOf()方法，
   如果valueOf()方法的返回值是一个原始值，则返回这个原始值。
3、否则，调用这个对象的toString()方法，如果toString()方法返回的是一个原始值，则返回这个原始值。
4、否则，抛出TypeError异常。
```

***如果PreferredType被标记为String，则会进行下面的操作流程来转换输入的值。***

```js
1、如果输入的值已经是一个原始值，则直接返回它
2、否则，调用这个对象的toString()方法，如果toString()方法返回的是一个原始值，则返回这个原始值。
3、否则，如果输入的值是一个对象，则调用该对象的valueOf()方法，
   如果valueOf()方法的返回值是一个原始值，则返回这个原始值。
4、否则，抛出TypeError异常。
```

*既然PreferredType是可选参数，那么如果没有这个参数时，怎么转换呢？PreferredType的值会按照这样的规则来自动设置：*

```js
1、该对象为Date类型，则PreferredType被设置为String
2、否则，PreferredType被设置为Number
```

### *valueOf方法和toString方法解析*

*上面主要提及到了valueOf方法和toString方法，那这两个方法在对象里是否一定存在呢？答案是肯定的。在控制台输出Object.prototype，你会发现其中就有valueOf和toString方法，而Object.prototype是所有对象原型链顶层原型，所有对象都会继承该原型的方法，故任何对象都会有valueOf和toString方法。*

*先看看对象的valueOf函数，其转换结果是什么？对于js的常见内置对象：`Date, Array, Math, Number, Boolean, String, Array, RegExp, Function`。*

*1、Number、Boolean、String这三种构造函数生成的基础值的对象形式，通过valueOf转换后会变成相应的原始值。如：*

```js
var num = new Number('123');
num.valueOf(); // 123

var str = new String('12df');
str.valueOf(); // '12df'

var bool = new Boolean('fd');
bool.valueOf(); // true
```

*2、Date这种特殊的对象，其原型Date.prototype上内置的valueOf函数将日期转换为日期的毫秒的形式的数值。*

```js
var a = new Date();
a.valueOf(); // 1515143895500
```

*3、除此之外返回的都为this，即对象本身*

```js
var a = new Array();
a.valueOf() === a; // true

var b = new Object({});
b.valueOf() === b; // true
```

*再来看看toString函数，其转换结果是什么？对于js的常见内置对象：`Date, Array, Math, Number, Boolean, String, Array, RegExp, Function`。*

*1、Number、Boolean、String、Array、Date、RegExp、Function这几种构造函数生成的对象，通过toString转换后会变成相应的字符串的形式，因为这些构造函数上封装了自己的toString方法。如：*

```js
Number.prototype.hasOwnProperty('toString'); // true
Boolean.prototype.hasOwnProperty('toString'); // true
String.prototype.hasOwnProperty('toString'); // true
Array.prototype.hasOwnProperty('toString'); // true
Date.prototype.hasOwnProperty('toString'); // true
RegExp.prototype.hasOwnProperty('toString'); // true
Function.prototype.hasOwnProperty('toString'); // true

var num = new Number('123sd');
num.toString(); // 'NaN'

var str = new String('12df');
str.toString(); // '12df'

var bool = new Boolean('fd');
bool.toString(); // 'true'

var arr = new Array(1,2);
arr.toString(); // '1,2'

var d = new Date();
d.toString(); // "Wed Oct 11 2017 08:00:00 GMT+0800 (中国标准时间)"

var func = function () {}
func.toString(); // "function () {}"
```

*除这些对象及其实例化对象之外，其他对象返回的都是该对象的类型，(有问题欢迎告知)，都是继承的Object.prototype.toString方法。*

```js
var obj = new Object({});
obj.toString(); // "[object Object]"
Math.toString(); // "[object Math]"
```

*从上面valueOf和toString两个函数对对象的转换可以看出为什么对于ToPrimitive(input, PreferredType?)，PreferredType没有设定的时候，除了Date类型，PreferredType被设置为String，其它的会设置成Number。*

*因为valueOf函数会将Number、String、Boolean基础类型的对象类型值转换成 **基础类型**，Date类型转换为毫秒数，其它的返回对象本身，而toString方法会将所有对象转换为**字符串**。显然对于大部分对象转换，valueOf转换更合理些，因为并没有规定转换类型，应该尽可能保持原有值，而不应该想toString方法一样，一股脑将其转换为字符串。*

*所以对于没有指定PreferredType类型时，先进行valueOf方法转换更好，故将PreferredType设置为Number类型。*

*而对于Date类型，其进行valueOf转换为毫秒数的number类型。在进行隐式转换时，没有指定将其转换为number类型时，将其转换为那么大的number类型的值显然没有多大意义。（不管是在+运算符还是==运算符）还不如转换为字符串格式的日期，所以默认Date类型会优先进行toString转换。故有以上的规则：*

*PreferredType没有设置时，Date类型的对象，PreferredType默认设置为String，其他类型对象PreferredType默认设置为Number。*

### *通过ToNumber将值转换为数字*

*根据参数类型进行下面转换：*

| *参数*      | *结果*                                                       |
| ----------- | ------------------------------------------------------------ |
| *undefined* | *NaN*                                                        |
| *null*      | *+0*                                                         |
| *布尔值*    | *true转换1，false转换为+0*                                   |
| *数字*      | *无须转换*                                                   |
| *字符串*    | *有字符串解析为数字，例如：‘324’转换为324，‘qwer’转换为NaN*  |
| *对象(obj)* | *先进行 ToPrimitive(obj, Number)转换得到原始值，在进行ToNumber转换为数字* |

### *通过ToString将值转换为字符串*

*根据参数类型进行下面转换：*

| *参数*      | *结果*                                                       |
| ----------- | ------------------------------------------------------------ |
| *undefined* | *'undefined'*                                                |
| *null*      | *'null'*                                                     |
| *布尔值*    | *转换为'true' 或 'false'*                                    |
| *数字*      | *数字转换字符串，比如：1.765转为'1.765'*                     |
| *字符串*    | *无须转换*                                                   |
| *对象(obj)* | *先进行 ToPrimitive(obj, String)转换得到原始值，在进行ToString转换为字符串* |

*讲了这么多，是不是还不是很清晰，先来看看一个例子：*

```
({} + {}) = ?
两个对象的值进行+运算符，肯定要先进行隐式转换为原始类型才能进行计算。
1、进行ToPrimitive转换，由于没有指定PreferredType类型，{}会使默认值为Number，进行ToPrimitive(input, Number)运算。
2、所以会执行valueOf方法，({}).valueOf(),返回的还是{}对象，不是原始值。
3、继续执行toString方法，({}).toString(),返回"[object Object]"，是原始值。
故得到最终的结果，"[object Object]" + "[object Object]" = "[object Object][object Object]"
```

*再来一个指定类型的例子：*

```
2 * {} = ?
1、首先*运算符只能对number类型进行运算，故第一步就是对{}进行ToNumber类型转换。
2、由于{}是对象类型，故先进行原始类型转换，ToPrimitive(input, Number)运算。
3、所以会执行valueOf方法，({}).valueOf(),返回的还是{}对象，不是原始值。
4、继续执行toString方法，({}).toString(),返回"[object Object]"，是原始值。
5、转换为原始值后再进行ToNumber运算，"[object Object]"就转换为NaN。
故最终的结果为 2 * NaN = NaN
```

### == 运算符隐式转换

**== 运算符的规则规律性不是那么强，按照下面流程来执行,es5文档**

```
比较运算 x==y, 其中 x 和 y 是值，返回 true 或者 false。这样的比较按如下方式进行：

1、若 Type(x) 与 Type(y) 相同， 则

    1* 若 Type(x) 为 Undefined， 返回 true。
    2* 若 Type(x) 为 Null， 返回 true。
    3* 若 Type(x) 为 Number， 则
  
        (1)、若 x 为 NaN， 返回 false。
        (2)、若 y 为 NaN， 返回 false。
        (3)、若 x 与 y 为相等数值， 返回 true。
        (4)、若 x 为 +0 且 y 为 −0， 返回 true。
        (5)、若 x 为 −0 且 y 为 +0， 返回 true。
        (6)、返回 false。
        
    4* 若 Type(x) 为 String, 则当 x 和 y 为完全相同的字符序列（长度相等且相同字符在相同位置）时返回 true。 否则， 返回 false。
    5* 若 Type(x) 为 Boolean, 当 x 和 y 为同为 true 或者同为 false 时返回 true。 否则， 返回 false。
    6*  当 x 和 y 为引用同一对象时返回 true。否则，返回 false。
  
2、若 x 为 null 且 y 为 undefined， 返回 true。
3、若 x 为 undefined 且 y 为 null， 返回 true。
4、若 Type(x) 为 Number 且 Type(y) 为 String，返回比较 x == ToNumber(y) 的结果。
5、若 Type(x) 为 String 且 Type(y) 为 Number，返回比较 ToNumber(x) == y 的结果。
6、若 Type(x) 为 Boolean， 返回比较 ToNumber(x) == y 的结果。
7、若 Type(y) 为 Boolean， 返回比较 x == ToNumber(y) 的结果。
8、若 Type(x) 为 String 或 Number，且 Type(y) 为 Object，返回比较 x == ToPrimitive(y) 的结果。
9、若 Type(x) 为 Object 且 Type(y) 为 String 或 Number， 返回比较 ToPrimitive(x) == y 的结果。
10、返回 false。
```

### 什么情况下会发生隐式类型转换

#### 1. `+`号

- 1.1 算数运算符 (除string类型外的原始数据类型进行加法运算时)
  - 非数字类型，会转为数字类型
- 1.2 字符串连接符（string类型以及引用数据类型时）
  - 非`string`类型会转为`string`类型

#### 2. 除加号以外的算数运算符(- * /)

- 非数字类型会转为数字类型
  - 如果是原始数据类型会调用`Number()`方法进行转换
  - 如果是引用数据类型会调用自身`valueOf`方法进行转换，如果转换后不是原始值，则会调用`toString`方法进行转换，如果转换后不是数字，则会调用`Number()`进行转换，如果转换后不是数字则会返回`NaN`。

#### 3.逻辑运算符（&& || ！）

- 非布尔类型会转为布尔类型
  - `a&&b` 如果`a`为`true`，则会返回`b`；如果`a`为`false`，则会返回`a`
  - `a||b` 如果`a`为`true`，则会返回`a`；如果`a`为`false`则会返回`b`
  - `!a` 如果`a`为布尔值，则直接取反，如果a为非布尔值，则会转换为布尔值然后取反
  - 引用数据类型转换为布尔值后总会是`true`

#### 4.条件判断if()等

- 将括号内的值转为布尔类型

#### 5.比较运算符==、>、<等

- `null`与`undefined`进行`==`比较时不会进行转换，总返回`true`。
- 引用数据类型，会先转换为`string`(先调用`valueOf`，后调用`toString`)，再转换为`number`
- 如果`==`左右都是引用数据类型，会进行地址比较

# == 和 === 和 Object.is() 的区别

```
==用于一般比较，===用于严格比较;
==在比较的时候可以转换数据类型，===严格比较，只要类型不匹配就返回flase。
```

==如果是基本数据类型类型之间的比较，会转换成数值类型进行比较，如果是存在引用类型，会先将引用类型转换原始类型，然后再进行比较。这里就涉及到另外两个函数`toString` 和`valueOf` 。

=== 比较运算符相比较`==` 运算符,不会**做隐式转换**，也就是不会自动调用`toString` `valueof` 方法。他会首先判断类型是否一致，然后再做比较

## ==的比较

![img](https://img.jbzj.com/file_images/article/201607/201607200933381.png)

```js
[] == [] // false
{} == {} // false
// 引用类型比较地址

[]==![] // true
// !操作即Boolean()后再取非，Boolean()将任何对象都转换为true
// []==false -> []==0 -> [].toString()==0 -> ''==0 -> 0==0

{}==!{} // false
// {}==false -> {}==0 -> {}.toString()==0 -> '[object Object]'==0 

[undefined] == false //true
```

`Object.is()`是ES6新增的方法，用来比较两个值是否是严格相等，与`===`大致一样，下面列出的是和`===`不一样的几个区别。

- +0不等于-0
- 0不等于-0
- NaN等于NaN

```javascript
+0 === -0 // true
0 === -0 // true
NaN === NaN // false

Object.is(0, -0) // false
Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```

# 字符串转数字

## parseInt

**定义和用法**

parseInt() 函数可解析一个字符串，并返回一个整数。

**语法**

parseInt(string, radix)

## parseFloat

**定义和用法**

parseFloat() 函数可解析一个字符串，并返回一个浮点数。

该函数指定字符串中的首个字符是否是数字。如果是，则对字符串进行解析，直到到达数字的末端为止，然后以数字返回该数字，而不是作为字符串。

**语法**

parseFloat(string)

## 按位非

可以把字符串转换成整数，但他不是浮点数。如果是一个字符串转换，它将返回0；

## Number

**定义和用法**

`Number()` 函数把对象的值转换为数字。

如果对象的值无法转换为数字，那么 Number() 函数返回 NaN。

**语法**

Number(object)

`Number`：几乎不用它。

## 一元运算符(重点)

加法运算符会触发三种类型转换：

- 转换为原始值
- 转换为数字
- 转换为字符串

> 注意：一元运算符与其它的解析方式不同，如果是一个 `NaN` 值，那么返回的也是 `NaN` 。基本使用`+`操作符，因为这个方式不容易混淆。虽然` -0` 的用法也很好，但它并没有很好的表达转换为数字的本意。 `+`是将字符串转换为数字的最快方法。

## 总结

负十六进制数字符串转换为数字时。最好使用带基数的 `parseInt` 解析为数字。明确知到进制数的也是通过带基数的 `parseInt` 解析为数字。其他的建议使用一元运算符`+`来将字符串转数字，也是最快的。

![](pics/字符串转数字.png)

# typeof,instanceof,constructor,valueOf 四种类型检测的区别toString

### **typeof**

- 1、定义：用来检测数据类型的运算符

​		原理：**不同的对象在底层都表示为二进制，在Javascript中二进制前（低）三位存储其类型信息**。000: 对象010: 浮点数100：字符串110：布尔1：整数

- 2、语法：`tyepof [value]`
- 3、返回值：
  - `typeof` 检测的结果首先是一个字符串；
  - 字符串中包含了对应的数据类型（例如：  `“number”`、`“string”`、`“boolean”`、`“undefined”`、`“object”`、`“function”`、`“symbol”`、`“bigint”`）

- 4、优点：使用起来简单，**基本数据类型值基本上都可以有效检测，引用数据类型值也可以检测出来**
- 5、**局限性（特殊值）**
  - 1)、`NaN` / `Infinity` 都是数字类型的，检测结果都是`“number”`;
  - 2)、typeof null的结果是“object”
    - （这是浏览器的`BUG`：所有的值在计算中都以二进制编码储存，浏览器中把前三位`000`的当作对象，而`null`的二进制前三位是`000`，所以被识别为对象，但是他不是对象，他是空对象指针，是基本类型值）
  - 3)、`typeof` `普通对象/数组对象/正则对象...`， 结果都是`“object”`，这样就无法基于`typeof` 区分是`普通对象`还是`数组对象``...`等了，**但是函数对象结果是function**
- 6、应用场景

> - 已知有一个变量x，但是我们无法确认其数据类型，我们需要有一个判断操作：当x的类型是对象的时候（什么对象都可以），则处理对应的事情

```
if (typeof x == "object") {         
    //=>null检测结果也会是"object"，所以结果是"object"不一定是对象，还可能是null呢
    ...
}
```

可以用👇的条件进行判断

```
if (x != null && typeof x == "object") {
    // ...
}
```

- 7、练习题

> console.log(typeof []); //=>"object"
>
> console.log(typeof typeof typeof []); //=>"string"



`typeof`返回的结果**永远是一个字符串**（字符串中包含了对应的类型），所以连续出现`两个及两个以上typeof检测`的时候，最后结果都是` "string"`

### instanceof

- 1、定义：用来检测某个**实例是否属于这个类**
  - 当前**类的原型只要出现在了实例的原型链上**就返回`true`
- 2、语法：`实例 instanceof 类`
- 3、属于返回`TRUE`，不属于返回`FALSE`
- 4、优点：
  - 基于这种方式，可以弥补 `typeof` 无法细分对象类型的缺点

```
let arr = [10, 20];

console.log(typeof arr); //=>"object"
console.log(arr instanceof Array); //=>true
console.log(arr instanceof RegExp); //=>false
console.log(arr instanceof Object); //=>true  不管是数组对象还是正则对象，都是Object的实例，检测结果都是TRUE，所以无法基于这个结果判断是否为普通对象 
```

- 5、局限性:
  - 1)、要求检测的实例必须**是对象数据类型**的
  - 2)、基本数据类型的实例是无法基于它检测出来的
    - 字面量方式创建的不能检测
    - 构造函数创建的就可以检测
  - 3)、不管是数组对象还是正则对象，**都是 Object 的实例**，检测结果都是 TRUE ，所以无法基于这个结果判断是否为普通对象

```
// instanceof检测的实例必须都是引用数据类型的，它对基本数据类型值操作无效
console.log(10 instanceof Number); //=>false
console.log(new Number(10) instanceof Number); //=>true

// instanceof检测机制：验证当前类的原型prototype是否会出现在实例的原型链__proto__上，只要在它的原型链上，则结果都为TRUE
function Fn() {}
Fn.prototype = Object.create(Array.prototype);
let f = new Fn;
console.log(f instanceof Array); //=>true f其实不是数组，因为它连数组的基本结构都是不具备的 
```

### constructor

- 1、定义：判断当前的实例的`constructor`的属性值是不是预估的类（利用他的实例数据类型检测）
- 2、语法：`实例.constructor === 类`
- 3、返回值：属于返回`TRUE`，不属于返回`FALSE`
- 4、优点：
  - `实例.constructor` 一般都等于 `类.prototype.constructor` 也就是当前类本身（前提是你的 `constructor` 并没有被破坏）
  - 能检测基本数据类型
- 5、局限性：
  - 1)、不能够给当前类的原型进行重定向，会造成检测的结果不准确(`Object`)
  - 2)、不能够给当前实例增加私有属性`constructor`，也会造成检测的结果不准确(`Object`)
  - 3)、非常**容易被修改**，因为`JS`中的`constructor`是不被保护的（用户可以自己随便改），这样基于`constructor`检测的值**存在不确定性**（但是项目中，基本没有人会改内置类的`constructor`）

```
let arr = [],
    obj = {},
    num = 10;
console.log(arr.constructor === Array); //=>true
console.log(arr.constructor === Object); //=>false
console.log(obj.constructor === Object); //=>true
console.log(num.constructor === Number); //=>true 
```

还可以使用 xx.constructor.name 构造函数来判断。

```
// constructor 构造来判断
console.log(fn.constructor.name) // Function
console.log(date.constructor.name)// Date
console.log(arr.constructor.name) // Array
console.log(reg.constructor.name) // RegExp
```



### toString

> 这个方法在Object的原型上

- 1、定义：找到`Object.prototype`上的`toString`方法，让`toString`方法执行，并且基于`call`让方法中的`this`指向检测的数据值，这样就可以实现数据类型检测了

- 2、原理：

  - 1.每一种数据类型的构造函数的原型上都有`toString`方法；

  - 2.除了`Object.prototype`上的`toString`是用来**返回当前实例所属类的信息**（检测数据类型的，是一个字符串），**其余的都是转换为字符串的**（因为被重写了）

  - 3.`对象实例.toString()` ：`toString`方法中的`THIS`是对象实例，也就是检测它的数据类型，也就是`THIS`是谁，就是检测谁的数据类型

  - 4.`Object.prototype.toString.call([value])` 所以我们是把`toString`执行，基于`call`改变`this`为要检测的数据值，

  - 之所以必须改变this指向是因为，`toString`内部是获取`this`指向那个对象的`[[Class]]`属性值的，如果不改变`this`指向为我们的目标变量，`this`将永远指向调用`toString`的`prototype`。
     另外也是因为，很多对象继承的`toString`方法重写了，为了能调用正确的`toString`，才间接的使用`call/apply`方法。

  ************

- 3、使用方法:

  - Object.prototype.toString.call(被检测的实例)
  - ({}).toString.call(被检测的实例)

```
Object.prototype.toString.call(10)
({}).toString.call(10)
({}).toString===Object.prototype.toString
```

- 4、返回值：
  - **是一个字符串**“[Object 当前被检测实例所属的类]”

```
let class2type = {};
let toString = class2type.toString; //=>Object.prototype.toString

console.log(toString.call(10)); //=>"[object Number]"
console.log(toString.call(NaN)); //=>"[object Number]"
console.log(toString.call("xxx")); //=>"[object String]"
console.log(toString.call(true)); //=>"[object Boolean]"
console.log(toString.call(null)); //=>"[object Null]"
console.log(toString.call(undefined)); //=>"[object Undefined]"
console.log(toString.call(Symbol())); //=>"[object Symbol]"
console.log(toString.call(BigInt(10))); //=>"[object BigInt]"
console.log(toString.call({xxx:'xxx'})); //=>"[object Object]"
console.log(toString.call([10,20])); //=>"[object Array]"
console.log(toString.call(/^\d+$/)); //=>"[object RegExp]"
console.log(toString.call(function(){})); //=>"[object Function]" 
```

- 5、优点：
  - 专门用来检测数据类型的方法，基本上不存在局限性的数据类型检测方式
  - 基于他可以有效的检测任何数据类型的值
- 6、局限性：
  - 1)、只能检测内置类，**不能检测自定义类**
  - 2)、只要是自定义类返回的都是‘[Object Object]’

**注意：此方法是基于JS本身专门进行数据检测的，所以是目前检测数据类型比较好的方法**

### valueOf

1.如果存在任意原始值，它就默认将对象转换为表示它的原始值

2.对象是复合值，而大多数对象无法真正表示为一个原始值，因此默认的valueOf()方法简单地返回对象本身，而不是返回一个原始值



# 如何判断数组类型的数据

因为数组属于引用类型，所以常规的typeof方法并不能判断数组类型。

### **instanceof**

instanceof用于检测**构造函数**的prototype属性是否出现在某个对象的原型链上

```js
var arr = [1,2,3];

console.log(arr instanceof Array);
```

### constructor

判断对象的**构造函数**是否是数组。

**`constructor`** 是一种用于创建和初始化`class`创建的对象的特殊方法。如果我们判断该对象的构造函数就是`Array`，那就是说该对象可以通过数组的构造函数（`Array`）创建得到，也就说明它是数组了。

```js
var arr = [1,2,3];

console.log(arr.constructor == Array);
```

### Object.prototype.toString.call()

`toString() ` 方法返回一个表示该对象的字符串。

每个对象都有一个 `toString()` 方法，当该对象被表示为一个文本值时，或者一个对象以预期的字符串方式引用时自动调用。默认情况下，`toString()` 方法被每个 `Object` 对象继承。如果此方法在自定义对象中未被覆盖，`toString()` 返回 "[object type]"，其中 `type` 是对象的类

```js
var arr = [1,2,3];
console.log(Object.prototype.toString.call(arr);
console.log(Object.prototype.toString.call(arr) === '[object Array]');
```

*![](pics/Object.prototype.toString.call().png)*

### Array.prototype.isPrototypeOf()

`isPrototypeOf() ` 方法用于测试一个对象是否存在于另一个对象的原型链上。

> `isPrototypeOf()` 与 [`instanceof`](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FJavaScript%2FReference%2FOperators%2Finstanceof) 运算符不同。在表达式 "`object instanceof AFunction`"中，`object` 的原型链是针对 `AFunction.prototype` 进行检查的，而不是针对 `AFunction` 本身

### Array.isArray

在ES6中提供了Array.isArray()方法来检测数组类型。

```js
var arr = [1,2,3];
console.log(Array.isArray(arr));
```



#  数组常用方法

#### **往数组里增加项**

`array.push(value, ...)` 给数组最后添加一个或多个元素

- 参数：`value`，要添加到 `array` 尾部的值，可以是一个或多个
- 返回值：把指定的值添加到数组后数组的新长度

`array.unshift(value, ...)` 给数组最前面添加一个或多个元素

- 参数：`value`，要插入数组头部的一个或多个值
- 返回值：把指定的值添加到数组后数组的新长度

#### 从数组里删除项

`array.pop()` 删除并返回数组的最后一个元素

- 参数：无
- 返回值：`array` 的最后一个元素
- 方法 `pop()` 将删除 `array` 的最后一个元素，把数组长度减 `1`，并且返回它删除的元素的值。
- 如果数组已经为空，则 `pop()` 不改变数组，返回 `undefined`

`array.shift()` 将数组中的第一个元素移出数组

- 参数：无
- 返回值：数组原来的第一个元素
- 方法 `shift()` 将把 `array` 的第一个元素移出数组，返回那个元素的值，并且将余下的所有元素前移一位，以填补数组头部的空缺。
- 如果数组是空的，`shift()` 将不进行任何操作，返回 `undefined`
- 方法 `shift()` 和 方法 `pop()` 相似，只不过它在数组头部操作，而不是在尾部操作。该方法常常和 `unshift()` 一起使用

#### **更改数组项**

`array.reverse()` 颠倒数组中元素的顺序

- `array` 对象的方法 `reverse()` 将颠倒数组中元素的位置。
- 它在原数组上实现这一操作，即重排指定的 `array` 元素，但并不创建新数组。
- 如果对 `array` 有多个引用，那么通过所有引用都可以看到数组元素的新顺序。

`array.sort(function)` 对数组元素进行排序

- 参数：`function` 可以控制数字排序
- 返回值：对数组的引用。
- 注意：数组在原数组上进行排序，不制作副本
- 如果调用方法 `sort()` 时没有使用参数，将按字母顺序（按照字符编码的顺序）对数组中的元素进行排序

#### **查询数组项**

- `indexOf()`
- `lastIndexOf()`
- 这两个属性可以参照字符串的定义和介绍
- includes()
- find()
- findIndex()

#### **迭代器方法**

- keys()
- values()
- entries()

#### **遍历数组**

```
array.forEach(item[, thisObject])
```

- 参数：
  - `item`：函数参数数组中每个元素
  - `thisObject`：对象作为该执行回调时使用
- 返回值：返回创建数组

```
array.map(item[, thisObject])
```

- 方法返回一个由原数组中的每个元素调用一个制定方法后的返回值组成的新数组

#### **截取数组值**

`array.slice(start, end)` 返回数组的一部分

- 参数：
  - `start`：数组片段开始处的数组下标。如果是负数，它声明从数组尾部开始算起的位置
  - `end`：数组片段结束处的后一个元素的数组下标
  - 如果没有指定 `end`，则默认包含从 `start` 开始到数组结束的所有元素。
- 返回值：一个新数组，包含从 `start` 到 `end` (不包括该元素)指定的 `array` 元素
- **注意**：不改变原数组！如果想删除数组中的一段元素，应该使用 `array.splice`

`array.splice(start, length, value, ...)` **插入**、删除或替换数组中的元素

- 参数：
  - `start`：开始插入或删除的数组元素的下标
  - `length`：从 `start` 开始，包括 `start` 所指的元素在内要删除的元素个数。如果没有指定 `length`，`splice()` 将删除从 `start` 开始到原数组结尾的所有元素。
  - `value`：要插入数组的零个或多个值，从 `start` 所指的下标处开始插入。
- 返回值：如果从 `array` 中删除了元素，返回的是被删除元素的数组。

#### 其他数组方法

`array.concat(value, ...)` 拼接数组

- 参数：`value，...，` 要增加到 `array` 中的值，可以是任意多个
- 返回值：一个新数组
- 方法 `concat()` 将创建并返回一个新数组，这个数组是将所有参数添加到 `array` 中生成的。
- 不修改原数组 `array`
- 如果要进行 `concat()` 操作的参数是一个数组，那么添加的是数组中的元素，而不是数组。

`array.join(separator)` 将数组元素连接起来以构建一个字符串

- 参数：`separator`，在返回的字符串中用于分割数组元素的字符或字符串，它是可选的。如果省略了这个参数，用逗号作为分隔符。
- 返回值：一个字符串，通过把 `array` 的每个元素转换成字符串，然后把这些字符串连接起来，在两个元素之间插入 `separator` 字符串而生成。

**copyWithin()**批量复制方法

**fill()**填充数组方法

**toLocalString()、toString()、valueOf()**

#### 迭代方法

**Array.filter()**方法在数组中查找**满足特定条件的所有元素**。返回的新数组，如果数组中没有项目符合条件，则返回一个空数组。

**every() 方法**用于检测数组**所有元素是否都符合指定条件**。如果数组中检测到有一个元素不满足，则整个表达式返回false，且剩余的元素不会再进行检测。如果所有元素都满足条件，则返回 true。 every() 不会对空数组进行检测。every() 不会改变原始数组。

#### **归并方法**

reduce（）

reduceRight（）

# ⚝map() reduce() filter() every() some() find() findIndex()用法

## ⚝map() 方法

```js
array.map(function(cur, index, arr), thisVal)
```

1. cur：必须。当前元素的的值。
2. index：可选。当前元素的索引。
3. arr：可选。当前元素属于的数组对象。
4. thisVal：可选。对象作为该执行回调时使用，传递给函数，用作"this"的值。

> map()方法定义在Array中，调用Array的map()方法，传入我们自己的函数，它返回一个新的[数组](https://so.csdn.net/so/search?q=数组&spm=1001.2101.3001.7020)，数组中的元素为原始数组调用函数处理后的值。map()不会改变原始数组。

```scss
// 下面的语句返回什么呢：
["1", "2", "3"].map(parseInt);
// 你可能觉得会是 [1, 2, 3]
// 但实际的结果是 [1, NaN, NaN]
```

通常使用 `parseInt` 时，只需要传递一个参数. 但实际上，`parseInt` 可以有两个参数，第二个参数是进制数. 可以通过语句 `alert(parseInt.length) === 2` 来验证. `map` 方法在调用 `callback` 函数时，会给它传递三个参数：当前正在遍历的元素，元素索引，原数组本身. 第三个参数 `parseInt` 会忽视，但第二个参数不会，也就是说： `parseInt` 把传过来的索引值当成进制数来使用. 从而返回了 NaN.

可以使用箭头函数：

```python
['1', '2', '3'].map( str => {
  parseInt(str)
})
['1', '2', '3'].map(Number);  // [1, 2, 3]
// 与 parseInt 不同，下面的结果会返回浮点数或指数：
['1.1', '2.2e2', '3e300'].map(Number);  // [1.1, 220, 3e+300]
```

## ⚝reduce()方法

```js
arr.reduce(function(prev,cur,index,arr){
		...
	   }, init);
```

1. arr： 可选，表示原数组；
2. prev ： 必需 ，表示上一次调用回调时的返回值，或者初始值 init;
3. cur ： 必需 ， 表示当前正在处理的数组元素；
4. index 表示当前正在处理的数组元素的索引，若提供 init 值，则索引为0， 否则索引为1；
5. init： 可选， 表示初始值。

> reduce() 方法接收一个函数作为累加器，数组中的每个值（从左到右）开始缩减，最终计算为一个值。

```js
//数组去重
var arr = [1,2,1,2,3,4];
var newArr = arr.reduce(function (prev, cur) {
    prev.indexOf(cur) === -1 && prev.push(cur);
    return prev;
},[]);
//初始化一个空数组，将需要去重的第一项在初始化数组中查找，找不到，就将该
//项添加到初始化数组中。。。最后返回这个初始化数组。
console.log(newArr);  // [1, 2, 3, 4]
```

善于利用初始值

## filter()方法

```js
arr.filter(funciton(element, index, array){
			...
		}[, thisArg])
```

1. callback返回true表示保留该元素，false则不保留。
2. thisArg 可选。执行 callback 时的用于 this 的值。
3. element:元素的值
4. index:元素的索引
5. array:被遍历的数组

> filter()方法用于将Array的某些元素过滤掉，然后返回剩下的元素，（筛选数组中满足条件的元素，返回一个筛选后的新数组）
> filter()接收一个函数，将传入的函数依次作用于Array的每个元素，然后根据返回值是true（保留）还是false（丢弃）决定保留还是不保留该元素。

```js
//利用filter去重
var arr = [2,4,2,8,4,6];
var newArr = arr.filter(function(ele,index,self){
	return self.indexOf(ele) === index;
});
console.log(newArr )  //[2, 4, 8, 6]
```

## every()方法

```js
array.every(function(currentValue,index,arr){
	   	 ...
	   }, thisValue)
```

1. currentValue 必须。当前元素的值
2. index 可选。当前元素的索引值
3. arr 可选。当前元素属于的数组对象
4. thisValue 可选。对象作为该执行回调时使用，传递给函数，用作 “this” 的值。如果省略了 thisValue ，“this” 的值为 “undefined”

> every() 方法用于检测数组所有元素是否都符合指定条件（通过函数提供）
> 如果数组中检测到有一个元素不满足，则整个表达式返回 false ，且剩余的元素不会再进行检测。
> 如果所有元素都满足条件，则返回 true

## some()方法

some() 和every()类似，区别在于：

> every()是对数组中每一项运行给定函数，如果该函数所有项返回true,则返回true。一旦有一项不满足则返回flase
> some()是对数组中每一项运行给定函数，如果该函数满足任一项返回true，则返回true

## find()方法

> find()方法用于查找符合条件的第一个元素，如果找到了，返回这个元素，之后的值不会再调用执行函数,如果没有符合条件的元素返回 undefined

```c
语法： array.find(function(currentValue, index, arr),thisValue)
```

## findIndex（）方法

> findIndex()和find()类似，也是查找符合条件的第一个元素，不同之处在于findIndex()会返回这个元素的索引，如果没有找到，返回-1



# forEach（）和map（）区别

### forEach

forEach方法用于调用数组的每个元素，并将元素传递给回调函数。

```
array.forEach(function(currentValue, index, arr), thisValue)
```

### map

返回一个新数组，并且照原始数组元素顺序依次处理元素，数组中的元素为原始数组元素调用函数处理后的值。

```
array.map(function(currentValue,index,arr), thisValue)
```

### 共同点

- 对空数组都是不会执行回调函数的
- 都能够对数组进行循环

### 区别

- forEach() 会改变原始数组，返回值为undefined
- map() 不会改变原始数组，并且会返回一个新的数组

### 总结

forEach适合于你并不打算改变数据的时候，而只是想用数据做一些事情 – 比如存入数据库或则打印出来。

```js
let arr = [ 'a' , 'b' , 'c' , 'd' ];
arr.forEach((letter) => {
  console.log(letter);
});
// a
// b
// c
// d
```

map()适用于你要改变数据值的时候。不仅仅在于它更快，而且返回一个新的数组。这样的优点在于你可以使用复合(composition)(map(), filter(), reduce()等组合使用)来玩出更多的花样。

```js
let arr = [1, 2, 3, 4, 5];
let arr2 = arr.map(num => num * 2).filter(num => num > 5);
// arr2 = [6, 8, 10]
123
```

我们首先使用map将每一个元素乘以2，然后紧接着筛选出那些大于5的元素。最终结果赋值给arr2。

核心要点

能用forEach()做到的，map()同样可以。反过来也是如此。

map()会分配内存空间存储新数组并返回，forEach()不会返回数据。

forEach()允许callback更改原始数组的元素。map()返回新的数组。

# 字符串的常用方法 

#### **增+ ${} contact()**

除了常用的**+以及${}**进行字符串拼接之外，还可以通过**contact()**方法用于讲一个或多个字符串拼接成一个新字符串。

#### 删substr()、slice()、substring()

string.substr(start,length) 方法可在字符串中抽取从 开始下标开始的指定数目的字符。length子串中的字符数。必须是数值。如果省略了该参数，那么返回从 stringObject 的开始位置到结尾的字串。

slice(start, end) 方法可提取字符串的某个部分，并以新的字符串返回被提取的部分。使用 start（包含） 和 end（不包含）参数来指定字符串提取的部分。

substring(from, to) 方法用于提取字符串中介于两个指定下标之间的字符。返回的子串包括开始处的字符，但不包括结束 处的字符。

#### 改

**trim()、trimLeft()、trimRight() 方法**用于删除前、后或前后所有空格符。

**repeat() 方法**表示将字符串复制多少次，然后返回。接受一个整数参数。

**toLowerCase()、toUpperCase()**方法用于大小写转换。

**padStart()、padEnd()** 用于头尾补全字符串

#### 查：charAt()、indexOf()、startWith()、includes()

**charAt() 方法**返回给定索引位置的字符。需传入一个整数参数。

**indexOf() 方法**返回某个字符在字符串中的位置。需传入一个字符。

**startsWith() 方法**返回一个布尔值，判断字符串是否以传入的字符开头。

**endsWith() 方法**返回一个布尔值，判断字符串是否以传入的字符结尾。

**includes() 方法**返回一个布尔值，判断字符串是否包含传入的字符。

#### 转换方法：split()

**split() 方法**把字符串按照指定的分隔符，返回一个数组。

#### 匹配方法：match()、search()、replace()

**match() 方法**接受一个参数（正则表达式字符串或者正则表达式对象）。返回是一个数组。

**search() 方法**接受一个参数（正则表达式字符串或者正则表达式对象）。返回是布尔值。

**replace() 方法**接受两个参数，第一个是匹配的内容，第二个是替换的元素。替换的是第一次匹配到的。

### 如何去除字符串中的最后一个字符？ 

有三种方法: str.slice(0,str.length-1)、substr(0,length-1)、str.substring(0,str.length-1)
### js 获取字符串最后一个字符？

**str.charAt(str.length-1)**

**str.substr(str.length-1,1)**

**tr.substring(str.length-1)** 

**str.slice(str.length-1)**

**let res = str.split("")；res[str.length - 1]**

#### js字符串拼接有哪些方法

1）使用 + 运算符；

2）使用concat()方法；

3）使用 join 方法。

# Object对象的常用方法

##### Object.values()

​	返回一个对象属性值的数组。

##### Object.keys()

​	返回一个对象属性名的数组。

##### Object.entries()

​	创建一个数组，其中包含一个对象的键/值对数组。

##### Object.is()

​	相等运算符（==）和严格相等运算符（===）。它们都有缺点，前者会自动转换数据类型，后者的NaN不等于自身，以及+0等于-0，Object.is就是用来解决这个问题，与“===”基本一致。

##### Object.assign()

​	浅拷贝：用于对象的合并。

##### Object spread (对象展开)

​	展开一个对象，允许向一个对象添加新的属性和值。

# Object对象实例的属性和方法

**constructor**：保存着用于创建当前对象的函数。如 Object()。
**hasOwnProperty**：用于检查给定的属性在当前对象实例中（而不是在实例的原型中）是否存在。其中，作为参数的属性名（propertyName）必须以[字符串](https://so.csdn.net/so/search?q=字符串&spm=1001.2101.3001.7020)形式指定（例如：o.hasOwnProperty(“name”)）。
**isPrototypeOf(object)**：用于检查传入的对象是否是传入对象的原型（第 5 章将讨论原型）。
**propertyIsEnumerable**：用于检查给定的属性是否能够使用 for-in 语句来枚举。与 hasOwnProperty()方法一样，作为参数的属性名必须以字符串形式指定。
**toLocaleString()**：返回对象的字符串表示，该字符串与执行环境的地区对应。
**toString()**：返回对象的字符串表示。
**valueOf()**：返回对象的字符串、数值或布尔值表示。通常与 toString()方法的返回值相同。

# for...of与for...in的区别

无论是`for...in`还是`for...of`语句都是迭代一些东西。它们之间的主要区别在于它们的迭代方式。

for...in语句以**任意顺序迭代对象的可枚举属性。**

for...of语句遍历**可迭代对象定义要迭代的数据。**（包括Array，Map，Set，String，TypedArray，arguments对象，DOM中的NodeList对象等等）



**使用for in 也可以遍历数组，但是会存在以下问题**：

1.index索引为字符串型数字，不能直接进行几何运算

2.遍历顺序有可能不是按照实际数组的内部顺序

3.使用for in会遍历数组所有的可枚举属性，包括原型。例如上栗的原型方法method和name属性

# for...of怎么遍历对象

因为for...of 语句遍历**可迭代对象**定义要迭代的数据。只有提供了 `Iterator` 接口的数据类型才可以使用 `for-of` 来循环遍历，只要将对象设置`Iterator` 接口即可。

因为ES6同时提供了 **Symbol.iterator 属性**，只要一个数据结构有这个属性，就会被视为有 `Iterator` 接口，接着就是如何实现这个接口了，实现：

```js
class User{
    constructor(name, gender, lv){
        this.name = name;
        this.gender = gender;
        this.lv = lv;
    }
    
    *[Symbol.iterator](){
        let keys = Object.keys( this )
            ;
        
        for(let i = 0, l = keys.length; i < l; i++){
            yield {
                key: keys[i]
                , value: this[keys[i]]
            };
        }
    }
}
let zhou = new User('zhou', 'male', 1);

for(let {key, value} of zhou){
    console.log(key, value);
}
// 输出结果
// name zhou
// gender male
// lv 1
```



# Object和Map的区别

### 构造方式不同

在 JavaScript 中创建 `Object` 最简单的方法是通过**字面量**，也可以用**构造函数**，也可以用Object.prototype.create来创建

```js
var obj = Object.create(null); //空对象
```

`Map` 则是通过内置构造函数 `Map` 创建。

### 对象中的键是字符串，Map中的键可以是任意类型

`Object` 是键值对的集合，但键只能是字符串。而 `Map` 的键可以是任意类型。

比如，如果用数字作 `Object` 的键，则该数字将转换为字符串。

因为键已经被转换成字符串，所以无论我们是获取数字 `1` 还是字符串 `'1'` 的值时，得到的结果都一样。

### Map 提供了更好的接口来访问键值对

插入元素：Map set ；Object [] .运算符

删除元素：Map delete(key) claer() ；Object delete运算符

访问元素：Map get(key) claer() ；Object  [] .运算符

检查键是否存在:Map has(key)；Object  hasOwnProperty(key)

获取大小:Map size ；Object **Object**.**keys**(obj).length

迭代： Map for...of  forEach()；Object for...in keys(obj)

### Map 会保留键的顺序，对象不会

### JSON支持对象，但不支持 Map

### 应用场景

- JSON直接支持Object，但尚未支持Map。因此，在某些我们必须使用JSON的情况下，应将Object视为首选。
- Map是一个纯哈希结构，而Object不是（它拥有自己的内部逻辑）。使用`delete`对Object的属性进行删除操作存在很多性能问题。所以，针对于存在**大量增删操作**的场景，使用Map更合适。
- 不同于Object，Map会保留所有元素的顺序。Map结构是在基于可迭代的基础上构建的，所以如果考虑到元素迭代或顺序，使用Map更好，它能够确保在所有浏览器中的迭代性能。
- Map在存储大量数据的场景下表现更好，尤其是在key为未知状态，并且所有key和所有value分别为相同类型的情况下。

# JS遍历对象

### for...in

`for...in`可以遍历对象的**所有可枚举属性**，**包括对象本身的和对象继承来的属性**

如果使用`Object.defineProperty`的形式来设置对象的描述对象，将enumerable设为`false`，使用`for...in`遍历，不会被打印

其实`for...in`的特性会导致一个问题，其继承的属性会被遍历到，所以当我们不想要遍历被继承的属性，那么我们就可以使用`Object.keys()`

### Object.keys()

`Object.keys()`方法可以遍历到所有**对象本身的可枚举属性**，但是其**返回值为数组**

```css
let obj = {
    name:"ned",
    like:"man"
}
Object.keys(obj) //  ['name', 'like']
```

到这里我们一般是使用`Object.keys()`来代替`for...in`遍历操作，除此之外，为了代替`for...in`，又新增了与`Object.keys()`配套的遍历方法，下面我们来看看

### Object.values()

`Object.values()`与`Object.keys()`遍历对象的特性是相同的，但是其返回的结构是以遍历的**属性值构成**的数组

### Object.entries()

`Object.entries()`的返回值为`Object.values()`与`Object.keys()`的结合，也就是说它会返回一个嵌套数组，数组内包括了属性名与属性值

其遍历特性与`Object.values()`和`Object.keys()`相同，不再赘述。

### Object.getOwnPropertyNames()

其返回结果与`Object.keys()`对应，但是他得特性与其相反，**会返回对象所有属性，包括了不可枚举属性**

```javascript
var arr = [];
console.log(Object.getOwnPropertyNames(arr)); // ['length']
Object.getOwnPropertyDescriptor(arr,"length").enumerable // false
```

上面我声明了一个空数组，而使用`Object.getOwnPropertyNames()`方法，返回了`length`，而length属性的`enumerable`正是`false`

### Object.getOwnPropertySymbols()

`Object.getOwnPropertySymbols()`会返回对象内的所有`Symbol`属性，返回形式依旧是数组，值得注意的是，在对象初始化的时候，内部是不会包含任何`Symbol`属性

```css
let obj = {
    name:"obj"
}
Object.getOwnPropertySymbols(obj) // []
```

所以我初始化一个对象，通过这个方法返回的是一个空数组

```javascript
let sym = Symbol()
console.log(sym)
obj[sym] = "hogskin" 
Object.getOwnPropertySymbols(obj) // Symbol()
```

我在后面新建`symbol`，再为上面声明好的对象添加进去，通过遍历得到`Symbol()`

### Object.getOwnPropertyDescriptor()

以上两者的结合

### Reflect.ownKeys()

`Reflect.ownKeys()`返回数组，即包含了对象的所有属性，无论是否可枚举还是属性是symbol，还是继承，将所有的属性返回

```css
let arr = [0,31,2]
Reflect.ownKeys(arr) // ['0', '1', '2', 'length']
```

上面的例子声明了一个数组，返回的是数组下标和`length`属性。

# 常用数组求和

*https://www.cnblogs.com/faithzzz/p/7063952.html*

#### *一、for循环*

```js
var arr = [1,2,3];
function sum(arr) {
  var s = 0;
  for (var i = 0;i<arr.length;i++) {
    s += arr[i];
  }
  return s;
}
console.log(sum(arr));//6
```

#### *二、forEach遍历*

```js
var arr = [1,2,3];
function sum(arr) {
  var s = 0;
  arr.forEach(function(val, idx, arr) {
    s += val;
  }, 0);
  return s;
};
console.log(sum(arr));//6
```

#### *三、reduce*

```js
var arr = [1,2,3];
function sum(arr) {
  return arr.reduce(function(acr, cur){
    return acr + cur;
  });
}
console.log(sum(arr));//6
```

#### *四、递归*

```js
var arr = [1,2,3];
function sum(arr) {
  if(arr.length == 0){
    return 0;
  } else if (arr.length == 1){
    return arr[0];
  } else {
    return arr[0] + sum(arr.slice(1));
  }
}
console.log(sum(arr));//6
```

#### *五、eval*

```js
var arr = [1,2,3];
function sum(arr) {
  return eval(arr.join("+"));
};
console.log(sum(arr));//6
```

# 数组去重

### 利用ES6 Set去重（ES6中最常用）

ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。Set 本身是一个构造函数，用来生成 Set 数据结构。

```js
let resultArr = Array.from(new Set(originalArray));

// 或者用扩展运算符
let resultArr = [...new Set(originalArray)];

// 再简化
let unique = (a) => [...new Set(a)]

console.log(resultArr);
// [1, "1", 2, true, "true", false, null, {…}, {…}, "abc", undefined, NaN]
```

Set 并不是真正的数组，这里的 `Array.from` 和 `...` 都可以将 Set 数据结构，转换成最终的结果数组。

这是最简单快捷的去重方法，但是细心的同学会发现，这里的 `{}` 没有去重。可是又转念一想，2 个空对象的地址并不相同，所以这里并没有问题，结果 ok。

### 利用for嵌套for，然后splice去重（ES5中最常用）

```js
for (let i = 0; i < originalArray.length; i++) {
    for (let j = (i + 1); j < originalArray.length; j++) {
        // 第一个等于第二个，splice去掉第二个
        if (originalArray[i] === originalArray[j]) {
            originalArray.splice(j, 1);
            j--;
        }
    }
}

console.log(originalArray);
// [1, "1", 2, true, "true", false, null, {…}, {…}, "abc", undefined, NaN, NaN]
```

splice 方法会修改源数组，所以这里我们并没有新开空数组去存储，最终输出的是修改之后的源数组。但同样的没有处理 `NaN`。

### indexOf 和 includes

```js
function unique(arr) {
    if (!Array.isArray(arr)) {
        console.log('type error!')
        return
    }
    var array = [];
    for (var i = 0; i < arr.length; i++) {
        if (array .indexOf(arr[i]) === -1) {
            array .push(arr[i])
        }
    }
    return array;
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
   // [1, "true", true, 15, false, undefined, null, NaN, NaN, "NaN", 0, "a", {…}, {…}]  //NaN、{}没有去重
```

indexOf 并不没处理 `NaN`。

再来看 includes，它是在 ES7 中正式提出的。

```js
const resultArr = [];
for (let i = 0; i < originalArray.length; i++) {
    if (!resultArr.includes(originalArray[i])) {
        resultArr.push(originalArray[i]);
    }
}
console.log(resultArr);
// [1, "1", 2, true, "true", false, null, {…}, {…}, "abc", undefined, NaN]
```

*includes 处理了 `NaN`，结果 ok。*

### sort

先将原数组排序，生成新的数组，然后遍历排序后的数组，相邻的两两进行比较，如果不同则存入新数组。



```js
const sortedArr = originalArray.sort();

const resultArr = [sortedArr[0]];

for (let i = 1; i < sortedArr.length; i++) {
    if (sortedArr[i] !== resultArr[resultArr.length - 1]) {
        resultArr.push(sortedArr[i]);
    }
}
console.log(resultArr);
// [1, "1", 2, NaN, NaN, {…}, {…}, "abc", false, null, true, "true", undefined]
```

### 对象的属性

每次取出原数组的元素，然后在对象中访问这个属性，如果存在就说明重复。

```js
const resultArr = [];
const obj = {};
for(let i = 0; i < originalArray.length; i++){
    if(!obj[originalArray[i]]){
        resultArr.push(originalArray[i]);
        obj[originalArray[i]] = 1;
    }
}
console.log(resultArr);
// [1, 2, true, false, null, {…}, "abc", undefined, NaN]
```

但这种方法有缺陷。从结果看，它貌似只关心值，不关注类型。还把 {} 给处理了，但这不是正统的处理办法，所以 **不推荐使用**。

### 利用filter

```js
function unique(arr) {
  return arr.filter(function(item, index, arr) {
    //当前元素，在原始数组中的第一个索引==当前索引值，否则返回当前元素
    return arr.indexOf(item, 0) === index;
  });
}
    var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
        console.log(unique(arr))
//[1, "true", true, 15, false, undefined, null, "NaN", 0, "a", {…}, {…}]
```

思想: 利用indexOf检测元素在数组中第一次出现的位置是否和元素现在的位置相等，如果不等则说明该元素是重复元素

### reduce 实现对象数组去重复

```js
var resources = [
            { name: "张三", age: "18" },
            { name: "张三", age: "19" },
            { name: "张三", age: "20" },
            { name: "李四", age: "19" },
            { name: "王五", age: "20" },
            { name: "赵六", age: "21" }
        ]
     var temp = {};
     resources = resources.reduce((prev, curv) => {
         // 如果临时对象中有这个名字，什么都不做
         if (temp[curv.name]) {
         }
         // 如果临时对象没有就把这个名字加进去，同时把当前的这个对象加入到prev中
         else {
             temp[curv.name] = true;
             prev.push(curv);
         }
         return prev
     }, []);
     console.log("结果", resources);
```

这种方法是利用高阶函数 reduce 进行去重， 这里只需要注意initialValue得放一个空数组[]，不然没法push。

### filter + hasOwnProperty

filter 方法会返回一个新的数组，新数组中的元素，通过 hasOwnProperty 来检查是否为符合条件的元素。

```js
const obj = {};
const resultArr = originalArray.filter(function (item) {
    return obj.hasOwnProperty(typeof item + item) ? false : (obj[typeof item + item] = true);
});

console.log(resultArr);
// [1, "1", 2, true, "true", false, null, {…}, "abc", undefined, NaN]
```

这 貌似是目前看来最完美的解决方案了。这里稍加解释一下：

- hasOwnProperty 方法会返回一个布尔值，指示对象自身属性中是否具有指定的属性。
- `typeof item + item` 的写法，是为了保证值相同，但类型不同的元素被保留下来。例如：第一个元素为 number1，第二第三个元素都是 string1，所以第三个元素就被去除了。
- `obj[typeof item + item] = true` 如果 hasOwnProperty 没有找到该属性，则往 obj 里塞键值对进去，以此作为下次循环的判断依据。
- 如果 hasOwnProperty 没有检测到重复的属性，则告诉 filter 方法可以先积攒着，最后一起输出。

看似完美解决了我们源数组的去重问题，但在实际的开发中，一般不会给两个空对象给我们去重。所以稍加改变源数组，给两个空对象中加入键值对。

```js
let originalArray = [1, '1', '1', 2, true, 'true', false, false, null, null, {a: 1}, {a: 2}, 'abc', 'abc', undefined, undefined, NaN, NaN];
```

然后再用 filter + hasOwnProperty 去重。

然而，结果竟然把 `{a: 2}` 给去除了！！！这就不对了。

所以，这种方法有点去重 `过头` 了，也是存在问题的

### lodash 中的 _.uniq

### lodash 如何实现去重

简单说下 `lodash` 的 `uniq` 方法的源码实现。

这个方法的行为和使用 Set 进行去重的结果一致。

当数组长度大于等于 `200` 时，会创建 `Set `并将 `Set` 转换为数组来进行去重（Set 不存在情况的实现不做分析）。当数组长度小于 `200` 时，会使用类似前面提到的 双重循环 的去重方案，**另外还会做 NaN 的去重**。



### 性能考虑

**双重 for 循环 > Array.filter()加 indexOf > Array.sort() 加一行遍历冒泡 > Object 键值对去重复 > ES6中的Set去重**

### 内存考虑

以上的所有数组去重方式，应该 Object 对象去重复的方式是时间复杂度是最低的，除了一次遍历时间复杂度为`O(n)` 后，查找到重复数据的时间复杂度是`O(1)`，类似散列表，大家也可以使用 ES6 中的 Map 尝试实现一下。

但是对象去重复的空间复杂度是最高的，因为开辟了一个对象，其他的几种方式都没有开辟新的空间，从外表看来，更深入的源码有待探究，这里只是要说明大家在回答的时候也可以考虑到`时间复杂度`还有`空间复杂度`。

另外补充一个**误区**，有的小伙伴会认为 `Array.filter()`加 `indexOf` 这种方式时间复杂度为 `O(n)` ,其实不是这样，我觉得也是`O(n^2)`。因为 `indexOf` 函数，源码其实它也是进行 for 循环遍历的。具体实现如下

```javascript
String.prototype.indexOf = function(s) {
    for (var i = 0; i < this.length - s.length; i++) {
        if (this.charAt(i) === s.charAt(0) &&
            this.substring(i, s.length) === s) {
            return i;
        }
    }
    return -1;
};
```



# 数组展平（扁平/拍平）

数组`扁平化`就是将一个`多维数组`，拍平为`一维数组`。

### 1.Array.flat

这个方法是[ES2019](https://link.juejin.cn/?target=https%3A%2F%2Ftc39.es%2Fproposal-flatMap%2F)提供的API，可以直接将数组进行扁平化处理

```js
let arr = [[1, 2, 3], [4, 5, 6, 7], [8, 9, 10, 11, [12, 13, [14, 15, 16]]], 17];
arr = arr.flat(3)
console.log(arr);
// 打印结果：
[
   1,  2,  3,  4,  5,  6,  7,
   8,  9, 10, 11, 12, 13, 14,
  15, 16, 17
]

```

`Array.prototype.flat([depth])`中的`depth`指的是`深度`，或者说`几维数组`。上方的`arr`最大维度是`3维`，所以指明参数`depth`为3。在不知道维度的情况下可以指明`depth`为`Infinity`，代表不管`几维数组`，都会降为`一维数组`。

特殊：原数组有空位，flat方法会消除空位，即使是flat(0)也会消除空位，所以第1点说的是“只是维数一样”。并且flat方法展开到哪一层，空位就会消除到哪一层，再深层的空位不会消除。

### 2. 递归

#### 2.1 常规递归

```js
function myFlat(arr) {
    let res = []
    // 遍历
    for (const a of arr) {
        // 判断是否是数组
        if(a instanceof Array){
            // 是,则递归调用
            res = res.concat(myFlat(a))
        }else{
            // 不是则直接放入
            res.push(a)
        }
    }
    return res;
}

console.log(myFlat([1, [2], [3, [[4]]]]));// [1,2,3,4]
```

#### 2.2 reduce 递归

```js
let a = [{ a: 1 }, { b: 2 }, 3, 4, 5, [6, [7, 8, [9]]]]

function myFlat(arr) {
    return arr.reduce((pre, cur) => {
        return pre.concat(Array.isArray(cur) ? myFlat(cur) : cur)
    }, [])
}
console.log(myFlat(a))
// [ { a: 1 }, { b: 2 }, 3, 4, 5, 6, 7, 8, 9 ]
```

#### 2.3 优化：深度优化和空位处理d

```js
function myFlat(arr, depth = 1) {
  if (depth <= 0) {
    return arr;
  }
  let res = [];
  for (const item of arr) {
    if (Array.isArray(item)) {
      res = res.concat(myFlat(item, depth - 1));
    } else {
      // 判断数组空位
      item !== undefined && res.push(item);
    }
  }
  return res;
}
```



### 3.toString

**如果数组中存储元素都为数字**，那么我们可以取巧，使用`toString`方法来解决。

#### 3.1 split

```js
var arr = [1,[2,[3,4]]];

function flatten(arr){
	return arr.toString().split(',').map(function(item){
		return item;
	})
}
console.log(flatten(arr)); //[ '1', '2', '3', '4' ]
```

#### 3.2 JSON.parse()

```js
var arr = [1,[2,[3,4]]];

function flatten(arr){
	return JSON.parse('[' + arr.toString() + ']');
}
console.log(flatten(arr)); //[ '1', '2', '3', '4' ]
```

也可以给数组字符串的首尾分别加中括号`[]`包裹，再通过`JSON.parse()`将数组字符串转换为`真正的数组`

#### 4.扩展运算符 使用while循环 + some

ES6增加了这个运算符，用于取出对象所有可遍历的属性，放到当前对象内，我们来试试这个方法。

```js
var arr = [1,[2,[3,4]]];
console.log([].concat(...arr)); //[ 1, 2, [ 3, 4 ] ]
```

将取出arr中所有可以遍历的属性，将他们放到一个新数组中。

但是只扁平了一层，于是可以

```js
while(arr.some(item=>Array.isArray(item))){
    // 只要有数组类型 都会通过展开运算符进行降维
    arr = [].concat(...arr);
}
console.log(arr);
// 打印结果:
[
   1,  2,  3,  4,  5,  6,  7,
   8,  9, 10, 11, 12, 13, 14,
  15, 16, 17
]
```





# const、let和var的区别

*![](pics/变量区别.jpg)*



## 区别

- var 声明的范围是函数作用域，let 和 const 声明的范围是块作用域

- var 声明的变量会被提升到函数作用域的顶部，let 和 const 声明的变量不存在提升，且具有暂时性死区特征
- var 允许在同一个作用域中重复声明同一个变量，let 和 const 不允许
- 在全局作用域中使用 var 声明的变量会成为 window 对象的属性，let 和 const 声明的变量则不会。
- const 的行为与 let 基本相同，唯一一个重要的区别是，使用 const 声明的变量必须进行初始化，且不能被修改

## 注意

**var**

在函数内部定义变量时省略`var`操作符，会创建一个全局变量，在严格模式下会报错

```scss
function test() {
    message = "hi"; // 全局变量
}
test();
console.log(message); 
```

**const**

`const`声明的限制只适用于它指向的变量的引用，换句话说，如果`const`变量引用的是一个对象，那么修改这个对象内部的属性并不违反`const`限制：

如果想让整个对象都不能修改，可以使用Object.freeze()

```ini
const obj = { x: 666 };
obj.x = 888; // ok
obj.y = '啊哈哈'; // ok
```

## 变量提升

> 通常JS引擎会在正式执行之前先进行一次预编译，在这个过程中，首先将变量声明及函数声明提升至当前作用域的顶端，然后进行接下来的处理。

使用 `var` 声明，存在变量提升的情况:

```js
console.log(a); // undefined
var a = 1;
console.log(a); // 1

// ======= 实际等效于 =========
var a;
console.log(a); // undefined
a = 1;
console.log(a); // 1
```

**函数声明式存在函数提升的情况**

`JavaScript` 不同于 `Java` 等强类型语言，是在一个 `class` 中执行；在遇到函数直接相互调用的情况，都是通过 `this` 的形式，所以不需要考虑`函数提升`；而对于`JavaScript`，`函数提升`就是**解决多个函数相互调用的情况**

```js
console.log(fn); // ƒ fn() {}
function fn() {}
```

在同一个作用域声明一个与函数名相同的变量名，**函数会覆盖变量名**：

```js
console.log(a); // ƒ a() {}
var a = 1;
function a() {};
console.log(a); // 1

// ======= 实际等效于 =========
function a() {};
console.log(a);  // ƒ a() {}
a = 1;
console.log(a); // 1

```

函数表达式的本质，是先声明一个变量，然后将一个匿名函数赋值给这个变量。

```js
console.log(fn); // undefined
var fn = function(){}
console.log(fn); // ƒ (){}
```

## js暂时性死区

ES6 规定，如果代码区块中存在 `let` 和 `const` 命令声明的变量，这个区块对这些变量从**一开始就形成了封闭作用域，直到声明语句完成，这些变量才能被访问（获取或设置），假如我们尝试在声明前去使用这类变量,就会报错**`ReferenceError`。这在语法上称为“暂时性死区”（英temporal dead zone，简 TDZ），即代码块开始到变量声明语句完成之间的区域。

1、通过 `var` 声明的变量拥有变量提升、**没有暂时性死区**，作用于函数作用域：

- 当进入变量的作用域（包围它的函数），立即为它创建（绑定）存储空间，**立即被初始化并被赋值为 `undefined`**
- 当执行到变量的声明语句时，如果变量定义了值则会被赋值

```js
(function fn() {  //函数作用域开始
        console.log(temp)  //undefined
        //声明
        var temp 
        console.log(temp)  //undefined
        //赋值
        temp = 123
        console.log(temp)  //123
    })()
    //在函数作用域外访问
    console.log(temp)  //ReferenceError: temp is not defined
```

2、通过 `let` 声明的变量没有变量提升、拥有暂时性死区，作用于块级作用域：

- 当进入变量的作用域（包围它的语法块），立即为它创建（绑定）存储空间，不会立即初始化，也不会被赋值
- 访问（获取或设置）该变量会抛出异常 `ReferenceError`
- 当执行到变量的声明语句时，如果变量定义了值则会被赋值，如果变量没有定义值，则被赋值为`undefined`d

```js
{  //函数作用域开始，TDZ开始
            console.log(temp)  //ReferenceError: temp is not defined
            //声明
            let temp  
            console.log(temp)  //ReferenceError: Cannot access 'temp' before initialization
            //赋值
            temp = 345  //TDZ结束
            console.log(temp)  //345
            //块级作用域结束
        }
        //在块级作用域外访问
        console.log(temp)  //ReferenceError: temp is not defined
```

3、通过 const 声明的常量，需要在定义的时候就赋值，并且之后不能改变，暂时性死区与 let 类似。

```js
 {   //作用域开始，TDZ开始
            console.log(temp)  //ReferenceError: temp is not defined
            //声明并赋值
            const temp = 789  //TDZ结束
            console.log(temp)  //789 
            //给常量赋值
            temp = 987  //TypeError: Assignment to constant variable
            //作用域结束
        }   
        //在作用域外访问
        console.log(temp)  //ReferenceError: temp is not defined
```

# 执行栈和执行上下文

### 什么是执行上下文

执行上下文是当前 JavaScript 代码被**解析和执行**时所在环境的抽象概念。

#### 执行上下文类型

JavaScript 中有三种执行上下文类型。

- **全局执行上下文** — 这是默认或者说基础的上下文，任何不在函数内部的代码都在全局上下文中。它会执行两件事：创建一个全局的 window 对象（浏览器的情况下），并且设置 `this` 的值等于这个全局对象。一个程序中只会有一个全局执行上下文。
- **函数执行上下文** — 每当一个函数被调用时, 都会为该函数创建一个新的上下文。每个函数都有它自己的执行上下文，不过是在函数被调用时创建的。函数上下文可以有任意多个。每当一个新的执行上下文被创建，它会按定义的顺序（将在后文讨论）执行一系列步骤。
- **Eval 函数执行上下文** — 执行在 `eval` 函数内部的代码也会有它属于自己的执行上下文，但由于 JavaScript 开发者并不经常使用 `eval`，所以在这里我不会讨论它。

### 执行栈

执行栈，也叫调用栈，具有 LIFO（后进先出）结构，用于存储在代码执行期间创建的所有执行上下文。

首次运行JS代码时，会创建一个**全局**执行上下文并Push到当前的执行栈中。每当发生**函数调用**，引擎都会为该函数创建一个**新的函数**执行上下文并Push到当前执行栈的栈顶。

根据执行栈LIFO规则，当栈顶函数运行完成后，其对应的**函数**执行上下文将会从执行栈中Pop出，上下文控制权将移到当前执行栈的**下一个**执行上下文。

### 执行上下文的生命周期

https://juejin.cn/post/6844903798784131079#heading-4

执行上下文的生命周期包括三个阶段：**创建阶段→执行阶段→回收阶段**。

#### 1.创建阶段

当函数被调用，但未执行任何其内部代码之前，会做以下三件事：

- 创建**变量对象**：首先初始化函数的参数arguments，提升函数声明和变量声明。
- 创建**作用域链**（Scope Chain）：在执行期上下文的创建阶段，作用域链是在变量对象之后创建的。作用域链本身包含变量对象。作用域链用于解析变量。当被要求解析变量时，JavaScript 始终从代码嵌套的最内层开始，如果最内层没有找到变量，就会跳转到上一层父作用域中查找，直到找到该变量。
- 确定**this指向**：包括多种情况，下文会详细说明

在一段 JS 脚本执行之前，要先解析代码（所以说 JS 是解释执行的脚本语言），解析的时候会先创建一个全局执行上下文环境，先把代码中即将执行的变量、函数声明都拿出来。变量先暂时赋值为undefined，函数则先声明好可使用。这一步做完了，然后再开始正式执行程序。

另外，一个函数在执行之前，也会创建一个函数执行上下文环境，跟全局上下文差不多，不过 函数执行上下文中会多出this arguments和函数的参数。

#### 2.执行阶段

执行变量赋值、代码执行

#### 3.回收阶段

执行上下文出栈等待虚拟机回收执行上下文

# 作用域和作用域链


### 什么是作用域、作用域链？

**作用域概念**：在某一区域执行Js代码时，需对变量或函数的值进行访问，而这一区域又提供对变量或函数的查找，并确定是否可以访问，则称该区域为作用域

**重要性**

1. **作用域最为重要的一点是安全。**变量只能在特定的区域内才能被访问，有了作用域我们就可以避免在程序其它位置意外对某个变量做出修改。
2. **作用域也会减轻命名的压力。**我们可以在不同的作用域下面定义相同的变量名。

**类型**

作用域又分为**全局作用域**和**局部作用域**。在ES6之前，局部作用域只包含了函数作用域，ES6的到来为我们提供了 **‘块级作用域’**（由一对花括号包裹），可以通过新增命令let和const来实现；而对于全局作用域这里有一个小细节需要注意一下：

> - 在 Web 浏览器中，全局作用域被认为是 **`window`** 对象，因此所有全局变量和函数都是作为 **`window`** 对象的属性和方法创建的。
> - 在 Node环境中，全局作用域是 **`global`** 对象。

**函数作用域**，所谓函数作用域，顾名思义就是由函数定义产生出来的作用域。

**块级作用域**，ES6 之前 JavaScript 没有块级作用域，只有全局作用域和函数（局部）作用域。块语句（ **`{}`** 中间的语句），如 if 和 switch条件语句， for 和 while循环语句，不同于函数，它们不会创建一个新的作用域；但是ES6及之后的版本，块语句也会创建一个新的作用域，**块级作用域可通过新增命令let和const声明**，所声明的变量在**指定块**的**作用域外无法被访问**。块级作用域在如下情况被创建：

> 1. 在一个函数内部
> 2. 在一个代码块（由一对花括号包裹）内部

let 声明的语法与 var 的语法一致。基本上可以用 let 来代替 var 进行变量声明，但会将变量的作用域限制在当前代码块中 **（注意：块级作用域并不影响var声明的变量）。** 但是使用let时有几点需要**注意**：

> - **声明变量不会提升到代码块顶部，即不存在变量提升**
> - **禁止重复声明同一变量**
> - **for循环语句中（）内部，即圆括号之内会建立一个隐藏的作用域，该作用域不属于for后边的{}中，并且只有for后边的{}产生的块作用域能够访问这个隐藏的作用域，这就使循环中** **绑定块作用域有了妙用**

这里分别演示一下ES5和ES6版本的代码，ES5：

```js
if(true) {
    var a = 1
}
for(var i = 0; i < 10; i++) {
    ...
}
console.log(a) // 1
console.log(i) // 10
```

ES6：

```js
for (let i = 0; i < 10; i++) {
            console.log(i);//0,1,2,3,4,5,6,7,8,9
 }
console.log(i);// Uncaught ReferenceError: i is not defined
```

```js
if (true) {
     let i = 9;
}
console.log(i);// Uncaught ReferenceError: i is not defined
```

**作用域链概念**：**变量取值会到创建这个变量的函数的作用域中取值，如果找不到，就会向上级作用域去查，直到查到全局作用域，这么一个查找过程形成的链条就叫做作用域链。**

# 原型和原型链

### **原型**

在JavaScript中，每当定义一个**函数数据类型**(普通函数、类)时候，都会**天生自带一个prototype属性，这个属性指向函数的原型对象**。上面定义的属性和方法可以被对象实例**共享**。

### **原型链**

1.`__proto__`和constructor

（Chrome、Safari、Firefox）每一个**对象数据类型**(普通的对象、实例、prototype......)也天生自带一个属性`__proto__`，属性值是当前实例所属类的原型(prototype)。原型对象中有一个属性**constructor**, 它指向函数对象。

> **but  `__proto__` 不是实例的属性，也不是构造函数的属性，在大多数的浏览器中都支持这种非正式的访问方式。实际上 `__proto__` 来自 `Object.prototype`，当使用 `obj.__proto__` 时，可以理解成返回了 `Object.getPrototypeOf(obj)`**

*![](pics/原型图.png)*



在JavaScript中万物都是对象，对象和对象之间也有关系，并不是孤立存在的。**对象之间的继承关系，在JavaScript中是实例通过proto指向原型对象，原型对象也指向父类的原型对象，直到指向Object对象为止，专业术语称之为原型链。**

当我们访问对象的一个属性或方法时，它会先在**对象自身**中寻找，如果有则直接使用，如果没有则会去**原型对象**中寻找，如果找到则直接使用。如果没有则去**原型的原型中寻找**,直到找到Object对象的原型，Object对象的原型没有原型，如果在Object原型中依然没有找到，则返回undefined。

*![](pics/原型链.png)*

据类型的基类(最顶层的类)在Object.prototype上没有__proto__这个属性。

```js
console.log(Object.prototype.__proto__ === null) // true
```

*![](pics/原型和原型链.png)*

小结

1、所有的实例的`_proto_`都指向该构造函数的原型对象（`prototype`）。

2、所有的函数（包括构造函数）是`Function`的实例，所以所有函数的`_proto_`的都指向`Function`的原型对象。

3、所有的原型对象（包括 `Function`的原型对象）都是Object的实例，所以`_proto_`都指向 `Object`（构造函数）的原型对象。而`Object`构造函数的 `_proto_`指向 null。

4、`Function`构造函数本身就是`Function`的实例，所以`_proto_`指向`Function`的原型对象。

### 哪些对象没有原型

1、Object.prototype，指向null

2、用Object.create(null)创建的对象没有原型



#### 怎么判断当前属性在实例上还是在原型上

in操作符： 在实例和原型上都会检测 （'name' in person）

hasOwnProperty() : 只会检测是否在实例上

如果要检测是否在原型上，则可以

```js
!obj.hasOwnProperty('name') && ('name' in obj)
```



### constructor 值只读吗

这个得分情况，对于引用类型来说 `constructor` 属性值是可以修改的，但是对于基本类型来说是只读的。

引用类型情况其值可修改这个很好理解，比如原型链继承方案中，就需要对 `constructor`重新赋值进行修正。

```js
// 木易杨
function Foo() {
    this.value = 42;
}
Foo.prototype = {
    method: function() {}
};

function Bar() {}

// 设置 Bar 的 prototype 属性为 Foo 的实例对象
Bar.prototype = new Foo();
Bar.prototype.foo = 'Hello World';

Bar.prototype.constructor === Object;
// true

// 修正 Bar.prototype.constructor 为 Bar 本身
Bar.prototype.constructor = Bar;

var test = new Bar() // 创建 Bar 的一个新实例
console.log(test);
```

*![image-20190210152306297](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/2/18/16900719887490c4~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)*

对于基本类型来说是只读的，比如 `1、“muyiy”、true、Symbol`，当然 `null` 和 `undefined` 是没有 `constructor` 属性的。

```js
// 木易杨
function Type() { };
var	types = [1, "muyiy", true, Symbol(123)];

for(var i = 0; i < types.length; i++) {
	types[i].constructor = Type;
	types[i] = [ types[i].constructor, types[i] instanceof Type, types[i].toString() ];
};

console.log( types.join("\n") );
// function Number() { [native code] }, false, 1
// function String() { [native code] }, false, muyiy
// function Boolean() { [native code] }, false, true
// function Symbol() { [native code] }, false, Symbol(123)
```

为什么呢？因为创建他们的是只读的原生构造函数（`native constructors`），这个例子也说明了依赖一个对象的 `constructor` 属性并不安全。

# 构造函数

### 实例成员 和 静态成员

**实例成员：** 实例成员就是在构造函数内部，通过this添加的成员。实例成员只能通过实例化的对象来访问。（对象本身 以及原型中的所有的属性和方法）

**静态成员：** 在构造函数本身上添加的成员，只能通过构造函数来访问

```js
    function Star(name,age) {
        //实例成员
        this.name = name;
        this.age = age;
    }
    //静态成员
    Star.sex = '女';

    let stars = new Star('小红',18);
    console.log(stars);      // Star {name: "小红", age: 18}
    console.log(stars.sex);  // undefined     实例无法访问sex属性

    console.log(Star.name); //Star     通过构造函数无法直接访问实例成员
    console.log(Star.sex);  //女       通过构造函数可直接访问静态成员
```

### 通过构造函数创建对象

#### 每个实例的方法是共享的吗？

这要看我们如何定义该方法了，分为两种情况。

##### 方法1：在构造函数上直接定义方法（不共享）

```
    function Star() {
        this.sing = function () {
            console.log('我爱唱歌');
        }
    }
    let stu1 = new Star();
    let stu2 = new Star();
    stu1.sing();//我爱唱歌
    stu2.sing();//我爱唱歌
    console.log(stu1.sing === stu2.sing);//false
```

很明显，stu1 和 stu2 指向的不是一个地方。 所以 在构造函数上通过this来添加方法的方式来生成实例，每次生成实例，都是新开辟一个内存空间存方法。这样会导致内存的极大浪费，从而影响性能。

##### 方法2：通过原型添加方法（共享）

构造函数通过原型分配的函数，是所有对象共享的。

```
    function Star(name) {
        this.name = name;
    }
    Star.prototype.sing = function () {
        console.log('我爱唱歌', this.name);
    };
    let stu1 = new Star('小红');
    let stu2 = new Star('小蓝');
    stu1.sing();//我爱唱歌 小红
    stu2.sing();//我爱唱歌 小蓝
    console.log(stu1.sing === stu2.sing);//true
```

#### 实例的属性为基本类型，它们是共享的吗？

属性存储的是如果存储的是基本类型，不存在共享问题，是否相同要看值内容。

```
    let stu1 = new Star('小红');
    let stu2 = new Star('小红');
    console.log(stu1.name === stu2.name);//true

    let stu1 = new Star('小红');
    let stu2 = new Star('小蓝');
    console.log(stu1.name === stu2.name);//false
```

#### 定义构造函数的规则

公共属性定义到构造函数里面，公共方法我们放到原型对象身上。



# new操作符

### 创建对象的几种方式

#### 对象字面量

```js
const obj = {
  name: 'dz',
  age: 23
}
```

这种方式死板, 灵活度不高, 每次都需要手动创建, 冗余度也很大, 适用于临时对象变量或者局部属性少的对象

#### new

**内置构造函数**

```js
const obj = new Object()
obj.name = name
obj.age = age
```

只能创建一次对象, 复用性较差, 容易创建多个相同内容的对象, 造成代码冗余

#### **工厂模式**

```js
function Person(name, age) {
  const obj = {}
  obj.name = name
  obj.age = age
  return obj
}

const person = Person('dz', 23)
const person1 = Person('dz1', 24)
console.log(person instanceof Person) // -> false
console.log(person1.__proto__ == person.__proto_) // -> false
```

对象无法识别(不能识别是被哪一个工厂函数创造的), 相同工厂产出的实例的原型 `不是同一个`

#### **构造函数**

```js
function Person(name, age) {
  this.name = name
  this.age  = age
  this.sayname = () => {
    console.log(this.name)
  }
}

const p1 = new Person('dz', 23)
const p2 = new Person('dz1', 24)
console.log(p1 instanceof Person, p2 instanceof Person)// --> true true
```

构造函数简化了工厂模式的操作过程, 并且通过实例化对象, 可以知道该对象的标识, 能识别是被哪一个 `构造函数` 创造的, 使用`instanceof` 来判断是否属于某个构造函数的实例

但是, 构造函数内部存在方法, `方法就是对象`, 就意味着每次创建对象(实例)的时候就会重新创建方法, 重复的创建方法开辟了新的内存来储存

(js高程)使用构造函数主要问题就是每个方法都要在每个实例上重新创建一遍, 每个方法作用和使用方法一样, 根本不用重复创建Function实例. 况且有this对象在, 不用在执行代码前就把函数绑定到特定的对象上面.

(js高程)可以将函数定义到 `构造函数外部` 解决问题. 这样虽然解决了重复做同一件事的问题, 但是这让一个在全局作用域的方法只能被特定的对象调用就有点让全局作用域名不其实.

#### **原型模式**

```js
function Person(name, age) {
  Person.prototype.name = name
  Person.prototype.age  = age
  Person.prototype.likes  = ['apple', 'banana', 'watermelon']
  Person.prototype.sayname = () => {
    console.log(this.name)
  }
}
const p1 = new Person('dz', 23)
const p2 = new Person('dz1', 24)

p1.likes.pop() // -> 删除 watermelon
console.log(p1.name == p2.name) // -> true,  p2的属性覆盖了p1的属性
console.log(p1.likes) // -> ['apple', 'banana']
console.log(p2.likes) // -> ['apple', 'banana']
```

创建对象之后将构造函数 `原型` 上添加属性和方法, 这样的好处就是每一个实例都共享以同一个方法, 避免了重复创建相同的方法, 但是有一个大问题就是, 大家都是共享的, 因此每一个实例都可能更改这个原型里面的属性, 后面创建的对象包含的属性会覆盖上次一创建的对象的属性

(js高程)不必在构造函数中定义对象实例的信息, 而是可以将这些信息直接添加到原型对象中, 可以是一般实例都需要属于自己的全部属性, 甚少有人但单独使用原型模式

#### **组合模式(构造函数模式+原型模式)**

```js
function Person(name, age) {
  this.name = name
  this.age  = age
}

Person.prototype.sayname = () => {
  console.log(this.name)
}

const p1 = new Person('dz', 23)
const p2 = new Person('dz1', 24)
console.log(p1.name, p2.name)// dz dz1
```

这种方式结合两者的有点, 每个实例拥有自己的属性和方法, 以及共享相同的方法, 用的较多一种模式

#### **动态原型模式**

```js
function Person(name, age) {
  this.name = name
  this.age  = age
  if(typeof this.sayname != 'function') {
    Person.prototype.sayname = () => {
      console.log(this.name)
    }
  }
}
const p1 = new Person('dz', 23)
console.log(p1.sayname) // -> dz
```

这里只在`sayname` 方法不存在的情况下才添加到原型中, 只会在`初次调用` 构造函数时才会执行.

这样的代码, 使得每个对象的name、age都是`各自的`(不共有), 然后函数写在原型上, 就又是共享的.

#### **寄生构造函数**

这种模式的基本思想是创建一个函数, 该函数的作用仅仅是封装创建对象的代码, 然后再返回新创建的对象；但从表面上看, 这个函数又很像是典型的构造函数.

```js
function SpecialArray(){
  var array = new Array();
  array.push.apply(array,arguments);
  array.toPipedString = function(){
      return this.join("|");
  };
  return array;
}
var colors = new SpecialArray("red","green","pink");
alert(colors.toPipedString());// red|green|pink
alert(colors instanceof SpecialArray); // false 
```

我们知道, 当我们自定义一个构造函数后, 使用 `new` 的方式来创建一个对象时, 默认会返回一个 `新对象` 实例, 构造函数中是没有return语句的. 而这里所谓的寄生构造函数, 基本思想是创建一个函数, 这个函数的作用仅仅是为了某一个`特定的功能`而添加一些代码, 最后再将这个对象返回.

除了使用了new操作符并把包装的函数叫做构造函数外, 这个模式跟工厂模式没有任何区别.

另外, 这个 `SpecialArray()` 返回的对象, 与 `SpecialArray()构造函数` 或者与 `构造函数的原型对象` 之间没有任何关系, 就像你在SpecialArray()外面创建的其他对象一样, 所以如果用 `instanceof` 操作符来检测的话, 结果只能是 `false` 咯. 所以这是它的问题

#### **稳妥构造函数模式**

先说稳妥二字, 别人定义了一个稳妥对象, 即没有公共属性, 而且其方法也 `不引用this对象`, 这种模式适应于一些安全环境中(禁止使用this和new), 或防止数据被其他应用程序改动, 像下面这样：

```js
function Person(name,age,gender){
  var obj = new Object();
  obj.sayName = function(){
      alert(name);
  };
  return obj;
}
var person = Person("Stan",0000,"male"); // 这里没有使用new操作符
person.sayName(); // Stan
alert(person instanceof Person); // false
```

这里 `person` 中保存了一个稳妥对象, 除了调用`sayName()`方法外, 没有别的方式可以访问其数据成员. 即使有其他代码会给这个对象添加方法或属性, 但也不可能有别的办法访问传入到构造函数中的原始数据 . 同样与寄生函数模式类似, 使用稳妥构造函数模式创建的对象与构造函数之间也没有任何关系.

#### **class(ES6)**

```js
class Person {
  constructor(name, age) { // constructor构造函数
    this.name = name
    this.age  = age
  }

  sayname() { //原型上的
    console.log(this.name)
  }
  static sayAge() {
    console.log(this.age)
  }
}

const per = new Person('dz', 23)
per.sayname() // -> dz
Person.sayAge() // 23
```

`constructor`是构造方法,类似构造函数, 定义这个方法里面的内容都是实例自身的属性和方法, 不会被其他实例共享, 而写在外面的`sayname`表示原型上的方法, 是会被`共享`的.

`static` 表示静态，加了static的函数`不会`挂载到`prototype` 上,而是挂载到 `class类` 上, 类似于:

```javascript
Promise.resolve(...)
Math.max(...)
```



### new的流程

new做了什么工作呢？

- 新建一个对象`obj`
- 把这个obj的 _ _proto__ 属性指向构造函数的 prototype 属性
- 将构造函数的`this`指向obj
- 如果该函数没有返回对象，则返回`this`

*![](pics/new流程.png)*



在内存中体现就是：

- 同样开辟一个私有作用域栈内存，形参赋值和变量提升
- **JS 代码执行之前，构造函数会在当前私有作用域内创建一个对象也就是开辟一个堆内存空间，但暂时不存储任何内容。浏览器会让函数中的主体 this 指向这个堆内存地址**
- 代码自上而下执行
- **最后代码执行结束后，浏览器会把创建的对像堆内存的对象默认返回，不需要写 `return`。返回的也就是一个实例**

### 实现一个new

```js
function create(Con, ...args) {
  let obj = {}
  Object.setPrototypeOf(obj, Con.prototype)
  let result = Con.apply(obj, args)
  return result instanceof Object ? result : obj
}

```

这就是一个完整的实现代码，我们通过以下几个步骤实现了它：

1. 首先函数接受不定量的参数，第一个参数为构造函数，接下来的参数被构造函数使用
2. 然后内部创建一个空对象 `obj`
3. 因为 `obj` 对象需要访问到构造函数原型链上的属性，所以我们通过 `setPrototypeOf` 将两者联系起来。这段代码等同于 `obj.__proto__ = Con.prototype`
4. 将 `obj` 绑定到构造函数上，并且传入剩余的参数
5. 判断构造函数返回值是否为对象，如果为对象就使用构造函数返回的值，否则使用 `obj`，这样就实现了忽略构造函数返回的原始值

接下来我们来使用下该函数，看看行为是否和 `new` 操作符一致

```js
function Test(name, age) {
  this.name = name
  this.age = age
}
Test.prototype.sayName = function () {
    console.log(this.name)
}
const a = create(Test, 'yck', 26)
console.log(a.name) // 'yck'
console.log(a.age) // 26
a.sayName() // 'yck'
```

**返回值**

构造函数执行，不写 return ，浏览器会默认返回创建的实例，但是如果我们自己写了 return 。

- 基本值： return 是一个基本值，返回的结果依然是类的实例，没有收到影响。
- 引用值：如果返回的是一个对象，则将默认的返回的实例覆盖，接收的结果就不再是当前类的实例。



#### Object.create()

```js
Object.create = (obj, properties) => {
  let F = new Function();
  F.prototype = obj; // 以 obj 作为 F 构造函数的原型对象
  if (properties) {
    Object.defineProperties(F, properties);
  }
  return new F();
}
```

`Object.create(obj, pro)` 方法将传递的参数 obj 作为原型对象，创建并返回一个新的对象 (可看作是传入参数对象的一个副本拷贝)，新创建的对象对应的原型对象为 obj。

[js继承实现之Object.create](https://segmentfault.com/a/1190000014592412)

### new 和 Object.create() 和 {}的区别

`new`操作符创建一个对象的过程

- 创建一个对象obj
- 将obj连接到原型链上，即设置`obj.__proto__ = Constructor.prototype`
- 绑定this指向，传参执行原型函数(参数应用到obj对象上)
- 判断执行结果，没有则返回obj

`Object.create`创建一个对象的过程

- 创造一个空构造函数
- 将空的构造函数prototype设为传入的prototype
- 返回实例

1、new的话只能是class（即函数），但是Object.create()的参数可以为对象也可以为函数，如果Object.create()的参数是对象的话，那么新的对象会继承原对象的属性；如果参数是类（函数）的话，它没有绑定this，**并没有属性被继承**。

2、`new`和`Object.create`创建的的实例都具备`prototype`的属性和参数，但是`new`创建的实例执行了`原来的构造函数A`使新对象具备了`原构造函数A自身的属性`，而`Object.create`先创建一个`空构造函数F`，再进行`new F()`操作，`原构造函数A`的自身属性不被继承.

3、Object.create(null)可以实现一个空对象，即没有原型的对象，但使用new就办不到。当我们需要一个干净且可定制的对象时, 考虑采用 `Object.create(null)` 创建空对象, 该对象不继承 `Object.prototype` 的任何属性和方法;

字面量和new一样

[setPrototypeOf 与 Object.create区别](https://juejin.cn/post/6844903527941144589)

# 常用八种继承方案

### **原型链继承**

- 核心思想：**将父类的实例作为子类的原型。**

- 优点：方法复用

  ​	由于方法定义在父类的原型上，复用了父类构造函数原型上的方法。

- 缺点：

  ​	创建的子类实例**不能传参**。

  ​	子类实例**共享了**父类构造函数的**引用类型属性**（如：arr）

- 但是如果**不是引用类型**就不存在这种问题，每创建一个实例，都会为每个实例的**基本数据类型**重新分配一个内存空间，相互之间不干扰。

*![](pics/原型链继承.png)*

### **借用构造函数继承**

- 核心思想：**使用父类的构造函数来增强子类实例**，等同于复制父类的实例给子类（不使用原型）

- 优点：

  ​	借用构造函数实现继承解决了原型链继承的 2 个问题：引用类型共享问题以及传参问题。

- 缺点：


​			只能继承父类的**实例**属性和方法，不能继承**原型**属性/方法

​			无法实现复用，每个子类都有父类实例函数的副本，影响性能

```js
function Animal(name) {
    this.name = name
    this.getName = function() {
        return this.name
    }
}
function Dog(name) {
    Animal.call(this, name)//核心代码是`SuperType.call(this)`，创建子类实例时调用`SuperType`构造函数，于是`SubType`的每个实例都会将SuperType中的属性复制一份
}
```



### **组合继承**

组合上述两种方法就是组合继承。用**原型链实现对原型属性和方法的继承，用借用构造函数技术来实现实例属性的继承。**

```js
function SuperType(name){
  this.name = name;
  this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function(){
  alert(this.name);
};

function SubType(name, age){
  // 继承属性
  // 第二次调用SuperType()
  SuperType.call(this, name);
  this.age = age;
}

// 继承方法
// 构建原型链
// 第一次调用SuperType()
SubType.pro	totype = new SuperType(); 
// 重写SubType.prototype的constructor属性，指向自己的构造函数SubType
SubType.prototype.constructor = SubType; 
SubType.prototype.sayAge = function(){
    alert(this.age);
};

var instance1 = new SubType("Nicholas", 29);
instance1.colors.push("black");
alert(instance1.colors); //"red,blue,green,black"
instance1.sayName(); //"Nicholas";
instance1.sayAge(); //29

var instance2 = new SubType("Greg", 27);
alert(instance2.colors); //"red,blue,green"
instance2.sayName(); //"Greg";
instance2.sayAge(); //27

```

*![](pics/组合继承.png)*

缺点：

- 第一次调用`SuperType()`：给`SubType.prototype`写入两个属性name，color。
- 第二次调用`SuperType()`：给`instance1`写入两个属性name，color。

实例对象`instance1`上的两个属性就屏蔽了其原型对象SubType.prototype的两个同名属性。所以，组合模式的缺点就是在使用子类创建实例对象时，其原型中会存在两份相同的属性/方法。

### 原型式继承

```js
function object(obj){
  function F(){}
  F.prototype = obj;
  return new F();
}
```

object()对传入其中的对象执行了一次`浅复制`，将构造函数F的原型直接指向传入的对象；和`Object.create()`方法一样

```js
var person = {
  name: "Nicholas",
  friends: ["Shelby", "Court", "Van"]
};

var anotherPerson = object(person);
anotherPerson.name = "Greg";
anotherPerson.friends.push("Rob");

var yetAnotherPerson = object(person);
yetAnotherPerson.name = "Linda";
yetAnotherPerson.friends.push("Barbie");

alert(person.friends);   //"Shelby,Court,Van,Rob,Barbie"

```

缺点：

- 原型链继承多个实例的引用类型属性指向相同，存在篡改的可能。
- 无法传递参数

另外，ES5中存在`Object.create()`的方法，能够代替上面的object方法。

### 寄生式继承

核心：在**原型式继承的基础**上，增强对象，返回构造函数

```js
function createAnother(original){
  var clone = object(original); // 通过调用 object() 函数创建一个新对象
  clone.sayHi = function(){  // 以某种方式来增强对象
    alert("hi");
  };
  return clone; // 返回这个对象
}
```

函数的主要作用是为构造函数新增属性和方法，以**增强函数**

```js
var person = {
  name: "Nicholas",
  friends: ["Shelby", "Court", "Van"]
};
var anotherPerson = createAnother(person);
anotherPerson.sayHi(); //"hi"

```

缺点（同原型式继承）：

- 原型链继承多个实例的引用类型属性指向相同，存在篡改的可能。
- 无法传递参数

### 寄生组合式继承（现在库使用的方法）

结合借用**构造函数传递参数和寄生模式**实现继承

```js
function inheritPrototype(subType, superType){
  var prototype = Object.create(superType.prototype); // 创建对象，创建父类原型的一个副本
  prototype.constructor = subType;                    // 增强对象，弥补因重写原型而失去的默认的constructor 属性
  subType.prototype = prototype;                      // 指定对象，将新创建的对象赋值给子类的原型
}

// 父类初始化实例属性和原型属性
function SuperType(name){
  this.name = name;
  this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function(){
  alert(this.name);
};

// 借用构造函数传递增强子类实例属性（支持传参和避免篡改）
function SubType(name, age){
  SuperType.call(this, name);
  this.age = age;
}

// 将父类原型指向子类
inheritPrototype(SubType, SuperType);

// 新增子类原型属性
SubType.prototype.sayAge = function(){
  alert(this.age);
}

var instance1 = new SubType("xyc", 23);
var instance2 = new SubType("lxy", 23);

instance1.colors.push("2"); // ["red", "blue", "green", "2"]
instance1.colors.push("3"); // ["red", "blue", "green", "3"]

```

*![](pics/寄生组合式继承.png)*

这个例子的高效率体现在它只调用了一次`SuperType` 构造函数，并且因此避免了在`SubType.prototype` 上创建不必要的、多余的属性。于此同时，原型链还能保持不变；因此，还能够正常使用`instanceof` 和`isPrototypeOf()`

**这是最成熟的方法，也是现在库实现的方法**

### 混入方式继承多个对象

```js
function MyClass() {
     SuperClass.call(this);
     OtherSuperClass.call(this);
}

// 继承一个类
MyClass.prototype = Object.create(SuperClass.prototype);
// 混合其它
Object.assign(MyClass.prototype, OtherSuperClass.prototype);
// 重新指定constructor
MyClass.prototype.constructor = MyClass;

MyClass.prototype.myMethod = function() {
     // do something
};

```

`Object.assign`会把 `OtherSuperClass`原型上的函数拷贝到 `MyClass`原型上，使 MyClass 的所有实例都可用 OtherSuperClass 的方法。

### ES6类class和extends/和es5的区别

`ES6` 的`class`可以看作只是一个`ES5`生成实例对象的构造函数的语法糖。它参考了`java`语言，定义了一个类的概念，让对象原型写法更加清晰，对象实例化更像是一种面向对象编程。`Class`类可以通过`extends`实现继承。它和ES5构造函数的不同点

- `ES6`的`class`类**必须用new命令操作**，而`ES5`的构造函数不用new也可以执行。
- `ES6`的`class`类**不存在变量提升**，必须先定义`class`之后才能实例化，不像`ES5`中可以将构造函数写在实例化之后。
- `ES5` 的继承，实质是先创造子类的实例对象`this`，然后再将父类的方法添加到`this`上面。`ES6` 的**继承机制完全不同**，实质是先将父类实例对象的属性和方法，加到`this`上面（所以必须先调用`super`方法），然后再用子类的构造函数修改`this`。
- class类有static静态方法。static静态方法只能通过类调用，不会出现在实例上；另外如果静态方法包含 this 关键字，这个 this 指的是类，而不是实例。static声明的静态属性和方法都可以被子类继承。



# this,bind,call,apply

**this解决的问题:**

this提供了一种更优雅的方法来**隐式'传递'一个对象的引用**，因此可以**将API设计得更加简洁并且易于复用**。

### this的指向

在 ES5 中，其实 this 的指向，始终坚持一个原理：**this 永远指向最后调用它的那个对象**，记住这句话，this 你已经了解一半了。

例 1：

```js
var name = "windowsName";
function a() {
    var name = "Cherry";

    console.log(this.name);          // windowsName

    console.log("inner:" + this);    // inner: Window
}
a();
console.log("outer:" + this)         // outer: Window
```

这个相信大家都知道为什么 log 的是 windowsName，因为根据刚刚的那句话“**this 永远指向最后调用它的那个对象**”，我们看最后调用 `a` 的地方 `a();`，前面没有调用的对象那么就是全局对象 window，这就相当于是 `window.a()`；注意，这里我们没有使用严格模式，如果使用严格模式的话，全局对象就是 `undefined`，那么就会报错 `Uncaught TypeError: Cannot read property 'name' of undefined`。

**this指向window对象，为window绑定**

例 2：

```js
var name = "windowsName";
var a = {
    name: "Cherry",
    fn : function () {
        console.log(this.name);      // Cherry
    }
}
a.fn();
```

在这个例子中，函数 fn 是对象 a 调用的，所以打印的值就是 a 中的 name 的值。

**此处为隐式绑定**

例 3：

```js
var name = "windowsName";
    var a = {
        name: "Cherry",
        fn : function () {
            console.log(this.name);      // Cherry
        }
    }
    window.a.fn();
```

这里打印 Cherry 的原因也是因为刚刚那句话“**this 永远指向最后调用它的那个对象**”，最后调用它的对象仍然是对象 a。

我们再来看一下这个例子：

```js
    var name = "windowsName";
    var a = {
        // name: "Cherry",
        fn : function () {
            console.log(this.name);      // undefined
        }
    }
    window.a.fn();
```

这里为什么会打印 `undefined` 呢？这是因为正如刚刚所描述的那样，调用 fn 的是 a 对象，也就是说 fn 的内部的 this 是对象 a，而对象 a 中并没有对 name 进行定义，所以 log 的 `this.name` 的值是 `undefined`。

这个例子还是说明了：**this 永远指向最后调用它的那个对象**，因为最后调用 fn 的对象是 a，所以就算 a 中没有 name 这个属性，也不会继续向上一个对象寻找 `this.name`，而是直接输出 `undefined`。

再来看一个比较坑的例子：
例 5：

```js
    var name = "windowsName";
    var a = {
        name : null,
        // name: "Cherry",
        fn : function () {
            console.log(this.name);      // windowsName
        }
    }

    var f = a.fn;
    f();
```

这里你可能会有疑问，为什么不是 `Cherry`，这是因为虽然将 a 对象的 fn 方法赋值给变量 f 了，但是没有调用，再接着跟我念这一句话：“**this 永远指向最后调用它的那个对象**”，由于刚刚的 f 并没有调用，所以 `fn()` 最后仍然是被 window 调用的。所以 this 指向的也就是 window。

由以上五个例子我们可以看出，this 的指向并不是在创建的时候就可以确定的，在 es5 中，永远是**this 永远指向最后调用它的那个对象**。

```js
  var name = "windowsName";

    function fn() {
        var name = 'Cherry';
        innerFunction();
        function innerFunction() {
            console.log(this.name);      // windowsName
        }
    }

    fn()
```

### 绑定的优先级

如果显示绑定和new绑定同时存在，或者更宽泛的说：**在某个调用位置多条绑定规则同时存在怎么办呢**？为了解决这个问题就必须给这些规则设定优先级，

绑定优先级顺序为：`new > 显式 > 隐式 > Window`。

于是我们判断this，就有了一个顺序：

1. 函数是否在new中调用？
2. 是否通过call、apply、bind等调用？
3. 是否在某个上下文对象中调用？
4. 都不是则是Window绑定。且严格模式下绑定到undefined

### 改变 this 的指向

改变 this 的指向我总结有以下几种方法：

- 使用 ES6 的箭头函数
- 在函数内部使用 `_this = this`
- 使用 `apply`、`call`、`bind`，为**显式绑定**
- new 实例化一个对象，为**new绑定**

例 7：

```js
    var name = "windowsName";

    var a = {
        name : "Cherry",

        func1: function () {
            console.log(this.name)     
        },

        func2: function () {
            setTimeout(  function () {
                this.func1()
            },100);
        }

    };

    a.func2()     // this.func1 is not a function
```

在不使用箭头函数的情况下，是会报错的，因为最后调用 `setTimeout` 的对象是 window，但是在 window 中并没有 func1 函数。

#### 箭头函数

众所周知，ES6 的箭头函数是可以避免 ES5 中使用 this 的坑的。**箭头函数的 this 始终指向函数定义时的 this，而非执行时。**，箭头函数需要记着这句话：“**箭头函数中没有 this 绑定，必须通过查找作用域链来决定其值，如果箭头函数被非箭头函数包含，则 this 绑定的是最近一层非箭头函数的 this，否则，this 为 undefined**”。

例 8 ：

```js
    var name = "windowsName";

    var a = {
        name : "Cherry",

        func1: function () {
            console.log(this.name)     
        },

        func2: function () {
            setTimeout( () => {
                this.func1()
            },100);
        }

    };

    a.func2()     // Cherry
```

关于箭头函数，我们还需要注意以下几点：

1. 函数体内的this就是定义时所在的对象，而非调用时所在的对象，和普通函数相反。
2. 箭头函数无法用做构造函数，即不能使用new调用
3. 不能使用arguments对象，函数中不存在这个对象。
4. 不可使用yield命令，即无法用做Generator函数。

其中第一点尤其值得注意，之所以this是固定的，是因为箭头函数本身没有this，箭头函数的this不是自己的。所以不能修改，也正因为没有this，所以不能用作构造函数。这些限制都是因为没有this导致的。

#### 在函数内部使用 `_this = this`

如果不使用 ES6，那么这种方式应该是最简单的不会出错的方式了，我们是先将调用这个**函数的对象保存在变量 `_this` 中**，然后在函数中都使用这个 `_this`，这样 `_this` 就不会改变了。

例 9：

```js
    var name = "windowsName";

    var a = {

        name : "Cherry",

        func1: function () {
            console.log(this.name)     
        },

        func2: function () {
            var _this = this;
            setTimeout( function() {
                _this.func1()
            },100);
        }

    };

    a.func2()       // Cherry
```

这个例子中，在 func2 中，首先设置 `var _this = this;`，这里的 `this` 是调用 `func2` 的对象 a，为了防止在 `func2` 中的 setTimeout 被 window 调用而导致的在 setTimeout 中的 this 为 window。我们将 `this(指向变量 a)` 赋值给一个变量 `_this`，这样，在 `func2` 中我们使用 `_this` 就是指向对象 a 了。

#### 使用call、apply、bind

#### 使用new

### call、apply、bind区别

#### **语法**

```js
fun.call(thisArg, param1, param2, ...)
fun.apply(thisArg, [param1,param2,...])
fun.bind(thisArg, param1, param2, ...)
```

**返回值：**

call/apply：`fun`执行的结果  bind：返回`fun`的拷贝，并拥有指定的`this`值和初始参数

#### **参数**

**调用`call`/`apply`/`bind`的必须是个函数**

传给`fun`的参数写法不同：

- `call`从第2~n的参数都是传给`fun`的。

- `apply`是 第2个参数，这个参数是一个数组：传给`fun`参数都写在数组中。

- call/apply与bind的区别

  

#### **执行**：

- call/apply改变了函数的this上下文后马上**执行该函数**
- bind则是返回改变了上下文后的函数,**不执行该函数**

#### **返回值**:

- call/apply 返回fun的执行结果
- bind返回fun的拷贝，并**指定了fun的this指向，保存了fun的参数。**

返回值这段在下方bind应用中有详细的示例解析。

```js
function fn(a, b) {
    console.log(this, a, b)
}

var obj = {
    name: '林一一'
}

fn.call(obj, 20, 23)   // {name: "林一一"} 20 23

fn.call(20, 23) // Number {20} 23 undefined

fn.call()   //Window {0: global, window: …} undefined undefined     | 严格模式下为 undefined

fn.call(null)   //Window {0: global, window: …} undefined undefined       | 严格模式下为 null

fn.call(undefined)  //Window {0: global, window: …} undefined undefined     | 严格模式下为 undefined
```

`fn`调用了`call`，`fn` 的 `this` 指向 `obj`，最后 `fn` 被执行；`this` 指向的值都是引用类型，在非严格模式下，不传参数或传递 `null/undefined`，`this` 都指向 `window`。传递的是原始值，原始值会被包装。严格模式下，`call` 的一个参数是谁就指向谁

```js
var obj1 = {
    a: 10,
    fn: function(x) {
        console.log(this.a + x)
    }
}

var obj2 = {
    a : 20,
    fn: function(x) {
        console.log(this.a - x)
    }
}

obj1.fn.call(obj2, 20) //40
```

稍微变量一下，原理不变`obj1.fn` 中 `fn`的 `this` 指向到 `obj2`，最后还是执行 `obj1.fn` 中的函数。

以上就是三种显示绑定的方法，但有三点需要注意：

1. call和apply是立即执行，bind则是返回一个绑定了this的新函数，只有你调用了这个新函数才真的调用了目标函数
2. bind函数存在多次绑定的问题，如果多次绑定this，则以**第一次为准**。绑定后的函数this无法改变，即使call/apply也不行
3. bind函数实际上是显示绑定（call、apply）的一个变种，称为**硬绑定**。由于硬绑定是一种非常常用的模式，所以在 ES5 中提供了内置的方法`Function.prototype.bind`

# 闭包、柯里化、内存泄漏、防抖节流、单例模式

```js
var n = 10
function fn(){
    var n =20
    function f() {
       n++;
       console.log(n)
    }
    f()
    return f
}

var x = fn()
x()
x()
console.log(n)
/* 输出
*  21
    22
    23
    10
/
```

> ****思路：`fn` 的返回值是什么变量 `x` 就是什么，这里 `fn` 的返回值是函数名 `f` 也就是 `f` 的堆内存地址，`x()` 也就是执行的是函数 `f()`，而不是 `fn()`，输出的结果显而易见****

**JS 堆栈内存释放**

堆内存：存储引用类型值，对象类型就是键值对，函数就是代码字符串。

堆内存释放：将引用类型的空间地址变量赋值成 `null`，或没有变量占用堆内存了浏览器就会释放掉这个地址

栈内存：提供代码执行的环境和存储基本类型值。

栈内存释放：一般当函数执行完后函数的私有作用域就会被释放掉。

> **但栈内存的释放也有特殊情况：① 函数执行完，但是函数的私有作用域内有内容被栈外的变量还在使用的，栈内存就不能释放里面的基本值也就不会被释放。② 全局下的栈内存只有页面被关闭的时候才会被释放**

### 闭包定义

> **闭包是指有权访问另一个函数作用域中变量的函数**，**通常在嵌套函数中实现**。

### 形成闭包的原因

> **内部的函数存在外部作用域的引用就会导致闭包**。从上面介绍的上级作用域的概念中其实就有闭包的例子 `return f`就是一个表现形式。

```js
var a = 0
function foo(){
    var b =14
    function fo(){
        console.log(a, b)
    }
    fo()
}
foo()
//这里的子函数 fo 内存就存在外部作用域的引用 a, b，所以这就会产生闭包
```

### 闭包变量存储的位置

> 直接说明：**闭包中的变量存储的位置是堆内存。**
>
> 假如闭包中的变量存储在栈内存中，那么栈的回收 会把处于栈顶的变量自动回收。所以闭包中的变量如果处于栈中那么变量被销毁后，闭包中的变量就没有了。所以闭包引用的变量是出于堆内存中的。

### 闭包的作用

- **保护**函数的私有变量不受外部的干扰。形成不销毁的栈内存。
- **保存**，把一些函数内的值保存下来。闭包可以实现方法和属性的私有化。

### 闭包经典使用场景

1. `return` 回一个函数

   ```js
   var n = 10
   function fn(){
       var n =20
       function f() {
          n++;
          console.log(n)
        }
       return f
   }
   
   var x = fn()
   x() // 21
   //这里的 return f, f()就是一个闭包，存在上级作用域的引用。
   ```

2. **函数作为参数**

   ```js
   var a = '林一一'
   function foo(){
       var a = 'foo'
       function fo(){
           console.log(a)
       }
       return fo
   }
   
   function f(p){
       var a = 'f'
       p()
   }
   f(foo())
   /* 输出
   *   foo
   / 
   //使用 return fo 返回回来，fo() 就是闭包，f(foo()) 执行的参数就是函数 fo，因为 fo() 中的 a 的上级作用域就是函数foo()，所以输出就是foo
   ```

3. IIFE（自执行函数）

   ```js
   var n = '林一一';
   (function p(){
       console.log(n)
   })()
   /* 输出
   *   林一一
   / 
   //同样也是产生了闭包p()，存在 window下的引用 n。
   ```

4. 循环赋值

   ```js
   for(var i = 0; i<10; i++){
     (function(j){
          setTimeout(function(){
           console.log(j)
       }, 1000) 
     })(i)
   }
   //因为存在闭包的原因上面能依次输出1~10，闭包形成了10个互不干扰的私有作用域。将外层的自执行函数去掉后就不存在外部作用域的引用了，输出的结果就是连续的 10。为什么会连续输出10，因为 JS 是单线程的遇到异步的代码不会先执行(会入栈)，等到同步的代码执行完 i++ 到 10时，异步代码才开始执行此时的 i=10 输出的都是 10。
   ```

5. 使用回调函数就是在使用闭包

   ```js
   window.name = '林一一'
   setTimeout(function timeHandler(){
     console.log(window.name);
   }, 100)
   ```

6. 单例模式

   单例模式是一种常见的设计模式，它**保证了一个类只有一个实例**。实现方法一般是先判断实例是否存在，如果存在就直接返回，否则就创建了再返回。单例模式的好处就是避免了重复实例化带来的内存开销：

   ```js
   const getSingleBuider = function(fn) {
       let instance;
       return function() {
           return instance || (instance = fn.apply(this, arguments));
       }
   }
   ```

   

### 函数防抖(debounce)

> 在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时。

方式：每次触发事件时设置一个延迟调用方法，并且取消之前的延时调用方法

```js
//防抖debounce代码：
function debounce(fn) {
    let timeout = null; // 创建一个标记用来存放定时器的返回值
    return function () {
        // 每当用户输入的时候把前一个 setTimeout clear 掉
        clearTimeout(timeout); 
        // 然后又创建一个新的 setTimeout, 这样就能保证interval 间隔内如果时间持续触发，就不会执行 fn 函数
        timeout = setTimeout(() => {
            fn.apply(this, arguments);
        }, 500);
    };
}
// 处理函数
function handle() {
    console.log(Math.random());
}
// 滚动事件
window.addEventListener('scroll', debounce(handle));
```

### 函数节流(throttle)

> 规定在一个单位时间内，只能触发一次函数。如果这个单位时间内触发多次函数，只有一次生效。

实现方式：每次触发事件时，如果当前有等待执行的延时函数，则直接return

```js
//节流throttle代码：
function throttle(fn) {
    let canRun = true; // 通过闭包保存一个标记
    return function () {
         // 在函数开头判断标记是否为true，不为true则return
        if (!canRun) return;
         // 立即设置为false
        canRun = false;
        // 将外部传入的函数的执行放在setTimeout中
        setTimeout(() => { 
        // 最后在setTimeout执行完毕后再把标记设置为true(关键)表示可以执行下一次循环了。
        // 当定时器没有执行的时候标记永远是false，在开头被return掉
            fn.apply(this, arguments);
            canRun = true;
        }, 500);
    };
}

function sayHi(e) {
    console.log(e.target.innerWidth, e.target.innerHeight);
}
window.addEventListener('resize', throttle(sayHi));
```

函数节流与函数防抖都是为了限制函数的执行频次，以优化函数触发频率过高导致的响应速度跟不上触发频率，出现延迟，假死或卡顿的现象。 **总结：**
**函数防抖**：将多次操作合并为一次操作进行。原理是维护一个计时器，规定在delay时间后触发函数，但是在delay时间内再次触发的话，就会取消之前的计时器而重新设置。这样一来，只有最后一次操作能被触发。

**函数节流**：使得一定时间内只触发一次函数。原理是通过判断是否有延迟调用函数未执行。

**场景区别**： 函数节流不管事件触发有多频繁，都会保证在规定时间内一定会执行一次真正的事件处理函数，而函数防抖只是在最后一次事件后才触发一次函数。 

防抖的场景

1. 登录、发短信等按钮避免用户点击太快，以致于发送了多次请求，需要防抖
2. 调整浏览器窗口大小时，resize 次数过于频繁，造成计算过多，此时需要一次到位，就用到了防抖
3. 文本编辑器实时保存，当无任何更改操作一秒后进行保存

节流的场景

1. `scroll` 事件，每隔一秒计算一次位置信息等
2. 浏览器播放事件，每个一秒计算一次进度信息等
3. input 框实时搜索并发送请求展示下拉列表，每隔一秒发送一次请求 (也可做防抖)

### 柯里化

#### 高阶函数（HOF）

高阶函数（`Higher-Order Function`）是至少满足如下条件之一的函数：

1. 函数可以作为参数被传递
2. 函数可以作为返回值输出

在JavaScript中常见于回调函数则是作为了参数被传递，闭包则是返回了函数

#### 函数柯里化

函数柯里化指的是**将能够接收多个参数的函数转化为接收单一参数的函数**，并且返回接收余下参数且返回结果的新函数的技术。

函数柯里化的主要作用和特点就是参数复用、提前返回和延迟执行。

**代码实现**

```js
const curry = function(fn) {
  return function inner() {
    // 浅拷贝入参
    const args = Array.prototype.slice.call(arguments);
    // 如果下一个参数的长度大于了函数的行参个数，则跳出递归
    if (arguments.length >= fn.length) {
      return fn.apply(undefined, args);
    } else {
      // 否则继续处理后续参数，返回curring函数
      return function() {
        // 获取合并上一次和下一次的入参
        const allArgs = args.concat(Array.prototype.slice.call(arguments));
        return inner.apply(undefined, allArgs);
      };
    }
  };
}

function sum(a, b, c) {
    return a + b + c;
}

const currySum = curry(sum);
console.log(currySum(1)(2)(3)); //6
```

**ES6实现**

```js
const curry = fn =>
  judge = (...args) =>
    args.length >= fn.length 
      ? fn(...args) 
      : arg => judge(...args, arg);

```

另一种实现

```js
function curry(fn, len = fn.length) {
    return _curry(fn, len)
}

function _curry(fn, len, ...arg) {
    return function (...params) {
        let _arg = [...arg, ...params]
        if (_arg.length >= len) {
            return fn.apply(this, _arg)
        } else {
            return _curry.call(this, fn, len, ..._arg)
        }
    }
}

let fn = curry(function (a, b, c, d, e) {
    console.log(a + b + c + d + e)
})

fn(1, 2, 3, 4, 5)  // 15
fn(1, 2)(3, 4, 5)
fn(1, 2)(3)(4)(5)
fn(1)(2)(3)(4)(5)
```

`javascript` 中常见的 `bind` 方法就可以用柯里化的方法来实现：

```js
Function.prototype.myBind = function (context = window) {
    if (typeof this !== 'function') throw new Error('Error');
    let selfFunc = this;
    let args = [...arguments].slice(1);
    
    return function F () {
        // 因为返回了一个函数，可以 new F()，所以需要判断
        if (this instanceof F) {
            return new selfFunc(...args, arguments);
        } else  {
            // bind 可以实现类似这样的代码 f.bind(obj, 1)(2)，所以需要将两边的参数拼接起来
            return selfFunc.apply(context, args.concat(arguments));
        }
    }
}
```


### 使用闭包需要注意什么

> ```
> 容易导致内存泄漏。闭包会携带包含其它的函数作用域，因此会比其他函数占用更多的内存。过度使用闭包会导致内存占用过多，所以要谨慎使用闭包。
> ```

### **内存泄漏**

#### **定义**

> 内存泄漏（Memory leak）是在计算机科学中，由于疏忽或错误造成程序未能释放已经不再使用的内存。并非指内存在物理上的消失，而是应用程序分配某段内存后，由于设计错误，导致在释放该段内存之前就失去了对该段内存的控制，从而造成了内存的浪费

#### 引起内存泄漏的情况

##### **意外的全局变量**

全局变量的生命周期最长，直到页面关闭前，它都存活着，所以全局变量上的内存一直都不会被回收

当全局变量使用不当，没有及时回收（手动赋值 null），或者拼写错误等将某个变量挂载到全局变量时，也就发生内存泄漏了

##### **遗忘的定时器**

**setTimeout 和 setInterval 是由浏览器专门线程来维护它的生命周期，所以当在某个页面使用了定时器，当该页面销毁时，没有手动去释放清理这些定时器的话，那么这些定时器还是存活着的**

也就是说，定时器的生命周期并不挂靠在页面上，所以当在当前页面的 js 里通过定时器注册了某个回调函数，**而该回调函数内又持有当前页面某个变量或某些 DOM 元素时，就会导致即使页面销毁了，由于定时器持有该页面部分引用而造成页面无法正常被回收，从而导致内存泄漏了**

如果此时再次打开同个页面，内存中其实是有双份页面数据的，如果多次关闭、打开，那么内存泄漏会越来越严重

而且这种场景很容易出现，因为使用定时器的人很容易遗忘清除

所以，当不需要 `interval` 或者 `timeout` 时，最好调用 `clearInterval` 或者 `clearTimeout`来清除，另外，浏览器中的 `requestAnimationFrame` 也存在这个问题，我们需要在不需要的时候用 `cancelAnimationFrame` API 来取消使用。

##### 使用不当的闭包

函数本身会持有它定义时所在的词法环境的引用，但通常情况下，使用完函数后，该函数所申请的内存都会被回收了

但当函数内再返回一个函数时，由于返回的函数持有外部函数的词法环境，而返回的函数又被其他生命周期东西所持有，导致外部函数虽然执行完了，但内存却无法被回收

所以，返回的函数，它的生命周期应尽量不宜过长，方便该闭包能够及时被回收

正常来说，闭包并不是内存泄漏，因为这种持有外部函数词法环境本就是闭包的特性，就是为了让这块内存不被回收，因为可能在未来还需要用到，但这无疑会造成内存的消耗，所以，不宜烂用就是了。

##### 遗漏的 DOM 元素

DOM 元素的生命周期正常是取决于是否挂载在 DOM 树上，当从 DOM 树上移除时，也就可以被销毁回收了

但如果**某个 DOM 元素，在 js 中也持有它的引用时，那么它的生命周期就由 js 和是否在 DOM 树上两者决定了，记得移除时，两个地方都需要去清理才能正常回收它**

还有 `DOM` 的事件绑定，移除 `DOM` 元素前如果忘记了注销掉其中绑定的事件方法，也会造成内存泄露：

```js
const wrapDOM = document.getElementById('wrap');
wrapDOM.onclick = function (e) {console.log(e);};

// some codes ...

// remove wrapDOM
wrapDOM.parentNode.removeChild(wrapDOM);
```

##### 网络回调

某些场景中，在某个页面发起网络请求，并注册一个回调，且回调函数内持有该页面某些内容，那么，当该页面销毁时，应该注销网络的回调，否则，因为网络持有页面部分内容，也会导致页面部分内容无法被回收。

##### **遗忘的事件监听器**

当事件监听器在组件内挂载相关的事件处理函数，而在组件销毁时不主动将其清除时，其中引用的变量或者函数都被认为是需要的而不会进行回收，如果内部引用的变量存储了大量数据，可能会引起页面占用内存过高，这样就造成意外的内存泄漏。

我们就拿 Vue 组件来举例子，React 里也是一样的：

```html
<template>
  <div></div>
</template>

<script>
export default {
  created() {
    window.addEventListener("resize", this.doSomething)
  },
  beforeDestroy(){
    window.removeEventListener("resize", this.doSomething)
  },
  methods: {
    doSomething() {
      // do something
    }
  }
}
</script>
```

##### **遗忘的监听者模式**

监听者模式想必我们都知道，不管是 Vue 、 React 亦或是其他，对于目前的前端开发框架来说，监听者模式实现一些消息通信都是非常常见的，比如 `EventBus`. . .

当我们实现了监听者模式并在组件内挂载相关的事件处理函数，而在组件销毁时不主动将其清除时，其中引用的变量或者函数都被认为是需要的而不会进行回收，如果内部引用的变量存储了大量数据，可能会引起页面占用内存过高，这样也会造成意外的内存泄漏。

还是用 Vue 组件举例子，因为比较简单：

```html
<template>
  <div></div>
</template>

<script>
export default {
  created() {
    eventBus.on("test", this.doSomething)
  },
  beforeDestroy(){
    eventBus.off("test", this.doSomething)
  },
  methods: {
    doSomething() {
      // do something
    }
  }
}
</script>
```

如上，我们只需在 `beforeDestroy` 组件销毁生命周期里将其清除即可。

##### **遗忘的Map、Set对象**

当使用 `Map` 或 `Set` 存储对象时，同 `Object` 一致都是**强引用**，如果不将其主动清除引用，其同样会造成内存不自动进行回收。

如果使用 `Map` ，对于键为对象的情况，可以采用 `WeakMap`，`WeakMap` 对象同样用来保存键值对，对于键是弱引用（注：`WeakMap` 只对于键是弱引用），且必须为一个对象，而值可以是任意的对象或者原始值，由于是对于对象的弱引用，不会干扰 `Js` 的垃圾回收。

如果需要使用 `Set` 引用对象，可以采用 `WeakSet`，`WeakSet` 对象允许存储对象弱引用的唯一值，`WeakSet` 对象中的值同样不会重复，且只能保存对象的弱引用，同样由于是对于对象的弱引用，不会干扰 `Js` 的垃圾回收。

这里可能需要简单介绍下，谈弱引用，我们先来说强引用，之前我们说 JS 的**垃圾回收机制**是如果我们持有对一个对象的引用，那么这个对象就不会被垃圾回收，这里的引用，指的就是 `强引用` ，而**弱引用就是一个对象若只被弱引用所引用，则被认为是不可访问（或弱可访问）的，因此可能在任何时刻被回收**。**

不明白？来看例子就晓得了：

```js
// obj是一个强引用，对象存于内存，可用
let obj = {id: 1}

// 重写obj引用
obj = null 
// 对象从内存移除，回收 {id: 1} 对象
```

上面是一个简单的通过重写引用来清除对象引用，使其可回收。

再看下面这个：

```js
let obj = {id: 1}
let user = {info: obj}
let set = new Set([obj])
let map = new Map([[obj, 'hahaha']])

// 重写obj
obj = null 

console.log(user.info) // {id: 1}
console.log(set)
console.log(map)
```

此例我们重写 `obj` 以后，`{id: 1}` 依然会存在于内存中，因为 `user` 对象以及后面的 `set/map` 都强引用了它，Set/Map、对象、数组对象等都是强引用，所以我们仍然可以获取到 `{id: 1}` ，我们想要清除那就只能重写所有引用将其置空了。

接下来我们看 `WeakMap` 以及 `WeakSet`：

```js
let obj = {id: 1}
let weakSet = new WeakSet([obj])
let weakMap = new WeakMap([[obj, 'hahaha']])

// 重写obj引用
obj = null

// {id: 1} 将在下一次 GC 中从内存中删除
```

如上所示，使用了 `WeakMap` 以及 `WeakSet` 即为弱引用，将 `obj` 引用置为 `null` 后，对象 `{id: 1}` 将在下一次 GC 中被清理出内存。

##### **未清理的console输出**

写代码的过程中，肯定避免不了一些输出，在一些小团队中可能项目上线也不清理这些 `console`，殊不知这些 `console` 也是隐患，同时也是容易被忽略的，我们之所以在控制台能看到数据输出，是因为浏览器保存了我们输出对象的信息数据引用，也正是因此未清理的 `console` 如果输出了对象也会造成内存泄漏。

#### 怎么检查内存泄露

- F12浏览器工具中的performance 面板 和 memory 面板可以找到泄露的现象和位置

#### 浏览器解决方法：

垃圾回收机制

### 闭包经典面试题

```js
var data = [];
for (var i = 0; i < 3; i++) {
  data[i] = function () {
    console.log(i);
  };
}

data[0]();
data[1]();
data[2]()
/* 输出
    3
    3
    3
/
```

> 这里的 `i` 是全局下的 `i`，共用一个作用域，当函数被执行的时候这时的 `i=3`，导致输出的结构都是3。

```js
//使用var声明for循环中的迭代变量
for(var i = 0;i < 5;++ i){
	setTimeout(()=>console.log(i),0);
}
```

> for循环是同步任务，setTimeout是异步任务中的宏任务，根据js的事件循环机制，js会执行完所有的同步任务再去执行异步任务。当5次循环结束后，i的值为5，i渗透到for循环外部，这时候同步任务都执行完毕了，开始执行定时器宏任务，因此输出5次均为5。

解决方法1：使用let

let声明的迭代变量i不能渗透到for循环外面，因此可以认为是，声明了5个块级作用域，在每个块级作用域里面各自去执行自己的代码，互不干扰。类似以下代码：

```javascript
{let i = 0;setTimeout(()=>console.log(i),0);}
{let i = 1;setTimeout(()=>console.log(i),0);}
{let i = 2;setTimeout(()=>console.log(i),0);}
{let i = 3;setTimeout(()=>console.log(i),0);}
{let i = 4;setTimeout(()=>console.log(i),0);}
{let i = 5;}
```

解决方法2：自执行函数和闭包

```js
var data = [];

for (var i = 0; i < 3; i++) {
    (function(j){
      setTimeout( data[j] = function () {
        console.log(j);
      }, 0)
    })(i)
}

data[0]();
data[1]();
data[2]()
```

函数执行完毕，上下文被销毁，但是在自执行函数我们使用了（外层）函数内的i变量，因此在（外层）函数执行完毕出栈后，i不会消失，仍有机会被外界访问到。

解决方法3：借用变量

```js
    for (var i = 0; i < 3; i++) {
        let j = i
        setTimeout(() => {
            console.log(j)
        },0)
    }
```

解决方法4：给定时器传入第三个参数

```js
for(var i=1;i<=5;i++){
    setTimeout(function timer(j){
        console.log(j)
    }, 0, i)
}

arg1, ..., argN 可选
附加参数，一旦定时器到期，它们会作为参数传递给function
```



# 垃圾回收

### 概念

`GC` 即 `Garbage Collection` ，程序工作过程中会产生很多 `垃圾`，这些**垃圾是程序不用的内存或者是之前用过了，以后不会再用的内存空间，**而 `GC` 就是负责回收垃圾的，因为他工作在引擎内部，所以对于我们前端来说，`GC` 过程是相对比较无感的，这一套引擎执行而对我们又相对无感的操作也就是常说的 `垃圾回收机制` 了

### 可达性

JavaScript 中内存管理的主要概念是可达性。

简单地说，**“可达性” 值就是那些以某种方式可访问或可用的值**，它们被保证存储在内存中。

**1. 有一组基本的固有可达值，由于显而易见的原因无法删除。例如:**

- 本地函数的局部变量和参数
- 当前嵌套调用链上的其他函数的变量和参数
- 全局变量
- 还有一些其他的，内部的

**这些值称为根。**

**2. 如果引用或引用链可以从根访问任何其他值，则认为该值是可访问的。**

例如，如果局部变量中有对象，并且该对象具有引用另一个对象的属性，则该对象被视为**可达性**， 它引用的那些也是可以访问的，详细的例子如下。

JavaScript 引擎中有一个后台进程称为[垃圾回收器](https://link.segmentfault.com/?enc=oacf9nqaahoxkw9sIAeKcw%3D%3D.w6cXsKFVJX7ldc8qs8n%2F10i2NTudcOP3T0spgYtM8CJw9HF5YWx0Y1QGHvPkgBDrXRQooaPtN254zorhhq8hpeyoLA0JenJ%2BdRFLhcjjWao%3D)，它监视所有对象，并删除那些不可访问的对象。



### 垃圾回收策略

我们都可以 Get 到这之中的重点，那就是怎样找出所谓的垃圾？

这个流程就涉及到了一些算法策略，有很多种方式，我们简单介绍两个最常见的

- 标记清除算法
- 引用计数算法

**标记清除法步骤**：

- 垃圾回收器**获取根**并**“标记”**(记住)它们。
- 然后它访问并“标记”所有来自它们的引用。
- 然后它访问标记的对象并标记它们的引用。所有被访问的对象都被记住，以便以后不再访问同一个对象两次。
- 以此类推，直到有未访问的引用(可以从根访问)为止。
- **除标记的对象外，所有对象都被删除。**

综上所述，标记清除算法或者说策略就有两个很明显的缺点

- **内存碎片化**，空闲内存块是不连续的，容易出现很多空闲内存块，还可能会出现分配所需内存过大的对象时找不到合适的块
- **分配速度慢**，因为即便是使用 `First-fit` 策略，其操作仍是一个 `O(n)` 的操作，最坏情况是每次都要遍历到最后，同时因为碎片化，大对象的分配效率会更慢

而 **标记整理（Mark-Compact）算法** 就可以有效地解决，它的标记阶段和标记清除算法没有什么不同，只是标记结束后，标记整理算法会将活着的对象（即不需要清理的对象）向内存的一端移动，最后清理掉边界的内存

**引用计数算法步骤**

它的策略是跟踪记录每个变量值被使用的次数

- 当声明了一个变量并且将一个引用类型赋值给该变量的时候这个值的引用次数就为 1
- 如果同一个值又被赋给另一个变量，那么引用数加 1
- 如果该变量的值被其他的值覆盖了，则引用次数减 1
- 当这个值的引用次数变为 0 的时候，说明没有变量在使用，这个值没法被访问了，回收空间，垃圾回收器会在运行的时候清理掉引用次数为 0 的值占用的内存

**优点**

引用计数算法的优点我们对比标记清除来看就会清晰很多，首先引用计数在引用值为 0 时，也就是在变成垃圾的那一刻就会被回收，所以它可以立即回收垃圾

而标记清除算法需要每隔一段时间进行一次，那在应用程序（JS脚本）运行过程中线程就必须要暂停去执行一段时间的 `GC`，另外，标记清除算法需要遍历堆里的活动以及非活动对象来清除，而引用计数则只需要在引用时计数就可以了

**缺点**

引用计数的缺点想必大家也都很明朗了，首先它需要一个计数器，而此计数器需要占很大的位置，因为我们也不知道被引用数量的上限，还有就是无法解决**循环引用**无法回收的问题，这也是最严重的



### 优化(谷歌浏览器的垃圾回收算法)

- **分代回收**——V8 将堆分为**新生代**和**老生代**，新生代中存放的是生存时间短的对象，老生代中存放的生存时间久的对象。新生代容量远小于老生代，同时两者的垃圾回收策略也不同，新生代使用Scavange算法（标记活动对象和非活动对象，复制 from space 的活动对象到 to space 并对其进行排序，释放 from space 中的非活动对象的内存，将 from space 和 to space 角色互换），老生代使用标记清除和标记整理算法。
- **增量回收**——如果有很多对象，并且我们试图一次遍历并标记整个对象集，那么可能会花费一些时间，并在执行中会有一定的延迟。因此，引擎试图将垃圾回收分解为多个部分。然后，各个部分分别执行。这需要额外的标记来跟踪变化，这样有很多微小的延迟，而不是很大的延迟。
- **空闲时间收集**——垃圾回收器只在 CPU 空闲时运行，以减少对执行的可能影响。

## v8垃圾回收机制

### 新生代和老生代

- 所谓新生代，指的是新产生的对象；
- 老生代就是经历过新生代垃圾回收后还“存活”下来的对象。

> 新生代 => 老生代
>
> - 这个变量经历过一次垃圾回收算法。
> - 新生代内存To 空间的占比大小超过 25 %。在这种情况下，为了不影响到内存分配，会将对象从新生代空间移到老生代空间中。

### **新生代算法**

新生代中的对象一般存活时间较短，使用 Scavenge GC 算法。

1. 我们把新生代对象的内存平均分开 2 份空间From 和 To

2. 每当有新生对象诞生，就会在 From 空间出现

3. 一旦 From 空间被占满，就触发 Scavenge GC

4. 从 From 空间拿出存活的对象，复制到 To 空间

5. 清空 From 空间 （这样就可以实现把不活跃的对象给回收掉

6. From To 空间角色互换，开始下一轮循环

### **老生代算法**

1.老生代中的对象一般存活时间较长且数量也多，使用了两个算法，分别是标记清除算法和标记压缩算法。

在老生代中，以下情况会先启动**标记清除算法**：

- 某一个空间没有分块的时候
- 空间中被对象超过一定限制
- 空间不能保证新生代中的对象移动到老生代中

在这个阶段中，会遍历堆中所有的对象，然后标记活的对象，在标记完成后，销毁所有没有被标记的对象。

2.清除对象后会造成堆内存出现碎片的情况，当碎片超过一定限制后会启动**标记压缩算法**。在压缩过程中，将活的对象向一端移动，直到所有对象都移动完成然后清理掉不需要的内存。



# *正则表达式* RegExp

> 正则表达式是用于匹配字符串中字符组合的模式。在 JavaScript 中，正则表达式也是对象。这些模式被用于 [`RegExp`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp) 的 [`exec`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec) 和 [`test`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test) 方法，以及 [`String`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String) 的 [`match`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/match)、[`matchAll`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/matchAll)、[`replace`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace)、[`search`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/search) 和 [`split`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split) 方法。

*语法*

```
/正则表达式主体/修饰符(可选)
```

### *模糊匹配*

#### *横向模糊匹配*

*横向模糊指的是，一个正则可匹配的字符串的长度不是固定的，可以是多种情况的。*

*其实现的方式是使用量词。譬如**{m,n}**，表示连续出现最少m次，最多n次。*

*比如`/ab{2,5}c/`表示匹配这样一个字符串：第一个字符是“a”，接下来是2到5个字符“b”，最后是字符“c”。*

#### ***纵向模糊匹配***

*纵向模糊指的是，一个正则匹配的字符串，具体到某一位字符时，它可以不是某个确定的字符，可以有多种可能。*

*其实现的方式是使用字符组。譬如**[abc]**，表示该字符是可以字符“a”、“b”、“c”中的**任何一个**。*

*比如`/a[123]b/`可以匹配如下三种字符串："a1b"、"a2b"、"a3b"。*

### *字符组*

*需要强调的是，虽叫字符组（字符类），但只是其中一个字符。例如`[abc]`，**表示匹配一个字符**，它可以是“a”、“b”、“c”之一。*

#### ***范围表示法***

*比如`[123456abcdefGHIJKLM]`，可以写成`[1-6a-fG-M]`。用连字符`-`来省略和简写。*

*因为连字符有特殊用途，那么要匹配“a”、“-”、“z”这三者中任意一个字符，该怎么做呢？*

*不能写成`[a-z]`，因为其表示小写字符中的任何一个字符。*

*可以写成如下的方式：`[-az]`或`[az-]`或`[a\-z]`。即要么放在开头，要么放在结尾，要么转义。总之不会让引擎认为是范围表示法就行了。*

#### ***排除字符组***

*此时就是排除字符组（反义字符组）的概念。例如`[^abc]`，表示是一个除"a"、"b"、"c"之外的任意一个字符。字符组的第一位放`^`（脱字符），表示求反的概念。*

#### ***常见的简写形式***

*有了字符组的概念后，一些常见的符号我们也就理解了。因为它们都是系统自带的简写形式。*

> ***`\d`**就是`[0-9]`。表示是一位数字。记忆方式：其英文是digit（数字）。*
>
> ***`\D`**就是`[^0-9]`。表示除数字外的任意字符。*
>
> ***`\w`**就是`[0-9a-zA-Z_]`。表示数字、大小写字母和下划线。记忆方式：w是word的简写，也称单词字符。*
>
> ***`\W`**是`[^0-9a-zA-Z_]`。非单词字符。*
>
> ***`\s`**是`[ \t\v\n\r\f]`。表示空白符，包括空格、水平制表符、垂直制表符、换行符、回车符、换页符。记忆方式：s是space character的首字母。*
>
> ***`\S`**是`[^ \t\v\n\r\f]`。 非空白符。*
>
> ***`.`**就是`[^\n\r\u2028\u2029]`。通配符，表示几乎任意字符。换行符、回车符、行分隔符和段分隔符除外。记忆方式：想想省略号...中的每个点，都可以理解成占位符，表示任何类似的东西。*

*如果要匹配任意字符怎么办？可以使用`[\d\D]`、`[\w\W]`、`[\s\S]`和`[^]`中任何的一个。*

### *量词*

#### ***简写形式***

> *`{m,}` 表示至少出现m次。*
>
> *`{m} 等价于`{m,m}，表示出现m次。*
>
> *`? 等价于`{0,1}，表示出现或者不出现。记忆方式：问号的意思表示，有吗？*
>
> *`+ `等价于`{1,}`，表示出现至少一次。记忆方式：加号是追加的意思，得先有一个，然后才考虑追加。*
>
> *`* 等价于`{0,}，表示出现任意次，有可能不出现。记忆方式：看看天上的星星，可能一颗没有，可能零散有几颗，可能数也数不过来。*

#### ***贪婪匹配和惰性匹配***

```ini
var regex = /\d{2,5}/g;
var string = "123 1234 12345 123456";
console.log( string.match(regex) ); 
// => ["123", "1234", "12345", "12345"]
复制代码
```

*其中正则`/\d{2,5}/`，表示数字连续出现2到5次。会匹配2位、3位、4位、5位连续数字。*

*但是其是贪婪的，它会尽可能多的匹配。你能给我6个，我就要5个。你能给我3个，我就3要个。反正只要在能力范围内，越多越好。*

*我们知道有时贪婪不是一件好事（请看文章最后一个例子）。而惰性匹配，就是尽可能少的匹配：*

```lua
var regex = /\d{2,5}?/g;
var string = "123 1234 12345 123456";、console.log( stri
    ng.match(regex)eib 
// => ["12", "12", "34", "12", "34", "12", "34", "56"]、
```

*其中`/\d{2,5}?/`表示，虽然2到5次都行，当2个就够的时候，就不在往下尝试了。*

*通过在量词后面加个问号就能实现惰性匹配，因此所有惰性匹配情形如下：*

> ```
> **{m,n}?** `
> `**{m,}?**`
> `**??**`
> `**+?**`
> `***?**
> ```

#### *多选分支*

*一个模式可以实现横向和纵向模糊匹配。而多选分支可以支持多个子模式任选其一。*

*具体形式如下：`(p1|p2|p3)`，其中`p1`、`p2`和`p3`是子模式，用`|`（管道符）分隔，表示其中任何之一。*

*例如要匹配"good"和"nice"可以使用`/good|nice/`。测试如下：*

```ini
var regex = /good|nice/g;
var string = "good idea, nice try.";
console.log( string.match(regex) ); 
// => ["good", "nice"]
```

*但有个事实我们应该注意，比如我用`/good|goodbye/`，去匹配"goodbye"字符串时，结果是"good"：*

```ini
var regex = /good|goodbye/g;
var string = "goodbye";
console.log( string.match(regex) ); 
// => ["good"]
```

*而把正则改成`/goodbye|good/`，结果是：*

```ini
var regex = /goodbye|good/g;
var string = "goodbye";
console.log( string.match(regex) ); 
// => ["goodbye"]
```

*也就是说，分支结构也是惰性的，即当前面的匹配上了，后面的就不再尝试了。*

### *常用方法*

#### *test()*

*test() 方法用于检测一个字符串是否匹配某个模式，如果字符串中含有匹配的文本，则返回 true，否则返回 false。*

```js
/e/.test("The best things in life are free!")
```

#### *exec()*

*exec() 方法用于检索字符串中的正则表达式的匹配。*

```js
/e/.exec("The best things in life are free!");
```

#### ***search()***

***search()** 方法用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串，并返回子串的起始位置。*

```js
var str = "Visit Runoob!"; 
var n = str.search(/Runoob/i);
var n = str.search("Runoob");//search 方法可使用字符串作为参数。字符串参数会转换为正则表达式：
```

#### *replace()*

***replace()** 方法用于在字符串中用一些字符串替换另一些字符串，或替换一个与正则表达式匹配的子串。*

```js
var str = document.getElementById("demo").innerHTML; 
var txt = str.replace(/microsoft/i,"Runoob");
```

### *常用判断*

#### ***判断是否全为A-Za-z英文字母***

```js
let str = "asaaa"
var isletter2 = /^[a-zA-Z]+$/.test(str);
```

#### *判断是否全为汉字*

```js
/^[\u4e00-\u9fa5]+$/.test(str)
```







# 期约Promise

是异步编程的一种解决方案，比传统的解决方案（回调函数）更加合理和更加强大

在 Promise 出现以前，在我们处理多个异步请求嵌套时，可能产生**回调地狱**，产生**回调地狱**的原因归结起来有两点：

1. **嵌套调用**，第一个函数的输出往往是第二个函数的输入；
2. **处理多个异步请求并发**，开发时往往需要同步请求最终的结果。

原因分析出来后，那么问题的解决思路就很清晰了：

1. **消灭嵌套调用**：通过 Promise 的链式调用可以解决；
2. **合并多个任务的请求结果**：使用 Promise.all 获取合并多个任务的错误处理。

### 状态

`promise`对象仅有三种状态

- `pending`（进行中）
- `fulfilled`（已成功）
- `rejected`（已失败）

### 特点

- 对象的状态不受外界影响，只有异步操作的结果，可以决定当前是哪一种状态
- **一旦状态改变（从`pending`变为`fulfilled`和从`pending`变为`rejected`），就不会再变，任何时候都可以得到这个结果**

*![](pics/Promise周期.png)*

`Promise`对象是一个构造函数，用来生成`Promise`实例

```
const promise = new Promise(function(resolve, reject) {});
Promise`构造函数接受一个函数作为参数，该函数的两个参数分别是`resolve`和`reject
```

- `resolve`函数的作用是，将`Promise`对象的状态从“未完成”变为“成功”
- `reject`函数的作用是，将`Promise`对象的状态从“未完成”变为“失败”

### Promise的缺点

- Promise的内部错误使用`try catch`捕获不到，只能只用`then`的第二个回调或`catch`来捕获
- Promise一旦新建就会立即执行，无法取消
- Promise 真正执行回调的时候，定义 Promise 那部分实际上已经走完了，所以 Promise 的报错堆栈上下文不太友好。

### 实例方法

`Promise`构建出来的实例存在以下方法：

- then()
- catch()
- finally()

#### then()

**“promise本身不是异步的，是用来管理异步的，但是then方法是异步的「微任务」”**

then是实例状态发生改变时的回调函数，第一个参数是`resolved`状态的回调函数，第二个参数是`rejected`状态的回调函数

then()中的参数是函数，**如果传递的不是函数就会造成`值穿透`**，也就是`resolve()/reject()` 的返回值会`.then()`中接收。

**`then`方法返回的是一个新的`Promise`实例，也就是`promise`能链式书写的原因**

```js
getJSON("/posts.json").then(function(json) {
  return json.post;
}).then(function(post) {
  // ...
});
```

#### catch() 

`catch()`方法是`.then(null, rejection)`或`.then(undefined, rejection)`的别名，用于指定发生错误时的回调函数

```js
getJSON('/posts.json').then(function(posts) {
  // ...
}).catch(function(error) {
  // 处理 getJSON 和 前一个回调函数运行时发生的错误
  console.log('发生错误！', error);
});
```

**`Promise `对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止**

```js
getJSON('/post/1.json').then(function(post) {
  return getJSON(post.commentURL);
}).then(function(comments) {
  // some code
}).catch(function(error) {
  // 处理前面三个Promise产生的错误
});
```

一般来说，使用`catch`方法代替`then()`第二个参数

`Promise `对象抛出的错误不会传递到外层代码，即不会有任何反应

```js
const someAsyncThing = function() {
  return new Promise(function(resolve, reject) {
    // 下面一行会报错，因为x没有声明
    resolve(x + 2);
  });
};
```

浏览器运行到这一行，会打印出错误提示`ReferenceError: x is not defined`，但是不会退出进程

`catch()`方法之中，还能再抛出错误，通过后面`catch`方法捕获到

#### finally()

`finally()`方法用于指定不管 Promise 对象最后状态如何，都会执行的操作

```js
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});
```

### 构造函数方法

`Promise`构造函数存在以下方法：

- all()
- race()
- allSettled()
- resolve()
- reject()
- try()

#### all()

`Promise.all()`方法用于**将多个 `Promise `实例，包装成一个新的 `Promise `实例**

```js
const p = Promise.all([p1, p2, p3]);
```

接受一个**数组**（迭代对象）作为参数，**数组成员都应为`Promise`实例**

实例`p`的状态由`p1`、`p2`、`p3`决定，分为两种：

- 只有`p1`、`p2`、`p3`的状态都变成`fulfilled`，`p`的状态才会变成`fulfilled`，此时`p1`、`p2`、`p3`的返回值组成一个数组，传递给`p`的回调函数
- 只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，此时第一个被`reject`的实例的返回值，会传递给`p`的回调函数

注意，如果作为参数的 `Promise` 实例，自己定义了`catch`方法，那么它一旦被`rejected`，并不会触发`Promise.all()`的`catch`方法

```js
const p1 = new Promise((resolve, reject) => {
  resolve('hello');
})
.then(result => result)
.catch(e => e);

const p2 = new Promise((resolve, reject) => {
  throw new Error('报错了');
})
.then(result => result)
.catch(e => e);

Promise.all([p1, p2])
.then(result => console.log(result))
.catch(e => console.log(e));
// ["hello", Error: 报错了]
```

如果`p2`没有自己的`catch`方法，就会调用`Promise.all()`的`catch`方法

```js
const p1 = new Promise((resolve, reject) => {
  resolve('hello');
})
.then(result => result);

const p2 = new Promise((resolve, reject) => {
  throw new Error('报错了');
})
.then(result => result);

Promise.all([p1, p2])
.then(result => console.log(result))
.catch(e => console.log(e));
// Error: 报错了
```

#### race()

`Promise.race()`方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例

```js
const p = Promise.race([p1, p2, p3]);
```

`.race()` 的作用也是接收一组异步任务，然后并行执行异步任务，只保留取一个最快执行完成的异步操作的结果，**其他的方法仍在执行，不过执行结果会被抛弃。**

```js
const p = Promise.race([
  fetch('/resource-that-may-take-a-while'),
  new Promise(function (resolve, reject) {
    setTimeout(() => reject(new Error('request timeout')), 5000)
  })
]);

p
.then(console.log)
.catch(console.error);
```

#### *allSettled()*

*`Promise.allSettled()`方法接受一组 Promise 实例作为参数，包装成一个新的 Promise 实例*

*只有等到所有这些参数实例**都返回结果**，不管是`fulfilled`还是`rejected`，包装实例才会结束*

```js
const promises = [
  fetch('/api-1'),
  fetch('/api-2'),
  fetch('/api-3'),
];

await Promise.allSettled(promises);
removeLoadingIndicator();
```

#### *resolve()*

*将现有对象转为 `Promise `对象*

```js
Promise.resolve('foo')
// 等价于
new Promise(resolve => resolve('foo'))
```

*参数可以分成四种情况，分别如下：*

- *参数是一个 Promise 实例，`promise.resolve`将不做任何修改、原封不动地返回这个实例*
- *参数是一个`thenable`对象，`promise.resolve`会将这个对象转为 `Promise `对象，然后就立即执行`thenable`对象的`then()`方法*
- *参数不是具有`then()`方法的对象，或根本就不是对象，`Promise.resolve()`会返回一个新的 Promise 对象，状态为`resolved`*
- *没有参数时，直接返回一个`resolved`状态的 Promise 对象*

#### *reject()*

```js
Promise.reject(reason)`方法也会返回一个新的 Promise 实例，该实例的状态为`rejected
const p = Promise.reject('出错了');
// 等同于
const p = new Promise((resolve, reject) => reject('出错了'))

p.then(null, function (s) {
  console.log(s)
});
// 出错了
```

*`Promise.reject()`方法的参数，会原封不动地变成后续方法的参数*

```js
Promise.reject('出错了')
.catch(e => {
  console.log(e === '出错了')
})
// true
```

#### *any()*

*`Promise.any()` 接收一个[`Promise`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)可迭代对象，只要其中的一个 `promise` 成功，就返回那个已经成功的 `promise` 。如果可迭代对象中没有一个 `promise` 成功（即所有的 `promises` 都失败/拒绝），就返回一个失败的 `promise `和[`AggregateError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/AggregateError)类型的实例，它是 [`Error`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Error) 的一个子类，用于把单一的错误集合在一起。本质上，这个方法和 [`Promise.all()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) 是相反的。*

***返回值***

- *如果传入的参数是一个空的可迭代对象，则返回一个 **已失败（already rejected）** 状态的 [Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)。*
- *如果传入的参数不包含任何 `promise`，则返回一个 **异步完成** （**asynchronously resolved**）的 [Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)。*
- *其他情况下都会返回一个**处理中（pending）** 的 [Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)。 只要传入的迭代对象中的任何一个 `promise` 变成成功（resolve）状态，或者其中的所有的 `promises` 都失败，那么返回的 `promise` 就会 **异步地**（当调用栈为空时） 变成成功/失败（resolved/reject）状态。*

```js
const pErr = new Promise((resolve, reject) => {
  reject("总是失败");
});

const pSlow = new Promise((resolve, reject) => {
  setTimeout(resolve, 500, "最终完成");
});

const pFast = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, "很快完成");
});

Promise.any([pErr, pSlow, pFast]).then((value) => {
  console.log(value);
  // pFast fulfils first
})
// 期望输出: "很快完成"
```



## Promise 常见的应用场景

### Promise.all

应用场景1：多个请求结果合并在一起

> 具体描述：一个页面，有多个请求，我们需求所有的请求都返回数据后再一起处理渲染

应用场景2：合并请求结果并处理错误

> 有时候页面挂掉了，可能因为接口异常导致，或许只是一个无关紧要的接口挂掉了。那么一个接口挂掉了为什么会导致整个页面无数据呢？Promise.all告诉我们，如果参数中 promise 有一个失败（rejected），此实例回调失败（reject），就不再执行then方法回调，以上用例 正好可以解决此种问题

应用场景3：验证多个请求结果是否都是满足条件

> 描述：在一个微信小程序项目中，做一个表单的输入内容安全验证，调用的是云函数写的方法，表单有多7个字段需要验证，都是调用的一个 内容安全校验接口，全部验证通过则 可以 进行正常的提交

### Promise.race

应用场景1：图片请求超时

```js
Promise
.race([requestImg(), timeout()])
.then(function(results){
    console.log(results);
})
.catch(function(reason){
    console.log(reason);
});
```

应用场景2：请求超时提示

```js
Promise.race([
    request(),
    timeout()
])
.then(res=>{
    console.log(res)
}).catch(err=>{
    console.log(err)//网络不佳
})
```

### Promise.prototype.then

应用场景1：下个请求依赖上个请求的结果

> 描述：类似微信小程序的登录，首先需要 执行微信小程序的 登录 wx.login 返回了code，然后调用后端写的登录接口，传入 code ，然后返回 token ，然后每次的请求都必须携带 token，即下一次的请求依赖上一次请求返回的数据

应用场景2：中间件功能使用

> 描述：接口返回的数据量比较大，在一个then 里面处理 显得臃肿，多个渲染数据分别给个then，让其各司其职



# 生成器Generator

### 介绍

- generator:可以将生成器视为可以暂停和恢复的进程（代码段),代码在执行的过程中可以主要交出控制权，是 ES6 提供的一种异步编程解决方案

- genearator 语法: function 是一个新的关键字用于生成器函数（也有生成器方法）。
  yield是generator可以自行暂停的运算符。此外，generator还可以通过yield接收输入和发送输出。

```js
function* example() {
 yield 1;
 yield 2;
 yield 3;
}
var iter=example();
iter.next();//{value:1，done:false}
iter.next();//{value:2，done:false}
iter.next();//{value:3，done:false}
iter.next();//{value:undefined，done:true}
```

Generator函数有多个返回值状态（每个yield关键字后跟一个状态），只有调用next()函数时才会返回值，每调用一次next()函数就返回一个对象{value: xxx, done: false}，直到没有对应的yield了就返回done状态为true的对象{value: undefined, done: true}。

**yield与return的区别**

相同点:

- 都能返回语句后面的那个表达式的值
- 都可以暂停函数执行

区别:

- 一个函数可以有多个 yield,但是只能有一个 return
- yield 有位置记忆功能,return 没有

**与普通函数区别:**

- 普通函数使用 function 声明，生成器函数用 function*声明
- 普通函数使用 return 返回值，生成器函数使用 yield 返回值
- 普通函数是 run to completion 模式，即普通函数开始执行后，会一直执行到该函数所有语句完成，在此期间别的代码语句是不会被执行的；生成器函数是 run-pause-run 模式，即生成器函数可以在函数运行中被暂停一次或多次，并且在后面再恢复执行，在**暂停期间允许其他代码语句被执行**

### 相比其他异步的主要特点

https://www.fly63.com/article/detial/776



### 使用案例

https://blog.csdn.net/qq_39903567/article/details/115188020



### *实现原理*

*先简单回顾一下JS的运行规则：*

1. *JS是单线程的，只有一个主线程*
2. *函数内的代码从上到下顺序执行，遇到被调用的函数先进入被调用函数执行，待完成后继续执行*
3. *遇到异步事件，浏览器另开一个线程，主线程继续执行，待结果返回后，执行回调函数*

Generator函数是如何进行异步化为同步操作的呢？ 实质上很简单， 和 yield 是一个标识符，在浏览器进行软编译的时候，遇到这两个符号，自动进行了代码转换：

```js
// 异步函数
function asy() {
    $.ajax({
        url: 'test.txt',
        dataType: 'text',
        success() {
            console.log("我是异步代码");
        }
    })
}

function* gener() {
    let asy = yield asy();
    yield console.log("我是同步代码");
}
let it = gener().next();
it.then(function() {
    it.next();
})
// 我是异步代码
// 我是同步代码
```

*浏览器编译之后:*

```js
function gener() {
    // let asy = yield asy(); 替换为
    $.ajax({
        url: 'test.txt',
        dataType: 'text',
        success() {
            console.log("我是异步代码");
            // next 之后执行以下
            console.log("我是同步代码");
        }
    })
    // yield console.log("我是同步代码");
}
```

**整个过程类似于，浏览器遇到标识符 * 之后，就明白这个函数是生成器函数，一旦遇到 yield 标识符，就会将以后的函数放入此异步函数之内，待异步返回结果后再进行执行。**

*更深一步，从内存上来讲：*
*普通函数在被调用时，JS 引擎会创建一个栈帧，在里面准备好局部变量、函数参数、临时值、代码执行的位置（也就是说这个函数的第一行对应到代码区里的第几行机器码），在当前栈帧里设置好返回位置，然后将新帧压入栈顶。待函数执行结束后，这个栈帧将被弹出栈然后销毁，返回值会被传给上一个栈帧。*

*当执行到 yield 语句时，Generator 的栈帧同样会被弹出栈外，但Generator在这里耍了个花招——它在堆里保存了栈帧的引用（或拷贝）！这样当 it.next 方法被调用时，JS引擎便不会重新创建一个栈帧，而是把堆里的栈帧直接入栈。因为栈帧里保存了函数执行所需的全部上下文以及当前执行的位置，所以当这一切都被恢复如初之时，就好像程序从原本暂停的地方继续向前执行了。*
*而因为每次 yield 和 it.next 都对应一次出栈和入栈，所以可以直接利用已有的栈机制，实现值的传出和传入。*



# async/await

#### 介绍

> `async`函数可以理解为内置自动执行器的`Generator`函数语法糖，它配合`ES6`的`Promise`近乎完美的实现了异步编程解决方案

async await 是用来解决异步的，是**Generator函数的语法糖，async函数其实就相当于funciton *的作用，而await就相当与yield的作用。**
**async函数返回一个 Promise 对象，可以使用then方法添加回调函数**
当函数执行的时候，一旦遇到await就会先返回，等到异步操作完成，再接着执行函数体内后面的语句

#### async较Generator的优势

（1）内置执行器。Generator 函数的执行必须依靠执行器，而 Aysnc 函数自带执行器，调用方式跟普通函数的调用一样
（2）更好的语义。async 和 await 相较于 * 和 yield 更加语义化　
（3）更广的适用性。yield命令后面只能是 Thunk 函数或 Promise对象，async函数的await后面可以是Promise也可以是原始类型的值
（4）**返回值是 Promise**。async 函数返回的是 Promise 对象，比Generator函数返回的Iterator对象方便，可以直接使用 then() 方法进行调用

```js
async function test() {
  return 'test';
}
console.log(test); // [AsyncFunction: test] async函数是[`AsyncFunction`]构造函数的实例
console.log(test()); // Promise { 'test' }

// async返回的是一个promise对象
test().then(res=>{
  console.log(res); // test
})

// 如果async函数没有返回值 async函数返回一个undefined的promise对象
async function fn() {
  console.log('没有返回');
}
console.log(fn()); // Promise { undefined }

// 可以看到async函数返回值和Promise.resolve()一样，将返回值包装成promise对象，如果没有返回值就返回undefined的promise对象
```

```js
async function test() {
  return new Promise((resolve)=>{
    setTimeout(() => {
        resolve('test 1000');
    }, 1000);
  })
}
function fn() {
  return 'fn';
}

async function next() {
    let res0 = await fn(),
        res1 = await test(),
        res2 = await fn();
    console.log(res0);
    console.log(res1);
    console.log(res2);
}
next(); // 1s 后才打印出结果 为什么呢 就是因为 res1在等待promise的结果 阻塞了后面代码。
```

#### async函数

async函数的返回值为Promise对象，所以它可以调用then方法

- return 的值是 promise resolved 时候的 value
- Throw 的值是 Promise rejected 时候的 reason

```js
async function fn() {
  return 'async'
}
fn().then(res => {
  console.log(res) // 'async'
})
```

#### await表达式

await右侧的表达式一般为promise对象, 但也可以是其它的值

1. 如果表达式是 promise 对象, await 返回的是 promise 成功的值
2. 如果表达式是其它值, 直接将此值作为 await 的返回值
3. await后面是Promise对象会阻塞后面的代码，Promise 对象 resolve，然后得到 resolve 的值，作为 await 表达式的运算结果
4. 所以这就是await必须用在async的原因，async刚好返回一个Promise对象，可以异步执行阻塞

```js
function fn() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(1000)
        }, 1000);
    })
}
function fn1() { return 'nanjiu' }
async function fn2() {
    // const value = await fn() // await 右侧表达式为Promise，得到的结果就是Promise成功的value
    // const value = await '南玖'
    const value = await fn1()
    console.log('value', value)
}
fn2() // value 'nanjiu'
```

#### 错误处理

如果`await`后面的异步操作出错，那么等同于`async`函数返回的 Promise 对象被`reject`。

防止出错的方法，也是将其放在`try...catch`代码块之中。

```js
run();

async function run() {
    try {
        await Promise.reject(new Error("Oops!"));
    } catch (error) {
        error.message; // "Oops!"
    }
}
```



# 异步方案比较

后三种方案都是为解决传统的回调函数而提出的，所以它们相对于回调函数的优势不言而喻。而`async/await`又是`Generator`函数的语法糖。

- Promise的内部错误使用`try catch`捕获不到，只能只用`then`的第二个回调或`catch`来捕获，而`async/await`的错误可以用`try catch`捕获
- `Promise`一旦新建就会立即执行，不会阻塞后面的代码，而`async`函数中await后面是Promise对象会阻塞后面的代码。
- `async`函数会隐式地返回一个`promise`，该`promise`的`reosolve`值就是函数return的值。
- 使用`async`函数可以让代码更加简洁，不需要像`Promise`一样需要调用`then`方法来获取返回值，不需要写匿名函数处理`Promise`的resolve值，也不需要定义多余的data变量，还避免了嵌套代码。

# 事件流/事件机制

事件流描述的是从页面中接收事件的顺序。

#### 事件冒泡流

当节点事件被触发时，会由内圈到外圈 div-->body-->html-->document 依次触发其祖先节点的同类型事件，直到DOM根节点。

#### 事件捕获流

当节点事件被触发时，会从DOM根节点开始，依次触发其子孙节点的同类型事件，直到当前节点自身。由外圈到内圈 document-->html-->body-->div。

#### DOM2级事件

1. 事件捕获阶段（目标在捕获阶段不接收事件）
2. 目标阶段 （事件的执行阶段，此阶段会被归入冒泡阶段）
3. 事件冒泡阶段 （事件传回Dom根节点）

### 事件处理程序

#### HTML事件处理程序

```js
<input id="btn" type="button" value="Button" onclick="alert(event.type)">
```

使用该方式绑定的事件，在事件函数中存在一个局部变量`event`，用来保存事件对象。

该方式存在的缺点：

- 1、JavaScript与HTML加载顺序的不确定性，可能在JavaScript未加载完成之前就触发相应的事件，产生错误。
- 2、作用域连在不同的浏览器中会导致不同的结果。
- 3、JavaScript与HTML代码的紧密耦合，如果需要更换事件处理程序，则需要更改两个地方。

#### DOM0级事件处理程序

```js
<input id="btn" type="button" value="Button">

let btn = document.getElementById('btn')
// 添加事件，多次定义后，最后一个将覆盖之前的定义
btn.onclick = function () {
  alert(this.id)
}
// 删除事件
btn.onclick = null

```

使用该方式绑定的事件，在事件函数中可以使用`this`访问元素的任何属性和方法，且在事件流的冒泡阶段被处理。

#### DOM2级事件处理程序

提供了两个方法`addEventListener()`、`removeEventListener()`分别用来指定和删除事件处理程序。所有的DOM节点都包含这两个方法。

接收三个参数：

- 事件名
- 事件处理程序的函数
- 布尔值，默认值为false。 `true`: 在**捕获阶段调用**事件处理程序; `false`: **冒泡阶段调用**事件处理程序

```js
<input id="btn" type="button" value="Button">

let btn = document.getElementById('btn')

function fn ( ) {
   alert('This is a function')
}

// 添加事件，可以定义多个，按照顺序执行
btn.addEventListener('click', function () {
  alert('click1')
}, false)
btn.addEventListener('click', fn, false)

// 删除事件
btn.removeEventListener('click', fn)
```

- 使用addEventListener添加的事件处理程序只能由removeEventListener删除，移除时与传入的三个参数必须完全相同。
- removeEventListener无法移除匿名函数。

#### IE事件处理程序

与DOM2级类似，也提供了两个方法`attachEvent()`、`detachEvent()`分别用来指定和删除事件处理程序。所有的DOM节点都包含这两个方法。

使用attachEvent()添加的事件处理程序，均在冒泡阶段被处理。

接收两个参数：

- 事件名
- 事件处理程序的函数

1、主要区别是事件处理程序的作用域。在使用attachEvent()方法的情况下，事件处理程序会在全局作用域中进行，this等于window。

2、第一个参数必须是`on + 事件名`的形式。

3、添加多个事件处理程序时，不是按照顺序依次执行，而是以相反的顺序执行。

### 事件流模型的应用(事件委托)

**事件委托** 又叫 `事件代理`，指的是利用事件**冒泡原理，只需给外层父容器添加事件，若内层子元素有点击事件，则会冒泡到父容器上，这就是事件委托**，简单说就是：子元素委托它们的父级代为执行事件。

```html
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
</ul>

let ulDom = document.getElementsByTagName('ul')
ulDom[0].addEventListener('click', function(event){
    alert(event.target.innerHTML)
})
```

我们现在有一个这样的列表，我想监听所有的`<li>`标签，`<li>`标签我这里只列了五个，但实际业务中这里有可能会循环出成千上万个`<li>`标签。如果我们给每个`<li>`都绑定事件，会**极大的影响页面性能**，这个时候我们就可以使用`事件委托`来进行优化。

总结一下事件委托的优化：

- 提高性能:每一个函数都会占用内存空间，只需添加一个事件处理程序代理所有事件，所占用的内存空间更少。
- 动态监听:使用事件委托可以自动绑定动态添加的元素，即新增的节点不需要主动添加也可以一样具有和其他元素一样的事件。

### event.target 和event.currentTarget 的区别

首先本质区别是：

- event.target返回**触发事件的元素**
- event.currentTarget返回**绑定事件的元素**

**event.target 属性**可以用来**实现事件委托** (event delegation)



# 模块

### 什么是模块?

- 将一个复杂的程序依据一定的规则(规范)封装成几个块(文件), 并进行组合在一起
- 块的内部数据与实现是私有的, 只是向外部暴露一些接口(方法)与外部其它模块通信

### 模块化解决的问题：

- 命名冲突
- 文件依赖

### 模块化的好处

- 避免命名冲突(减少命名空间污染)
- 更好的分离, 按需加载
- 更高复用性
- 高可维护性

# 模块对比(CommonJS、AMD、CMD、UMD)

### CommonJS

**概述**

Node 应用由模块组成，采用 CommonJS 模块规范。每个文件就是一个模块，有自己的作用域。在一个文件里面定义的变量、函数、类，**都是私有的**，对其他文件不可见。**在服务器端，模块的加载是运行时同步加载的；在浏览器端，模块需要提前编译打包处理。**

**特点**

- 所有代码都运行在模块作用域，不会污染全局作用域。
- 模块可以多次加载，但是**只会在第一次加载时运行一次**，然后运行结果就被**缓存**了，以后再加载，就直接读取缓存结果。要想让模块再次运行，必须清除缓存。
- 模块加载的顺序，按照其在代码中出现的**顺序**。

**基本语法**

- 暴露模块：`module.exports = value`或`exports.xxx = value`
- 引入模块：`require(xxx)`,如果是第三方模块，xxx为模块名；如果是自定义模块，xxx为模块文件路径

**模块的加载机制**

**CommonJS模块的加载机制是，输入的是被输出的值的拷贝。也就是说，一旦输出一个值，模块内部的变化就影响不到这个值**。这点与ES6模块化有重大差异。**除非写成一个函数，才能得到内部变动后的值**。

### AMD

**概述**

CommonJS规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。AMD规范则是**非同步加载模块，允许指定回调函数**。由于Node.js主要用于服务器编程，模块文件一般都已经存在于本地硬盘，所以加载起来比较快，不用考虑非同步加载的方式，所以CommonJS规范比较适用。但是，**如果是浏览器环境，要从服务器端加载模块，这时就必须采用非同步模式，因此浏览器端一般采用AMD规范**。此外AMD规范比CommonJS规范在浏览器端实现要来着早。

**基本语法**

- 暴露模块：define(function(){   return 模块 })
- 引入模块：`require(xxx)`

**实现了AMD规范的require.js**

**AMD模块定义的方法非常清晰，不会污染全局环境，能够清楚地显示依赖关系**。AMD模式可以用于浏览器环境，并且允许非同步加载模块，也可以根据需要动态加载模块。

### CMD

**概述**

CMD规范专门用于浏览器端，模块的加载是**异步**的，模块使用时才会加载执行。**CMD规范整合了CommonJS和AMD规范的特点**。在 Sea.js 中，所有 JavaScript 模块都遵循 CMD模块定义规范。

### ES6模块化

**概述**

ES6 模块的设计思想是尽量的静态化，使得**编译时就能确定模块的依赖关系**，以及输入和输出的变量。CommonJS 和 AMD 模块，都只能在运行时确定这些东西。比如，CommonJS 模块就是对象，输入时必须查找对象属性。

**基本语法**

export命令用于规定模块的对外接口，import命令用于输入其他模块提供的功能。

### UMD

UMD 是一种 JavaScript **通用模块定义规范**，让你的模块能在 JavaScript 所有运行环境中发挥作用。

规定如下：

1. 优先判断是否存在 exports 方法，如果存在，则采用 CommonJS 方式加载模块；
2. 其次判断是否存在 define 方法，如果存在，则采用 AMD 方式加载模块；
3. 最后判断 global 对象上是否定义了所需依赖，如果存在，则直接使用；反之，则抛出异常。



### ES6 模块与 CommonJS 模块的差异

①CommonJS支持动态导入，ES6不支持，是静态编译。

②CommonJS同步加载，用于服务端，文件放在本地磁盘，读取速度快。同步导入卡住主线程也并无影响。ES6是异步加载，用于浏览器端，不能同步加载，会导致页面渲染，用户体验差。

③CommonJS模块输出的是值拷贝，内部的变化影响不到值的变化。ES6模块输出的是值引用，原始值变化，加载的值也会跟着变化，ES6模块是动态引用，并且不会缓存值。

④CommonJS模块是运行时加载，ES6模块是编译时输出接口。CommonJS模块就是对象，输入时先加载整个模块，生成一个对象，然后从对象读取方法。ES6模块不是对象，export输出指定代码，import导入加载某个值，而不是整个模块。

⑤关于模块顶层的this指向问题，在CommonJS顶层，this指向当前模块；而在ES6模块中，this指向undefined。

⑥ES6模块当中，是支持加载CommonJS模块的。但是反过来，CommonJS并不能requireES6模块，在NodeJS中，两种模块方案是分开处理的。

### 总结

CommonJS规范主要用于**服务端编程**，加载模块是**同步**的，这并不适合在浏览器环境，因为同步意味着阻塞加载，浏览器资源是异步加载的，因此有了AMD CMD解决方案。

AMD规范在**浏览器环境中异步加载模块**，而且可以并行加载多个模块。不过，AMD规范开发成本高，代码的阅读和书写比较困难，模块定义方式的语义不顺畅。

CMD规范与AMD规范很相似，都用于浏览器编程，依赖就近，延迟执行，可以很容易在Node.js中运行。不过，依赖SPM 打包，模块的加载逻辑偏重

**ES6 在语言标准的层面上，实现了模块功能，而且实现得相当简单，完全可以取代 CommonJS 和 AMD 规范，成为浏览器和服务器通用的模块解决方案**。

# 浏览器进程模型

## 进程 (process) 和线程 (thread)

**进程是 CPU 资源分配的最小单位（是能拥有资源和独立运行的最小单位）。**

**线程是 CPU 调度的最小单位（是建立在进程基础上的一次程序运行单位）。**

现代操作系统都是可以同时运行多个任务的,比如:用浏览器上网的同时还可以听音乐。

**对于操作系统来说,一个任务就是一个进程**,比如打开一个浏览器就是启动了一个浏览器进程,打开一个 Word 就启动了一个 Word 进程。

有些进程同时不止做一件事,比如 Word,它同时可以进行打字、拼写检查、打印等事情。**在一个进程内部,要同时做多件事,就需要同时运行多个“子任务”,我们把进程内的这些“子任务”称为线程**。

## 浏览器的多进程架构

一个好的程序常常被划分为几个相互独立又彼此配合的模块,浏览器也是如此。

以 Chrome 为例,它由多个进程组成,每个进程都有自己核心的职责,它们相互配合完成浏览器的整体功能,

每个进程中又包含多个线程,一个进程内的多个线程也会协同工作,配合完成所在进程的职责。

Chrome 采用多进程架构,其顶层存在一个 Browser process 用以协调浏览器的其它进程。

#### 优点 

由于默认 新开 一个 tab 页面 新建 一个进程,所以单个 tab 页面崩溃不会影响到整个浏览器。

同样,第三方插件崩溃也不会影响到整个浏览器。

多进程可以充分利用现代 CPU 多核的优势。

方便使用沙盒模型隔离插件等进程,提高浏览器的稳定性。

#### 缺点 

系统为浏览器新开的进程分配内存、CPU 等资源,所以内存和 CPU 的资源消耗也会更大。

不过 Chrome 在内存释放方面做的不错,基本内存都是能很快释放掉给其他程序运行的。

## 浏览器的主要进程和职责

#### 主进程 Browser Process 

> 负责浏览器界面的显示与交互。各个页面的管理,创建和销毁其他进程。网络的资源管理、下载等。

#### 第三方插件进程 Plugin Process 

> 每种类型的插件对应一个进程,仅当使用该插件时才创建。

#### GPU 进程 GPU Process 

> 最多只有一个,用于 3D 绘制等

#### 渲染进程 Renderer Process 

> 称为浏览器渲染进程或浏览器内核,内部是多线程的。主要负责页面渲染,脚本执行,事件处理等。 (本文重点分析)

#### 渲染进程 (浏览器内核)

**浏览器的渲染进程是多线程的**,我们来看看它有哪些主要线程 :

*![](pics/浏览器内核.png)*


### 1. GUI 渲染线程 

- 负责渲染浏览器界面,**解析 HTML,CSS,构建 DOM 树和 RenderObject 树,布局和绘制等**。
- 当界面需要重绘（Repaint）或由于某种操作引发**回流**(reflow)时,该线程就会执行。
- 注意**,GUI 渲染线程与 JS 引擎线程是互斥的**,当 JS 引擎执行时 GUI 线程会被挂起（相当于被冻结了）,GUI 更新会被保存在一个队列中等到 JS 引擎空闲时立即被执行。

### 2. JS 引擎线程 

- Javascript 引擎,也称为 JS 内核,负责**处理 Javascript 脚本程序**。（例如 V8 引擎）
- JS 引擎线程负责解析 Javascript 脚本,运行代码。
- JS 引擎一直等待着任务队列中任务的到来,然后加以处理,一个 Tab 页（renderer 进程）中无论什么时候都只有一个 JS 线程在运行 JS 程序。
- 注意,GUI 渲染线程与 JS 引擎线程是互斥的,所以如果 JS 执行的时间过长,这样就会造成页面的渲染不连贯,导致页面渲染加载阻塞。

### **3. 事件触发线程** 

- 归属于浏览器而不是 JS 引擎,用来控制事件循环（可以理解,JS 引擎自己都忙不过来,需要浏览器另开线程协助）
- 当 JS 引擎执行代码块如 setTimeOut 时（也可来自浏览器内核的其他线程,如鼠标点击、AJAX 异步请求等）,会将对应**任务添加到事件线程**中
- 当**对应的事件符合触发条件被触发时**,该线程会把**事件添加到待处理队列的队尾,等待 JS 引擎的处理**
- 注意,由于 JS 的单线程关系,所以这些待处理队列中的事件都得排队等待 JS 引擎处理（当 JS 引擎空闲时才会去执行）

### 4. 定时触发器线程 

- 传说中的 **setInterval 与 setTimeout 所在线程**
- 浏览器定时计数器并不是由 JavaScript 引擎计数的,（因为 JavaScript 引擎是单线程的, 如果处于阻塞线程状态就会影响记计时的准确）
- 因此通过**单独线程来计时并触发定时**（计时完毕后,添加到事件队列中,等待 JS 引擎空闲后执行）
- 注意,W3C 在 HTML 标准中规定,规定要求 setTimeout 中低于 4ms 的时间间隔算为 4ms。

### 5. 异步 http 请求线程 

- 在 **XMLHttpRequest** 在连接后是通过浏览器新开一个线程请求。
- 将检测到状态变更时,如果设置有回调函数,异步线程就产生状态变更事件,将这个回调再放入事件队列中。再由 JavaScript 引擎执行。

### 为什么 Javascript 要是单线程的 ? 

首先是历史原因，在创建 javascript 这门语言时，多进程多线程的架构并不流行，硬件支持并不好。

这是因为 Javascript 这门脚本语言诞生的使命所致!JavaScript 为处理页面中用户的交互,以及操作 DOM 树、CSS 样式树来给用户呈现一份动态而丰富的交互体验和服务器逻辑的交互处理。

如果 JavaScript 是多线程的方式来操作这些 UI DOM,则可能出现 **UI 操作的冲突**。

如果 Javascript 是多线程的话,在多线程的交互下,处于 UI 中的 **DOM 节点**就可能成为一个临界资源,

假设存在两个线程同时操作一个 DOM,一个负责修改一个负责删除,那么这个时候就需要浏览器来裁决如何生效哪个线程的执行结果。

当然我们可以通过锁来解决上面的问题。但为了避免因为引入了锁而带来更大的**复杂性**,Javascript 在最初就选择了单线程执行。

### 为什么 JS *阻塞页面加载*（GUI 渲染线程与 JS 引擎线程互斥）

由于 **JavaScript 是可操纵 DOM 的**,如果在修改这些元素属性同时渲染界面（即 JavaScript 线程和 UI 线程同时运行）,那么渲染线程前后获得的元素数据就可能不一致了。

因此为了防止渲染出现不可预期的结果,浏览器设置 **GUI 渲染线程与 JavaScript 引擎为互斥**的关系。

当 JavaScript 引擎执行时 GUI 线程会被挂起,GUI 更新会被保存在一个队列中等到引擎线程空闲时立即被执行。

从上面我们可以推理出,由于 GUI 渲染线程与 JavaScript 执行线程是互斥的关系,

当浏览器在执行 JavaScript 程序的时候,GUI 渲染线程会被保存在一个队列中,直到 JS 程序执行完成,才会接着执行。

因此如果 JS 执行的时间过长,这样就会造成页面的渲染不连贯,导致页面渲染加载阻塞的感觉。

### css 加载会造成阻塞吗 

由上面浏览器渲染流程我们可以看出 :

DOM 和 CSSOM 通常是并行构建的,所以 **CSS 加载不会阻塞 DOM 的解析**。

然而,由于 Render Tree 是依赖于 DOM Tree 和 CSSOM Tree 的,

所以他必须等待到 CSSOM Tree 构建完成,也就是 CSS 资源加载完成(或者 CSS 资源加载失败)后,才能开始渲染。

因此,**CSS 加载会阻塞 Dom 的渲染**。

由于 JavaScript 是可操纵 DOM 和 css 样式 的,如果在修改这些元素属性同时渲染界面（即 JavaScript 线程和 UI 线程同时运行）,那么渲染线程前后获得的元素数据就可能不一致了。

因此为了防止渲染出现不可预期的结果,浏览器设置 **GUI 渲染线程与 JavaScript 引擎为互斥**的关系。

因此,样式表会在后面的 js 执行前先加载执行完毕,所以**css 会阻塞后面 js 的执行**。

### DOMContentLoaded 与 load 的区别 

当 DOMContentLoaded 事件触发时,仅当 **DOM 解析完成**后,不包括样式表,图片。前面提到 **CSS 加载会阻塞 Dom 的渲染和后面 js 的执行,js 会阻塞 Dom 解析**,所以我们可以得到结论:
当**文档中没有脚本时,浏览器解析完文档便能触发 DOMContentLoaded 事件**。如果文档中**包含脚本**,则脚本会阻塞文档的解析,**而脚本需要等 CSSOM 构建完成才能执行**。在任何情况下,DOMContentLoaded 的触发不需要等待图片等其他资源加载完成。

当 onload 事件触发时,页面上所有的 DOM,样式表,脚本,图片等资源已经加载完毕。

DOMContentLoaded -> load。



# 事件循环Event Loop

### 概述

`Event Loop`即事件循环，是指浏览器或`Node`的一种解决`javaScript`**单线程运行**时不会阻塞的一种**机制**，也就是我们经常使用**异步**的原理。

在`JavaScript`中，任务被分为两种，一种宏任务（`MacroTask`）也叫`Task`，一种叫微任务（`MicroTask`），在最新标准中，它们被分别称为task与jobs。

### MacroTask（宏任务）

- `script`全部代码、`setTimeout`、`setInterval`、`setImmediate`（浏览器暂时不支持，只有IE10支持，具体可见[`MDN`](https://link.juejin.cn/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FWindow%2FsetImmediate)）、`I/O`、`UI Rendering`。

我们可以将每次执行栈执行的代码当做是一个宏任务(包括每次从事件队列中获取一个事件回调并放到执行栈中执行)， 每一个宏任务会从头到尾执行完毕，不会执行其他

由于`JS引擎线程`和`GUI渲染线程`是互斥的关系，浏览器为了能够使`宏任务`和`DOM任务`有序的进行，**会在一个`宏任务`执行结果后，在下一个`宏任务`执行前，`GUI渲染线程`开始工作，对页面进行渲染**

### MicroTask（微任务）

- `Process.nextTick（Node独有）`、`Promise`、`Async/Await(实际就是promise)`、`Object.observe(废弃)`、`MutationObserver`（H5中的，具体使用方式查看[这里](https://link.juejin.cn/?target=http%3A%2F%2Fjavascript.ruanyifeng.com%2Fdom%2Fmutationobserver.html)）(Mutation Observer API 用来监视 DOM 变动。DOM 的任何变动，比如节点的增减、属性的变动、文本内容的变动，这个 API 都可以得到通知。)

ES6新引入了Promise标准，同时浏览器实现上多了一个`microtask`微任务概念，在ECMAScript中，`microtask`也被称为`jobs`

我们已经知道`宏任务`结束后，会执行渲染，然后执行下一个`宏任务`， 而微任务可以理解成在当前`宏任务`执行后立即执行的任务

当一个`宏任务`执行完，会在渲染前，将执行期间所产生的所有`微任务`都执行完

### Event Loop流程

`Javascript` 有一个 `main thread` 主线程和 `call-stack` 执行栈，所有的任务都会被放到调用栈等待主线程执行。	

`Javascript`单线程任务被分为**同步任务**和**异步任务**，

**具体流程**

首先，整体的script(作为第一个宏任务)开始执行的时候，会把所有代码分为`同步任务`、`异步任务`两部分

同步任务会在执行栈中按照顺序等待主线程依次执行

异步任务会再分为宏任务和微任务

宏任务进入到Event Table中，并在里面注册回调函数，每当指定的事件完成时，Event Table会将这个函数移到Event Queue中

微任务也会进入到另一个Event Table中，并在里面注册回调函数，每当指定的事件完成时，Event Table会将这个函数移到Event Queue中

当主线程内的任务执行完毕，主线程为空时，会检查微任务的Event Queue，如果有任务，就添加到执行栈中执行，如果没有就执行下一个宏任务

上述过程会不断重复，这就是Event Loop，比较完整的事件循环

<img src="pics/完整事件循环.png" style="zoom: 50%;" />





一段简单的代码

```javascript
let setTimeoutCallBack = function() {
  console.log('我是定时器回调');
};
let httpCallback = function() {
  console.log('我是http请求回调');
}

// 同步任务
console.log('我是同步任务1');

// 异步定时任务
setTimeout(setTimeoutCallBack,1000);

// 异步http请求任务
ajax.get('/info',httpCallback);

// 同步任务
console.log('我是同步任务2');
```

上述代码执行过程

JS是按照顺序从上往下依次执行的，可以先理解为这段代码时的执行环境就是主线程，也就是也就是当前执行栈

首先，执行`console.log('我是同步任务1')`

接着，**执行到`setTimeout`时，会移交给`定时器线程`**，**通知`定时器线程` 1s 后将 `setTimeoutCallBack` 这个回调交给`事件触发线程`处理，在 1s 后`事件触发线程`会收到 `setTimeoutCallBack` 这个回调并把它加入到`事件触发线程`所管理的事件队列中等待执行**

接着，执行http请求，会移交给`异步http请求线程`发送网络请求，请求成功后将 `httpCallback` 这个回调交由事件触发线程处理，`事件触发线程`收到 `httpCallback` 这个回调后把它加入到`事件触发线程`所管理的事件队列中等待执行

再接着执行`console.log('我是同步任务2')`

至此主线程执行栈中执行完毕，`JS引擎线程`已经空闲，开始向`事件触发线程`发起询问，询问`事件触发线程`的事件队列中是否有需要执行的回调函数，如果有将事件队列中的回调事件加入执行栈中，开始执行回调，如果事件队列中没有回调，`JS引擎线程`会一直发起询问，直到有为止

### 为什么要区分微任务和宏任务？

js机制在对待任务时，认为他们应该是不平等的，任务有执行速度和**优先级**的区别。也就是说**执行更快的任务**应该可以**插队**，不必等执行耗时就的先执行完。从更底层来说：

- 微任务是线程之间的切换，速度快。不用进行上下文切换，可以快速的一次性做完所有的微任务。
- 宏任务是进程之间的切换，速度慢，且每次执行需要切换上下文。因此一个Eventloop中只执行一个宏任务。
- 微任务执行快，一次性可以执行很多个，在当前宏任务执行后立刻清空微任务可以达到**伪同步**的效果，这对视图渲染效果起到至关重要的作用。

如果不区分宏任务和微任务，那就无法在下一次 Event loop 之前进行插队，其中新注册的任务得等到下一个宏任务完成之后才能进行，这中间可能你需要的状态就无法在下一个宏任务中得到**同步**。



### 关于Promise

`new Promise(() => {}).then()` ，我们来看这样一个Promise代码

前面的 `new Promise()` 这一部分是一个构造函数，这是一个同步任务

后面的 `.then()` 才是一个异步微任务，这一点是非常重要的

### 关于 async/await 函数

在使用await关键字与Promise.then效果类似

```javascript
setTimeout(() => console.log(4))

async function test() {
  console.log(1)
  await Promise.resolve()
  console.log(3)
}

test()

console.log(2)
```

上述代码输出`1 2 3 4`

可以理解为，`await` 以前的代码，相当于与 `new Promise` 的同步代码，`await` 以后的代码相当于 `Promise.then`的异步

# ⚝SetTimeout、SetInterVal、setImmediate和process.nextTick的理解

```js
var timeoutID = scope.setTimeout(function[, delay, arg1, arg2, ...]);
```

**`setTimeout()`**方法设置一个定时器，该定时器在定时器到期后执行一个函数或指定的一段代码。

```js
var intervalID = setInterval(func, [delay, arg1, arg2, ...]);
```

**`setInterval()`** 方法重复调用一个函数或执行一个代码片段，在每次调用之间具有固定的时间间隔。

```js
var immediateID = setImmediate(func, [param1, param2, ...]);
```

**`setImmediate()`** 方法用来把一些需要长时间运行的操作放在一个回调函数里，在浏览器完成后面的其他语句后，就立刻执行这个回调函数。

`process.nextTick()`用法和setImmediate相似

## setImmediate和SetTimeout执行顺序问题

**结论：不一定，在一个异步流程里，`setImmediate`会比定时器先执行，直接写在最外层不一定，看机器状态。**

Node.js是运行在服务端的js，虽然他也用到了V8引擎，但是他的服务目的和环境不同，导致了他API与原生JS有些区别，他的Event Loop还要处理一些I/O，比如新的网络连接等，所以与浏览器Event Loop也是不一样的。Node的Event Loop是分阶段的，如下图所示：

![image-20200322203318743](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3027cbdfe546433a84fd8f80733e99dd~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

> 1. timers: 执行`setTimeout`和`setInterval`的回调
> 2. pending callbacks: 执行延迟到下一个循环迭代的 I/O 回调
> 3. idle, prepare: 仅系统内部使用
> 4. poll: 检索新的 I/O 事件;执行与 I/O 相关的回调。事实上除了其他几个阶段处理的事情，其他几乎所有的异步都在这个阶段处理。
> 5. check: `setImmediate`在这里执行
> 6. close callbacks: 一些关闭的回调函数，如：`socket.on('close', ...)`

每个阶段都有一个自己的先进先出的队列，只有当这个队列的事件执行完或者达到该阶段的上限时，才会进入下一个阶段。在每次事件循环之间，Node.js都会检查它是否在等待任何一个I/O或者定时器，如果没有的话，程序就关闭退出了。

**不在一个异步流程的情况**

流程：

> 1. 外层同步代码一次性全部执行完，遇到异步API就塞到对应的阶段
> 2. 遇到`setTimeout`，虽然设置的是0毫秒触发，但是被node.js强制改为1毫秒，塞入`times`阶段
> 3. 遇到`setImmediate`塞入`check`阶段
> 4. 同步代码执行完毕，进入Event Loop
> 5. 先进入`times`阶段，检查当前时间过去了1毫秒没有，如果过了1毫秒，满足`setTimeout`条件，执行回调，如果没过1毫秒，跳过
> 6. 跳过空的阶段，进入check阶段，执行`setImmediate`回调

通过上述流程的梳理，我们发现关键就在这个1毫秒，如果同步代码执行时间较长，进入Event Loop的时候1毫秒已经过了，`setTimeout`执行，如果1毫秒还没到，就先执行了`setImmediate`。每次我们运行脚本时，机器状态可能不一样，导致运行时有1毫秒的差距，一会儿`setTimeout`先执行，一会儿`setImmediate`先执行。但是这种情况只会发生在还没进入`timers`阶段的时候。因为已经在`timers`阶段，所以里面的`setTimeout`只能等下个循环了，所以`setImmediate`肯定先执行。同理的还有其他`poll`阶段的API也是这样的。

# ⚝为什么要用 setTimeout 模拟 setInterval

定时器代码：

```js
setInterval(fn(), N);
```

上面这句代码的意思其实是**fn()将会在 N 秒之后被推入任务队列**。

所以，在 setInterval 被推入任务队列时，如果在它前面有很多任务或者某个任务等待时间较长比如网络请求等，那么这个定时器的执行时间和我们预定它执行的时间可能并不一致。

假设n为100，setInterval 每隔 100ms 往队列中添加一个事件；100ms 后，添加 T1 定时器代码至队列中，主线程中还有任务在执行，所以等待，some event 执行结束后执行 T1 定时器代码；又过了 100ms，T2 定时器被添加到队列中，主线程还在执行 T1 代码，所以等待；又过了 100ms，理论上又要往队列里推一个定时器代码，**但由于此时 T2 还在队列中，所以 T3 不会被添加（T3 被跳过）**，结果就是此时被跳过；这里我们可以看到，T1 定时器执行结束后马上执行了 T2 代码，所以并没有达到定时器的效果。

综上所述，setInterval 有**两个缺点**：

- 使用 setInterval 时，**某些间隔会被跳过**；
- 可能多个定时器**会连续执行**；

可以这么理解：**每个 setTimeout 产生的任务会直接 push 到任务队列中；而 setInterval 在每次把任务 push 到任务队列前，都要进行一下判断(看上次的任务是否仍在队列中，如果有则不添加，没有则添加)。**

因而我们一般用 setTimeout 模拟 setInterval，来规避掉上面的缺点。

## 用setTimeout和clearTimeout实现setInterval与clearInterval

**SetInterval实现**

主要思想就是递归调用自身

```js
let timeMap = {}
let id = 0 // 简单实现id唯一
const mySetInterval = (cb, time) => {
  let timeId = id // 将timeId赋予id
  id++ // id 自增实现唯一id
  let fn = () => {
    cb()
    timeMap[timeId] = setTimeout(() => {
      fn()
    }, time)
  }
  timeMap[timeId] = setTimeout(fn, time)
  return timeId // 返回timeId
}
```

`id` 值是全局变量 `timeMap` 里的一个键的内容；

为了存每个定时器的id，用闭包；

每次更新 `setTimeout` 的 `id` 并不是去更新 `timeId`，相应的，我们去更新 `timeMap[timeId]` 里的值。

为了保证 `timeId` 的唯一性，在这里我简单用了一个自增的全局变量 `id` 来保证唯一。

**ClearInterval实现**

由于 `mySetInterval` 返回的 `timeId` 并不是真正的 `setTimeout` 返回的 `id` ，所以并不能简单地通过 `clearTimeout(timeId)` 来清除计时器。

不过其实原理也是很类似的，我们只要能拿到真正的 `id` 就行了：

```bash
const myClearInterval = (id) => {
  clearTimeout(timeMap[id]) // 通过timeMap[id]获取真正的id
  delete timeMap[id]
}
```



# 前端如何实现高精度定时器

### 用setTimeout代替setInterval

setInterval 有**两个缺点**：

- 使用 setInterval 时，**某些间隔会被跳过**；
- 可能多个定时器**会连续执行**；

可以这么理解：**每个 setTimeout 产生的任务会直接 push 到任务队列中；而 setInterval 在每次把任务 push 到任务队列前，都要进行一下判断(看上次的任务是否仍在队列中，如果有则不添加，没有则添加)。**

因而我们一般用 setTimeout 模拟 setInterval，来规避掉上面的缺点。

主要思想就是递归调用自身

```js
let timeMap = {}
let id = 0 // 简单实现id唯一
const mySetInterval = (cb, time) => {
  let timeId = id // 将timeId赋予id
  id++ // id 自增实现唯一id
  let fn = () => {
    cb()
    timeMap[timeId] = setTimeout(() => {
      fn()
    }, time)
  }
  timeMap[timeId] = setTimeout(fn, time)
  return timeId // 返回timeId
}
```

`id` 值是全局变量 `timeMap` 里的一个键的内容；

为了存每个定时器的id，用闭包；

每次更新 `setTimeout` 的 `id` 并不是去更新 `timeId`，相应的，我们去更新 `timeMap[timeId]` 里的值。

为了保证 `timeId` 的唯一性，在这里我简单用了一个自增的全局变量 `id` 来保证唯一。

### requestAnimationFrame

```
window.requestAnimationFrame(callback);
```

1. requestAnimationFrame 的回调执行间隔和浏览器刷新频率有关。浏览器一秒刷新60次，那么执行间隔是 `1 / 60 = 16.7ms`；如果因为性能原因，浏览器进行降频，那么间隔时间会相应改变。
2. 相对于setInterval的好处在于“踩点”。回调一定在浏览器渲染前执行，页面变化刚好可以体现出来。这是setInterval设置相同时间间隔也无法做到的。
3. 但它存在和setInterval相同的问题：回调函数仍在主线程中执行，也会被阻塞，回调中也需要进行校正。浏览器后台运行时，有可能会被停掉。

重绘之前调用，甚至可以每秒 60 次，和刷新率一样，看起来不错，可以解决 UI 延迟更新的问题。 与此同时我们 UI 的更新的精确度，来到了 16.67ms。

```js
let start = Date.now();
function timer1() {
  let gaps = Date.now() - start;
  let seconds = Math.floor(gaps / 1000);
  updateUI(seconds);
  requestAnimationFrame(timer1);
}
timer1();
```

但是我们计时器是 1 秒更新一次UI，这就意味着浪费了 59 次。可不可以不让 `requestAnimationFrame` 不 60 秒调用一次呢？对你想的没错，`setTimeout`

### 两者结合

```js
function interval(ms, callback) {
  const start = document.timeline
    ? document.timeline.currentTime
    : performance.now();
    
  function timer1(time) {
    const gaps = time - start;
    const seconds = Math.round(gaps / ms);
    callback(seconds);
    console.log(seconds);
    const targetNext = (seconds + 1) * ms + start; // 算出下次interval开始的时间
    const delay = document.timeline
      ? document.timeline.currentTime
      : performance.now(); // 取出更新完UI的时间
    setTimeout(
      () => {
        requestAnimationFrame(timer1);
      },
      targetNext - delay // 算出距离下次interval开始时间
    );
  }
  timer1(start);
}

interval(1000, updateUI);
```

### web worker

通过新建一个线程来执行回调，这样回调函数的执行不受主线程执行队列的阻塞，比setInterval更精确一些。

计算完成后，最终仍要通知主线程执行后续操作。



### 利用服务端修正时间进行倒计时

秒杀场景下，需要服务端修正客户端倒计时时间。

原理是利用一个计时器计时，另一个计时器更新时间变量，对时间进行修正。

demo 如下

```typescript
const interval = 1000  // 计时间隔
const debounce = 3000  // 修正时间，请求接口间隔
const endTime = Date.now() + 5 * 1000  // 计时终点
let now = Date.now()  // 初始时间
let timer1: any = null  // 倒计时计时器
let updateNowTimer: any = null  // 请求接口计时器

// 倒计时计时器
timer1 = setInterval(() => {
  now = now + interval
  const leftT = Math.round((endTime - now) / 1000)

  if (leftT < 0) {
    clearInterval(timer1)
    clearInterval(updateNowTimer)
    return
  }
}, interval)

// 模拟请求接口，更新 now 值
updateNowTimer = setInterval(() => {
  new Promise((resolve) => {
    setTimeout(() => {
      now = Date.now()
      resolve(void 0)
    }, 1000)
  })
}, debounce)
```

当有多个倒计时实例时，只需要在 `updateNowTimer` 中更新多个实例的 now 值即可。

demo 中除了代码不优雅，不好管理计时器外，还存在着一些问题，比如没有考虑多个实例下，何时清除`updateNowTimer` 计时器等问题。



# ⚝webWorker

Web Worker 是 HTML5 标准的一部分，这一规范定义了一套 API，**它允许一段 JavaScript 程序运行在主线程之外的另外一个线程中。**

这并不意味着 `JavaScript `语言本身就支持了多线程，**对于JavaScript 语言本身它仍是运行在单线程上的， Web Worker 只是浏览器（宿主环境）提供的一个能力／API。**

WebWorker 允许在主线程之外再创建一个 worker 线程，**在主线程执行任务的同时，worker 线程也可以在后台执行它自己的任务，互不干扰**。

## 应用场景

1. 数学运算

2. 图像、影音等文件处理

3. 大量数据检索

   比如用户输入时，我们在后台检索答案，或者帮助用户联想，纠错等操作.

4. 耗时任务都丢到 webworker 解放我们的主线程。

## 主线程可以

##### 1. 主线程与 worker 线程通信:

##### 2. 监听 worker **线程返回的信息**

##### 3. 主线程关闭 worker 线程

##### 4. 监听错误

## Worker 线程可以

##### 监听主线程传过来的信息：

##### 发送信息给主线程

##### worker 线程关闭自身

##### worker 线程加载脚本

## ⚝Worker 限制

因为 worker 创造了另外一个线程，不在主线程上，相应的会有一些限制，我们无法使用下列对象：

1. window 对象
2. document 对象
3. DOM 对象
4. parent 对象

**我们可以使用下列对象/功能**：

1. 浏览器：navigator 对象
2. URL：location 对象，只读
3. 发送请求：XMLHttpRequest 对象
4. 定时器：setTimeout/setInterval，在 worker 线程轮询也是很棒！
5. 应用缓存：Application Cache

## 其他种类的 worker 

- **Shared Workers** 可被**不同的窗体的多个脚本运行**，例如 IFrames 等，只要这些 workers 处于同一主域。共享 worker 比专用 worker 稍微复杂一点 — 脚本必须通过活动端口进行通讯。详情请见[`SharedWorker`](https://developer.mozilla.org/zh-CN/docs/Web/API/SharedWorker)。
- [Service Workers](https://developer.mozilla.org/zh-CN/docs/Web/API/Service_Worker_API) 一般作为 web 应用程序、浏览器和网络（如果可用）之间的代理服务。他们旨在（除开其他方面）创建有效的**离线体验**，拦截网络请求，以及根据网络是否可用采取合适的行动，更新驻留在服务器上的资源。他们还将允许访问推送通知和后台同步 API。
- Chrome Workers 是一种仅适用于 firefox 的 worker。如果您正在开发附加组件，希望在扩展程序中使用 worker 且可以访问 [js-ctypes](https://developer.mozilla.org/en/js-ctypes)，那么可以使用 Chrome Workers。详情请见`ChromeWorker`
- 音频 [Workers](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Audio_API#audio_workers)可以在网络 worker 上下文中直接完成脚本化音频处理。



# 浏览器渲染过程

### 浏览器主要组件结构

*![](pics/浏览器组件.png)*

#### **渲染引擎**

常见的浏览器内核可以分为这四种：Trident（IE）、Gecko（火狐）、Blink（Chrome、Opera）、Webkit（Safari）

### webkit渲染引擎流程

*![](pics/渲染引擎流程.png)*

#### 渲染过程

**1、解析：**浏览器会解析三个东西

- 一是HTML/SVG/XHTML，HTML字符串描述了一个页面的结构，浏览器会把HTML结构字符串解析转换DOM树形结构。

- 二是CSS，解析CSS会产生CSS规则树，它和DOM结构比较像。
- 三是Javascript脚本，等到Javascript 脚本文件加载后， 通过 DOM API 和 CSSOM API 来操作 DOM树 和 CSS规则树。

**2、构建渲染树**：解析完成后，浏览器引擎会通过DOM树 和 CSS规则树 来构造 渲染树。

- Rendering Tree 渲染树并不等同于DOM树，渲染树只会包括需要显示的节点和这些节点的样式信息。
- CSS 的 Rule Tree主要是为了完成匹配并把CSS Rule附加到DOM渲染树上的每个Element（也就是每个Frame）。
- 然后，计算每个Frame 的位置，这又叫layout和reflow过程。

**3、布局和绘制**

#### 构建DOM树

浏览器会遵守一套步骤将HTML 文件转换为 DOM 树。宏观上，可以分为几个步骤：

*![](pics/构建dom.png)*

- 浏览器从磁盘或网络读取HTML的原始字节，并根据文件的指定编码（例如 UTF-8）将它们转换成字符串。

在网络中传输的内容其实都是 0 和 1 这些字节数据。当浏览器接收到这些字节数据以后，它会将这些字节数据转换为字符串，也就是我们写的代码。

- 将字符串转换成Token，例如：`<html>`、`<body>`等。**Token中会标识出当前Token是“开始标签”或是“结束标签”亦或是“文本”等信息**。

节点与节点之间的关系维护由Token要标识“起始标签”和“结束标签”等标识的作用。例如“title”Token的起始标签和结束标签之间的节点肯定是属于“head”的子节点。

- 生成节点对象并构建DOM

事实上，构建DOM的过程中，不是等所有Token都转换完成后再去生成节点对象，而是一边生成Token一边消耗Token来生成节点对象。换句话说，每个Token被生成后，会立刻消耗这个Token创建出节点对象。**注意：带有结束标签标识的Token不会创建节点对象。**

#### 构建CSSOM

DOM会捕获页面的内容，但浏览器还需要知道页面如何展示，所以需要构建CSSOM。

构建CSSOM的过程与构建DOM的过程非常相似，当浏览器接收到一段CSS，浏览器首先要做的是识别出Token，然后构建节点并生成CSSOM。

*![](pics/cssom.png)*

在这一过程中，浏览器会确定下每一个节点的样式到底是什么，并且这一过程其实是很消耗资源的。因为样式你可以自行设置给某个节点，也可以通过继承获得。在这一过程中，浏览器得递归 CSSOM 树，然后确定具体的元素到底是什么样式。

**注意：CSS匹配HTML元素是一个相当复杂和有性能问题的事情。所以，DOM树要小，CSS尽量用id和class，千万不要过渡层叠下去**。

#### 构建渲染树

当我们生成 DOM 树和 CSSOM 树以后，就需要将这两棵树组合为渲染树。

*![](pics/渲染树.png)*

在这一过程中，不是简单的将两者合并就行了。**渲染树只会包括需要显示的节点和这些节点的样式信息**，如果某个节点是 `display: none` 的，那么就不会在渲染树中显示。

**浏览器如果渲染过程中遇到JS文件怎么处理**？

渲染过程中，如果遇到`<script>`就停止渲染，执行 JS 代码。因为浏览器有GUI渲染线程与JS引擎线程，为了防止渲染出现不可预期的结果，这两个线程是互斥的关系。 JavaScript的加载、解析与执行会阻塞DOM的构建，也就是说，在构建DOM时，HTML解析器若遇到了JavaScript，那么它会暂停构建DOM，将控制权移交给JavaScript引擎，等JavaScript引擎运行完毕，浏览器再从中断的地方恢复DOM构建。

也就是说，如果你想首屏渲染的越快，就越不应该在首屏就加载 JS 文件，这也是都建议将 script 标签放在 body 标签底部的原因。当然在当下，并不是说 script 标签必须放在底部，因为你可以给 script 标签添加 defer 或者 async 属性（下文会介绍这两者的区别）。

**JS文件不只是阻塞DOM的构建，它会导致CSSOM也阻塞DOM的构建**。

原本DOM和CSSOM的构建是互不影响，井水不犯河水，但是一旦引入了JavaScript，CSSOM也开始阻塞DOM的构建，只有CSSOM构建完毕后，DOM再恢复DOM构建。

这是什么情况？

这是因为JavaScript不只是可以改DOM，它还可以更改样式，也就是它可以更改CSSOM。因为不完整的CSSOM是无法使用的，如果JavaScript想访问CSSOM并更改它，那么在执行JavaScript时，必须要能拿到完整的CSSOM。所以就导致了一个现象，如果浏览器尚未完成CSSOM的下载和构建，而我们却想在此时运行脚本，那么浏览器将延迟脚本执行和DOM构建，直至其完成CSSOM的下载和构建。也就是说，**在这种情况下，浏览器会先下载和构建CSSOM，然后再执行JavaScript，最后在继续构建DOM**。

*![](pics/js构建阻塞.png)*

#### 布局与绘制

当**浏览器生成渲染树以后**，就会根据渲染树来进行**布局**（也可以叫做**回流**）。这一阶段浏览器要做的事情是要弄清楚各个节点在页面中的确切位置和大小。通常这一行为也被称为“自动重排”。

布局流程的输出是一个“盒模型”，它会精确地捕获每个元素在视口内的确切位置和尺寸，**所有相对测量值都将转换为屏幕上的绝对像素。**

**布局完成后**，浏览器会立即发出“Paint Setup”和“Paint”事件，将**渲染树**转换成屏幕上的像素。

**绘制**（Paint）：遍历渲染树，调用渲染器的 `paint()` 方法在屏幕上绘制出节点内容，本质上是一个像素填充的过程。这个过程也出现于回流或一些不影响布局的 CSS 修改引起的屏幕局部重画，这时候它被称为**重绘**（Repaint）。实际上，**绘制过程是在多个层上完成的**，这些层我们称为**渲染层**（RenderLayer）。

**渲染层合成**（Composite）：多个绘制后的渲染层按照恰当的重叠顺序进行合并，而后生成位图，最终通过显卡展示到屏幕上。



#### 什么是渲染层合并 (Composite) ?

渲染层合并,**对于页面中 DOM 元素的绘制(Paint)是在多个层上进行的。**

在每个层上完成绘制过程之后,浏览器会将绘制的位图发送给 GPU 绘制到屏幕上,将所有层按照合理的顺序合并成一个图层,然后在屏幕上呈现。

对于有位置重叠的元素的页面,这个过程尤其重要,因为一旦图层的合并顺序出错,将会导致元素显示异常。

*![](pics/渲染层合并.png)*

**RenderObjects** 保持了树结构对应的是**DOM树中的node**。

RenderLayers **渲染层**,这是负责对应 DOM 子树。**一个或多个RenderObject对应一个RenderLayer（多对一）**

GraphicsLayers **图形层**,这是负责对应 RenderLayers子树。**一个或多个RenderLayer对应一个GraphicsLayer（多对一）**

**RenderObjects** 通过向一个绘图上下文（GraphicsContext）发出必要的绘制调用来绘制 nodes。

**每个 GraphicsLayer 都有一个 GraphicsContext,GraphicsContext 会负责输出该层的位图,位图是存储在共享内存中,作为纹理上传到 GPU 中,最后由 GPU 将多个位图进行合成,然后 draw 到屏幕上,此时,我们的页面也就展现到了屏幕上。**

**GraphicsContext 绘图上下文的责任就是向屏幕进行像素绘制**(这个过程是先把像素级的数据写入位图中,然后再显示到显示器),在 chrome 里,绘图上下文是包裹了的 Skia（chrome 自己的 2d 图形绘制库）

**某些特殊的渲染层会被认为是合成层（Compositing Layers）,合成层拥有单独的 GraphicsLayer,而其他不是合成层的渲染层,则和其第一个拥有 GraphicsLayer 父层公用一个。**

#### 合成层的优点

一旦 renderLayer 提升为了合成层就会有自己的绘图上下文,并且会开启**硬件加速,有利于性能提升**。

- 合成层的位图,**会交由 GPU 合成**,比 CPU 处理要快 (提升到合成层后合成层的位图会交 GPU 处理,但请注意,仅仅只是合成的处理（把绘图上下文的位图输出进行组合）需要用到 GPU,生成合成层的位图处理（绘图上下文的工作）是需要 CPU。)
- **当需要 repaint 时,只需要 repaint 本身,不会影响到其他的层** (当需要 repaint 的时候可以只 repaint 本身,不影响其他层,但是 paint 之前还有 style, layout,那就意味着即使合成层只是 repaint 了自己,但 style 和 layout 本身就很占用时间。)
- 对于 transform 和 opacity 效果,不会触发 layout 和 paint (仅仅是 transform 和 opacity 不会引发 layout 和 paint,其他的属性不确定。)

一般一个元素开启硬件加速后会变成合成层,可以独立于普通文档流中,改动后可以避免整个页面重绘,提升性能。

注意不能滥用 GPU 加速,一定要分析其实际性能表现。因为 GPU 加速创建渲染层是有代价的,每创建一个新的渲染层,就意味着新的内存分配和更复杂的层的管理。并且在移动端 GPU 和 CPU 的带宽有限制,创建的渲染层过多时,合成也会消耗跟多的时间,随之而来的就是耗电更多,内存占用更多。过多的渲染层来带的开销而对页面渲染性能产生的影响,甚至远远超过了它在性能改善上带来的好处。



#### defer 和 async 属性的区别：

*![](pics/async和defer对比.png)*

其中蓝色线代表JavaScript加载；红色线代表JavaScript执行；绿色线代表 HTML 解析。

1）情况1`<script src="script.js"></script>`

没有 defer 或 async，浏览器会立即加载并执行指定的脚本，也就是说不等待后续载入的文档元素，读到就加载并执行。

2）情况2`<script async src="script.js"></script>`  (**异步下载**)

async 属性表示异步执行引入的 JavaScript，与 defer 的区别在于，如果**已经加载好，就会开始执行**——无论此刻是 HTML 解析阶段还是 DOMContentLoaded 触发之后。需要注意的是，这种方式加载的 JavaScript **依然会阻塞 load 事件**。换句话说，async-script 可能在 DOMContentLoaded 触发之前或之后执行，但一定在 load 触发之前执行。

3）情况3 `<script defer src="script.js"></script>`(**延迟执行**)

defer 属性表示延迟执行引入的 JavaScript，即这段 JavaScript 加载时 HTML 并未停止解析，这两个过程是并行的。整个 **document 解析完毕**且 defer-script 也加载完成之后（这两件事情的顺序无关），才会**执行**所有由 defer-script 加载的 JavaScript 代码，**然后触发 DOMContentLoaded 事件**。

defer 与相比普通 script，有两点区别：**载入 JavaScript 文件时不阻塞 HTML 的解析，执行阶段被放到 HTML 标签解析完成之后。 在加载多个JS脚本的时候，async是无顺序的加载，而defer是有顺序的加载**

#### 为什么操作 DOM 慢

把 DOM 和 JavaScript 各自想象成一个岛屿，它们之间用收费桥梁连接。——《高性能 JavaScript》

因为 DOM 是属于渲染引擎中的东西，而 JS 又是 JS 引擎中的东西。当我们用 JS 去操作 DOM 时，本质上是 **JS 引擎和渲染引擎之间进行了“跨界交流**”。这个“跨界交流”的实现并不简单，它依赖了桥接接口作为“桥梁”。

过“桥”要收费——这个开销本身就是不可忽略的。我们每操作一次 DOM（不管是为了修改还是仅仅为了访问其值），都要过一次“桥”。过“桥”的次数一多，就会产生比较明显的性能问题。因此“减少 DOM 操作”的建议，并非空穴来风。

#### 回流和重绘

- 重绘：当我们对 DOM 的修改导致了样式的变化、却**并未影响其几何属性**（比如修改了颜色或背景色）时，浏览器不需重新计算元素的几何属性、**直接为该元素绘制新的样式**。
- 回流：当我们对 DOM 的修改引发了 **DOM 几何尺寸、结构的变化**（比如修改元素的宽、高或隐藏元素等）时，浏览器需要重新计算元素的几何属性（其他元素的几何属性和位置也会因此受到影响），然后再将计算的结果绘制出来。这个过程就是回流（也叫重排）

我们知道，当网页生成的时候，至少会渲染一次。在用户访问的过程中，还会不断重新渲染。重新渲染会重复回流+重绘或者只有重绘。 **回流必定会发生重绘，重绘不一定会引发回流**。重绘和回流会在我们设置节点样式时频繁出现，同时也会很大程度上影响性能。**回流所需的成本比重绘高的多，改变父节点里的子节点很可能会导致父节点的一系列回流。**

#### 引起回流重绘的方法

1）常见引起回流属性和方法

任何会**改变元素几何信息(元素的位置和尺寸大小)的操作，都会触发回流**，

- 添加或者删除可见的DOM元素；
- 元素尺寸改变——边距、填充、边框、宽度和高度
- 内容变化，比如用户在input框中输入文字
- 浏览器窗口尺寸改变——resize事件发生时
- 计算 offsetWidth 和 offsetHeight 属性
- 设置 style 属性的值
- 激活`CSS`伪类（例如：`:hover`）

一些常用且会导致回流的属性和方法：

- `clientWidth`、`clientHeight`、`clientTop`、`clientLeft`
- `offsetWidth`、`offsetHeight`、`offsetTop`、`offsetLeft`
- `scrollWidth`、`scrollHeight`、`scrollTop`、`scrollLeft`
- `scrollIntoView()`、`scrollIntoViewIfNeeded()`
- `getComputedStyle()`
- `getBoundingClientRect()`
- `scrollTo()`

*2）常见引起重绘属性和方法*


*![](pics/重绘的方法.png)*

造成回流的属性：

width、height、padding、margin、border、position、top、left、bottom、right、float、clear、text-align、vertical-align、line-height、font-weight、font-size、font-family、overflow、white-space

造成重绘的属性：

color、border-style、border-radius、text-decoration、box-shadow、outline、background

#### 如何减少回流、重绘

##### CSS

- 避免使用 table 布局。
- 尽可能在 DOM 树的最末端改变 class。
- 避免设置多层内联样式。
- 将动画效果应用到 position 属性为 absolute 或 fixed 的元素上。
- 避免使用 CSS 表达式（例如：calc()）。

##### Javascript

- 避免频繁操作样式,最好一次性重写 style 属性,或者将样式列表定义为 class 并一次性更改 class 属性。
- 避免频繁操作 DOM,创建一个 documentFragment,在它上面应用所有 DOM 操作,最后再把它添加到文档中。
- 也可以先为元素设置 display: none,操作结束后再把它显示出来。因为在 display 属性为 none 的元素上进行的 DOM 操作不会引发回流和重绘。
- 避免频繁读取会引发回流/重绘的属性,如果确实需要多次使用,就用一个变量缓存起来。
- 对具有复杂动画的元素使用绝对定位,使它脱离文档流,否则会引起父元素及后续元素频繁回流。

# window.onload 和 DOMContentLoaded 的区别

在js中DOMContentLoaded方法是在**HTML文档**被完全的加载和解析之后才会触发的事件，他并不需要等到（样式表/图像/子框架）加载完成之后再进行。

在看load事件（onload事件），用于检测一个**加载完全**的页面。

### Dom解析过程

1. 解析HTML结构。
2. 加载外部脚本和样式表文件。
3. 解析并执行脚本代码。//js之类的
4. DOM树构建完成。//DOMContentLoaded
5. 加载图片等外部文件。
6. 页面加载完毕。//load
   在第4步的时候`DOMContentLoaded`事件会被触发。
   在第6步的时候`load`事件会被触发。

### ⚝结论

DOMContentLoaded 事件在 **html文档加载完毕，并且 html 所引用的内联 js、以及外链 js 的同步代码都执行完毕后触发**。

当页面 DOM 结构中的 js、css、图片，以及 js **异步加载**的 js、css 、图片**都加载完成**之后，才会触发 load 事件。

注意：

- 页面中引用的js 代码如果有异步加载的 js、css、图片，是会影响 load 事件触发的。
- video、audio、flash 不会影响 load 事件触发。

两者的使用：

```js
//onload
window.onload = function() {
    document.getElementById("bg").style.background="yellow"
}
//DOMContentLoaded
document.addEventListener("DOMContentLoaded", function() {
console.log('DOMContentLoaded回调') // 不兼容老的浏览器，兼容写法见[jQuery中ready与load事件] ，原理看下文
}, false);
```

### 为什么要区分？

开发中我们经常需要给一些元素的事件绑定处理函数。但问题是，如果那个元素还没有加载到页面上，但是绑定事件已经执行完了，是没有效果的。这两个事件大致就是用来避免这样一种情况，将绑定的函数放在这两个事件的回调中，保证能在页面的某些元素加载完毕之后再绑定事件的函数。
当然DOMContentLoaded机制更加合理，因为我们可以容忍图片，flash延迟加载，却不可以容忍看见内容后页面不可交互。



# ES6专区

## const、let和var的区别

见上



## **⚝箭头函数**

箭头函数与普通函数，主要区别包括：

**1.没有 this**

**箭头函数没有 this，所以需要通过查找作用域链来确定 this 的值。**

在某些情况下可以通过这个特性来巧妙的绑定this到想要的对象上。

**因为箭头函数没有 this，所以也不能用 call()、apply()、bind() 这些方法改变 this 的指向**，可以看一个例子：

```
var value = 1;
var result = (() => this.value).bind({value: 2})();
console.log(result); // 
```

**2. 没有 arguments**

箭头函数没有自己的 arguments 对象，这不一定是件坏事，因为箭头函数可以访问外围函数的 arguments 对象：

```
function constant() {
    return () => arguments[0]
}

var result = constant(1);
console.log(result()); // 1
```

那如果我们就是要访问箭头函数的参数呢？

你可以通过命名参数或者 rest 参数的形式访问参数:

```
let nums = (...nums) => nums;
```

**3. 不能通过 new 关键字调用**

JavaScript 函数有两个内部方法：[[Call]] 和 [[Construct]]。

当通过 new 调用函数时，执行 [[Construct]] 方法，创建一个实例对象，然后再执行函数体，将 this 绑定到实例上。

当直接调用的时候，执行 [[Call]] 方法，直接执行函数体。

箭头函数并没有 [[Construct]] 方法，不能被用作构造函数，如果通过 new 的方式调用，会报错。

```
var Foo = () => {};
var foo = new Foo(); // TypeError: Foo is not a constructor
```

**4. 没有 new.target**

因为不能使用 new 调用，所以也没有 new.target 值。

**5. 没有原型**

由于不能使用 new 调用箭头函数，所以也没有构建原型的需求，于是箭头函数也不存在 prototype 这个属性。

```
var Foo = () => {};
console.log(Foo.prototype); // undefined
```

**6. 没有 super**

连原型都没有，自然也不能通过 super 来访问原型的属性，所以箭头函数也是没有 super 的，不过跟 this、arguments、new.target 一样，这些值由外围最近一层非箭头函数决定。

**7.不可以使用yield命令，因此箭头函数不能用作 Generator 函数**

## 什么时候不能使用箭头函数？

#### 1. 对象方法中，不适用箭头函数

在箭头函数中，this 指向的是定义时所在的对象，而不是使用时所在的对象。换句话说，箭头函数没有自己的 this，而是继承父作用域中的 this。

#### 2. 原型方法中，不适用箭头函数

和上面理由一样

#### 3. 构造函数中，**不适用箭头函数**

构造函数是通过 new 关键字来生成对象实例，生成对象实例的过程也是通过构造函数给实例绑定 this 的过程，而箭头函数没有自己的 this。因此不能使用箭头作为构造函数，也就不能通过 new 操作符来调用箭头函数。

#### 4. 动态上下文中的回调函数中不能使用

我们需要给一个按钮添加点击事件：

```
const btn1 = document.getElementById('btn1')
btn1.addEventListener('click', () => {
   this.innerHTML = 'clicked'
})1.2.3.4.
```

箭头函数的 this 指向的是他的父作用域（这里就指向了 window），而不是指向这个button。这时候我们需要使用普通函数才可以。

#### 5. Vue 生命周期和 method 中也不能使用箭头函数

Vue 本质上是一个对象，我们说过对象方法中，不适用箭头函数。他的本质上的和对象方法中，不适用箭头函数是一样的。

## 什么时候用箭头函数

### 1.优化代码

比如你有一个有值的数组，你想去map遍历每一项，这时箭头函数就非常推荐:

```javascript
const words = ['hello', 'WORLD', 'Whatever'];
const downcasedWords = words.map(word => word.toLowerCase());
```

一个及其常见的例子就是返回一个对象的某个值:**实际上箭头函数会直观的保持`this`来自于父一级**:

```javascript
const names = objects.map(object => object.name);
```

### 2.Promise和Promise链

当在编写异步编程时，箭头函数也会让代码更加直观和简洁。
这是箭头函数的理想位置，特别是如果您生成的函数是有状态的，同时想引用对象中的某些内容。

```javascript
this.doSomethingAsync().then((result) => {
  this.storeResult(result);
});
```

### 3.对象转换

箭头函数的另一个常见而且十分有用的地方就是用于封装的对象转换。
例如在`Vue.js`中，有一种通用模式，就是使用`mapState`将`Vuex`存储的各个部分，直接包含到`Vue`组件中。
这涉及到定义一套`mappers`，用于从原对象到完整的转换输出，这在组件问题中实十分有必要的。这一系列简单的转换,使用箭头函数是最合适不过的。比如：

```javascript
export default {
  computed: {
    ...mapState([
      'results',
      'users'
    ])
  }
}
```



## 字符串扩展

#### 新增方法

`ES6`在`String`原型上新增了`includes`方法，用于取代传统的`indexOf`方法。`includes`可直接返回`true`或者`false`，使语义更清晰，更加明确。此外还新增了`startsWith()`、`endWith()`、`padWidth()`、`padEnd()`、`repeat()`等方法，可方便的用于查找，补全字符串。

#### 模板字符串

优点：方便字符串拼接；可以折行



## *函数扩展*

#### *函数参数默认值* 

*ES6 之前，不能直接为函数的参数指定默认值，只能采用变通的方法。*

*ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面。*

*参数变量是默认声明的，所以不能用`let`或`const`再次声明。*

*使用参数默认值时，函数不能有同名参数。*

```javascript
// 不报错
function foo(x, x, y) {
  // ...
}

// 报错
function foo(x, x, y = 1) {
  // ...
}
```

*另外，一个容易忽略的地方是，参数默认值不是传值的，而是每次都重新计算默认值表达式的值。也就是说，参数默认值是惰性求值的。*

```javascript
let x = 99;
function foo(p = x + 1) {
  console.log(p);
}

foo() // 100

x = 100;
foo() // 101
```

*上面代码中，参数`p`的默认值是`x + 1`。这时，每次调用函数`foo()`，都会重新计算`x + 1`，而不是默认`p`等于 100。*



#### *rest 参数*

*ES6 引入 rest 参数（形式为`...变量名`），用于获取函数的多余参数，这样就不需要使用`arguments`对象了。**rest 参数搭配的变量是一个数组，该变量将多余的参数放入数组中**。*

```javascript
function add(...values) {
  let sum = 0;

  for (var val of values) {
    sum += val;
  }

  return sum;
}

add(2, 5, 3) // 10
```



## 数组的扩展

#### 扩展运算符

扩展运算符（spread）是三个点（`...`）。它好比 rest 参数的逆运算，将**一个数组转为用逗号分隔的参数序列**。

```javascript
console.log(...[1, 2, 3])
// 1 2 3

console.log(1, ...[2, 3, 4], 5)
// 1 2 3 4 5

[...document.querySelectorAll('div')]
// [<div>, <div>, <div>]
```

**运算符可用于函数调用。**

```javascript
function push(array, ...items) {
  array.push(...items);
}

function add(x, y) {
  return x + y;
}

const numbers = [4, 38];
add(...numbers) // 42
```

上面代码中，`array.push(...items)`和`add(...numbers)`这两行，都是函数的调用，它们都使用了扩展运算符。该运算符将一个数组，变为参数序列。

扩展运算符与正常的函数参数可以结合使用。

```javascript
function f(v, w, x, y, z) { }
const args = [0, 1];
f(-1, ...args, 2, ...[3]);
```

**扩展运算符后面还可以放置表达式。**

```javascript
const arr = [
  ...(x > 0 ? ['a'] : []),
  'b',
];
```

如果扩展运算符后面是一个空数组，则不产生任何效果。

```javascript
[...[], 1]
// [1]
```

注意，只有函数调用时，扩展运算符才可以放在圆括号中，否则会报错。

**实现了 Iterator 接口的对象**

任何定义了遍历器（Iterator）接口的对象（参阅 Iterator 一章），都可以用扩展运算符转为真正的数组。比如**Map 和 Set 结构，Generator 函数**

#### Array.from()

`Array.from()`方法用于**将两类对象转为真正的数组**：类似数组的对象（array-like object）和可遍历（**iterable**）的对象（包括 ES6 新增的数据结构 Set 和 Map）。

下面是一个类似数组的对象，`Array.from()`将它转为真正的数组。

```javascript
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};

// ES5 的写法
var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c']

// ES6 的写法
let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
```

实际应用中，常见的类似数组的对象是 DOM 操作返回的 NodeList 集合，以及函数内部的`arguments`对象。`Array.from()`都可以将它们转为真正的数组。

```javascript
// NodeList 对象
let ps = document.querySelectorAll('p');
Array.from(ps).filter(p => {
  return p.textContent.length > 100;
});

// arguments 对象
function foo() {
  var args = Array.from(arguments);
  // ...
}
```

上面代码中，`querySelectorAll()`方法返回的是一个类似数组的对象，可以将这个对象转为真正的数组，再使用`filter()`方法。

**只要是部署了 Iterator 接口的数据结构，`Array.from()`都能将其转为数组。**

#### Array.of()

`Array.of()`方法用于将**一组值，转换为数组**。

```javascript
Array.of(3, 11, 8) // [3,11,8]
Array.of(3) // [3]
Array.of(3).length // 1
```

#### 新增方法

新增了`find`方法，用于取代传统的`indexOf`,此外还新增了`copyWithin()`, `includes()`, `fill()`,`flat()`等方法，可方便的用于数组的查找、补全、转换等。

**flat()，flatMap()**

数组的成员有时还是数组，`Array.prototype.flat()`用于将嵌套的数组“拉平”，变成一维的数组。该方法返回一个新数组，对原数据没有影响。

**flat()默认只会“拉平”一层**，如果想要“拉平”多层的嵌套数组，可以将`flat()`方法的参数写成一个整数，表示想要拉平的层数，默认为1。

如果原数组有空位，`flat()`方法会跳过空位。



`flatMap()`方法对原数组的每个成员执行一个函数（相当于执行`Array.prototype.map()`），然后对返回值组成的数组执行`flat()`方法。该方法返回一个新数组，不改变原数组。

```javascript
// 相当于 [[2, 4], [3, 6], [4, 8]].flat()
[2, 3, 4].flatMap((x) => [x, x * 2])
// [2, 4, 3, 6, 4, 8]
```

`flatMap()`只能展开一层数组。

```javascript
// 相当于 [[[2]], [[4]], [[6]], [[8]]].flat()
[1, 2, 3, 4].flatMap(x => [[x * 2]])
// [[2], [4], [6], [8]]
```

上面代码中，遍历函数返回的是一个双层的数组，但是默认只能展开一层，因此`flatMap()`返回的还是一个嵌套数组。

`flatMap()`方法的参数是一个遍历函数，该函数可以接受三个参数，分别是当前数组成员、当前数组成员的位置（从零开始）、原数组。

```javascript
arr.flatMap(function callback(currentValue[, index[, array]]) {
  // ...
}[, thisArg])
```

`flatMap()`方法还可以有第二个参数，用来绑定遍历函数里面的`this`。



## *数值的扩展*

#### ***Number.isFinite(), Number.isNaN()***

*ES6 在`Number`对象上，新提供了`Number.isFinite()`和`Number.isNaN()`两个方法。*

*`Number.isFinite()`用来检查一个数值是否为有限的（finite），即不是`Infinity`。*

```javascript
Number.isFinite(15); // true
Number.isFinite(0.8); // true
Number.isFinite(NaN); // false
Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false
Number.isFinite('foo'); // false
Number.isFinite('15'); // false
Number.isFinite(true); // false
```

*注意，如果参数类型不是数值，`Number.isFinite`一律返回`false`。*

*`Number.isNaN()`用来检查一个值是否为`NaN`。*

```javascript
Number.isNaN(NaN) // true
Number.isNaN(15) // false
Number.isNaN('15') // false
Number.isNaN(true) // false
Number.isNaN(9/NaN) // true
Number.isNaN('true' / 0) // true
Number.isNaN('true' / 'true') // true
```

*如果参数类型不是`NaN`，`Number.isNaN`一律返回`false`。*

#### ***`Math`对象***

*上新增了`Math.cbrt()`，`trunc()`，`hypot()`等等较多的科学计数法运算方法，可以更加全面的进行立方根、求和立方根等等科学计算*

***BigInt 数据类型***

*JavaScript 所有数字都保存成 64 位浮点数，这给数值的表示带来了两大限制。一是数值的精度只能到 53 个二进制位（相当于 16 个十进制位），大于这个范围的整数，JavaScript 是无法精确表示，这使得 JavaScript 不适合进行科学和金融方面的精确计算。二是大于或等于2的1024次方的数值，JavaScript 无法表示，会返回`Infinity`。*

*[ES2020](https://github.com/tc39/proposal-bigint) 引入了一种新的数据类型 BigInt（大整数），来解决这个问题，这是 ECMAScript 的第八种数据类型。BigInt 只用来表示整数，没有位数的限制，任何位数的整数都可以精确表示。*

***转换规则***

*可以使用`Boolean()`、`Number()`和`String()`这三个方法，将 BigInt 可以转为布尔值、数值和字符串类型。*

***数学运算***

*数学运算方面，BigInt 类型的`+`、`-`、`*`和`**`这四个二元运算符，与 Number 类型的行为一致。除法运算`/`会舍去小数部分，返回一个整数。*



#### ***数值分隔符***

*欧美语言中，较长的数值允许每三位添加一个分隔符（通常是一个逗号），增加数值的可读性。比如，`1000`可以写作`1,000`。*

*[ES2021](https://github.com/tc39/proposal-numeric-separator)，允许 JavaScript 的数值使用下划线（`_`）作为分隔符。*



## *对象的扩展*

*`ES6`直接以**变量形式声明对象属性或方法**，比传统的键值对方式更加简洁更加方便*

*ES6 允许在大括号里面，直接写入变量和函数，作为对象的属性和方法。这样的书写更加简洁。*

```javascript
const foo = 'bar';
const baz = {foo};
baz // {foo: "bar"}

// 等同于
const baz = {foo: foo};
```

*上面代码中，变量`foo`直接写在大括号里面。这时，属性名就是变量名, 属性值就是变量值。下面是另一个例子。*

```javascript
function f(x, y) {
  return {x, y};
}

// 等同于

function f(x, y) {
  return {x: x, y: y};
}

f(1, 2) // Object {x: 1, y: 2}
```

*除了属性简写，方法也可以简写。*

```javascript
const o = {
  method() {
    return "Hello!";
  }
};

// 等同于

const o = {
  method: function() {
    return "Hello!";
  }
};
```

*对象的**解构赋值***

*对象的**扩展运算符***

#### *对象方法*

1. *super关键字，`ES6`在`class`新增了类似`this`的关键词`super`。同`this`当前函数所在的对象不同，`super`总是指向当前函数所在对象的原型对象*
2. *`ES6`在`Object`原型上新增了`is`方法，做两个目标对象的相等比较。用来完善===方法，'==='方法中`NaN === NaN //false` 其实是不合理的，`Object.is`修复了这个小bug。*
3. *`ES6`在`Object`原型上新增了`assign()`方法，**用于对象新增属性或者多个对象合并**。*
4. *`ES6`在`Object`原型上新增了`getOwnPropertyDescriptors()`方法，此方法增强了`ES5`中`getOwnPropertyDescriptor()`方法，可以获取指定对象所有自身属性的描述对象。结合`defineProperties()`方法，可以完美复制对象，包括复制`get`和`set`属性*
5.  *`ES6`在`Object`原型上新增了`getPrototypeOf()`和`setPrototypeOf()`方法，用来获取或设置当前对象的`prototype`对象。这个方法存在的意义在于，`ES5`中获取设置`prototype`对像是通过`__proto__`属性来实现的，然而`__proto__`属性并不是`ES`规范中的明文规定的属性，只是浏览器各大产商“私自”加上去的属性，只不过因为适用范围广而被默认使用了，再非浏览器环境中并不一定就可以使用，所以为了稳妥起见，获取或设置当前对象的`prototype`对象时，都应该采用`ES6`新增的标准用法。*
6. *`ES6`在`Object`原型上还新增了`Object.keys()`，`Object.values()`，`Object.entries()`方法，用来获取对象的所有键、所有值和所有键值对数组*



## Symbol

ES6 引入了一种新的原始数据类型`Symbol`，表示**独一无二的值**。

Symbol 值通过`Symbol()`函数生成。这就是说，**对象的属性名现在可以有两种类型，一种是原来就有的字符串，另一种就是新增的 Symbol 类型。凡是属性名属于 Symbol 类型，就都是独一无二的，可以保证不会与其他属性名产生冲突。**

Symbol()函数不能和new关键字一起作为构造函数使用；

ES6中引入一批常用内置符号，用于暴露语言内部行为，这些内部行为就是以Symbol工厂函数字符串的形式存在的，比如@@iterator就是Symbol.iterator，instaceof就是Symbol.iterator等

```javascript
let s = Symbol();

typeof s
// "symbol"
//即使是传入相同的参数，生成的 symbol 值也是不相等的，
const foo = Symbol('foo');
const bar = Symbol('foo');
console.log(foo === bar); // false

//Symbol.for方法可以检测上下文中是否已经存在使用该方法且相同参数创建的 symbol 值，如果存在则返回已经存在的值，如果不存在则新建。
const s1 = Symbol.for('foo');
const s2 = Symbol.for('foo');
console.log(s1 === s2); // true

//Symbol.keyFor 方法返回一个使用 Symbol.for 方法创建的 symbol 值的 key
const foo = Symbol.for("foo");
const key = Symbol.keyFor(foo);
console.log(key) // "foo"
```

对象中`Symbol()`属性不能被`for...in`遍历，但是也不是私有属性

### 使用场景

1、作为**对象属性** 当一个复杂对象中含有多个属性的时候，很容易将某个属性名覆盖掉，利用 Symbol 值作为属性名就能保证不会出现同名的属性，可以很好的避免这一现象。

2、定义常量，用唯一的值作为**标志**

3、在对象中存放自定义元数据

对象中的元数据可以理解为 —— 相对次要的，不影响对象内容的特殊属性。

对象的主要内容和属性是那些可以由 Object.getOwnProperties() 获取到的属性。

自定义元数据可以

1. 给目标对象添加标记，以便做特殊处理
2. 作为对象内部的一个“私有”属性

因为使用for-in、**Object**.**keys**(obj)、Object.getOwnPropertyNames打印不出 类型为Symbol属性

## Proxy

#### 简介

Proxy`是`ES6新增的一个构造函数，可以理解为JS语言的一个代理，用来**改变JS默认的一些语言行为**，包括拦截默认的`get/set`等底层方法，使得JS的使用自由度更高，可以最大限度的满足开发者的需求。比如通过拦截对象的`get/set`方法，可以轻松地定制自己想要的`key`或者`value`。下面的例子可以看到，随便定义一个`myOwnObj`的`key`,都可以变成自己想要的函数

**Proxy是构造函数，它有两个参数target和handler**

target是用Proxy包装的目标对象（可以是任何类型的对象，包括原生数组，函数，甚至另一个代理）。

handler是一个对象，其属性是当执行一个操作时定义代理的行为的函数。

`Proxy`只有一个静态方法`revocable(target, handler)`可以用来创建一个可撤销的代理对象。两个参数和构造函数的相同。它返回一个包含了所生成的代理对象本身以及该代理对象的撤销方法的对象。

*下面是 Proxy 支持的拦截操作一览，一共 13 种。*

- ***get(target, propKey, receiver)**：拦截对象属性的读取，比如`proxy.foo`和`proxy['foo']`。*
- ***set(target, propKey, value, receiver)**：拦截对象属性的设置，比如`proxy.foo = v`或`proxy['foo'] = v`，返回一个布尔值。*
- ***has(target, propKey)**：拦截`propKey in proxy`的操作，返回一个布尔值。*
- ***deleteProperty(target, propKey)**：拦截`delete proxy[propKey]`的操作，返回一个布尔值。*
- ***ownKeys(target)**：拦截`Object.getOwnPropertyNames(proxy)`、`Object.getOwnPropertySymbols(proxy)`、`Object.keys(proxy)`、`for...in`循环，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而`Object.keys()`的返回结果仅包括目标对象自身的可遍历属性。*
- ***getOwnPropertyDescriptor(target, propKey)**：拦截`Object.getOwnPropertyDescriptor(proxy, propKey)`，返回属性的描述对象。*
- ***defineProperty(target, propKey, propDesc)**：拦截`Object.defineProperty(proxy, propKey, propDesc）`、`Object.defineProperties(proxy, propDescs)`，返回一个布尔值。*
- ***preventExtensions(target)**：拦截`Object.preventExtensions(proxy)`，返回一个布尔值。*
- ***getPrototypeOf(target)**：拦截`Object.getPrototypeOf(proxy)`，返回一个对象。*
- ***isExtensible(target)**：拦截`Object.isExtensible(proxy)`，返回一个布尔值。*
- ***setPrototypeOf(target, proto)**：拦截`Object.setPrototypeOf(proxy, proto)`，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截。*
- ***apply(target, object, args)**：拦截 Proxy 实例作为函数调用的操作，比如`proxy(...args)`、`proxy.call(object, ...args)`、`proxy.apply(...)`。*
- ***construct(target, args)**：拦截 Proxy 实例作为构造函数调用的操作，比如`new proxy(...args)`。*

#### this指向的问题

Proxy对象可以对我们的目标对象进行访问，但没有做任何拦截时，也不能保证与目标对象的行为一致，因为**目标对象内部的this会自动改变为Proxy代理对象**。

```
const obj={
 name:'_island',
 foo:function(){
   return this === objProxy
 }
}

const objProxy=new Proxy(obj,{})
console.log(obj.foo()); // false
console.log(objProxy.foo()); // true
```

## Proxy 与 Object.defineProperty 优劣对比

### Proxy的优势如下:

- `Proxy`可以直接监听对象而非属性；
- `Proxy`可以直接监听数组的变化；
- `Proxy`有多达13种拦截方法,不限于`apply、ownKeys、deleteProperty、has`等等是`Object.defineProperty`不具备的；
- `Proxy`返回的是一个新对象,我们可以只操作新的对象达到目的,而`Object.defineProperty`只能遍历对象属性直接修改；
- `Proxy`作为新标准将受到浏览器厂商重点持续的性能优化，也就是传说中的新标准的性能红利；

### Object.defineProperty的优势如下:

- 兼容性好（支持IE9）

`Object.defineProperty (obj, prop, descriptor)` 的问题主要有三个：

- **无法监听数组的变化**
  `Vue` 把会修改原来数组的方法定义为变异方法。
  变异方法例如 `push、pop、shift、unshift、splice、sort、reverse`等，是无法触发 `set` 的。
  非变异方法，例如 `filter，concat，slice` 等，它们都不会修改原始数组，而会返回一个新的数组。
  `Vue` 的做法是**把这些变异方法重写来实现监听数组变化。**
- 必须[遍历](https://so.csdn.net/so/search?q=遍历&spm=1001.2101.3001.7020)对象的每个属性
  使用 `Object.defineProperty` 多数情况下要配合 `Object.keys` 和遍历，于是就多了一层嵌套。
  并且由于遍历的原因，假如对象上的某个属性并不需要“劫持”，但此时依然会对其添加“劫持”。
- 必须深层遍历嵌套的对象
  当一个对象为深层嵌套的时候，必须进行逐层遍历，直到把每个对象的每个属性都调用 `Object.defineProperty()` 为止。



## Reflect

`Reflect`是一个内置的对象，**它提供拦截 JavaScript 操作的方法。Reflect不是一个函数对象，因此它是不可构造的。`Reflect`的所有的方法都是静态的**就和`Math`一样，目前它**还没有静态属性。**

主要作用有两点，一是将原生的一些零散分布在Object、Function或者全局函数里的方法(如`apply`、`delete`、`get`、`set`等等)，统一整合到`Reflect`上，这样可以更加方便更加统一的**管理一些原生API**。其次就是因为`Proxy`可以改写默认的原生API，如果一旦原生`API`别改写可能就找不到了，所以`Reflect`也可以起到**备份原生API**的作用，使得即使原生`API`被改写了之后，也可以在被改写之后的`API`用上默认的API

`Reflect`对象一共有 13 个静态方法。

- Reflect.**get**(target, name, receiver)：对应**获取属性值**，如`obj.a`，不存在则返回`undefined`。这里需要注意第三个参数`receiver`，若获取的属性是`getter`，那么`getter`的`this`会指向`receiver`对象。
- Reflect.**set**(target, name, value, receiver)：对应**设置属性值**，如`obj.a = 'value'`。`receiver`对象同样影响`setter`的`this`指向。
- Reflect.**has**(obj, name)：对应`**in**`**运算符**，如`'a' in obj`。
- Reflect.**deleteProperty**(obj, name)：对应`**delete**`**运算符**，如`delete obj.a`。方法返回布尔值，若删除成功或被删除的属性不存在，返回`true`；删除失败，即被删除的属性依然存在（`[[configurable]] = false`），返回`false`。
- Reflect.**construct**(target, args)：对应`**new**`**运算符**，如`new Fn(...args)`。
- Reflect.**getPrototypeOf**(obj)：对应`**Object.getPrototypeOf()**`。两者差别在于当参数为非对象时，`Object.getPrototypeOf()`会被"包装"成对象后传入；而`Reflect.getPrototypeOf()`会报错。
- Reflect.**setPrototypeOf**(obj, newProto)：对应`**Object.setPrototypeOf()**`。方法返回布尔值，表示是否设置成功。
- Reflect.**apply**(func, thisArg, args)：对应`**Function.prototype.apply.call()**`。
- Reflect.**defineProperty**(target, propKey, attributes)：对应**`Object.defineProperty()`**。方法返回布尔值，表示是否定义成功。
- Reflect.**getOwnPropertyDescriptor**(target, propKey)：对应`**Object.getOwnPropertyDescriptor()**`。
- Reflect.**isExtensible**(target)：对应**`Object.isExtensible()`**。
- Reflect.**preventExtensions**(target)：对应**`Object.preventExtensions()`**。
- Reflect.**ownKeys**(target)：对应**`Object.getOwnPropertyNames()`**与**`Object.getOwnPropertySymbols()`**。



## Set、Map、WeakSet、WeakMap

### Set

> ES6 提供了新的数据结构 Set（集合）。

`Set`本身是一个构造函数，用来生成 Set 数据结构。

`Set` 对象允许你存储任何类型的唯一值，无论是**基本类型**或者是**对象引用**。

`Set`对象是**值的集合**，实现了 iterator接口，所以可以使用**扩展运算符**和`for…of…`进行遍历，你可以按照**插入的顺序**迭代它的元素。

Set中的元素只会**出现一次**，即 Set 中的元素是唯一的。

#### 属性和方法

##### 属性

Set 结构的实例有以下属性。

- `Set.prototype.constructor`：构造函数，默认就是`Set`函数。
- `Set.prototype.size`：返回`Set`实例的成员总数。

##### 操作方法

Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。下面先介绍四个操作方法。

- `Set.prototype.add(value)`：添加某个值，返回 Set 结构本身。
- `Set.prototype.delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。
- `Set.prototype.has(value)`：返回一个布尔值，表示该值是否为`Set`的成员。
- `Set.prototype.clear()`：清除所有成员，没有返回值。

##### 遍历操作

Set 结构的实例有四个遍历方法，可以用于遍历成员。

- `Set.prototype.keys()`：返回**键名**的遍历器
- `Set.prototype.values()`：返回**键值**的遍历器
- `Set.prototype.entries()`：返回**键值对**的遍历器
- `Set.prototype.forEach()`：使用回调函数遍历每个成员

需要特别指出的是，`Set`的遍历顺序就是**插入顺序**。这个特性有时非常有用，比如使用 Set 保存一个回调函数列表，调用时就能保证按照添加顺序调用。 **（1）`keys()`，`values()`，`entries()`**

`keys`方法、`values`方法、`entries`方法返回的都是遍历器对象。由于 Set 结构没有键名，**只有键值（或者说键名和键值是同一个值）** ，所以`keys`方法和`values`方法的行为完全一致。

```js
let set = new Set(['red', 'green', 'blue']);

for (let item of set.keys()) {
  console.log(item);
}
// red green blue

for (let item of set.values()) {
  console.log(item);
}
// red green blue

for (let item of set.entries()) {
  console.log(item);
}
// ["red", "red"] ["green", "green"] ["blue", "blue"]
```

上面代码中，`entries`方法返回的遍历器，同时包括键名和键值，所以每次输出一个数组，它的两个成员完全相等。

Set 结构的实例默认可遍历，它的默认遍历器生成函数就是它的`values`方法。

```js
Set.prototype[Symbol.iterator] === Set.prototype.values
// true
```

这意味着，可以省略`values`方法，直接用`for...of`循环遍历 Set。

```js
let set = new Set(['red', 'green', 'blue']);

for (let x of set) {
  console.log(x);
}
// red green blue
```

**（2）`forEach()`**

Set 结构的实例与数组一样，也拥有`forEach`方法，用于对每个成员执行某种操作，没有返回值。

```js
let set = new Set([1, 4, 9]);
set.forEach((value, key) => console.log(key + ' : ' + value))
// 1 : 1
// 4 : 4
// 9 : 9
```

上面代码说明，`forEach`方法的参数就是一个处理函数。该函数的参数与数组的`forEach`一致，依次为键值、键名、集合本身（上例省略了该参数）。这里需要注意，Set 结构的键名就是键值（两者是同一个值），因此第一个参数与第二个参数的值永远都是一样的。

另外，`forEach`方法还可以有第二个参数，表示绑定处理函数内部的`this`对象。

**（3）遍历的应用**

扩展运算符（`...`）内部使用`for...of`循环，所以也可以用于 Set 结构。

```js
let set = new Set(['red', 'green', 'blue']);
let arr = [...set];
// ['red', 'green', 'blue']
```

而且，数组的`map`和`filter`方法也可以**间接**用于 Set 了。

```js
let set = new Set([1, 2, 3]);
set = new Set([...set].map(x => x * 2));
// 返回Set结构：{2, 4, 6}

let set = new Set([1, 2, 3, 4, 5]);
set = new Set([...set].filter(x => (x % 2) == 0));
// 返回Set结构：{2, 4}
```

#### Set应用

##### 去重

> 数组去重

`Set`函数可以接受一个数组（或者具有 `iterable` 接口的其他数据结构）作为参数，用来初始化。

```js
/**
 * set 集合
 * 成员的值都是唯一的
 */
const unique = arr =>{
    const res = new Set(arr);
    //通过扩展运算符拆分再放入数组中
    return [...res];
}
```

`Array.from`方法可以将 Set 结构转为数组。

```js
const items = new Set([1, 2, 3, 4, 5]);
const array = Array.from(items);
```

这就提供了**去除数组重复成员的另一种方法**。

```js
function dedupe(array) {
  return Array.from(new Set(array));
}

dedupe([1, 1, 2, 3]) // [1, 2, 3]
```

> 去除字符串里面的重复字符

```js
[...new Set('ababbc')].join('')
// "abc"
```

注：`NaN`和`undefined`也可以存储在Set中。所有`NaN`值都是相等的(即NaN被认为与NaN相同，即使`NaN !== NaN`)。

##### 并集、交集和差集

```js
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
// set {2, 3}

// （a 相对于 b 的）差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}
```

### WeakSet

和Set结构类似，也是不重复的值的集合，但WeakSet的成员**只能是对象。**

其次，WeakSet 中的对象都是**弱引用**，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，**如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中。**

这是因为垃圾回收机制根据对象的可达性（reachability）来判断回收，如果对象还能被访问到，垃圾回收机制就不会释放这块内存。结束使用该值之后，有时会忘记取消引用，导致内存无法释放，进而可能会引发内存泄漏。WeakSet 里面的引用，都不计入垃圾回收机制，所以就不存在这个问题。因此，**WeakSet 适合临时存放一组对象，以及存放跟对象绑定的信息。只要这些对象在外部消失，它在 WeakSet 里面的引用就会自动消失。**

由于上面这个特点，WeakSet 的成员是不适合引用的，因为它会随时消失。另外，由于 WeakSet 内部有多少个成员，取决于垃圾回收机制有没有运行，运行前后很可能成员个数是不一样的，而垃圾回收机制何时运行是不可预测的，因此 **ES6 规定 WeakSet 不可遍历。**

```js
let obj1 ={name:"leslie1"};
let obj2 ={name:"leslie2"};
let ws = new WeakSet();
let s = new Set();
ws.add(obj1);
s.add(obj2)
console.log(ws);
console.log(s);
```

[![HY66a9.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/02571826612c4b349fa755a24b9e094e~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)](https://link.juejin.cn/?target=https%3A%2F%2Fimgtu.com%2Fi%2FHY66a9)

```js
let obj1 ={name:"leslie1"};
let obj2 ={name:"leslie2"};
let ws = new WeakSet();
let s = new Set();
ws.add(obj1);
s.add(obj2)
// console.log(ws);
// console.log(s);
obj1=null;
obj2=null;
console.log(ws);
console.log(s);
```

[![HY6rb4.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c425ca8eb9de4f129ac202f35478c204~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)](https://link.juejin.cn/?target=https%3A%2F%2Fimgtu.com%2Fi%2FHY6rb4)

作为构造函数，**WeakSet 可以接受一个数组或类似数组的对象作为参数**。（实际上，任何具有 Iterable 接口的对象，都可以作为 WeakSet 的参数。）**该数组的所有成员**，都会自动成为 WeakSet 实例对象的成员。**数组的成员只能是对象**。

#### 实例方法

- `WeakSet.prototype.add(value)`

  向`WeakSet`对象追加`value`。返回带有附加值的`WeakSet`对象。

- `WeakSet.prototype.delete(value)`

  移除与该`value`关联的元素，并返回一个布尔值，判断元素是否被成功移除。`WeakSet.prototype.has(value)`之后将返回`false`。

- `WeakSet.prototype.has(value)`

  返回一个布尔值，断言对象中是否存在具有给定值的元素`WeakSet`。

**注意：WeakSet没有size属性，因为它不可遍历/迭代。**

**WeakSet 不能遍历，是因为成员都是弱引用，随时可能消失，遍历机制无法保证成员的存在，很可能刚刚遍历结束，成员就取不到了。**

#### WeakSet 应用场景

JavaScript垃圾回收是一种内存管理技术。在这种技术中，不再被引用的对象会被自动删除，而与其相关的资源也会被一同回收。

Map和Set中对象的引用都是强类型化的，并不会允许垃圾回收。这样一来，如果Map和Set中引用了不再需要的大型对象，如已经从DOM树中删除的DOM元素，那么其回收代价是昂贵的。

为了解决这个问题，ES6还引入了另外两种新的数据结构，即称为WeakMap和WeakSet的弱集合。这些集合之所以是“**弱的**”，是因为它们允许从内存中清除不再需要的被这些集合所引用的对象。

使用场景：**储存 DOM 节点，而不用担心这些节点从文档移除时，会引发内存泄漏**。

```html
<div id="wrap">
    <buttton id="btn1">确认1</buttton>
    <buttton id="btn2">确认2</buttton>
    <buttton id="btn">确认</buttton>
</div>
<script>
    let wrap = document.getElementById('wrap');
    let btn1 = document.getElementById('btn1');
    let btn2 = document.getElementById('btn2');
    const ws = new WeakSet();
    ws.add(btn2);
    console.log(ws)    
</script>
```

[![HY6c5R.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6e568cd0b33e4077ae762c018795954d~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)](https://link.juejin.cn/?target=https%3A%2F%2Fimgtu.com%2Fi%2FHY6c5R)

注意：**浏览器的垃圾回收可能不是立刻执行**。



### Set和WeakSet的区别

- Set允许存储任何类型的唯一值，无论是**基本类型**或者是**对象引用**。WeakSet 中的元素**只能是对象**，不能是其他类型的值。
- Set是强引用的；WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果该对象不在被其他变量引用，那么垃圾回收机制就会自动回收该对象所占用内存，所以只要 WeakSet 成员对象在外部消失，它们在 WeakSet 里面的引用就会自动消失。
- 由于 WeakSet 内部有多少个成员，取决于垃圾回收机制有没有运行，运行前后很可能成员个数是不一样的，而垃圾回收机制何时运行是不可预测的，因此 **ES6 规定 WeakSet 不可遍历，也没有size属性**。

### Map

**`Map` 对象保存键值对，并且能够记住键的原始插入顺序。任何值(对象或者基础类型) 都可以作为一个键或一个值。**

> 描述

一个Map对象在迭代时会根据对象中元素的**插入顺序**来进行 — 一个 [`for...of`](https://link.juejin.cn/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FJavaScript%2FReference%2FStatements%2Ffor...of) 循环在每次迭代后会返回一个形式为`[key，value]`的数组。

> 键的相等(Key equality)

- 键的比较是基于 `sameValueZero` 算法：
- [`NaN`](https://link.juejin.cn/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FJavaScript%2FReference%2FGlobal_Objects%2FNaN) 是与 `NaN` 相等的（虽然 `NaN !== NaN`），剩下所有其它的值是根据 `===` 运算符的结果判断是否相等。
- 在目前的ECMAScript规范中，`-0`和`+0`被认为是相等的，尽管这在早期的草案中并不是这样。

#### 实例的属性和操作方法

Map 结构的实例有以下属性和操作方法。

**（1）size 属性**

`size`属性返回 Map 结构的成员总数。

**（2）Map.prototype.set(key, value)**

`set`方法设置键名`key`对应的键值为`value`，然后返回整个 Map 结构。如果`key`已经有值，则键值会被更新，否则就新生成该键。

`set`方法返回的是当前的`Map`对象，因此可以采用链式写法。

```js
let map = new Map()
  .set(1, 'a')
  .set(2, 'b')
  .set(3, 'c');
```

**（3）Map.prototype.get(key)**

`get`方法读取`key`对应的键值，如果找不到`key`，返回`undefined`。

**（4）Map.prototype.has(key)**

`has`方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。

**（5）Map.prototype.delete(key)**

`delete`方法删除某个键，返回`true`。如果删除失败，返回`false`。

**（6）Map.prototype.clear()**

`clear`方法清除所有成员，没有返回值。

#### 遍历方法

Map 结构原生提供三个遍历器生成函数和一个遍历方法。

- `Map.prototype.keys()`：返回**键名**的遍历器。
- `Map.prototype.values()`：返回**键值**的遍历器。
- `Map.prototype.entries()`：返回**所有成员**的遍历器。
- `Map.prototype.forEach()`：遍历 Map 的所有成员。

需要特别注意的是，Map 的遍历顺序就是**插入**顺序。

Map 结构转为数组结构，比较快速的方法是使用扩展运算符（`...`）。

结合数组的`map`方法、`filter`方法，可以实现 Map 的遍历和过滤（Map 本身没有`map`和`filter`方法）。

```js
const map0 = new Map()
  .set(1, 'a')
  .set(2, 'b')
  .set(3, 'c');

const map1 = new Map(
  [...map0].filter(([k, v]) => k < 3)
);
// 产生 Map 结构 {1 => 'a', 2 => 'b'}

const map2 = new Map(
  [...map0].map(([k, v]) => [k * 2, '_' + v])
    );
// 产生 Map 结构 {2 => '_a', 4 => '_b', 6 => '_c'}
```

此外，Map 还有一个`forEach`方法，与数组的`forEach`方法类似，也可以实现遍历。

```js
map.forEach(function(value, key, map) {
  console.log("Key: %s, Value: %s", key, value);
});
```

`forEach`方法还可以接受第二个参数，用来绑定`this`。

```js
const reporter = {
  report: function(key, value) {
    console.log("Key: %s, Value: %s", key, value);
  }
};

map.forEach(function(value, key, map) {
  this.report(key, value);
}, reporter);
```

上面代码中，`forEach`方法的回调函数的`this`，就指向`reporter`。

### WeakMap

`WeakMap`结构与`Map`结构类似，也是用于生成键值对的集合。

`WeakMap`与`Map`的区别有两点。

首先，`WeakMap`**只接受对象**作为键名（`null`除外），不接受其他类型的值作为键名。

其次，`WeakMap`的**键名**所指向的对象，**不计入垃圾回收机制。**

`WeakMap`的设计目的在于，有时我们想在某个对象上面存放一些数据，但是这会形成对于这个对象的引用。请看下面的例子。

```js
const e1 = document.getElementById('foo');
const e2 = document.getElementById('bar');
const arr = [
  [e1, 'foo 元素'],
  [e2, 'bar 元素'],
];
```

上面代码中，`e1`和`e2`是两个对象，我们通过`arr`数组对这两个对象添加一些文字说明。这就形成了`arr`对`e1`和`e2`的引用。

一旦不再需要这两个对象，我们就必须手动删除这个引用，否则垃圾回收机制就不会释放`e1`和`e2`占用的内存。

```js
// 不需要 e1 和 e2 的时候
// 必须手动删除引用
arr [0] = null;
arr [1] = null;
```

上面这样的写法显然很不方便。一旦忘了写，就会造成内存泄露。

WeakMap 就是为了解决这个问题而诞生的，它的键名所引用的对象都是弱引用，即垃圾回收机制不将该引用考虑在内。因此，**只要所引用的对象的其他引用都被清除，垃圾回收机制就会释放该对象所占用的内存。也就是说，一旦不再需要，WeakMap 里面的键名对象和所对应的键值对会自动消失，不用手动删除引用。**

基本上，如果你要往对象上添加数据，又不想干扰垃圾回收机制，就可以使用 WeakMap。一个典型应用场景是，在网页的 DOM 元素上添加数据，就可以使用`WeakMap`结构。当该 DOM 元素被清除，其所对应的`WeakMap`记录就会自动被移除。

```js
const wm = new WeakMap();

const element = document.getElementById('example');

wm.set(element, 'some information');
wm.get(element) // "some information"
```

上面代码中，先新建一个 WeakMap 实例。然后，将一个 DOM 节点作为键名存入该实例，并将一些附加信息作为键值，一起存放在 WeakMap 里面。这时，WeakMap 里面对`element`的引用就是弱引用，不会被计入垃圾回收机制。

也就是说，上面的 DOM 节点对象除了 WeakMap 的弱引用外，其他位置对该对象的引用一旦消除，该对象占用的内存就会被垃圾回收机制释放。WeakMap 保存的这个键值对，也会自动消失。

总之，`WeakMap`的专用场合就是，它的键所对应的对象，可能会在将来消失。`WeakMap`结构有助于防止内存泄漏。

注意，WeakMap **弱引用的只是键名**，而不是键值。键值依然是正常引用。

```html
<div id="wrap">
    <buttton id="btn1">确认1</buttton>
    <buttton id="btn2">确认2</buttton>
    <buttton id="btn">确认</buttton>
</div>
<script>
    let wrap = document.getElementById('wrap');
    let btn1 = document.getElementById('btn1');
    let btn2 = document.getElementById('btn2');
    const wm = new WeakMap();
    wm.set(btn2, 'some information');
    wm.get(btn2) // "some information"
    console.log(wm.get(btn2))    
</script>
```

[![HY62P1.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0c1dd1800a454a28b8b2945ae04926ba~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)](https://link.juejin.cn/?target=https%3A%2F%2Fimgtu.com%2Fi%2FHY62P1)

注意：**WeakMap的键所对应的对象在垃圾回收时可能不是立刻执行的**。

#### 语法

WeakMap 与 Map 在 API 上的区别主要是两个，一是**没有遍历操作**（即没有`keys()`、`values()`和`entries()`方法），也没有`size`属性。因为没有办法列出所有键名，某个键名是否存在完全不可预测，跟**垃圾回收机制是否运行相关**。这一刻可以取到键名，下一刻垃圾回收机制突然运行了，这个键名就没了，为了防止出现不确定性，就统一规定不能取到键名。二是**无法清空**，即不支持`clear`方法。因此，`WeakMap`只有四个方法可用：`get()`、`set()`、`has()`、`delete()`。

#### 用途

##### DOM 节点作为键名

前文说过，WeakMap 应用的典型场合就是 DOM 节点作为键名。下面是一个例子。

```js
let myWeakmap = new WeakMap();

myWeakmap.set(
  document.getElementById('logo'),
  {timesClicked: 0})
;

document.getElementById('logo').addEventListener('click', function() {
  let logoData = myWeakmap.get(document.getElementById('logo'));
  logoData.timesClicked++;
}, false);
```

上面代码中，`document.getElementById('logo')`是一个 DOM 节点，每当发生`click`事件，就更新一下状态。我们将这个状态作为键值放在 WeakMap 里，对应的键名就是这个节点对象。一旦这个 DOM 节点删除，该状态就会自动消失，不存在内存泄漏风险。

##### 部署私有属性

WeakMap 的另一个用处是**部署私有属性**。

```js
const _counter = new WeakMap();
const _action = new WeakMap();

class Countdown {
  constructor(counter, action) {
    _counter.set(this, counter);
    _action.set(this, action);
  }
  dec() {
    let counter = _counter.get(this);
    if (counter < 1) return;
    counter--;
    _counter.set(this, counter);
    if (counter === 0) {
      _action.get(this)();
    }
  }
}

const c = new Countdown(2, () => console.log('DONE'));

c.dec()
c.dec()
// DONE
```

上面代码中，`Countdown`类的两个内部属性`_counter`和`_action`，是实例的弱引用，所以如果删除实例，它们也就随之消失，不会造成内存泄漏。

### 针对深拷贝循环引用的问题

以下是一个**循环引用**（circular reference）的对象：

```js
const foo = { name: 'Frankie' }
foo.bar = foo
```

使用**WeakMap**解决

首先，Map 的键属于强引用，而 WeakMap 的键则属于弱引用。且 WeakMap 的键必须是对象，WeakMap 的值则是任意的。

由于它们的键与值的引用关系，决定了 Map 不能确保其引用的对象不会被垃圾回收器回收的引用。假设我们使用的 Map，那么 `foo` 对象和我们深拷贝内部的 `const map = new Map()` 创建的 `map` 对象一直都是强引用关系，那么在程序结束之前，`foo` 不会被回收，其占用的内存空间一直不会被释放。

相比之下，原生的 WeakMap 持有的是每个键对象的“弱引用”，这意味着在没有其他引用存在时**垃圾回收能正确进行**。原生 WeakMap 的结构是特殊且有效的，其用于映射的 key 只有在其没有被回收时才是有效的。

基本上，如果你要往对象上添加数据，又不想干扰垃圾回收机制，就可以使用 WeakMap。



### Map和WeakMap的区别

- Map允许存储任何类型的唯一值，无论是**基本类型**或者是**对象引用**，WeakMap 只接受对象作为键名（不包括null）

- **Map是强引用的；WeakMap是弱引用**，也就是说，如果该对象不在被其他变量引用，那么垃圾回收机制就会自动回收该对象所占用内存，所以只要 WeakMap 成员对象在外部消失，它们在 WeakMap 里面的引用就会自动消失。
- 由于 WeakMap 内部有多少个成员，取决于垃圾回收机制有没有运行，运行前后很可能成员个数是不一样的，而垃圾回收机制何时运行是不可预测的，因此 **ES6 规定 WeakMap 不可遍历**。**也无法清空**，即不支持`clear`方法。因此，`WeakMap`只有四个方法可用：`get()`、`set()`、`has()`、`delete()`。



## *Iterator*

*JavaScript 原有的表示“集合”的数据结构，主要是数组（`Array`）和对象（`Object`），ES6 又添加了`Map`和`Set`。这样就有了四种数据集合，用户还可以组合使用它们，定义自己的数据结构，比如数组的成员是`Map`，`Map`的成员是对象。这样就需要一种统一的接口机制，来处理所有不同的数据结构。*

**遍历器（Iterator）就是这样一种机制。它是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 Iterator 接口，就可以完成遍历操作**（即依次处理该数据结构的所有成员）。

*Iterator 的作用有三个：一是为各种数据结构，提供一个统一的、简便的访问接口；二是使得数据结构的成员能够按某种次序排列；三是 ES6 创造了一种新的遍历命令`for...of`循环，Iterator 接口主要供`for...of`消费。*

*Iterator 的遍历过程是这样的。*

*（1）创建一个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是一个指针对象。*

*（2）第一次调用指针对象的`next`方法，可以将指针指向数据结构的第一个成员。*

*（3）第二次调用指针对象的`next`方法，指针就指向数据结构的第二个成员。*

*（4）不断调用指针对象的`next`方法，直到它指向数据结构的结束位置。*

*每一次调用`next`方法，都会返回数据结构的当前成员的信息。具体来说，就是返回一个包含`value`和`done`两个属性的对象。其中，`value`属性是当前成员的值，`done`属性是一个布尔值，表示遍历是否结束。*

*iterator和for of*

*Iterator 接口的目的，就是为所有数据结构，提供了一种统一的访问机制，即`for...of`循环（详见下文）。当使用`for...of`循环遍历某种数据结构时，该循环会自动去寻找 Iterator 接口。*

*一种数据结构只要部署了 Iterator 接口，我们就称这种数据结构是“可遍历的”（iterable）。*

*ES6 规定，默认的 Iterator 接口部署在数据结构的`Symbol.iterator`属性，或者说，一个数据结构只要具有`Symbol.iterator`属性，就可以认为是“可遍历的”（iterable）。`Symbol.iterator`属性本身是一个函数，就是当前数据结构默认的遍历器生成函数。执行这个函数，就会返回一个遍历器。至于属性名`Symbol.iterator`，它是一个表达式，返回`Symbol`对象的`iterator`属性，这是一个预定义好的、类型为 Symbol 的特殊值，所以要放在方括号内（参见《Symbol》一章）。*

```javascript
const obj = {
  [Symbol.iterator] : function () {
    return {
      next: function () {
        return {
          value: 1,
          done: true
        };
      }
    };
  }
};
```

*上面代码中，对象`obj`是可遍历的（iterable），因为具有`Symbol.iterator`属性。执行这个属性，会返回一个遍历器对象。该对象的根本特征就是具有`next`方法。每次调用`next`方法，都会返回一个代表当前成员的信息对象，具有`value`和`done`两个属性。*

*ES6 的有些数据结构原生具备 Iterator 接口（比如数组），即不用任何处理，就可以被`for...of`循环遍历。原因在于，这些数据结构原生部署了`Symbol.iterator`属性（详见下文），另外一些数据结构没有（比如对象）。凡是部署了`Symbol.iterator`属性的数据结构，就称为部署了遍历器接口。调用这个接口，就会返回一个遍历器对象。*

*原生具备 Iterator 接口的数据结构如下。*

- *Array*
- *Map*
- *Set*
- *String*
- *TypedArray*
- *函数的 arguments 对象*
- *NodeList 对象*

## Promise

如上

## Generator

如上

## async函数

如上


## *Class和extends*

*产生的原因： 原ES5语法的没有成型的类的概念。而面向对象编程又离不开类的概念。*

*ES5定义一个类:*

```javascript
function Point(x, y) {
this.x = x;
this.y = y;
}
  
var p = new Point(1, 2)
```

*ES6的class:*

```javascript
class Point {
constructor(x, y) {
this.x = x;
this.y = y;
}
}
```

*其中：*

1. *constructor方法是类的默认方法，通过new 命令生成对象时会调用该方法，如果声明类时没有定义constructor，会默认定义一个空的。*
2. *生成实例时必须用new ,不用会报错*
3. *不存在变里提升（选定义类，再new实例）*

#### *类的静态方法：*

*所有在类中定义的方法都会被实例继承，如果不想被继承，可以在定义时加上static。表示为静态方法。*

```javascript
class Foo {
static match() {}
}
Foo.match()
const f = new Foo()
f.match() // 报错
```

#### *类的静态属性*

*很遗憾，ES6没有给类设静态属性，但是可以用以下方法定义(有提案，写方同静态方法)*

```javascript
class Foo {}
Foo.porp = 1
// 使用
Foo.porp // 1
```

#### *类的实例属性*

*类的方法默认被实例继承，那么属性呢？也是继承的，写法如下：*

```javascript
class Foo {
myProp = 111;
...
}
```

#### *class的继承 extends*

```javascript
class Point {}
class PointSon extends Point {
    constructor(x, y, z) {
        super(x, y)
        this.z = z
    }
}
```

*其中：*

1. *super等同于父类的constructor。*
2. *子类必须在constructor中调用super， 也就是说用extends去继承一个类，就必须调用这个类（父类）的constructor。是因为子类没有自己的this对象，而是继承父类的this，然后对其进行加工*
3. *如果了类没有写constructor，会默认生成一个，并包含了super(...args)*



## *解构赋值*

*什么是解构赋值？*

*按照一定模式从**数组**或**对象**中提取值，然后对变量进行赋值（先提取，再赋值）*

#### *数组：*

```javascript
let [a, b] = [1, 2]
// 以下的结果为右边数剩下的值所组成的数组
let [c, d ,...e] = [1, 2, 3, 4]
// 有默认值的写法
let [f = 100] = [] // f = 100
// 其中String也被视为类数组
let [a, b] = 'abcd' // a = a; b = b
```

#### *对象:*

*变理名要与对象的**属性名一样**才可以：*

```gauss
let { foo } = { foo: 1, bar: 2 } // foo = 1
// 重新命名（后面那个才是新变量）
let { foo: newFoo } = { foo: 1, bar: 2 } // newFoo = 1
```

*实际使用：*

1. *交换两个变量的值*

```javascript
[x, y] = [y, x]
```

1. *函数的封装*

```javascript
function fn({ x, y } = {}) {
console.log(x, y)
}
```

*其中，函数参数为一个对象，不会像`(x, y)`的形式这样固定参数的顺序，而`{} = {}`后面又赋一个空的对象就是为了在调用fn时不传参数而不会抛出错误导至程序中止*

1. *函数返回值的解构*

*函数返回多个值*

```javascript
// 有次序的
function foo() {
return [a, b, c]
}
const [a, b, c] = foo()
// 无次序的
function foo() {
return { a, b, c}
}
const { b, a, c} = foo()
```

#### *iterator接口*

*如果等号的右边不是数组（或者严格地说，不是可遍历的结构，参见《Iterator》一章），那么将会报错。*

```javascript
// 报错
let [foo] = 1;
let [foo] = false;
let [foo] = NaN;
let [foo] = undefined;
let [foo] = null;
let [foo] = {};
```

*上面的语句都会报错，因为等号右边的值，要么转为对象以后不具备 Iterator 接口（前五个表达式），要么本身就不具备 Iterator 接口（最后一个表达式）。*

## BigInt

JavaScript的基本数据类型`Number`只能安全存储`-(2 ** 53 - 1)`到`2 ** 53 - 1`（包含边界）的值；

`安全存储`指的是能正确的区分两个值是否相等，比如JavaScript中`2 ** 54 + 1 === 2 ** 54 + 2`为`true`，但是很显然这在数学上是`不对`的，为了能正确的存储所有的值，**ES2020**引入了`BigInt`，可以安全的存储任意的整数；

### 使用

`BigInt`的使用有以下两种方法：

1. 在`Number`类型的整数后面加上`n`；如`10n`；
2. 使用函数`BigInt()`；如`BigInt(10)`；

### 注意点

1. BigInt只能表示`整数`;
2. `BigInt`不能与`Number`类型进行`计算`操作；
3. 不允许在`BigInt`上使用一元加号（`+`）运算符
4. BigInt的`toString`方法和Number的toString方法一样，可以传一个`radius`，用于`转换进制`；
5. 使用`/`对BigInt类型的整数进行计算操作时，会自动`忽略小数`部分；
6. BigInt无法使用`Math`中的方法；
7. BigInt无法使用`JSON.stringify`方法；

# 前端调试方法

## **Console**

Chrome JS调试控制台

## **JS断点调试**

### **Sources断点**

给一段代码添加断点的流程是"F12（Ctrl + Shift + I）打开开发工具"——"点击**Sources源代码菜单**"——"左侧树中找到相应文件"——"点击行号列"即完成在当前行添加/删除断点操作。当断点添加完毕后，刷新页面JS执行到断点位置停住，在Sources界面会看到**当前作用域中所有变量和值**，只需对每个值进行验证即可完成我们题设验证要求。

### **Debugger断点**

在打断点的地方添上debugger，既然除了设置断点的方式不一样，功能和Sources面板添加断点效果一样

### **XHR断点**

我们可以通过"XHR Breakpoints"右侧的"+"号为异步断点添加断点条件，当异步请求触发时的URL满足此条件，JS逻辑则会自动产生断点。

### 事件监听器断点

事件监听器断点，即根据事件名称进行断点设置。当事件被触发时，断点到事件绑定的位置。事件监听器断点，列出了所有页面及脚本事件，包括：鼠标、键盘、动画、定时器、XHR等等。极大的降低了事件方面业务逻辑的调试难度。

# 控制台

### Console面板

#### $家族

$_  ：返回上一个被执行过的值~

$0、$1、$2、$3、$4： 五个指令相当于在Elements面板最近选择过的五个引用

$  ： 类似于`document.querySelector()`

$$  ： 与上面的 `$` 选择器类似，只不过使用的是 `document.querySelectorAll()` 方法。

#### API工具方法

keys/values 见名知意。功能类似于`Object.keys`，`Object.values`

monitor/unmonitor 用来观察函数调用的工具方法

monitorEvents/unmonitorEvents 可以观察对象的事件

getEventListeners 获取注册到一个对象上的所有事件监听器

#### 网络network

显示页面静态资源