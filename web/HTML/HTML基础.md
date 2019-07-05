## HTML介绍

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8"/>
        <title>demo</title>
        <link rel="stylesheet" href="">
        <style></style>
    </head>
    <body>
        
    </body>
    <script></script>
    <script src=""></script>
</html>
```

## HTML标签

```html
文本
<br/>
<p></p>
<hx></hx>
<q></q>
<blockquote></blockquote>
<b></b>
<strong></strong>
<i></i>
<u></u>
<em></em>
<code></code>
<pre></pre>
空格 &nbsp;
大于号 &gt;
小于号 &lt;
版权符 &copy;
注册商标 &reg;
列表
<ul><li></li></ul>
<ol><li></li></ol>
<dl>
    <dt></dt>
    <dd></dd>
</dl>
图片
<img src="" alt="" width="" height="" title=""/>

超链接
<a href="https://www.baidu.com" title="" target="_blank _self">链接</a>
<a href="mailto:2622860598@qq.com">邮件链接</a>
<a href="#">空链接</a>
<a href="a.zip">下载链接</a>
<a name=“b_name”>命名锚点</a>
<p id="name">命名锚点</p>
<a href="#name">锚点链接</a>

图像映射
<img src="" alt="" title="" usemap="#map1"/>
<map name="map.1" id="map">
    <area shape="circle" coords="90,60,20" href=""/><!--圆心坐标和半径-->
    <area shape="rect" coords="25,25,160,160" href=""/><!--左上角 右下角坐标-->
</map>

表格
<table>
    <tr><th></th></tr>
    <tr><td></td></tr>
</table>
cellpadding :  单元边沿与其内容之间的距离
cellspacing :  单元格之间的空白（pixels或%）
colspan: 可以横跨的列数
rowspan: 可以横跨的行数

表单：
<form enctype="" acrion="" method=""></form>
form表单中enctype属性的三种类型：
1. multipart/form-data不对字符编码，用于发送二进制的文件，其他两种类型不能用于发送文件；
2. text/plain 用于发送纯文本内容，空格转换为 "+" 加号，不对特殊字符进行编码，一般用于email之类的；
3. application/x-www-form-urlencoded，在发送前会编码所有字符，即在发送到服务器之前，所有字符都会进行编码（空格转换为 "+" 加号，"+"加号转换为空格，特殊符号转换为 ASCII HEX 值）。其中application/x-www-form-urlencoded为默认类型。 

表单控件：
<input type="text"/>
<input type="password"/>
<input type="radio" name="sex"/>name要一致
<input type="checkbox" name="habit"/>name要一致
<input type="file" multiple/>
<input type="submit"/>
<input type="reset"/>
<input type="button" value=""/>
<textarea rows="3" cols="4">
</textarea>
<select>
    <option value="1" selected>1</option>
</select>

iframe
<iframe src="https://www.baidu.com" width="300" height="500" name=""></iframe>
```

