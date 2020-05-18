# 13. JavaScript
脚本语言

ECMAScript是JavaScript的一个标准。最新版本到了es6，但是浏览器只支持es5，会导致开发环境和线上环境不一致。

## 13.1. 入门
### 13.1.1. 引入JavaScript
一般放在body或者头部
```html
<!-- 内部标签 -->
<script>
    alert("hello");
</script>
<!--外部引入 script标签必须成对出现-->
<script src="js/1.js"></script>
```
### 13.1.2. 数据类型
- number 不区分小数和整数 支持科学计数法 NaN Infinity
- 字符串 单引号双引号都可以
- 布尔值
- 逻辑运算
  - && || ！
- 比较运算符
  - = 赋值
  - == 等于（类型不一样，值一样也会为true）
  - === 绝对等于（类型一样，值一样）
  - NaN不绝对等于NaN 与谁都不相等
  - 只能通过isNaN(NaN)
- null和undefined
  - null 空
  - undefined 未定义
- 数组
  - `var arr=[1,2,3,null,"hello"];`
  - 不需要同一个类型
  - 超过了数组长度arr[8]会显示undefined，不会报错
- 对象
  - `var person={name:"wyj", age:3, tags=['js','java']}`
- 变量都是用var声明

### 13.1.3. 严格检查模式
局部变量用`let`定义

在最前面加`use strict;` 表示使用了严格检查模式，预防JavaScript的随意性产生问题。必须写在第一行

## 13.2. 数据类型
### 13.2.1. 字符串
1. 正常字符串是用单引号或者双引号包裹
2. 转义字符`\`
3. 多行字符串编写 是用` tab上面那个
   ```javaScript
   var msg=`
   hello
   nihao
   `
   ```
4. 模板字符串 是用` tab上面那个
   ```javaScript
    let name = "wyj";
    let msg=`你好,${name}`;
   ```
5. 字符串长度 `str.length`
6. 字符串的可变性，不可变 可赋值但是其实不会变
7. `str.indexOf('t')`
8. `str.substring(1,3)` 左闭右开

### 13.2.2. 数组
Array可以包含任何的数据类型

- 长度 `arr.length`
- 可变性
  ```javaScript
    arr[0]=0; //会改变
    arr.length=10;//长度会扩展成10 多出来的几个为undefined。如果赋值变小，元素会丢失
  ```
- `indexOf` 通过元素获得下标索引
- `slice(1,3)` 截取array的一部分，返回一个新的数组
- `push('a','b')` 往数组最后压入数据
- `pop()` 推出最后一位数据
- `shift()` 去掉头部的
- `unshift()` 往数组头部压入数据
- `sort()`
- `reverse()`
- `concat()` 不修改数组，返回一个新的数组
- `join()` 连接符，使用特定的字符链接

### 13.2.3. 对象
若干个键值对

用大括号括起来，多个属性之间使用逗号隔开。

- 对象复制 `person.name="wyj"`
- 使用一个不存在的对象不会保存 undefined
- 动态的删减属性 `delete person.name`
- 动态的添加属性 `person.haha = "haha"` 直接给新的属性添加值即可
- 判断属性值是否在属性中 `'haha' in person`
- 判断一个属性是否是这个对象自身拥有的 `person.hasOwnProperty('haha')`

### 13.2.4. 流程控制
`if{}else if{} else{}`

`for(var num in age)` 这个打印出来的是0，1，2，3，4下标。不建议使用，会有bug

`for(var num of age)` 这个打印出来的是age中按顺序的值

`age.forEach(function(value){console.log(value)});`

### 13.2.5. map和set
```JavaScript
// Map
var map=new Map([['tom',90],['jack',80]]);
var score = map.get('tom');
map.set('admin',123456);
map.get('admin');
map.delete('tom');

// Set 无序不重复的集合
var set= new Set([3,1,1,1,1]);
set
// 打印出来是[3,1]  会去重
set.add(2);
set.delet(2);
set.has(3);
```
### 13.2.6. iterator
使用`for of`来遍历

## 13.3. 函数
### 13.3.1. 定义函数
```javascript
// 方式1
function abs(x){
    if(x>=0){
        return x;
    }else{
        return -x;
    }
}

// 方式2 匿名函数，结果赋值给abs变量
var abs = function (x){
    if(x>=0){
        return x;
    }else{
        return -x;
    }
}
```
如果没有return 也会返回结果 是undefined

### 13.3.2. 函数调用
参数问题：`abs(10,10,20)` 不会出问题，可以传任意参数，也可以不传

```javascript
var abs=function(x){
    // 手动抛出异常
    if(typeof x!==`number`){
        throw `not a number`;
    }
    ...
}
```
#### 13.3.2.1. 多参数问题
[arguements]
```javascript
var abs=function(x){
    console.log(x);
    for(var i=0;i<arguments.length;i++){
        console.log(arguments[i]);
    }
}
abs(10,20,30,40,50);
// 第一个log(x)只能输出10
// 第二个可以输出全部
```
arguments代表传递进来的所有参数，代表一个数组

[rest]
获取除了已经定义了的参数之外的所有参数
```javascript
function aaa(a,b,...rest){
    if(rest==="a"){
        console.log(a);
    }else{
        console.log(b);
    }
}
```
只能写在最后面，必须...rest

### 13.3.3. 变量的作用域
会自动提升变量的作用域，自动定义，但是不会赋值
```javascript
var x = "x" + y;
console.log(x);
y = "y";
```
这里提示xundefined。因为y会被自动的声明，但是没有赋值。导致x为undefined

一般所有的变量都在头部先声明了。

全局变量：声明在最外面。默认所有的全局变量都会绑定在`window`对象下

为了防止不同js文件中的变量名冲突。可以这么做：
```javascript
var file1_global = {};
// 定义全局变量
file1_global.name="...";
filr1_global.add = function(a,b){
    return a+b;
}
```
把自己的代码全部放入自己定义的唯一命名空间中

### 13.3.4. 局部作用域
```javascript
for(var i=0; i<100; i++){
    console.log(i);
}
console.log(i+1); //输出101

for(let i=0; i<100; i++){
    console.log(i);
}
console.log(i+1); //报错
```
建议局部变量用let声明 es6的新特性

### 13.3.5. 常量
ES6之前，约定全部大写命名的就是常量，建议不要修改它。

在ES6之后，引入了`const`。直接`const a=1`

### 13.3.6. 方法
写到对象中的函数就叫做方法

对象只有属性和方法

```javascript
// 方法一
var obj={
    name: xxx,
    birth: 2000,
    age: function(){
        return 2020-this.birth;
    }
}

// 方法二
function getage(){
    return 2020-this.birth;
}
var obj={
    name: xxx,
    birth: 2000,
    age: getage
}
// 这样是可以的 但是下面这个会NaN
getage(); // 直接使用
```

apply可以控制this的指向
```javascript
function getage(){
    return 2020-this.birth; 
}
var obj={
    name: xxx,
    birth: 2000
}

getage.apply(obj,[]); //第二个参数是这个函数需要的参数
```

## 13.4. 内部对象
### 13.4.1. Date
`var now = new Date()`
- getFullYear()
- getMonth() 0-11
- getDate() 日
- getDay() 周几
- getHours()
- ....
- toLocalString()
- toGMTString()

### 13.4.2. json
格式：
- 对象都用{}
- 数组都用[]
- 键值对都用Key：value

```javascript
var obj={
    'name': 'wyj',
    'age': 3
};
// 对象转json字符串
var jsonString = JSON.stringify(obj);
// json字符串转对象
var new_obj = JSON.parse(jsonString);
```

### 13.4.3. ajax
- 原生的js写法 xhr异步请求

## 13.5. 面向对象编程
原型(类似于继承)
```javascript
var user={
    name: "wyj",
    age: 3,
    run: function(){
        console.log(this.name + " run");
    }
}
var xiaoming={
    name:"xiaoming"
}

xiaoming.__proto__ = user;
xiaoming.run(); // 输出 xiaoming run
xiaoming.age; // 输出3
```

class继承（es6引入）
```javascript
class Student{
    constructor{name}{
        this.name = name;
    }
    hello(){
        alsert("hello");
    }
}

var xiaoming = new Student("xiaoming");

class primaryStudent extends Student{
    constructor(name, age){
        super(name);
        this.age = age;
    }
    ....
}
```
## 13.6. 操作BOM对象(重点)
BOM：浏览器对象模型
- IE
- Chrome
- Safari
- FireFox

### 13.6.1. window
- `window.innerHeight`
- `window.innerWidth`
- `window.outerHeight`
- `window.outerWidth`

### 13.6.2. screen
代表屏幕尺寸
- `screen.width` 
- `screen.height`

### 13.6.3. location(重要)
代表当前页面的url信息
- `location.host` 主机
- `location.href` 链接
- `location.protocol` 协议
- `location.reload()` 重新加载
- `location.assign("地址")` 跳转

### 13.6.4. document
当前页面信息
- `document.title` 标题
- `document.getElementById('')` 获取具体的文档树节点。html中的id
- `document.cookie()` 获取cookie 可能会被劫持 可以在http端设置httpOnly来保护

### 13.6.5. history
支持网页中的后退 前进功能
- `history.back()` 返回
- `history.forward()` 前进

## 13.7. 操作DOM（重要）
DOM：文档对象模型

浏览器网页就是一个DOM树形结构
- 更新：更新DOM节点
- 遍历DOM节点
- 删除DOM节点
- 添加DOM节点

### 13.7.1. 获取DOM
```html
<div id="father">
    <h1></h1>
    <p id="p1"><p>
    <p class="p2"><p>
</div>
```
```javascript
var h1 = ducument.getElementByTagName("h1"); // 拿到标签名为h1的
var p1 = document.getElementById("p1");
var p2 = document.getElementByClass("p2");
var father = document.getElementById("father");

var children = father.children; //获取父节点下的所有子节点
father.firstChild
```
### 13.7.2. 更新节点
```javascript
var id1 = document.getElementById("id1");
id1.innerText="456";// 修改文本的值
id1.innerHTML='<strong>456</strong>'; // 加粗
id1.style.colore = 'red'; // 变色
id1.style.fontSize = '20px'; //字体大小
```

### 13.7.3. 删除节点
删除节点的步骤：先获取父节点，再通过父节点删除自己
```javascript
var father = p1.parentElement;
father.removeChild(p1);
```
### 13.7.4. 插入节点
获得DOM节点，假设为空，通过innerHTML就可以增加一个元素，但是如果已经存在了，会产生覆盖。
```javascript
// 插入在目标节点前面
father.insertBefore(新节点, 目标节点);
// 插入在最后
father.appendChild();

var newP = document.createElement('p'); // 创建一个新p标签
newP.id = 'newP';
newP.innerText = 'Hello';

// 创建一个标签节点 添加没有的属性
var myScript = document.createElement('script');
myScript.setAttribute("type","text/javascript");
```
## 13.8. 操作表单(验证判断)
```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--md5工具类  加密用-->
    <script src="https://cdn.bootcss.com/blueimp-md5/2.12.0/js/md5.js"></script>
<body>

<form method="post" onsubmit="return fun()">
    <p>
        <span>用户名:</span> <input type="text" id="username" name="username">
    </p>
    <p>
        <span>密码:</span> <input type="password" id="password">
    </p>
    <p>
        <input type="radio" name="sex" value="man" id="man" checked="checked">男
        <input type="radio" name="sex" value="woman" id="woman">女
    </p>
    <input type="hidden" id="md5-password" name="password">
    <button type="button"> 提交</button>
</form>
<script>
    var input_text = document.getElementById("username");
    var value = input_text.value;
    console.log(value);

    // 单选与多选框被选定
    console.log(document.getElementById("man").checked)

    function fun(){
        console.log(document.getElementById("username").value);

        // 建议这样加密 把真正的加密密码从表单中隐藏起来
        var md5pwd = document.getElementById("md5-password");
        md5pwd.value=md5(pwd.value);
        // 这里如果return false 则表单提交了不会跳转 否则跳转
    }
</script>
</body>
</html>
```
## 13.9. jQuery
javascript的库，存在大量的JavaScript函数

![文档](jquery.cuishifeng.cn)

`$`代表jquery。

**公式:`$(selector).action()`**

```javascript
$('#test').click(function(){
    alert('hello');
})
```
### 13.9.1. 选择器
选择器就是css的选择器

```javascript
// 标签
document.getElementsByTagName();
$('p').click();
// id
document.getElementById();
$('#class').click();
// 类
document.getElementsByClassName();
$(`.class`).click();
```

### 13.9.2. 操作DOM元素
```javascript
<body>

<ul class="test-ul">
    <li class="js">js</li>
    <li class="python">python</li>
</ul>

<script>
    $('#test-url li[name=python]').text();
    $('#test-url li[name=python]').css("color","red");
    $('#test-url li[name=python]').hide();
    $('#test-url li[name=python]').show();
    $('#test-url').html();
</script>
</body>
</html>
```
