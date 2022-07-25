# ES6一些简单语法
## let、const和var的区别

1. var定义的变量，作⽤域是整个封闭函数，是全域的；let定义的变量，作⽤域是在块级或者字块中；
1. 变量提升：不论通过var声明的变量处于当前作用域的第⼏⾏，都会提升到作⽤域的最顶部。⽽let声明的变量不会在顶部初始化，凡是在let声明之前使⽤该变量都会报错（引⽤错误ReferenceError）；
1. 只要块级作⽤域内存在let，它所声明的变量就会绑定在这个区域；
1. let不允许在相同作⽤域内重复声明（报错同时使⽤var和let，两个let）。
1. const⽤来专门声明⼀个常量，它跟let⼀样作⽤于块级作⽤域，没有变量提升，重复声明会报错，不同的是const声明的常量不可改变，声明时必须初始化（赋值）。
```javascript
var x = 10;
// 这里输出 x 为 10
{ 
    let x = 2;
    // 这里输出 x 为 2
}
// 这里输出 x 为 10
```
```javascript
const PI = 3.141592653589793;
PI = 3.14;      // 报错
PI = PI + 10;   // 报错
```
## for...in  &  for...of

- for-in循环主要用于遍历对象，for()中的格式：for(keys in zhangsan){}
- for…of循环可以使用的范围包括数组、Set 和 Map 结构、某些类似数组的对象（比如arguments对象、DOM NodeList 对象）、后文的 Generator 对象，以及字符串
```javascript
// 数组
let arr = [3, 5, 7];
arr.foo = 'hello';
for (let i in arr) {  console.log(i);   }        // "0", "1", "2", "foo"
for (let i of arr) {  console.log(i);   }				 //  "3", "5", "7"

// 对象
let es6 = {edition: 6, committee: "TC39", standard: "ECMA-262" };
for (let e in es6) {  console.log(e);  }					// edition		committee		standard
for (let e of es6) {  console.log(e);  }						// TypeError: es6[Symbol.iterator] is not a function
```
# BOM对象

- BOM对象（浏览器对象模型）
- 对浏览器打开的窗口进行设置
## 计时事件
```javascript
setInterval()   // 间隔指定的毫秒数不停地执行指定的代码。
setTimeout()    // 在指定的毫秒数后执行指定代码。
setInterval(function(){alert("Hello")},3000);		// 每三秒弹出 "hello" 

clearInterval()  // 方法用于停止 setInterval() 方法执行的函数代码。
```

## 一些小方法（不常用）
```javascript
// 获取浏览、屏幕高度宽度
window.innerHeight   // 浏览器窗口的内部高度(包括滚动条)
window.innerWidth    // 浏览器窗口的内部宽度(包括滚动条)
screen.availWidth    // 返回访问者屏幕的宽度，以像素计，减去界面特性，比如窗口任务栏。
screen.availHeight   // 返回访问者屏幕的高度，以像素计，减去界面特性，比如窗口任务栏。

location.hostname  	 // 返回 web 主机的域名
location.pathname    // 返回当前页面的路径和文件名
location.port        // 返回 web 主机的端口 （80 或 443）
location.protocol    // 返回所使用的 web 协议（http: 或 https:）
location.href				 // 返回当前页面的 URL      https://www.runoob.com/js/js-window-location.html
location.pathname		 // 返回 URL 的路径名


history.back() 			 // 与在浏览器点击后退按钮相同
history.forward() 	 //  与在浏览器中点击向前按钮相同
// go() 里面的参数表示跳转页面的个数
history.go(1);       // 例如 history.go(1) 表示前进一个页面
history.go(-1);      // 例如 history.go(-1) 表示后退一个页面
history.go(0);       // go() 里面的参数为0,表示刷新页面
```

## 弹窗

- **警告框	alert()		**
   - 警告框经常用于确保用户可以得到某些信息。
   - 当警告框出现后，用户需要点击确定按钮才能继续进行操作。
```javascript
alert("你好，我是一个警告框！");
```

- **确认框	confirm()**
   - 确认框通常用于验证是否接受用户操作。
   - 当确认框弹出时，用户可以点击 "确认" 或者 "取消" 来确定用户操作。
   - 当你点击 "确认"， 确认框返回 true， 如果点击 "取消"，确认框返回 false。
```javascript
var r=confirm("按下按钮");
if (r==true)
{
    x="你按下了\"确定\"按钮!";
}
else
{
    x="你按下了\"取消\"按钮!";
}
```

- **提示框	prompt()**
   - 提示框经常用于提示用户在进入页面前输入某个值。
   - 当提示框出现后，用户需要输入某个值，然后点击确认或取消按钮才能继续操纵。
   - 如果用户点击确认，那么返回值为输入的值。如果用户点击取消，那么返回值为 null。
```javascript
var person=prompt("请输入你的名字","Harry Potter");
if (person!=null && person!="")
{
    x="你好 " + person + "! 今天感觉如何?";
    document.getElementById("demo").innerHTML=x;
}
```

# DOM对象

- DOM（文档对象模型），当网页被加载时，浏览器会创建页面的文档对象模型。
- 对页面上的显示的元素进行设置
## Collection & NodeList

1. **Collection()   **类似包含 HTML 元素的一个数组
```javascript
// 获取 <p> 元素的集合：
var myCollection = document.getElementsByTagName("p");		// getElementsByTagName 返回 Collection()对象
// 显示集合元素个数：
document.getElementById("demo").innerHTML = myCollection.length;


// length属性常用于遍历集合中的元素
// 修改所有 <p> 元素的背景颜色:
var myCollection = document.getElementsByTagName("p");
for (var i = 0; i < myCollection.length; i++) {
    myCollection[i].style.backgroundColor = "red";
}
```
**注：**Collection 看起来可能是一个数组，但其实**不是数组**。可以像数组一样，使用索引来获取元素。

2. **NodeList()	** 对象是一个从文档中获取的节点列表 (集合) 。
```javascript
// 获取 <p> 元素的集合：
var myNodelist = document.querySelectorAll("p");		// querySelectorAll() 返回 NodeList对象
// 显示节点列表的元素个数：
document.getElementById("demo").innerHTML = myNodelist.length;

// length 属性常用于遍历节点列表。
// 修改节点列表中所有 <p> 元素的背景颜色:
var myNodelist = document.querySelectorAll("p");
for (var i = 0; i < myNodelist.length; i++) {
    myNodelist[i].style.backgroundColor = "red";
}
```
**注：**NodeList() 看起来可能是一个数组，但其实不是数组，可以使用索引来获取元素。

3. **不同点**
```javascript
// querySelectorAll 方法接收的参数是一个 CSS 选择符。而 getElementsBy 系列接收的参数只能是单一的className、tagName 和 name
var c1 = document.querySelectorAll('.b1 .c');
var c2 = document.getElementsByClassName('c');
var c3 = document.getElementsByClassName('b2')[0].getElementsByClassName('c');

// querySelectorAll 方法接收的参数是一个 CSS 选择符。而 getElementsBy 系列接收的参数只能是单一的className、tagName 和 name
try 
{
		var e1 = document.getElementsByClassName('1a2b3c');
		var e2 = document.querySelectorAll('.1a2b3c');
} catch (e) {
		console.error(e.message);
}
console.log(e1 && e1[0].className);
console.log(e2 && e2[0].className);

```
## 元素节点

- **appendChild()**   创建新的Html元素（节点），它用于添加新元素到尾部
```javascript
// 以下代码是用于创建 <p> 元素:
var para = document.createElement("p");

// 为 <p> 元素创建一个新的文本节点：
var node = document.createTextNode("这是一个新的段落。");

// 将文本节点添加到 <p> 元素中：
para.appendChild(node);

// 最后，在一个已存在的元素中添加 p 元素。
// 查找已存在的元素：
var element = document.getElementById("div1");
// 添加到已存在的元素中:
element.appendChild(para);
```

- **insertBefore()**  ** **创建新的Html元素（节点），它用于添加新元素到开始位置
```javascript
var para = document.createElement("p");
var node = document.createTextNode("这是一个新的段落。");
para.appendChild(node);
 
var element = document.getElementById("div1");
var child = document.getElementById("p1");
element.insertBefore(para, child);
```

- **removeChild() **  要移除一个元素，你需要知道该元素的父元素。
```javascript
<div id="div1">
		<p id="p1">这是一个段落。</p>
		<p id="p2">这是另外一个段落。</p>
</div>
<script>
		var parent = document.getElementById("div1");
		var child = document.getElementById("p1");
		parent.removeChild(child);
</script>
```

- **replaceChild()**	使用 replaceChild() 方法来替换 HTML DOM 中的元素
```javascript
<div id="div1">
		<p id="p1">这是一个段落。</p>
		<p id="p2">这是另外一个段落。</p>
</div>
 
<script>
		var para = document.createElement("p");
		var node = document.createTextNode("这是一个新的段落。");
		para.appendChild(node);
		 
		var parent = document.getElementById("div1");
		var child = document.getElementById("p1");
		parent.replaceChild(para, child);
</script>
```
## 监听属性   EventListener

- addEventListener() 方法用于向指定元素添加事件句柄。
- 使用 removeEventListener() 方法来移除 addEventListener() 方法添加的事件句柄。
```javascript
document.getElementById("要绑定的id").addEventListener("要绑定的事件", function(){
    // 要执行的方法
});
// removeEventListener() 方法移除由 addEventListener() 方法添加的事件句柄:
element.removeEventListener("mousemove", myFunction);
```

- element.addEventListener(event， function， useCapture);
- event 必须。字符串，指定事件名。
   - 注意： 不要使用 “on” 前缀。 例如，使用 “click” ，而不是使用 “onclick”。
   - function 必须。指定要事件触发时执行的函数。
   - 当事件对象会作为第一个参数传入函数。 事件对象的类型取决于特定的事件。例如，“click” 事件属于 MouseEvent(鼠标事件) 对象。
   - useCapture 可选。布尔值，指定事件是否在捕获或冒泡阶段执行。
可能值：
true - 事件句柄在捕获阶段执行；
false- false- 默认。事件句柄在冒泡阶段执行；
## 事件

- **事件举例**
   - 当用户点击鼠标时、当网页已加载时、当图像已加载时、当鼠标移动到元素上时、当输入字段被改变时、当提交 HTML 表单时、当用户触发按键时
- **onclick()点击事件**	向button元素分配onclick事件
```javascript
<button onclick="displayDate()">点这里</button>
// displayDate() 声明的方法
document.getElementById("myBtn").onclick=function(){displayDate()};
```

- **onload 和 onunload 事件**
- onload 和 onunload 事件会在用户进入或离开页面时被触发。

onload 事件可用于检测访问者的浏览器类型和浏览器版本，并基于这些信息来加载网页的正确版本。
onload 和 onunload 事件可用于处理 cookie。
```javascript
<body onload="checkCookies()">		// 在页面加载时执行checkCookies()方法
<body onunload="checkCookies()">		// 在页面加载完成时执行checkCookies()方法
```

- **onchage事件**	当用户改变输入字段的内容时，会调用 upperCase() 函数。
```javascript
<input type="text" id="fname" onchange="upperCase()">
// 当内容该改变时会触发upperCase()方法
```

- **onmouseover 和 onmouseout 事件		鼠标移入移出**

onmouseover 和 onmouseout 事件可用于在用户的鼠标移至 HTML 元素上方或移出元素时触发函数。
```javascript
<div onmouseover="this.innerHTML='Thank You'" onmouseout="this.innerHTML='Mouse Over Me'"></div>
// 移入时文字变为 Thank You
// 移出时文字变为 Mouse Over Me
```

- **onmousedown、onmouseup 以及 onclick 事件**
   - onmousedown，onmouseup 以及 onclick 构成了鼠标点击事件的所有部分。
   - 首先当点击鼠标按钮时，会触发 onmousedown 事件。
   - 当释放鼠标按钮时，会触发 onmouseup 事件。
   - 当完成鼠标点击时，会触发 onclick 事件。

## 改变Html页面元素内容 & 相应的css样式

- 向Html中输出流写内容
```javascript
document.write(Date());
```

- 改变Html中的内容
```javascript
document.getElementById("id").innerHTML='新文本'
```

- 改变Html属性
```javascript
document.getElementById("id").src='xxx/aaa.jpg'
// 改变插入图片的路径
```

- 改变相应的css样式
```javascript
document.getElementById("p2").style.color='blue';
document.getElementById("p2").style.fontFamily="Arial";
document.getElementById("p2").style.fontSize="larger";
```
## 查找页面元素

- 通过id查找Html元素
```javascript
var x = document.getElementById("要查找的id");
```

- 通过标签名查找Html元素
```javascript
var x = document.getElementByTagName("要查找的标签名字")
```

- 通过类名查找Html元素
```javascript
var x = document.getElementByClassName("要查找的类名");
```
# js闭包

- 闭包就是函数嵌套函数，内部函数就是闭包。
   - 特性：正常情况__函数执行完成内部变量会销毁；而闭包__内部函数没有执行完成，外部函数变量不会被销毁。
```javascript
// 闭包
function outerfun() {
    let a = 10;//特性执行完销毁
    function innerFun() {//内部函数
        console.log(a);
    }
    return innerFun;
}
let fun = outerfun();/*outerfun作为返回值赋值给fun 没有执行完成
所以有10，如果内部函数被调用那么外部函数不会被销毁 let 有 10*/

fun();//10
```
```javascript
// 应用 
//未封装
let aa = 10;//全局变量
let bb = 20;
function add() {
    return aa + bb;
}
function sub() {
    return aa - bb;
}
let res1 = add();
let res2 = sub();
console.log(res1, res2);//30.-10
```
```javascript
//闭包封装   利用闭包实现模块化的功能
let modouble = (function () {//匿名函数
    let aa = 10;//局部变量
    let bb = 20;
    function add() {
        return aa + bb;
    }
    function sub() {
        return aa - bb;
    }
    return {
        add: add,
        sub: sub,
    }
})()//小括号 声明后直接调用
let ress = modouble.add();
let resss = modouble.sub();
console.log(ress, resss);//30,-10
```
# 常用方法
## 函数中arguments 对象
JavaScript 函数有个内置的对象 arguments 对象。argument 对象包含了函数调用的参数数组。
```javascript
// 查找中间的最大值
x = findMax(1, 123, 500, 115, 44, 88);
 
function findMax() {
    var i, max = arguments[0];
   
    if(arguments.length < 2) return max;
 
    for (i = 0; i < arguments.length; i++) {
        if (arguments[i] > max) {
            max = arguments[i];
        }
    }
    return max;	
}					// 输出 500
```
## 数据类型转换
**数字转字符串**

- String() & num.toString()数字转字符串
```javascript
var num = 123;
console.log(typeof(num));   	// number
var str1 = String(num);
console.log(typeof(str1));		// string
var str2 = num.toString();
console.log(typeof(str2));		// string
```
**字符串转数字**

- Number() & parseFloat()数字转字符串
   - Number()：如果要转换的字符串中有非数字字符会设置为  NaN
   - parseFloat()
      - 会在字符串中第一个非数字字符前停下，把当前字符前的数字输出出来
      - 如果第一个字符为非数字字符，会直接返回  NaN
```javascript
var str = '123.456';
console.log(typeof(str));				// string
var num1 = Number(str);
console.log(typeof(num1));			// number
var num2 = parseFloat(str);
console.log(typeof(num2));			// number

var str = '123.4s56';
console.log(typeof(str));				// string
var num1 = Number(str);
console.log(num1)						// NaN
console.log(typeof(num1));			// number
var num2 = parseFloat(str);
console.log(num2)						// 123.4
console.log(typeof(num2));			// number
```
## typeof()   	检查当前的数据类型

- typeof()   	检查当前的数据类型
```javascript
typeof "John"                 // 返回 string
typeof 3.14                   // 返回 number
typeof NaN                    // 返回 number
typeof false                  // 返回 boolean
typeof [1,2,3,4]              // 返回 object
typeof {name:'John', age:34}  // 返回 object
typeof new Date()             // 返回 object
typeof function () {}         // 返回 function
typeof myCar                  // 返回 undefined (如果 myCar 没有声明)
typeof null                   // 返回 object

var person = null;           // 值为 null(空), 但类型为对象
var person = undefined;     // 值为 undefined, 类型为 undefined
```
## break   continue   return
| break | 
1. break 语句可用于跳出循环。
1. break 语句跳出循环后，会继续执行该循环之后的代码（如果有的话）：
 |
| --- | --- |
|  continue | 
- continue 语句中断当前的循环中的迭代，然后继续循环下一个迭代。
 |
|  return | 
- 跳出当前方法
 |

## while与do-while  循环

- while 循环会在指定条件为真时循环执行代码块。
```javascript
while (i<5)
{
    x=x + "The number is " + i + "<br>";
    i++;
}
```

- do/while 循环是 while 循环的变体。该循环会在检查条件是否为真之前执行一次代码块，然后如果条件为真的话，就会重复这个循环。
```javascript
do
{
    x=x + "The number is " + i + "<br>";
    i++;
}
while (i<5);
```
## switch-case  循环

1. 首先设置表达式 _n_（通常是一个变量）。
1. 随后表达式的值会与结构中的每个 case 的值做比较。
1. 如果存在匹配，则与该 case 关联的代码块会被执行。
- break来阻止代码自动地向下一个 case 运行
- default 关键词来规定匹配不存在时做的事情
```javascript
var d = new Date().getDay();
switch (d) {												// switch(n){
	case 6:														//  		 case 1:
		x = "今天是星期六";							 // 					执行代码块 1
		break;													//          break;
	case 0:														//      case 2:
		x = "今天是星期日";							 //          执行代码块 2
		break;													//          break;
	default:													//			 default:
		x = "期待周末";									 //					 与 case 1 和 case 2 不同时执行的代码
}																	  // }
console.log(x);
```
# 对象
## Map()   哈希
看环境
```javascript
// map()对象保存键值对，并且能够记住键的原始插入顺序。任何值（对象或者基本类型）都可以作为一个键或一个值。
var map = new Map();
map.has( key );   // 检查当前数组中是否包含此数据
map.set( key, value )  // 存储数据
map.get( key, value )	 // 读取数据
map.size	 						 // 获取当前map中所存储数据的数量
map.delete( key )			 // 删除键
```
### map()和Object()的区别
Object和 Map 类似的是，它们都允许你按键存取一个值、删除键、检测一个键是否绑定了值。因此（并且也没有其他内建的替代方式了）过去我们一直都把对象当成 Map 使用。

| 

 | Map | Object |
| --- | --- | --- |
| 意外的键 | Map 默认情况不包含任何键。只包含显式插入的键。 | 一个 Object 有一个原型，原型链上的键名有可能和你自己在对象上的设置的键名产生冲突。
**备注：**虽然从 ES5 开始可以用 [Object.create(null)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create) 来创建一个没有原型的对象，但是这种用法不太常见。 |
| 键的类型 | 一个 Map 的键可以是**任意值**，包括函数、对象或任意基本类型。 | 一个 Object 的键必须是一个 [String](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String) 或是 [Symbol](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol)。 |
| 键的顺序 | Map 中的键是有序的。因此，当迭代的时候，一个 Map 对象以插入的顺序返回键值。 | 虽然 Object 的键目前是有序的，但并不总是这样，而且这个顺序是复杂的。因此，最好不要依赖属性的顺序。
自 ECMAScript 2015 规范以来，对象的属性被定义为是有序的；ECMAScript 2020 则额外定义了继承属性的顺序。参见 [OrdinaryOwnPropertyKeys](https://tc39.es/ecma262/#sec-ordinaryownpropertykeys) 和 [EnumerateObjectProperties](https://tc39.es/ecma262/#sec-enumerate-object-properties) 抽象规范说明。但是，请注意没有可以迭代对象所有属性的机制，每一种机制只包含了属性的不同子集。（[for-in](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in) 仅包含了以字符串为键的属性；[Object.keys](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) 仅包含了对象自身的、可枚举的、以字符串为键的属性；[Object.getOwnPropertyNames](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyNames) 包含了所有以字符串为键的属性，即使是不可枚举的；[Object.getOwnPropertySymbols](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols) 与前者类似，但其包含的是以 Symbol 为键的属性，等等。） |
| Size | Map 的键值对个数可以轻易地通过 [size](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map/size) 属性获取。 | Object 的键值对个数只能手动计算. |
| 迭代 | Map 是 [可迭代的](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols) 的，所以可以直接被迭代。 | Object 没有实现 [迭代协议](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols#the_iterable_protocol)，所以使用 JavaSctipt 的 [for...of](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of) 表达式并不能直接迭代对象。
备注：
- 对象可以实现迭代协议，或者你可以使用 [Object.keys](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) 或 [Object.entries](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)。
- [for...in](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in) 表达式允许你迭代一个对象的可枚举属性。
 |
| 性能 | 在频繁增删键值对的场景下表现更好。 | 在频繁添加和删除键值对的场景下未作出优化。 |
| 序列化和解析 | 没有元素的序列化和解析的支持。
（但是你可以使用携带 replacer 参数的 [JSON.stringify()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 创建一个自己的对 Map 的序列化和解析支持。参见 Stack Overflow 上的提问：[How do you JSON.stringify an ES6 Map?](https://stackoverflow.com/q/29085197/)） | 原生的由 [Object](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object) 到 JSON 的序列化支持，使用 [JSON.stringify()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)。
原生的由 JSON 到 [Object](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object) 的解析支持，使用 [JSON.parse()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)。 |

## Array() 数组对象
### toString()	转化为字符串

- toString()	转换数组到字符串
```javascript
var num1 = ['1','2','3','4','5'];
var num2 = num1.toString();
console.log(num2);  // num2 = 1,2,3,4,5
```
### sort()	数组排序

- sort()	对数组中的内容进行排序
```javascript
var num1 = ['3','2','1','4'];
console.log(num1);		// 输出  3 2 1 4
var num2 = num1.sort();
console.log(num2);		// 输出  1 2 3 4
```
### slice()	 截取数组中的元素

- slice()	在数组中选择元素
```javascript
var num1 = ['1','2','3','4','5'];
var num2 = num1.slice(1,3);
console.log(num2);		// num2 = 2  3
```
### reverse()    反转数组

- reverse()    将一个数组中的元素的顺序反转排序
```javascript
var num1 = ['1','2','3','4'];
var num2 = num1.reverse();
console.log(num2);		// num2 = 4,3,2,1
```
### 删除/增加数组的元素

- pop() 	删除数组的最后一个元素
```javascript
var num1 = ['1','2','3','4','5'];
var num2 = num1.pop();
console.log(num1);   // num2 = 1,2,3,4
```

- shift()	删除数组的第一个元素
```javascript
var num1 = ['1','2','3','4','5'];
var num2 = num1.shift();
console.log(num1);   // num2 = 2,3,4，5
```

- push()	增加数组的最后一个元素
```javascript
var num1 = ['1','2','3','4','5'];
var num2 = num1.push('6')；
console.log(num1);		// num1 = 1,2,3,4,5,6
```

- unshift()	在数组的最前端插入数据
```javascript
var num1 = ['1','2','3'];
var num2 = num1.unshift('4','5','6');
console.log(num1);	// '4', '5', '6', '1', '2', '3'
```
### splice()      删除/插入/替换数组中的元素

1. 删除-用于删除元素，两个参数，第一个参数（要删除第一项的位置），第二个参数（要删除的项数） 
1. 插入-向数组指定位置插入任意项元素。三个参数，第一个参数（起始位置），第二个参数（0），第三个参数（插入的项数） 
1. 替换-向数组指定位置插入任意项元素，同时删除任意数量的项，三个参数。第一个参数（起始位置），第二个参数（删除的项数），第三个参数（插入任意数量的项）
```javascript
var list = [];
list.push(1);
list.push(2);
list.push(3);
console.log(list); // [1, 2, 3]

// 删除
list.splice(0,1);  // 删除  -> 从下标为0开始,项数为1
console.log(list); // [2,3]
list.splice(0,2);  // 删除  -> 从下标为0开始,项数为2
console.log(list); // []

//替换
list.splice(0,1,4); // 替换 -> 从下标为0开始,项数为1的数组元素替换成4
console.log(list);  // [4,2,3]
list.splice(0,2,4); // 替换 -> 从下标为0开始,项数为2的数组元素替换成4(即4,2整体替换成4)
console.log(list);  // [4,3]

//添加
list.splice(1,0,5); // 表示在下标为1处添加一项5
console.log(list);    // [1,5,2,3]    

```

### concat()	将多个数组进行合并

- concat()将多个数组进行合并
```javascript
var A = ["aa", "aaa"];
var B = ["bb", "bbb"];
var C = ["cc", "ccc"];
var abc = A.concat(B, C);
document.write(abc);		// aa,aaa,bb,bbb,cc,ccc
```
### join()   split()	 替换分隔符/将字符串以某个字符分开

- join()方法可以用符不同的[分隔符](https://so.csdn.net/so/search?q=%E5%88%86%E9%9A%94%E7%AC%A6&spm=1001.2101.3001.7020)来构建这个字串。join方法值接受一个参数，即用作分隔符的字符串，然后返回所有数组项的字符串。
- split()方法用于把一个字符串分割成字符串数组。
```javascript
var arr = ["red","yellow","blue"];
var array = [];
array = arr.join(" | ");
console.log(array);    
// 输出结果为： red | yellow | blue。
```
```javascript
var str="How are you doing today?";
var n=str.split(" ");
console.log(n)  // How,are,you,doing,today?
```
## string() 字符串对象
### trim()	移除字符串两端空格

- trim()	移除字符串两端空格
```javascript
var str = '    aBcDe       ';
console.log(str.trim());			// aBcDe
```
### substr() & substring()	截取字符串
**区别：**第一个参数一样是start下标值，第二个参数substr的是长度，substring的是下标值。

- substr()方法可在字符串中抽取从start下标开始的指定数目的字符
```javascript
var str = "abdcdsnfe";
var str1 = str.substr(1, 3);
console.log(str1);		//bdc
```

- substring() 方法用于提取字符串中介于两个指定下标之间的字符。
```javascript
var str = "abdcdsnfe";
var str1 = str.substring(1, 3);
console.log(str1);//bd
```
### concat()	拼接字符串

- concat()	将多个字符串进行拼接
```javascript
var a = '123';
console.log(a);		// 输出  123
var b = 'abc';
console.log(b);		// 输出  abc
var c = '111'
console.log(c);		// 输出  111
var d = a.concat(b,c);
console.log(d);		// 输出  123abc111
```
### indexOf()	获取元素出现的位置

- indexOf() 方法可返回某个指定的字符串值在字符串中首次出现的位置。
- 如果没有找到匹配的字符串则返回 -1。
```javascript
var str="Hello world, welcome to the universe.";
var n=str.indexOf("welcome");
Console.log(n);		// 13
```
### 字符串字母大小写转换

- toUpperCase() 方法用于把字符串转换为大写。
- toLowerCase() 方法用于把字符串转换为小写。
```javascript
var str = 'aBcDe';
console.log(str.toUpperCase());		// ABCDE
console.log(str.toLowerCase());		// abcde

```

## Number()  数字对象

- toFixed()	给数字添加指定小数位数
```javascript
var y = 1000;
var b = y.toFixed(2)
console.log(b)		// 1000.00
```
## Date()  日期对象
**JavaScript 月数是从0至11。10是11月。**
**星期：0-6  以此是周日   周一…周六**

- Date()  获取当日的日期
```javascript
var d = new Date();
document.write(d);		// Fri Jul 22 2022 15:37:52 GMT+0800 (中国标准时间)

var d2 = new Date(79,5,24)
console.log(d2);			// Sun Jun 24 1979 00:00:00 GMT+0800 (中国标准时间)
```

- getFullYear()	获取年份
```javascript
var a = new Date();
var b = a.getFullYear(a);
console.log(b)
```

- getTime()	获取时间戳
```javascript
var a = new Date();			// 从 1970 年 1 月 1 日至今的毫秒数
var b = a.getTime();
console.log(b);			// 获取当前时间的时间戳
```

- setFullYear()		设置具体的日期
```javascript
var a = new Date();
a.setFullYear(1999,1,1)
console.log(a);		// Mon Feb 01 1999 15:49:19 GMT+0800 (中国标准时间)
```


## Math()	 对象
| abs(x) | 返回 x 的绝对值。 |
| --- | --- |
| acos(x) | 返回 x 的反余弦值。 |
| ceil(x) | 对数进行上舍入。 |
| floor(x) | 对 x 进行下舍入。 |
| max(x，y，z，...，n) | 返回 x，y，z，...，n 中的最高值。 |
| min(x，y，z，...，n) | 返回 x，y，z，...，n中的最低值。 |
| pow(x，y) | 返回 x 的 y 次幂。 |
| random() | 返回 0 ~ 1 之间的随机数。 |
| round(x) | 四舍五入。 |

## RegExp()  正则表达式对象

- 语法：`var patt = new RegExp(pattern，modifiers);`
		或者更简单的方式：`var patt = /pattern/modifiers;`
   - pattern（模式） 描述了表达式的模式
   - modifiers(修饰符) 用于指定全局匹配、区分大小写的匹配和多行匹配
#### 修饰符
修饰符用于执行区分大小写和全局匹配:

| 修饰符 | 描述 |
| --- | --- |
| [i](https://www.runoob.com/js/jsref-regexp-i.html) | 执行对大小写不敏感的匹配。 |
| [g](https://www.runoob.com/js/jsref-regexp-g.html) | 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。 |
| m | 执行多行匹配。 |

#### 方括号
方括号用于查找某个范围内的字符：

| 表达式 | 描述 |
| --- | --- |
| [abc] | 查找方括号之间的任何字符。 |
| [^abc] | 查找任何不在方括号之间的字符。 |
| [0-9] | 查找任何从 0 至 9 的数字。 |
| [a-z] | 查找任何从小写 a 到小写 z 的字符。 |
| [A-Z] | 查找任何从大写 A 到大写 Z 的字符。 |
| [A-z] | 查找任何从大写 A 到小写 z 的字符。 |
| [adgk] | 查找给定集合内的任何字符。 |
| [^adgk] | 查找给定集合外的任何字符。 |
| (red&#124;blue&#124;green) | 查找任何指定的选项。 |

#### 元字符
元字符（Metacharacter）是拥有特殊含义的字符：

| 元字符 | 描述 |
| --- | --- |
| [.](https://www.runoob.com/jsref/jsref-regexp-dot.html) | 查找单个字符，除了换行和行结束符。 |
| [\\w](https://www.runoob.com/jsref/jsref-regexp-wordchar.html) | 查找数字、字母及下划线。 |
| [\\W](https://www.runoob.com/jsref/jsref-regexp-wordchar-non.html) | 查找非单词字符。 |
| [\\d](https://www.runoob.com/jsref/jsref-regexp-digit.html) | 查找数字。 |
| [\\D](https://www.runoob.com/jsref/jsref-regexp-digit-non.html) | 查找非数字字符。 |
| [\\s](https://www.runoob.com/jsref/jsref-regexp-whitespace.html) | 查找空白字符。 |
| [\\S](https://www.runoob.com/jsref/jsref-regexp-whitespace-non.html) | 查找非空白字符。 |
| [\\b](https://www.runoob.com/jsref/jsref-regexp-begin.html) | 匹配单词边界。 |
| [\\B](https://www.runoob.com/jsref/jsref-regexp-begin-not.html) | 匹配非单词边界。 |
| \\0 | 查找 NULL 字符。 |
| [\\n](https://www.runoob.com/jsref/jsref-regexp-newline.html) | 查找换行符。 |
| \\f | 查找换页符。 |
| \\r | 查找回车符。 |
| \\t | 查找制表符。 |
| \\v | 查找垂直制表符。 |
| [\\xxx](https://www.runoob.com/jsref/jsref-regexp-octal.html) | 查找以八进制数 xxx 规定的字符。 |
| [\\xdd](https://www.runoob.com/jsref/jsref-regexp-hex.html) | 查找以十六进制数 dd 规定的字符。 |
| [\\uxxxx](https://www.runoob.com/jsref/jsref-regexp-unicode-hex.html) | 查找以十六进制数 xxxx 规定的 Unicode 字符。 |

#### 量词
| 量词 | 描述 |
| --- | --- |
| [n+](https://www.runoob.com/jsref/jsref-regexp-onemore.html) | 匹配任何包含至少一个 n 的字符串。
例如，/a+/ 匹配 "candy" 中的 "a"，"caaaaaaandy" 中所有的 "a"。 |
| [n*](https://www.runoob.com/jsref/jsref-regexp-zeromore.html) | 匹配任何包含零个或多个 n 的字符串。
例如，/bo*/ 匹配 "A ghost booooed" 中的 "boooo"，"A bird warbled" 中的 "b"，但是不匹配 "A goat grunted"。 |
| [n?](https://www.runoob.com/jsref/jsref-regexp-zeroone.html) | 匹配任何包含零个或一个 n 的字符串。
例如，/e?le?/ 匹配 "angel" 中的 "el"，"angle" 中的 "le"。 |
| [n{X}](https://www.runoob.com/jsref/jsref-regexp-nx.html) | 匹配包含 X 个 n 的序列的字符串。
例如，/a{2}/ 不匹配 "candy，" 中的 "a"，但是匹配 "caandy，" 中的两个 "a"，且匹配 "caaandy." 中的前两个 "a"。 |
| [n{X，}](https://www.runoob.com/jsref/jsref-regexp-nxcomma.html) | X 是一个正整数。前面的模式 n 连续出现至少 X 次时匹配。
例如，/a{2，}/ 不匹配 "candy" 中的 "a"，但是匹配 "caandy" 和 "caaaaaaandy." 中所有的 "a"。 |
| [n{X，Y}](https://www.runoob.com/jsref/jsref-regexp-nxy.html) | X 和 Y 为正整数。前面的模式 n 连续出现至少 X 次，至多 Y 次时匹配。
例如，/a{1，3}/ 不匹配 "cndy"，匹配 "candy，" 中的 "a"，"caandy，" 中的两个 "a"，匹配 "caaaaaaandy" 中的前面三个 "a"。注意，当匹配 "caaaaaaandy" 时，即使原始字符串拥有更多的 "a"，匹配项也是 "aaa"。 |
| [n$](https://www.runoob.com/jsref/jsref-regexp-ndollar.html) | 匹配任何结尾为 n 的字符串。 |
| [^n](https://www.runoob.com/jsref/jsref-regexp-ncaret.html) | 匹配任何开头为 n 的字符串。 |
| [?=n](https://www.runoob.com/jsref/jsref-regexp-nfollow.html) | 匹配任何其后紧接指定字符串 n 的字符串。 |
| [?!n](https://www.runoob.com/jsref/jsref-regexp-nfollow-not.html) | 匹配任何其后没有紧接指定字符串 n 的字符串。 |

#### RegExp 对象方法
| 方法 | 描述 |
| --- | --- |
| [compile](https://www.runoob.com/jsref/jsref-regexp-compile.html) | 在 1.5 版本中已废弃。 编译正则表达式。 |
| [exec](https://www.runoob.com/jsref/jsref-exec-regexp.html) | 检索字符串中指定的值。返回找到的值，并确定其位置。 |
| [test](https://www.runoob.com/jsref/jsref-test-regexp.html) | 检索字符串中指定的值。返回 true 或 false。 |
| [toString](https://www.runoob.com/jsref/jsref-regexp-tostring.html) | 返回正则表达式的字符串。 |

#### 支持正则表达式的 String 对象的方法
| 方法 | 描述 | FF | IE |
| --- | --- | --- | --- |
| [search](https://www.runoob.com/js/jsref-search.html) | 检索与正则表达式相匹配的值。 | 1 | 4 |
| [match](https://www.runoob.com/js/jsref-match.html) | 找到一个或多个正则表达式的匹配。 | 1 | 4 |
| [replace](https://www.runoob.com/js/jsref-replace.html) | 替换与正则表达式匹配的子串。 | 1 | 4 |
| [split](https://www.runoob.com/js/jsref-split.html) | 把字符串分割为字符串数组。 | 1 | 4 |

#### RegExp 对象属性
| 属性 | 描述 |
| --- | --- |
| [constructor](https://www.runoob.com/jsref/jsref-regexp-constructor.html) | 返回一个函数，该函数是一个创建 RegExp 对象的原型。 |
| [global](https://www.runoob.com/jsref/jsref-regexp-global.html) | 判断是否设置了 "g" 修饰符 |
| [ignoreCase](https://www.runoob.com/jsref/jsref-regexp-ignorecase.html) | 判断是否设置了 "i" 修饰符 |
| [lastIndex](https://www.runoob.com/jsref/jsref-lastindex-regexp.html) | 用于规定下次匹配的起始位置 |
| [multiline](https://www.runoob.com/jsref/jsref-multiline-regexp.html) | 判断是否设置了 "m" 修饰符 |
| [source](https://www.runoob.com/jsref/jsref-source-regexp.html) | 返回正则表达式的匹配模式 |


