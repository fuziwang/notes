```javascript
从基础到JS-Web-API

全面考虑：JS内置的全局函数和对象有哪些？
Object Array Boolean String Math 

常说的JS（浏览器执行的JS）包含两部分：
1. JS基础知识（ECMA262标准）
2. JS-Web-API（w3c标准）

DOM操作
Document Object Model文档对象模型

题目
1. DOM是哪种基本的数据结构？
树
2. DOM操作的常用API有哪些？
获取DOM节点，以及节点的property和attribute
获取父节点，获取子节点
新增节点，删除节点
3. DOM结点的attribute和property有何区别？
property只是一个JS对象的属性的修改
attribute是对HTML标签属性的修改

知识点：

DOM本质
HTML就是字符串，其中DOM可以理解为：
浏览器把拿到的HTML代码，结构化一个浏览器能识别并且JS可操作的一个模型（生成一个DOM树）而已

DOM节点操作

获取DOM结点：
var div1 = document.getElementById('div1');// 元素
var divList = document.getElementsByTagName('div');// 集合
console.log(divList.length);
console.log(divList[0]);

var containerList = document.getElementsByClassName('.container');// 集合
var pList = document.querySelectorAll('p');// 集合
var p1list = document.querySelector('#p1');// 元素
// 获取的是JS对象，因此JS可操作

porperty(修改的JS对象的标准属性)
var pList = document.querySelectorAll('p');
var p = pList[0];
console.log(p.style.width);// 获取样式
p.style.width = '100px'; // 修改样式
console.log(p.className);// 获取class
p.className = 'p1';// 修改class

// 获取 nodeName和nodeType
console.log(p.nodeName);
console.log(p.nodeType);

attribute(修改的是文档内容标签的属性内容，是死的)
var pList = document.querySelectorAll('p');
var p = pList[0];
p.getAttribute('date-name');
p.setAttribute('date-name','imooc');
p.getAttribute('style');
p.setAttribute('style','font-size:30px;');
<img style="" src=""/> 其中img有两个arrtibute一个是style src


DOM结构操作
DOM是一种树的数据类型
1. 新增节点
var div1 = document.getElementById('div1');
// 添加新节点
var p1 = document.createElement('p');
p1.innerHTML = 'this is p1';
div1.appendChild(p1);// 添加新创建的元素
// 移动已有的节点
var p2 = document.getElementById('p2');
div1.appendChild(p2);// 原有的p2的位置就会消失
2. 获取父元素
var div1 = document.getElementById('div1');
var parent = div1.parentElement;

var child = div1.childNodes;
// div1.childNodes[0].nodeType text 3
// div1.childNodes[0].nodeName p #text
// div1.childNodes[0].nodeType p 1
// div1.childNodes[0].nodeName p P
div1.removeChild(child[0]);
3. 获取子元素
4. 删除节点
```





```javascript
BOM操作
Browser Object Model浏览器对象模型

题目：
1. 如何检测浏览器的类型？
2. 解析url的各部分？

知识点
1. navigator
2. screen
3. location
4. history
```

