## 非布局样式--字体

### 字体族

+ `serif`：衬线字体族。serif 典型的字体有：Times New Roman、MS Georgia、宋体……

+ `sans-serif`：无衬线字体族。sans-serif 典型的字体有：MS Trebuchet、MS Arial、MS Verdana、幼圆、隶书、楷体……

+ `cursive`：手写字体族。cursive 典型的字体有：Caflisch Script、Adobe Poetica、迷你简黄草、华文行草……

+ `fantasy`：梦幻字体族。fantasy 典型的字体有：WingDings、WingDings 2、WingDings 3、Symbol……

+ `monospace`：等宽字体族。monospace 典型的字体有：Courier、MS Courier New、Prestige……

### 多字体fallback

就是设置字体的`font-family`属性

```css
body{
    /* font-family: "monaco";*/
    /* font-family: "monaco", "PingFang SC";*/
    font-family: "aaaaa", "monaco", "PingFang SC";
    /*设置font-family英文字体为monaco，当遇到中文的时候，不符合对应的字体，
    匹配另一种字体"PingFang SC"*/
}
.chinese{
    font-family: "PingFang SC", "Microsoft Yahei", monospace;
    /*字体族在设置fallback的时候不用加引号*/
    /* font-family: "Microsoft Yahei", serif; */
    /* font-family: "serif"; */
}
```

### 网络字体、自定义字体

**实现网页中插入特殊字体的过程**

1. 获取特殊字体：http://www.dafont.com/single-malta.font
2. 获取@font-face所需字体格式：http://www.fontsquirrel.com/tools/webfont-generator
3. 应用@font-face到项目中

```css
@font-face {
    font-family: "IF";
    src: url("./IndieFlower.ttf");
}
.custom-font{
    font-family: IF;/*名字可以随便取值*/
}
```

### Iconfont

详情请看链接：https://www.iconfont.cn/help/detail?spm=a313x.7781069.1998910419.d8cf4382a&helptype=code

**font-class引用**

+ 第一步：拷贝项目下面生成的`fontclass`代码：

```js
//at.alicdn.com/t/font_8d5l8fzk5b87iudi.css
```

+ 第二步：挑选相应图标并获取类名，应用于页面：

```css
<i class="iconfont icon-xxx"></i>
```

## 非布局样式--行高

### 行高的取值

行高可以有以下几种取值方式：

- `normal` 默认值，由浏览器计算得出，一般位于 `1.0` ~ `1.2` 之间
- `inherit` 从父级继承
- 百分比值（如 `150%`）：`14px(body 默认字体大小) * 1.5 = 21px`
- 具体长度值（如`2em`）
- 数字（如`1.5`）

使用数字值或者 normal（默认值）设置行高，行高会根据字号的大小进行缩放。

使用百分比或者固定值定义的行高，其值是固定的，不会随字号进行缩放。

### 行高的构成

首先明确基线的概念：

![](http://image.zhangxinxu.com/image/blog/200911/base_line.jpg)

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>inline</title>
    <style>
        span{background:red;}
        .c1{line-height: 20px;}
        .c2{line-height: 8px;}
        .c3{line-height: 30px;}
        .c5{line-height: 28px;}
        .c6{line-height: 21px;}
    </style>
</head>
<body>
    <div>
        <span class="c1">inline box xfg中文</span>
        <span class="c2">inline box</span>
        <span class="c3">inline box</span>
        inline box
        <span class="c5">inline box</span>
        <div class="c6">linebox</div>
    </div>
</body>
</html>
```

+ 每个单词（或者段落中的行内元素）就是一个 `inline-box`，更严格点来讲，没有标签包裹的纯文本元素也被成为匿名 `inline-box`。顾名思义，`inline-box` 是内联元素，其让段落中的内联元素一个挨着一个水平排列，而不是想块级元素的那样垂直排列。

+ 一个 `line-box` 由一个或者多个 `inline-box` 构成。可以这么理解：段落中的每一行就是一个 `line-box`

+ 一个或多个 `line-box` 构成了一个 `containing-box`。可以这么理解：一个段落就是一个 `containing-box`。

**几个 box 之间的关系**

每个 `containing-box` 是由一个或多个 `line-box` 构成的，因此我们可以说 `line-box` 的总高度决定了 `containing-box` 的高度，而每个 `line-box` 是由一个或多个 `inline-box` 构成的，每个 `line-box` 的高度是由构成 `line-box` 的几个 `inline-box` 中的高度最大的那一个构成的，因此我们可以说：`inline-box` 决定了一个段落也就是 `containing-box` 的高度。
那么 `inline-box` 的高度是由什么决定的呢？就是**行高**。

### inline-box 和字号的关系

段落的高度并不是由字号决定的，而与行高有关在，字号并不能影响段落的高度。

我们知道 `line-height` 可以设置为两种值：一种是大于字号（`font-size`）的值，另一种是小于字号（`font-size`）的值，这两种值和元素的表现有什么关系呢？

+ content-box 就是 inline-box 中文本所占的空间，其高度就是文本本身的高度，不包括文本自身高度以外的空白，content-box 的高度使用文本的 font-size 决定的。

+ content-box 的上下还有一些空白区域，这些区域就是半行间距，其值的计算方式为：`(行高-字号)/ 2`

**行高小于字号的情况**：当行高小于字号时，计算出的半行间距为负值，于是其会向陷。**半行间距影响了 line-box 之间的距离**，因此当半行间距大于0 时，随着其的逐渐增大，line-box 之间的距离也逐渐增大，视觉效果上段落结果愈松散。当半行间距小于 0 时，随着其的逐渐减小，line-box 之间的距离也逐渐减小，视觉效果上越来越紧凑。

### 行高的调整

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>line-height</title>
    <style>
        .cc1{font-size:12px;}
        .cc2{font-size:18px;}
        .cc3{font-size:24px;}
    </style>
</head>
<body>
    <div class="c1">
        <span class="cc1">第一段</span>
        <span class="cc2">第二段</span>
        <span class="cc3">第三段</span>
    </div>
    <div style="background:red">
        <span>文字</span>
        <img src="test.png"/>
    </div>
</body>
</html>
```

上述代码说明：

+ 字体是按照baseline基线对齐的，字体的底部（不是底线）就是基线的位置，改变字体的对齐方式，可以通过`vertical-align`属性（属性值包括：top、bottom、middle、baseline、20px）

+ 图片下面会有空白，图片也是`inline`的元素，因此也会按照基线(`baseline`)对齐，如果要去掉下方的空白，就是解决图片3px（字体大小为12px）经典问题。

```css
display:block;/*独占一行，无间隙*/
vertical-align:bottom;
```

## 非布局样式--背景

### 背景颜色







```css
背景
背景颜色
red blue green black
#f00
rgba
hsl(色相 饱和度 亮度)
hsla

渐变色背景
background: linear-gradient(0deg, red, green);
0deg表示的是从下往上
45deg表示的是从左下到右上的方向
多背景叠加

背景图片和属性（雪碧图） 减少http请求，对性能优化很好
css雪碧图的原理和作用
base64和性能优化
多分辨率的适配
```