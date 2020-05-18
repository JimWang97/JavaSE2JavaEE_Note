# 12. CSS
html（框架）+CSS（表现）+JavaScript（交互）

如何学习
1. CSS是什么
2. CSS怎么用
3. **CSS选择器（重点）**
4. 美化网页（文字，阴影，超链接，列表，渐变..）
5. 盒子模型
6. 浮动
7. 定位
8. 网页动画（特效）

## 12.1. 什么是CSS
Cascading Style Sheet 层叠级联样式表

### 12.1.1. 入门
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <!--规范 在style中编写css代码
    语法：
        选择器{
            声明;
        }
    -->
    <style>
        h1{
            color: red;
        }
    </style>
</head>
<body>

<h1>我是标题</h1>

</body>
</html>
```

### 12.1.2. html和css分离
在html中的head中加一句话
`<link rel="stylesheet" href="css/style.css">`

css文件中不再需要style标签

### 12.1.3. css的3中导入方式
```html
<!--行内样式-->
<h1 style="color: red">我是标题</h1>
<!--外部样式-->
<link rel="stylesheet" href="css/style.css">

<!--内部样式-->
<style>
    h1{
        color:green;
    }
</style>
```
优先级：就近原则，谁是最后一个定义的，谁做主

## 12.2. 选择器
作用：选择页面上的某一个或者某一类元素

### 12.2.1. 基本选择器
1. 标签选择器
2. 类选择器
3. id选择器

优先级 3>2>1

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        /*标签选择器会选择页面上所有的这个标签*/
        h1{
            color: red;
        }
        /*类选择器的格式 .class的名称{}
        好处：可以多个标签归类，同一个class*/
        .java{
            color: green;
        }
        /*id选择器 #id的名称{}
        id必须保证全局唯一*/
        #id1{
            color: deepskyblue;
        }
        /*不遵循就近原则，优先级：id选择器>类选择器>标签选择器*/
    </style>
</head>
<body>

<h1>java</h1>
<h1 id="id1">java</h1>
<h1 class="java">java</h1>
<p class="java">python</p>

</body>
</html>
```

### 12.2.2. 层次选择器
1. 后代选择器
2. 子选择器
3. 相邻兄弟选择器
4. 通用选择器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        p{
            background: green;
        }
        /*后代选择器
        body后面的所有p标签*/
        body p{
            background: red;
        }
        /*子选择器 body下面的第一层p*/
        body>p{
            background: deepskyblue;
        }
        /*相邻兄弟选择器（向下的）
        class=active的周围标签
        这里只有p2会变颜色*/
        .active+p{
            background: brown;
        }
        /*通用选择器
        当前选中元素的向下的所有兄弟元素变色
        2 3 都会变色*/
        .active~p{
            background: aquamarine;
        }
    </style>
</head>
<body>
<p>p0</p>
<p class="active">p1</p>
<p>p2</p>
<p>p3</p>
<ul>
    <li>
        <p>p4</p>
    </li>
    <li>
        <p>p5</p>
    </li>
    <li>
        <p>p6</p>
    </li>
</ul>
</body>
</html>
```

### 12.2.3. 结构伪类选择器
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--避免使用class和id选择器-->
    <style>
        /*ul的第一个子元素*/
        ul li:first-child{
            background: brown;
        }
        /*ul的最后一个子元素*/
        ul li:last-child{
            background: aquamarine;
        }
        /*选中p1 定位到父元素，选择当前的第一个元素
        选中当前p元素的父元素的第n个子元素，并且是当前元素才生效*/
        p:nth-child(1){
            background: deepskyblue;
        }

        /*选择父元素中，第二个类型为p的元素*/
        p:nth-of-type(2){
            background: yellow;
        }
    </style>
</head>
<body>
<p>p0</p>
<p>p1</p>
<p>p2</p>
<p>p3</p>
<ul>
   <li>li1</li>
   <li>li2</li>
   <li>li3</li>
</ul>
</body>
</html>
```
### 12.2.4. 属性选择器(重要)
结合了id+class
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .demo a{
            float: left;
            display: block;
            height: 50px;
            width: 50px;
            border-radius: 10px;
            background: deepskyblue;
            text-align: center;
            color: grey;
            text-decoration: none; /*去下划线*/
            margin-right: 5px;
            font: bold 20px/50px Arial;
        }

        /*存在id的属性 a[]{}*/
        /*a标签中存在id属性的*/
        a[id]{
            background: yellow;
        }
        /*a标签中存在id属性=first的 支持正则*/
        a[id="first"]{
            background: brown;
        }
        /*class 中有link的元素
        这里用*= 表示包含*/
        a[class*="links"]{
            background: green;
        }
        /*选中href中以http开头的元素*/
        a[href^=http]{
            background: antiquewhite;
        }
        /*href中以pdf结尾*/
        a[href$=pdf]{
            background: yellow;
        }
    </style>
</head>
<body>
<p class="demo">
    <a href="" class="links items first" id="first">1</a>
    <a href="" class="links items activate">2</a>
    <a href="" class="links items">3</a>
    <a href="" class="links items">4</a>
    <a href="">5</a>
    <a href="">6</a>
    <a href="" class="links items last">7</a>
</p>
</body>
</html>
```

## 12.3. 美化网页元素
### 12.3.1. span
重要要突出的字用span标签括起来

约定俗成

### 12.3.2. 字体样式
- font-family:字体
- font-size:字体大小
- font-weight:粗细
- color:颜色

### 12.3.3. 文本样式
1. 颜色
2. 文本对齐方式 text-align
3. 首行缩进 text-indent: 2em 
4. 行高 line-height
5. 装饰 text-decoration:

水平对齐

```html
.img,.text{
    vertical-align:middle
}
```
### 12.3.4. 超链接伪类
```css
/* 鼠标悬浮的颜色 */
a:hover{
    color:orange; 
}
a:active{
    color:green;
}
```
