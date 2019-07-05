## 异步和单线程

### 基础知识

什么是异步

```javascript
// 异步 执行结果是 100 300 200 这种执行结果没有阻塞现象
console.log(100);
setTimtout(function(){
    console.log(200);
});
console.log(300);

// 对比同步 同步是阻塞的
console.log(100);
alert(200);
console.log(300);
```

何时需要异步
+ 在可能发生等待的情况
+ 等待过程中不能像alert一样阻塞程序运行
+ 因此，所有的`等待情况`需要异步执行

前端使用异步的场景

+ 定时任务：`setTimeout` `setInterval`

```javascript
console.log(100);
setTimtout(function(){
    console.log(200);
});
console.log(300);
// 执行结果 100 300 200
```

+ 网络请求：`ajax`请求，动态`<img>`加载

```javascript
// ajax请求示例
console.log('start');
$.get('/test.json',(data)=>{
   console.log(data); 
});
console.log('end');
// 执行结果 start end data

// 动态加载<img>示例
console.log('start');
var img = document.createElement('img');
img.onload = function(){
    console.log('loaded');
}
img.src = '/xxx.png';
console.log('end');
// 执行结果 start end loaded
```

+ 事件绑定

```javascript
// 事件绑定示例
console.log('start');
document.getElementById('btn').addEventListener('click',()=>{console.log('clicked');});
console.log('end');
```

异步和单线程

+ 单线程：一次只能干一件事 只能一个一个排队进行。异步是为了让单线程更加的顺畅，不阻塞

```javascript
console.log(100);
setTimeout(()=>{
    console.log(200);
}) // 因为setTimeout是一个异步的场景 需要先拿到一边 等有空了执行
console.log(300);
/*
执行过程：
1. 执行第一行，打印100
2. 执行setTimeout后，传入setTimeout的函数会被暂存起来，不会立即执行（单线程的特点，不能同时干两件事）
3. 执行最后一行，打印300
4. 待所有程序执行完，处于空闲状态时，会立马看有没有暂存起来的要执行
5. 发现暂存起来的setTimeout中的函数无需等待时间，就立即来过来执行(如果有等待时间，先封存，然后等到达等待时间的时候，就直接执行)

同样，对于ajax请求，也会将ajax封存，等请求达到时，解除封存，执行代码
同样，对于事件绑定，也会将事件进行封存，当执行事件的时候，会解除封存，执行多次(事件执行是多次的)
*/
```

### 面试题目

+ 同步和异步的区别是什么？分别举一个同步和异步的例子

```javascript
1. 同步会阻塞代码执行，而异步不会
2. alert是同步，setTimeout是异步
```

+ 一个关于 `setTimeout` 的笔试题

```javascript
console.log(1);
setTimeout(function(){
    console.log(2);
},0);
console.log(3);
setTimeout(function(){
    console.log(4);
},1000);
console.log(5);
// 执行结果 1 3 5 2 4
```

+ 前端使用异步的场景有哪些

```javascript
1. 定时任务：setTimeout、setInterval
2. 网络请求：ajax请求、动态<img>加载
3. 事件绑定
特点：都需要等待，所以需要异步，避免阻塞，原因是因为JavaScript是单线程的
```

## 其他知识

### 基础知识

日期

```javascript
Date.now(); // 获取当前事件的毫秒数 从1970年开始算起
var dt = new Date();
dt.getTime(); // 获取毫秒数
dt.getFullYear(); // 年
dt.getMonth(); // 月（0-11）获取之后要加1
dt.getDate();// 日（0-31）
dt.getDay();// 星期（1-7）
dt.getHours();// 小时（0-23）
dt.getMinutes();// 分钟（0-59）
dt.getSeconds();// 秒（0-59）
```

Math API

```javascript
Math.random();// 生成的随机数在0到1之间 生成随机数的长度不一定相同
// 生成随机颜色
'#' + (Math.random()+'').slice(2,8);
// 生成0-20的随机数
Math.random()*20;
```

数组

+ `forEach`遍历所有元素

```javascript
var arr = ['a','b','c','d'];
arr.forEach(function(item,index){
    // 遍历数组的所有元素
    console.log(index,item);
});
// 执行结果：0 "a"	1 "b"	2 "c"	3 "d"
```

+ `every` 判断所有元素是否都符合条件

```javascript
var arr = [1,2,3,4];
var result = arr.every(function(item,index){
    // 用来判断所有的数组元素，都满足一个条件
    if(item<5){
        return true;
    }
});
console.log(result);
// 执行结果：true
```

+ `some` 判断是否至少有一个元素符合条件

```javascript
var arr = [1,2,3];
var result = arr.every(function(item,index){
    // 用来判断所有的数组元素，只有一个满足条件即可
    if(item<2){
        return true;
    }
});
console.log(result);
// 执行结果：false
```

+ `sort` 排序

```javascript
var arr = [1,4,2,3,5,4];
var arr2 = arr.sort(function(a,b){
    // 从小到大排序
    return a-b;
    
    // 从大到小排序
    return b-a;
});
console.log(arr,arr2); // [1, 2, 3, 4, 4, 5] [1, 2, 3, 4, 4, 5]
// 应用：数组去重并且排序
var arr1 = new Set(arr);
arr1 = Array.from(arr1);
arr1.sort((a,b)=>{return a-b;})
// 执行结果 [1, 2, 3, 4, 5]
```

+ `map` 对元素重新组装，生成新数组

```javascript
var arr = [1,2,3,4];
var arr2 = arr.map(function(item,index){
    // 将元素重新组装，并返回
    return '<b>' + item + '</b>';
});
console.log(arr2);// ["<b>1</b>", "<b>2</b>", "<b>3</b>", "<b>4</b>"]
```

+ `filter` 过滤符合条件的元素

```javascript
var arr = [1,2,3];
var arr2 = arr.filter(function(item,index){
    // 通过某一个条件过滤数组
    if(item >= 2){
        return true;
    }
})
console.log(arr2);// [2, 3]
```

对象API

```javascript
var obj = {
    x:100,
    y:200,
    z:300
}
for(var key in obj){
    if(obj.hasOwnProperty(key)){
        console.log(key,obj[key]);
    }
}
// 执行结果：x 100	y 200	z 300
```

### 面试题目

+ 获取 `2017-06-10`格式的日期

```javascript
function formatDate(dt){
    if(!dt){
        dt = new Date();
    }
    var year = dt.getFullYear();
    var month = dt.getMonth() + 1;
    var date = dt.getDate();
    if(month < 10){
        // 强制类型转换
        month = '0' + month;
    }
    if(date < 10){
        // 强制类型转换
        date = '0' + date;
    }
    return year + '-' + month + '-' + date;
}
var dt = new Date();
var formatDate = formatDate(dt);
console.log(formatDate)
```

+ 生成随机数，要求是长度一致的字符串格式

```javascript
var random = Math.random;
var random = random + '0000000000';// 后面加上10个零
var random = random.slice(0,10);
console.log(random);
```

+ 写一个能遍历对象和数组的`forEach`函数

```javascript
// 设计思路
var arr = [1,2,3];
// 注意，这里参数的顺序换了，为了能够和对象的遍历格式一致
forEach(arr,function(index,item){
    console.log(index,item)
})

var obj = {x:100.y:200};
forEach(obj,function(key,value){
    console.log(key,value);
});

// 具体实现
function forEach(obj,fn){
    var key;
    if(obj instanceof Array){
        // 准确判断是不是数组
        obj.forEach(function(item,index){
            fn(index,item);
        })
    } else {
        // 不是数组就是对象
        for(key in obj){
            fn(key,obj[key]);
        }
    }
}
var arr = [1,2,3];
// 注意，这里参数的顺序换了，为了能够和对象的遍历格式一致
forEach(arr,function(index,item){
    console.log(index,item)
})

var obj = {x:100.y:200};
forEach(obj,function(key,value){
    console.log(key,value);
});
```