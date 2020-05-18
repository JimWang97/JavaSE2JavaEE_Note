# 11. HTML
超文本标记语言

## 11.1. W3C标准
- 结构化标准语言（HTML、XML）
- 表现标准语言（CSS）
- 行为标准（DOM 文档对象模型、ECMAScript）

## 11.2. HTML基本结构
`<body>`,`</body>`分别叫开放标签和闭合标签

单独呈现的`<hr/>` 叫自闭合标签

### 11.2.1. 网页的基本信息
```html
<!--DOCTYPE: 高速浏览器要使用什么规范-->
<!DOCTYPE html>
<html lang="en">
<!--head标签代表网页头部-->
<head>
    <!--meta描述性标签，用来描述网页信息-->
    <!--meta一般用来做SEO-->
    <meta charset="UTF-8">
    <meta name="keywords" content="学习html">
    <meta name="description" content="第一个html网页">
    <!--网页标题-->
    <title>Title</title>
</head>
<!--body标签代表网页主体-->
<body>
</body>
</html>
```

### 11.2.2. 网页基本标签
- 标题标签
- 段落标签
- 换行标签
- 水平线标签
- 字体样式标签
- 注释和特殊符号

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!--标题标签-->
<h1>一级</h1>
<h2>二级</h2>

<!--段落标签-->
<p>两只老虎 跑得快</p>
<p>两只老虎 跑得快</p>
<p>两只老虎 跑得快</p>

<!--水平线标签-->
<hr/>

<!--换行标签 间距比段落标签小-->
两只老虎 跑得快<br/>
两只老虎 跑得快<br/>
两只老虎 跑得快<br/>

<!--粗体 斜体-->
<h1>字体样式</h1>
<strong>粗体</strong>
<em>斜体</em>

<!--特殊符号-->
空&nbsp;&nbsp;&nbsp;格
&gt;
&lt;
&copy; 版权所有

</body>
</html>
```

### 11.2.3. 图像标签
`<img src="path" alt="text" title="text" width="x" height="y"/>`
- src图像地址 必填
- alt图像的替代文字（当图片没加载出来时显示的字） 必填
- title鼠标悬浮提示文字
- width宽度
- height高度

### 11.2.4. 链接标签
`<a href="path" target="目标窗口位置">链接文本或图像</a>`
- href链接路径 必填
- target链接在哪个敞口打开（常用属性_self默认、_blank）

图像点击跳转到别的网站
```html
<a href="我的第一个网页.html">
    <img ...>
</a>
```

#### 11.2.4.1. 锚链接
1. 需要一个锚标记 用name
2. 跳转到标记
```html
<a name="top">顶部</a>

...

<a href="#top">回到顶部</a>
<!-- 跳转到页面4的top标记点 -->
<a href="页面4.html#top">回到顶部</a>
```

#### 11.2.4.2. 功能性链接
邮件链接

`<a href="mailto:1111@qq.com">点击联系</a>`

## 11.3. 行内元素和块元素
- 块元素
  - 无论内容多选，该元素独占一行
  - 用了会另起一段的，比如`<h1>``<p>`等
- 行内元素
  - 内容撑开宽度，左右都是行内元素的可以排在一行
  - 一行内可以放多个标签的，比如`<strong><em>`

## 11.4. 列表
- 无序列表(·)
  ```html
  <ul>
        <li>java</li>
        <li>python</li>
  </ul>
  ```
- 有序列表(1,2,3,4,5)
  ```html
  <ol>
        <li>java</li>
        <li>python</li>
  </ol>
  ```
- 自定义列表
  - dl 标签
  - dt 列表名称
  - dd 列表内容
  ```html
  <dl>
        <dt>学科</dt>
        <dd>java</dd>
        <dd>python</dd>
        <dt>城市</dt>
        <dd>杭州</dd>
        <dd>上海</dd>
  </dl>
  ```
## 11.5. 表格
```html
<table>
    <tr>
        <!-- 跨三列 -->
        <td colspan="3">1-1</td>
    </tr>
    <tr>
        <!-- 跨两行 -->
        <td rowspan="2">2-1</td>
        <td>2-2</td>
        <td>2-3</td>
    </tr>
    <tr>
        <td>3-1</td>
        <td>3-2</td>
    </tr>
</table>
```
## 11.6. 视频和音频
`<video src="" controls autoplay></video>` & `<audio src="" controls autoplay></audio>`
- src资源路径
- controls 控制条
- autoplay 自动播放

## 11.7. 页面结构分析
- header 头部区域内容
- footer 标记脚部区域的内容
- section 一块独立区域
- article 文章内容
- aside 相关内容或应用
- nav 导航类辅助内容

## 11.8. iframe内联框架
`<iframe src="path" name="mainFrame"></iframe>`
- src引用页面地址
- name框架标识名

配合a标签使用
```html
<iframe src="" name="hello"></iframe>
<a href="baidu.com" target="hello">点击跳转</a>
```
会在内联框架中打开百度

## 11.9. 表单
```html
<form method="" action="">
    <p>名字：<input type="text" name="username"></p>
    <p>密码：<input type="password" name="pwd"></p>
    <p>密码：<input type="submit" name="提交"></p>
</form>
```

### 11.9.1. input标签
属性
- type 指定元素的类型
- name 指定表单元素的名称
- value 元素的初始值
- size 初始宽度
- maxlength 当为text或password时，输入的最大字符数
- checked type为radio或checkbox时，指定按钮是否是被选中

[单选框标签]
```html
<input type="radio" value="boy" name="sex"/>男
<input type="radio" value="girl" name="sex"/>女
```
两个的name必须一致

[多选标签]
```html
<input type="checkbox" value="sleep" name="hobby"/>男
<input type="checkbox" value="code" name="hobby"/>女
```
两个的name也要一致

[按钮]
- button 普通按钮
- image 图像按钮
- submit 提交按钮
- reset 重置

[文件域]

`<input type="file" name="files">`

```html
<!--邮箱-->
<p>
    邮箱：<input type="email" name="email">
</p>
<!--url-->
<p>
    url：<input type="url" name="url">
</p>
<!--数字-->
<p>
    数字：<input type="number" name="num" max="100" min="0" step="1">
</p>
<!--滑块-->
<p>滑块
    <input type="range" min="0" max="100" name="voice" step="2">
</p>
<!--搜索-->
<p>搜索
    <input type="search" name="search">
</p>
```
### 11.9.2. 下拉框
```html
<select name="列表名称">
    <option value="china" selected>中国</option>
    <option value="usa">美国</option>
    <option value="uk">英国</option>
</select>
```

### 11.9.3. 文本域
`<textarea name="textarea" cols="10" rows="50">文本内容</textarea>`

### 11.9.4. 鼠标增强可用性
```html
<p>
    <label for="mark">鼠标增强可用性</label>
    <input type="text" id="mark">
</p>
```
