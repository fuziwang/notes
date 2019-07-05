## JavaScript基础

+ `JavaScript`在页面中的位置：https://www.cnblogs.com/IcemanZB/p/4118257.html

+ 变量的命名规范

```html
变量名可以任意取名，但要遵循命名规则:
1. 变量必须使用字母、下划线(_)或者美元符($)开始。
2. 然后可以使用任意多个英文字母、数字、下划线(_)或者美元符($)组成。
3. 不能使用JavaScript关键词与JavaScript保留字。
4. 在JS中区分大小写，如变量mychar与myChar是不一样的，表示是两个变量
```

## 常用互动方法

### 基本知识

+ `document.write()` 可用于直接向 HTML 输出流写内容。简单的说就是直接在网页中输出内容。

问题解决：`JavaScript`中如何输出空格 http://www.imooc.com/wiki/view?pid=167

+ `JavaScript`警告：`alert` 消息对话框

+ `JavaScript`确认：`confirm` 消息对话框。`confirm` 消息对话框通常用于允许用户做选择的动作，弹出对话框(包括一个确定按钮和一个取消按钮)。

```javascript
// 语法：
confirm(str);
// 参数说明：
str：在消息对话框中要显示的文本
返回值: Boolean值
// 返回值:
当用户点击"确定"按钮时，返回true
当用户点击"取消"按钮时，返回false
```

+ `JavaScript`提问：`prompt` 消息对话框。`prompt`弹出消息对话框,通常用于询问一些需要与用户交互的信息。弹出消息对话框（包含一个确定按钮、取消按钮与一个文本输入框）。

```javascript
// 语法:
prompt(str1, str2);
// 参数说明：
str1: 要显示在消息对话框中的文本，不可修改
str2：文本框中的内容，可以修改
// 返回值:
1. 点击确定按钮，文本框中的内容将作为函数返回值
2. 点击取消按钮，将返回null
```

+ `JavaScript`打开新窗口：`window.open`。`open()` 方法可以查找一个已经存在或者新建的浏览器窗口。

```JavaScript
// 语法：
window.open([URL], [窗口名称], [参数字符串])
// 参数说明：
URL：可选参数，在窗口中要显示网页的网址或路径。如果省略这个参数，或者它的值是空字符串，那么窗口就不显示任何文档。
窗口名称：可选参数，被打开窗口的名称。
    1.该名称由字母、数字和下划线字符组成。
    2."_top"、"_blank"、"_self"具有特殊意义的名称。
       _blank：在新窗口显示目标网页
       _self：在当前窗口显示目标网页
       _top：框架网页中在上部窗口中显示目标网页
    3.相同 name 的窗口只能创建一个，要想创建多个窗口则 name 不能相同。
    4.name 不能包含有空格。
参数字符串：可选参数，设置窗口参数，各参数用逗号隔开。
```

参数表：

![img](http://img.mukewang.com/52e3677900013d6a05020261.jpg)

例如：打开http://www.imooc.com网站，大小为300px 200px，无菜单，无工具栏，无状态栏，有滚动条窗口。

```html
<script type="text/javascript"> window.open('http://www.imooc.com','_blank','width=300,height=200,menubar=no,toolbar=no, status=no,scrollbars=yes')
</script>
```

+ `JavaScript`关闭窗口：`window.close`。`close()`关闭窗口

```JavaScript
// 用法：
window.close();   //关闭本窗口
或
<窗口对象>.close();   //关闭指定的窗口

// 关闭新建的窗口
var mywin=window.open('http://www.imooc.com'); //将新打的窗口对象，存储在变量mywin中
mywin.close();
// 注意:上面代码在打开新窗口的同时，关闭该窗口，看不到被打开的窗口。
```

### 编程练习

1、新窗口打开时弹出确认框，是否打开

```
提示: 使用 if 判断确认框是否点击了确定，如点击弹出输入对话框，否则没有任何操作。
```

2、通过输入对话框，确定打开的网址，默认为 `http://www.imooc.com/`

3、打开的窗口要求，宽400像素，高500像素，无菜单栏、无工具栏。

```html
<!DOCTYPE html>
<html>
 <head>
  <title> new document </title>  
  <meta http-equiv="Content-Type" content="text/html; charset=gbk"/>   
  <script type="text/javascript">
    function openWindow(){
        var isOpen = confirm("是否打开新窗口");
        if(isOpen == true){
            var url = prompt("请输入要打开的URL地址","");
            if(url === ""){
                url = "http：//www.imooc.com/";
            }
            window.open(url,"_blank","width=400,height=500,menubar=no,toolbar=no");
        }
    }
  </script> 
 </head> 
 <body> 
	  <input type="button" value="新窗口打开网站" onclick="openWindow()" /> 
 </body>
</html>
```

## DOM操作

### DOM概念

文档对象模型`DOM`（`Document Object Model`）定义访问和处理`HTML`文档的标准方法。`DOM` 将`HTML`文档呈现为带有元素、属性和文本的树结构（节点树）。

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8"/>
        <title>demo</title>
    </head>
    <body>
        <h2><a href="http://www.imooc.com"></a></h2>
        <p>对HTML元素进行操作，可添加、改变或移除CSS样式等</p>
        <ul>
            <li>JavaScript</li>
            <li>DOM</li>
            <li>CSS</li>
        </ul>
    </body>
</html>
```

将HTML代码分解为DOM节点层次图：

![img](http://img.mukewang.com/52e4bd0f0001dd8d04830279.jpg)

HTML文档可以说由节点构成的集合，三种常见的DOM节点

+ 元素节点：上图中`<html>`、`<body>`、`<p>`等都是元素节点，即标签。

+ 文本节点：向用户展示的内容，如`<li>...</li>`中的JavaScript、DOM、CSS等文本。

+ 属性节点:元素属性，如`<a>`标签的链接属性`href="http://www.imooc.com"`。

```html
<a href="http://www.imooc.com">JavaScript DOM</a>
```

![img](http://img.mukewang.com/52e4bdb80001064c04480196.jpg)

### 通过ID获取元素

```JavaScript
 // 语法：
 document.getElementById(“id”) 
// 结果:null或[object HTMLParagraphElement]
// 注:获取的元素是一个对象，如想对元素进行操作，我们要通过它的属性或方法。
```

### 改变 HTML 样式

```javascript
// 语法:
Object.style.property=new style;
```

基本属性表（property）：

![img](http://img.mukewang.com/52e4d4240001dd6c04850229.jpg)

### 控制类名（className 属性）

`className` 属性设置或返回元素的class 属性。

```JavaScript
// 语法：
object.className = classname

/*
* 作用:
* 1.获取元素的class 属性
* 2. 为网页内的某个元素指定一个css样式来更改该元素的外观
*/
```

## 编程挑战

请编写"改变颜色"、"改变宽高"、"隐藏内容"、"显示内容"、"取消设置"的函数，点击相应按钮执行相应操作，点击"取消设置"按钮后，提示是否取消设置，如是执行操作，否则不做操作。

```html
<!DOCTYPE HTML>
<html>
  <head>
    <meta charset="utf-8"/>
    <title>javascript</title>
    <style type="text/css">
      body{font-size:12px;}
      #txt{height:400px;width:600px;border:#333 solid 1px;padding:5px;}
      p{line-height:18px;text-indent:2em;}
    </style>
  </head>
  <body>
    <h2 id="con">JavaScript课程</H2>
    <div id="txt"> 
       <h5>JavaScript为网页添加动态效果并实现与用户交互的功能。</h5>
       <p>1. JavaScript入门篇，让不懂JS的你，快速了解JS。</p>
       <p>2. JavaScript进阶篇，让你掌握JS的基础语法、函数、数组、事件、内置对象、BOM浏览器、DOM操作。</p>
       <p>3. 学完以上两门基础课后，在深入学习JavaScript的变量作用域、事件、对象、运动、cookie、正则表达式、ajax等课程。</p>
    </div>
    <form>
    <!--当点击相应按钮，执行相应操作，为按钮添加相应事件-->
      <input type="button" value="改变颜色" onclick="changeColor()">  
      <input type="button" value="改变宽高" onclick="changeWH()">
      <input type="button" value="隐藏内容" onclick="changeHide()">
      <input type="button" value="显示内容" onclick="changeShow()">
      <input type="button" value="取消设置" onclick="changeDel()">
    </form>
    <script type="text/javascript">
      var txt = document.querySelector('#txt');
      var width = txt.style.width,
          height = txt.style.height;
      //定义"改变颜色"的函数
      function changeColor(){
        txt.style.color = "red";
        txt.style.backgroundColor = 'transparent';
      }

      //定义"改变宽高"的函数
      function changeWH(){
        txt.style.width = "600px";
        txt.style.height = "600px";
      }

      //定义"隐藏内容"的函数
      function changeHide(){
        txt.style.display = "none";
      }

      //定义"显示内容"的函数
      function changeShow(){
        txt.style.display = "block";
      }

      //定义"取消设置"的函数
      function changeDel(){
        txt.style.color = "black";
        txt.style.width = width;
        txt.style.height = height;
        txt.style.display = "block";
      }
    </script>
  </body>
</html>
```

