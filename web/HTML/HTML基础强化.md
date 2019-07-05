## HTML常见元素和理解

### 常见元素

```html
meta
title
style
link
script
base
div/section/article/aside/header/footer/section/nav
p
span/em/strong
i/icon
table/thead/tbody/tr/td[colspan,rowspan]
ul/ol/li/dl/dt/dd
a[href,target]
img[src,alt]
form[target,action,method,enctype]/input[type,value]/select/textarea/button[type]
select>option[value]
label[for] 和一个表单项相关联
```

### 常见元素的理解

**meta**

```html
<meta charset="utf-8"/> 页面使用的字符集 使用的是哪一种编码的字符
<meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no">
视口的宽度等于屏幕的宽度 适配移动端的第一步
```

**base**

```html
<base> 标签为页面上的所有链接规定默认地址或默认目标
<base href="/" target="_blank"/>
```

请看下面的demo，链接会在新窗口中打开，即使链接中没有 `target="_blank"` 属性。这是因为 base 元素的 `target` 属性已经被设置为 `"_blank"` 了。

```html
<!DOCTYPE html>
<html>
    <head>
        <base href="http://www.w3school.com.cn/i/" />
        <base target="_blank" />
    </head>

    <body>
        <a href="http://www.w3school.com.cn">W3School</a>
    </body>
</html>
```

**em/strong**

+ 在HTML4.01中用`b` 和 `i`表示加粗和倾斜表示，其中`<b>`标签和`<i>`标签是样式标签
+ 在HTML5中用`em` 和 `strong` 表示倾斜和加粗表示，其中这两个标签是语义化的标签
+ 其中在HTML5中将`i`标签进行了保留，更多的是作为`Iconfont`图标

### 如何理解HTML

其中HTML是一个通过程序员编写的字符串文本，是一篇文档，作为文档，就有结构，就会有一级标题、二级标题等区块和大纲（`section article nav`），对于搜索引擎来说，一个好的HTML文档应该有清晰的大纲。

可以通过https://h5o.github.io/查看HTML文档的大纲

## HTML的版本

+ `HTML4/4.01(SGML)` 基于标记语言`SGML`来编写，属于`SGML`，`SGML`其实是`XML`的超集
+ `XHTML(XML)`要求非常严格，比如要求所有的标签必须是小写的，所有的属性必须是小写的，所有的属性必须要有值
+ `HTML5`基于`HTML4.01`，但是它不属于`SGML`和`XML`，是一种新的标准
+ https://validator.w3.org/  可以通过这个网址查看自己编写的HTML是否符合规范

| HTML4               | XHTML               | HTML5             |
| ------------------- | ------------------- | ----------------- |
| 标签允许不结束      | 标签必须结束        | 标签允许不结束    |
| 属性不用带引号      | 属性必须带引号      | 属性不用带引号    |
| 标签属性可大写      | 标签属性必须小写    | 标签属性可大写    |
| Boolean属性可省略值 | Boolean属性必须写值 | Boolean属性可省略 |

## HTML元素的分类

### 按照默认样式分

+ 块级元素`(block)`，例如：`div article section aside`
+ 行内元素`(inline)`，其中对于行内元素来说，加入width属性是没有任何效果的，根据宽度进行自适应。例如：`em strong span`
+ `inline-block`，对外表现是`inline`，对内展示是`block`，例如：`select`

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>default style</title>
</head>
<body>
    <div>DIV元素</div>
    <p><span>内联元素</span><em>内联元素</em><strong>好巧，我也是内联元素</strong></p>
    <p><select><option>下拉框</option></select><span>你猜左边是什么元素</span></p>
</body>
</html>
<!--
上述demo中如果改变文档宽度，内联元素会进行换行，但是块级元素和inline-block元素都会保持自己本身的宽度不换行
-->
```

### 按照内容分

详情内容请看：https://www.cnblogs.com/ganmingying/p/7390719.html

![](https://images2017.cnblogs.com/blog/1158992/201708/1158992-20170818163439459-369372948.png)

根据HTML5.1推荐标准，HTML元素主要分为7大类：

+ `metadata` ：原数据的内容是设置其余内容的表现或行为的内容、或者是建立文档与其他文档关系的内容、或者是传达文档之外信息的内容。

```html
base link meta noscript script style template title
```

+ `flow`：流数据的内容是应用于文档或者应用程序的主体的大多数元素。

+ `sectioning`：分节的内容是定义标题和页脚范围的内容。

```html
article aside nav section
```

+ `heading`：标题的内容是定义文档某一节的标题。

```html
h1 h2 h3 h4 h5 h6
```

+ `phrasing`：短语的内容是文档的文本以及在段落层次上标记该文本的元素

```html
em span strong ...
```

+ `embedded`：嵌入的内容是将另一种资源导入文档的内容，或从另一个资源中插入到文档中的内容。

```html
audio canvas iframe img svg video
```

+ `interactive`：交互式内容是专门用于用户交互的内容。

```html
a (if the href attribute is present) 
audio (if the controls attribute is present) 
img (if the usemap attribute is present) 
input (if the type attribute isnot in the hidden state) ...
```

## HTML元素嵌套关系

具体内容请看：http://www.cnblogs.com/xiaohuochai/p/5433698.html

+ 块级元素可以包含行内元素。例如：`div>a div>span`

+ 块级元素不一定能包含块级元素。例如：`div>div section>div` 但是，块级元素不能放在`<p>`里面

+ 行内元素一般不能包含块级元素。

```html
a是否可以包含div吗？
一般在处理过程中将a元素当做透明元素，如果将a去掉合法，那么就合法
<a><div>123</div></a> --合法
<p><a><div>123</div></a></p> --不合法
```

+ `li` 内可以包含 `div` 标签。`li` 和 `div`标签都是装载内容的容器，地位平等，没有级别之分，`li` 标签连它的父级 `ul` 或者是 `ol` 都 可以容纳的。

## HTML默认样式

在HTML元素中，有些是默认含有样式的。

```css
/*html元素本身是有样式的*/
html{
    display:block;
}

/*body元素的默认样式*/
body{
    display:block;
    margin:8px;
}
/*ul元素也有默认样式*/
ul{
    margin:16px;
    padding:40px;
    list-style-position:outside;/*代表原点在元素外面*/
}
```

如何去掉HTML的默认样式呢？

+ CSS Reset（将默认的样式全部去掉，对样式进行重写，并不是w3c的标准，只不过是人们总结的写法）

```css
/*详情内容见：https://meyerweb.com/eric/tools/css/reset/*/
html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed, 
figure, figcaption, footer, header, hgroup, 
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
	margin: 0;
	padding: 0;
	border: 0;
	font-size: 100%;
	font: inherit;
	vertical-align: baseline;
}
/* HTML5 display-role reset for older browsers */
article, aside, details, figcaption, figure, 
footer, header, hgroup, menu, nav, section {
	display: block;
}
body {
	line-height: 1;
}
ol, ul {
	list-style: none;
}
blockquote, q {
	quotes: none;
}
blockquote:before, blockquote:after,
q:before, q:after {
	content: '';
	content: none;
}
table {
	border-collapse: collapse;
	border-spacing: 0;
}
```

+ 使用通配符选择器进行样式的重写

```css
/*这种方式不会带来性能的问题*/
*{
	padding:0;
    margin:0;
}
```

+ `normalize.css`：并不改变默认样式，它在默认的HTML元素样式上提供了跨浏览器的高度一致性

详情请看：https://necolas.github.io/normalize.css/

## HTML面试真题

+ `doctype`的意义是什么？

```md
(历史背景 ie中的盒子模型 ie包含的宽度包含padding)
让浏览器以标准模式渲染
让浏览器知道元素的合法性 doctype html
```

+ HTML XHTML HTML5的关系？

```md
HTML属于SGML
XHTML属于XML，是HTML进行XML严格化的结果
HTML5不属于SGML或XML，比XHTML宽松
```

+ HTML5有什么变化？

```md
新的语义化的元素
表单增强(新的类型和属性)
新的API(离线、音视频、图形canvas svg、实时通信、本地存储、设备能力)
分类和嵌套变更
```

+ `em`和`i`有什么区别？

```md
em是语义化的标签，表示强调
i是纯样式的标签，表示斜体
HTML5中i不推荐使用，一般用作icon图标
```

+ 语义化的意义是什么？

```md
开发者容易理解
机器容易理解结构(搜索、读屏软件)
有助于SEO
semantic microdata(不是全部用div来标记)
```

+ 哪些元素可以自闭合？

```html
表单元素input
图片img
br hr
meta link
```

+ HTML和DOM的关系

```html
HTML是“死”的,就是一个字符串文本
DOM是由HTML解析而来的,是活的
JS可以维护的是DOM innerHTML其实写的是DOM
```

+ property和attribute的区别

```md
attribute是死的 属性
property是活的  特性
```

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>property / attribute</title>
</head>
<body>
    <input type="text" value="1" />
</body>
</html>
```

+ form的作用？

```md
直接提交表单
使用submit/reset的按钮
便于浏览器保存表单
第三方库可以整体提取值
第三方库可以进行表单验证
```