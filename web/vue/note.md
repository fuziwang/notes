### 学习环境

```md
nvm （node.js的版本管理工具）
安装淘宝镜像
vue的调试环境 基于chrome浏览器的插件 vue-devtools
npm i -g vue-cli
```

### Vue 实例

```js
var obj = {a:1};
var vm = new Vue({
    el:'#app',
    data:obj,
});
// vm.a === obj.a
// vm.a = 2; obj.a === 2
// obj.a = 3; vm.a == 3
// 当实例被创建时 data 中存在的属性是响应式的
```

### 模板语法

数据绑定最常见的形式就是使用“Mustache”语法 (双大括号) 的文本插值：

```html
<span>Message: {{ msg }}</span>
```

双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 `v-html` 指令：

```html
<div id="app">
    {{template}}
    <span v-html="template"></span>
</div>
<!-- <span>内容</span> 内容 -->
<script type="text/javascript">
	var vm = new Vue({
		el:'#app',
		data:{
			template:'<span>内容</span>',// 这里相当于在span里面嵌套了一个span
		}
	})
</script>
```

Mustache 语法不能作用在 HTML 特性上，遇到这种情况应该使用 `v-bind` 指令

```html
<div v-bind:id="dynamicId"></div>
<button v-bind:disabled="isdisabled">btn</button>
<script type="text/javascript">
	var vm = new Vue({
		el:'#app',
		data:{
			dynamicId:'vid',
			isdisabled:false
		}
	})
</script>
```

对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript 表达式支持。

```html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```