## HTML5标签及特性

+ 结构性元素

```html
<header></header>
<nav></nav>
<div id="main">
  <article>
    <section></section>
  </article>
  <aside></aside>
</div>
<footer></footer>

# 说明
1. header 定义头部信息，包括logo、标题、搜索框等信息（一个网页不限制只有一个header）
2. nav 定义导航信息，包括导航内容，引导用户对网站有整体的把握（一个网页不限制有一个nav）
3. article 定义正文内容，定义文章的正文内容（一个article里有自己的header和footer）
4. section 定义文章中的段内容，定义文章中的段信息
5. aside 定义边栏内容，包括广告等内容（一方面可以作为article的文章附加信息，另一方面可以作为文章列表）
6. footer 定义页脚内容，包括版权信息等内容（一个页面不限制有一个footer）
```

+ 功能性标签

```md
<audio> 插入音频
<video> 插入视频
1. 基本使用
<audio src="1.mp3" loop="true" autoplay="true" controls="controls" width="200" height="200"></audio>

<video src="1.mp4" loop="true" controls="controls" autoplay="true" muted width="200" height="200"></video>

2. 解决兼容性问题
<audio controls="controls">
	<source src="1.mp3"/>
	<source src="2.mp3"/>
	您的浏览器不支持音频
</audio>

3. 相关api
play()
pause()
load()
```

+ 经典案例：音乐播放器

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>audio</title>
		<style>
			div{
				width:300px;
			}
			span{float: right;cursor: pointer;}
		</style>
	</head>
	<body>
		<div>
			<p>你不要担心<span>播放</span></p>
			<p>写给父亲的散文诗<span>播放</span></p>
			<p>认真的老去<span>播放</span></p>
		</div>
		<audio src="audio/01.mp3" controls="controls"></audio>
	</body>
	<script>
		var span = document.querySelectorAll('span'),
			audio = document.querySelector('audio');
			
		for(var i = 0;i<span.length;i++){
			(function(j){
				span[j].onclick = function(){
					audio.src="audio/0" + (j+1) + '.mp3';
					if(this.textContent == '播放'){
						change();
						this.textContent = '暂停';
						audio.play();
					}else{
						this.textContent = '播放';
						audio.pause();
					}
				}
			})(i)
		}
		
		function change(){
			for(var i = 0;i<span.length;i++){
				span[i].textContent = '播放';
			}
		}
	</script>
</html>
```

## 表单

+ 新增input输入类型

```html
<input type="number" max="10" step="2" min="1"/>
<input type="email" placeholder="请输入邮箱"/>
<input type="url" placeholder="请输入网址"/>
<input type="tel" placeholder="请输入电话号码"/>
<input type="date"/> # 输入日期仅包括年月日
<input type="month"/> # 输入年和月
<input type="week"/> # 输入年和周
<input type="time"/> # 输入时间点
<input type="datetime-local"/> # 年月日加上时间点
<input type="color"/>
<input type="search"/>
<input type="range" setp="2" min="1" max="10" value="5"/>
```

+ 新增表单元素

```html
<input type="text" list="data">
<datalist id="data">
	<option value="硬件"></option>
  	<option value="软件"></option>
</datalist>
```

+ 新增的表单属性

```md
1. form表示这个控件是隶属于表单的 from=from表单的id属性值
2. formaction 表示这个控件数据提交到的服务器地址
3. formmethod 表示这个控件数据用什么方法提交到服务器
4. placeholder
5. min max setp
6. multiple 支持多文件上传
7. required 必须填写
8. list属性 id="list"
```

+ 案例：个人信息页

## 地理位置定位

+ 地理位置定位的作用、地理位置的获取方式

```md
1. 地理位置定位的作用
可以通过地理位置定位从而满足不同的需求，比如对于地图，可以通过地理定位知道自己要去往的目的地；比如旅行，可以通过地理定位描述自己附近的旅店和景点。

2. 地理位置的获取方式
通过GPS、IP地址、WIFI信号、蜂窝网络、手机等方式进行获取

3. Web程序获取定位信息
(1) 用户打开需要获取地理位置的 web 应用。
(2) 应用向浏览器请求地理位置，浏览器询问用户是否共享地理位置。
(3) 假设用户允许，浏览器从设备咨询相关信息。
(4) 浏览器将相关信息发送到一个信任的位置服务器，服务器返回具体的地理位置。
```

+ `Geolocation`

```md
1. 判断是否支持Geolocation
  if(navigator.geolocation){
    // 支持
  } else{
    // 不支持
  }

2. 使用API
navigator.geolocation.getCurrentPosition(sucess,error,json);
navigator.geolocation.watchPosition(suncess,error,json); return id // 只针对移动设备
navigator.geolocation.clearWatch(id);
```

+ 案例实战：获取当前的位置

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>地理位置</title>
	</head>
	<body>
	</body>
	<script>		
		function sucess(position){
			alert(position.coords.latitude);	
			alert(position.coords.longitude);	
			alert(position.coords.accuracy);	
		}
		
		function error(err){
			alert(err.code);
			alert(err.message);
		}
		var obj = {
			enableHighAcuracy:false,
			timeout:1000,
			maximumAge:0
		}
		window.onload = function(){
			if(navigator.geolocation){
				navigator.geolocation.getCurrentPosition(sucess,error,obj);
			} else {
				alert('您的浏览器不支持Geolocation');
			}
		}
	</script>
</html>
```

## 离线应用

+ HTML5离线应用的特点和应用

```md
1. 离线应用的特点
（1）离线浏览 - 用户可在应用离线时使用它们
（2）加快速度 - 已缓存资源加载得更快
（3）减少服务器负载 - 浏览器将只从服务器下载更新过或更改过的资源

2. 离线应用的应用
邮件、个人事务管理、博客发布平台
```

+ 创建离线应用程序

```md
离线技术包含的两个部分：缓存清单文件、JavaScript接口

第一步：创建缓存清单文件（manifest 文件）
文件扩展名为：appcache 该文件的MIME类型为text/cache-manifest

第二步：在html标记中指定使用缓存文件
<html manifest="manifest.appcache">

mainifest文件包括三个部分
CACHE MANIFEST
在此标题下列出的文件将在首次下载后进行缓存，写在第一行，必须有该部分
NETWORK
在此标题下列出的文件需要与服务器的连接，且不会被缓存
FALLBACK
提供了获取不到缓存资源时的备选资源路径（比如 404 页面）
```

+ 离线应用的更新

```md
构建离线应用后，即使在线状态，用户也会访问缓存文件。及时更新用户的缓存文件非常重要。

HTML5离线缓存更新：
1. 用户清空浏览器缓存
2. manifest 文件被修改
3. 由程序来更新应用缓存
```

## web存储

+ web存储的优点

```md
存储空间更大，浏览器至少提供 5M 的空间
数据仅仅存储在本地，不会自动向服务器发送
提供丰富的接口，方便的操作数据
独立的存储空间，每个域都有各自存储空间，不会造成混乱
```

+ Web存储提供两种存储机制：LocalStorage SessionStorage

```md
LocalStorage：将数据保存在客户端本地的硬件设备中，即使浏览器被关闭了，该数据仍然存在，下次打开浏览器访问网站时仍然可以继续使用。

Session Storage：将数据保存在session对象中。session 是指用户在浏览某个网站时，从进入网站到浏览器关闭所经过的这段时间。session 对象可以用来保存在这段时间内所要求保存的任何数据。当用户关闭浏览器窗口后，数据会被删除。
sessionStorage为临时保存，而localStorage为永久保存。
```

+ LocalStorage的使用方法

```md
1. localStorage.setItem(key,value); localStorage['key'] = value;
2. localStorage.getItem(key); localStorage['key'];
3. localStorage.removeItem(key);
4. localStorage.key(index);
5. localStorage.length
6. localStorage.clear(); //清楚全部数据
```

+ sessionStorage方法和localStorage方法一致
+ 案例实战：web留言板和页面换肤

## WebSocket

+ WebSocket原理

浏览器和服务器只需要做一个握手的动作，便形成了一条快速通道。两者之间就直接可以数据互相传送。

+ WebSocket特点

1. 建立在 TCP 协议之上，服务器端的实现比较容易。
2. 与 HTTP 协议有着良好的兼容性。默认端口也是80和443，握手阶段采用 HTTP 协议，不容易屏蔽，能通过各种 HTTP 代理服务器。
3. 数据格式比较轻量，性能开销小，通信高效。
4. 可以发送文本，也可以发送二进制数据。
5. 没有同源限制，客户端可以与任意服务器通信。
6. 协议标识符是ws（如果加密，则为wss），服务器网址就是 URL。

## 文件操作

+ FileList 对象与 file对象

```html
<!DOCTYPE html>
<html>
  <head>
    <title>filelist and file</title>
    <meta charset="utf-8"/>
  </head>
  <body>
    <input type="file" multiple id="file">
    <input type="submit" value="put" id="btn"/>
  </body>
  <script>
    var input = document.querySelector('#file'),
        btn = document.querySelector('#btn');
    btn.onclick = function(){
  		console.log(input.files);
      	console.log(input.files.length);
      	for(var i = 0;i<input.files.length;i++){
  			console.log(input.files[i].name);
          	console.log(input.files.item(i).lastModifiedDate);
		}
	}
  </script>
</html>
```

+ blob对象

```html
<!DOCTYPE html>
<html>
  <head>
    <title>filelist and file</title>
    <meta charset="utf-8"/>
  </head>
  <body>
    <input type="file" multiple id="file">
    <input type="submit" value="put" id="btn"/>
  </body>
  <script>
    var input = document.querySelector('#file'),
        btn = document.querySelector('#btn');
    btn.onclick = function(){
      	for(var i = 0;i<input.files.length;i++){
      		if(!/image\/\w+/.test(input.files[i].type)){
      			console.log('不是图像文件');
      			continue;
      		}
  			console.log(input.files[i].size);
          	console.log(input.files.item(i).type);
		}
	}
  </script>
</html>
```

+ FileReader对象

```html
<!DOCTYPE html>
<html>
  <head>
    <title>filereader</title>
    <meta charset="urf-8"/>
  </head>
  <body>
    <input type="file" id="file"/>
    <input type="submit" value="put" id="btn"/>
    <div></div>
  </body>
  <script>
    var file = document.querySelector('#file'),
        btn = document.querySelector('#btn');
    btn.onclick = function(){
  		var reader = new FileReader();
      	reader.readAsDataURL(input.files[0]);
      	reader.onload = function(){
  			div.innerHTML = '<img src="' + this.result + '"/>';
		}
        // reader.readAsBinaryString(input.files[0]); this.result
        // reader.readAsText(input.files[0],'utf-8'); this.result
	}
  </script>
</html>
<!--
1. 方法：abort() readAsText(file,'utf-8'); readAsDataURL(file); readAsBinaryString(file);
2. 事件：onload onloadstart onloadend onabort onerror onprogress
-->
```

## 拖放

+ dataTransfer对象

```md
DataTransfer，拖拽数据传递对象。为事件对象的一个属性
方法和属性
e.dataTransfer.setData('text',e.target.innerHTML); 向DataTransfer对象存入数据
e.dataTransfer.getData('text'); 从DataTransfer对象取出数据
types 属性 是一个数组 存入数据的数据种类
dropEffect 属性 表示拖放操作的视觉效果。
files 表示拖放的filelist文件列表
```

+ 实现拖放的步骤

```md
1. 将需要拖放的对象元素的draggable属性设为true draggable="true"
2. 编写与拖放有关的事件处理代码
```

+ 拖放的相关事件

```md
dragstart 拖放开始
dragend 拖放结束
dragover 被拖放的元素正在本元素范围内移动
dragleave 被拖放的元素离开本元素范围
drag 拖放过程中
drop 有其他元素被拖放到了本元素中
dragenter 被拖放的元素开始进入本元素的范围内
```

+ 实战案例（上传头像未完成）

```html
<!DOCTYPE html>
<html>
  <head>
    <title>drag</title>
    <meta charset="utf-8"/>
    <style type="text/css">
    	div{
    		width:200px;
    		height:200px;
    		border:1px solid #000;
    	}
    </style>
  </head>
  <body>
    <img src="images/cat.jpg" id="img" draggable="true" ondragstart="dragstart(event)"/>
    <div ondrop="drop(event)" ondragover="allowdrop(event)"></div>
  </body>
  <script>
    function dragstart(e){
  		e.dataTransfer.setData('Text',e.target.id);
	}
    function allowdrop(e){
  		e.preventDefault();
	}
    function drop(e){
  		e.preventDefault();
      	var child = e.dataTransfer.getData('Text');
      	e.target.appendChild(document.getElementById(child));
      	console.log(e.target.childNodes);
	}
  </script>
</html>
```
## 画布

+ 创建Canvas元素

```md
<canvas id="canvas" width="300" height="150"></canvas>// 默认的大小为300 * 150

建议直接设置width和height属性，同时改变canvas元素的大小和元素绘图表面的大小

1. 在页面添加 canvas 元素，定义 id 属性以便后续调用。
<canvas id="canvas" width="300" height="300"></canvas>
2. 使用id寻找 canvas 元素。
var canvas = document.querySelector('#canvas');
3. 通过 canvas 元素的 getContext 方法来获取其上下文，创建Context 对象，以获取允许进行绘制的 2D 环境。
var context = canvas.getContext('2d');
4. 使用 JavaScript 进行绘制。
```

+ Canvas绘制直线、多边形、填充、圆弧、矩形、文字的方法

```md
// 绘制直线、多边形

context.moveTo(20,20);  规定起点如果没有用moveTo() 规定子路径的起点，则 lineTo(x,y)等同于moveTo(x,y)
context.lineTo(20,100);
context.stroke(); 绘制的线是黑色的1px的

线条样式设置
lineWidth = 5;  绘制的线条的宽度
strokeStyle = "red"; 绘制的颜色

// 填充
fill();  填充当前绘图（路径）。默认颜色是黑色。如果路径未关闭，那么 fill() 方法会自动从路径结束点到开始点之间添加一条线，以关闭该路径，然后填充该路径。
fillStyle 属性 fillStyle="red";建议先填充再描边

canvas基于路径绘图
beginPath() 开始一条新的路径（路径开始点）
closePath() 创建从当前点到开始点的路径（路径结束点）。关闭一条打开的子路径。 会先连一条线的
canvas 之中只能有一条路径存在，称之为“当前路径”

练习：七巧板 案例实战

// 绘制圆弧

context.arc(x,y,radius,0,2*Math.PI,true);

练习：同心圆 八卦 案例实战

// 绘制矩形

context.rect(x,y,width,height);
context.fillRect(x,y,width,height);  绘制 "已填色" 的矩形（实心）
context.strokeRect(x,y,width,height); 绘制不填色的矩形（空心）
context.clearRect(x,y,width,height); 清空给定矩形内的指定像素 相当于一个填满的中间空了一块

// 绘制文字

context.fillText('text',x,y,maxWidth); //设置起始位置以及最大的宽度 绘制填充文字 可以结合fillStyle
context.font 设置字体的样式 context.font="italic 25px 幼圆";
context.strokeText('text',x,y,maxWidth);// 绘制描边文字 可以结合strokeStyle 和font
context.measureText('text'); 返回一个对象，该对象包含以像素计的指定字体宽度。.width

textAlign 属性（center | end | left | right） 根据锚点，设置文本内容的当前对齐方式。
textBaseline 属性（top | middle | bottom） 根据锚点，设置在绘制文本时的当前文本基线。

案例实战：钟表
```

+ 画布转换和状态保存

##### 平移 translate(dx, dy)

- 平移画布的用户坐标系统，即重新映射画布上的 (0,0) 位置。
- 参数 dx —— 坐标原点沿水平方向的偏移量
- 参数 dy —— 坐标原点沿垂直方向的偏移量


- **translate() 平移，坐标系会累加平移。**

##### 旋转 rotate(angle)

- 旋转画布的用户坐标系统，即改变坐标系 x 与 y 轴的指向。
- 参数 angle —— 旋转角度，以弧度计。正值表示顺时针方向旋转、负值表示逆时针方向旋转、将角度转换为弧度公式


- `rotate()` 旋转，坐标系会累加旋转。

##### 缩放 scale(swidth, sheight)

- 缩放画布的用户坐标系统。
- 参数 swidth —— 坐标系 x 轴缩放倍数。
- 参数 sheight —— 坐标系 y 轴缩放倍数。


- `scale()` 缩放，坐标系会累加缩放。

##### 画布坐标状态保存和恢复

- `save()`：保存当前 canvas 绘图环境的所有属性、坐标变换信息等。
- `restore()`：将绘图环境状态恢复为保存值。
- **可以嵌套式的调用 save( )、restore( ) 方法**。save() 把当前状态的一份拷贝压入到一个保存图像状态的栈中。restore() 是出栈。