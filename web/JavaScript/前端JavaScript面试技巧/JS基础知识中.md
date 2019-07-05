## 作用域和闭包

### 基础知识

执行上下文

+ 范围：一段 `<script></script>` 或者一个函数都会形成一个执行上下文

+ 全局：变量定义、函数声明

+ 函数：变量定义、函数声明、`this`、`arguments`（`JS`参数的集合）

```javascript
// 全局
console.log(a);//undefined
var a = 100;

fn('zhangsan');// zhangsan 20
function fn(name){
    // 函数
    console.log(this);
    console.log(arguments);
    
    age = 20;
    console.log(name,age);
    var age;
    
    bar(100);
    function bar(num){
        console.log(num);
    }
}
```

**PS：** 注意“函数声明”和“函数表达式”的区别

```javascript
fn(); // 不会报错
function fn(){};

fn1(); // 会报错 因为会进行变量提升 var fn1; 所以执行fn1();会报错
var fn1 = function(){};
```

this

`this`要在执行时才能确认值，定义时无法确认

```javascript
var a = {
    namr:'A',
    fn:function(){
        console.log(this.name);
    }
}
a.fn(); // this === a
a.fn.call({name:'B'}); // this === {name:'B'}
var fn1 = a.fn;
fn1(); // this === window
```

+ 作为构造函数执行

```javascript
function Foo(name){
    // this = {}
    this.name = name;
    // return this;
}
var f = new Foo('zhangsan');
```

+ 作为对象属性的执行

```javascript
var obj = {
    name:'A',
    printName:function(){
        console.log(this.name);
    }
};
obj.printName();// this指向obj当前的对象
```

+ 作为普通函数执行（`window`）

```javascript
function fn(){
    console.log(this); // this === window
}
fn();
```

+ `call` `bind` `apply`

```javascript
// call apply
function fn1(name,age){
    console.log(name,age);
    console.log(this); // this === window
}
fn1.call({x:100},'zhangsan',20);
fn1.apply({x:100},['zhangsan',20]);

// bind
function fn2(name,age){
    console.log(name,age);
    console.log(this);
}
var fn3 = fn2.bind({x:300});
fn3('zhangsan',20);
```

作用域

+ 在`JavaScript`中没有块级作用域
+ 只有函数和全局作用域

```javascript
// 无块级作用域
if(true){
    var name = 'zhangsan';
}
console.log(name);

// 函数和全局作用域
var a = 100;
function fn(){
    var a = 200;
    console.log('fn',a);
}
console.log('global',a);
fn();
```

作用域链

```javascript
// 作用域链
var a = 100;
function fn(){
    var b = 200;
    
    // 当前作用域没有定义的变量，即"自由变量"
    console.log(a);// 当前作用域没有 就去父级作用域(函数在哪定义，父级作用域就在哪)一层一层进行查找
    console.log(b);
}
fn();

var a = 100;
function F1(){
    var b = 200;
    function F2(){
        var c = 300;
        console.log(a);// a是自由变量
        console.log(b);// b是自由变量
        console.log(c);
    }
    F2()
}
F1();
```

闭包

闭包就是能够读取其他函数内部变量的函数

+ 函数作为返回值

```javascript
function F1(){
    var a = 100;
    
    // 返回一个函数（函数作为返回值）
    return function(){
        console.log(a);// 自由变量，父作用域寻找
    }
}

// f1 得到一个函数
var f1 = F1();
var a = 200;
f1();// 100
```

+ 函数作为参数传递

```javascript
function F1(){
    var a = 100;
    return function(){
        console.log(a);
    };
}

var f1 = F1();

function F2(fn){
    var a = 200;
    fn();
}
F2(f1);
```


### 面试题目

+ 说一下对变量提升的理解

```javascript
// 变量定义
// 函数声明(注意和函数表达式的区别)
```

+ 说明 `this` 几种不同的使用场景

```javascript
1. 作为构造函数执行
2. 作为对象属性执行
3. 作为普通函数执行
4. call apply bind
```

+ 创建10个`<a>`标签 点击的时候弹出来对应的序列

```javascript
var a;
for(var i = 0;i<10;i++){
    (function(i){
        a = document.createElement('a');
        a.innerHTML = i + '<br/>';
        a.addEventListener('click',function(e){
            e.preventDefault();
            alert(i);
        });
        document.body.appendChild(a);
    })(i);
}
```

+ 如何理解作用域

```javascript
1. 自由变量
2. 作用域链，即自由变量的查找
3. 闭包的理解
```

+ 实际开发中闭包的应用

```javascript
function isFirstLoad(){
    var _list = [];
    return function(id){
        if(_list.indexOf(id)>=0){
            return false;
        } else {
            _list.push(id);
            return true;
        }
    }
}

var firstLoad = isFirstLoad();
firstLoad(10);
firstLoad(10);
firstLoad(20);
```

