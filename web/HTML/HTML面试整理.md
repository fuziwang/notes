+ HTML的全称为：Hyper Text Markup Language 超文本标记语言，其中超文本的理解就是超链接

+ HTML的版本

```md
HTML4.01 基于SGML是一种标记语言，因此在文档类型声明的时候，需要引入DTD
XHTML 是更严格意义上的HTML4.01，比如所有的标签需要小写，所有的属性需要有属性值
HTML5不属于SGML和XHTML，是一种新的标准，不需要在文档类型声明的时候，引入DTD
```

+ HTML5的文档类型声明

```html
<!DOCTYPE html>
声明该文档是html文档，如果不写，浏览器不知道以什么规范来解析文档
```

+ meta相关的内容

```html
<meta name="description" content="不超过150个字符"/>
<meta charset="utf-8"/>
<meta name="keywords" content=""/>
<meta http-equiv=“X-UA-Compatible” content=“IE=edge,chrome=1”/>  <!--优先使用 IE 最新版本和 Chrome-->
<meta name="viewport" content="width=device-width,inital-scale=1.0,minimum-sacle=1.0,maximum-scale-1.0,user-scaleable=no">

// width    设置viewport宽度，为一个正整数，或字符串‘device-width’
// device-width  设备宽度
// height   设置viewport高度，一般设置了宽度，会自动解析出高度，可以不用设置
// initial-scale    默认缩放比例（初始缩放比例），为一个数字，可以带小数
// minimum-scale    允许用户最小缩放比例，为一个数字，可以带小数
// maximum-scale    允许用户最大缩放比例，为一个数字，可以带小数
// user-scalable    是否允许手动缩放
```

+ src和href的区别是什么

```html
<link rel="stylesheet" href=""/>
<script src=""></script>
1. src用于替换当前元素，href用于在当前文档和引用资源之间确立联系。
2. src指向的内容将会嵌入到文档中当前标签所在位置；在请求src资源时会将其指向的资源下载并应用到文档内，例如js脚本，img图片和frame等元素
3. href指向网络资源所在位置，建立和当前元素（锚点）或当前文档（链接）之间的链接，如果我们在文档中添加<link href="common.css" rel="stylesheet"/>那么浏览器会识别该文档为css文件，就会并行下载资源并且不会停止对当前文档的处理。
```

+ strong和em的异同点

```html
strong:粗体强调标签，强调，表示内容的重要性
em:斜体强调标签，更强烈强调，表示内容的强调点
这种是语义化的标签，而对于<b></b> 和 <i></i>是样式化的标签
```

+ `<img>`的`title`和`alt`有什么区别

```md
1. title当鼠标悬停在图片上方的时候显示的内容
2. alt是当图片没有加载完成时的占位符，更利于搜索引擎对内容的解读，可提图片高可访问性
```

+ less：https://segmentfault.com/a/1190000012360995?utm_source=tag-newest#articleHeader11

+ iframe有那些缺点？

```md
1. iframe会阻塞主页面的onload事件
2. 搜索引擎的检索程序无法解读这种页面，不利于SEO
3. iframe和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载
4. 使用iframe之间需要考虑这两个缺点。如果需要使用iframe可以通过JavaScript动态的给iframe添加src属性值，这样可以绕开以上两个问题
```