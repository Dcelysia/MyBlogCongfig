---
title: javaScript
toc: true
date: 
cover: /img/明日方舟小人4.jpg
categories: 个人笔记
description: 想不出来介绍了
tags: 后端
post_meta:
  page:
    date_type: both # created or updated or both 主頁文章日期是創建日或者更新日或都顯示
    date_format: relative # date/relative 顯示日期還是相對日期
    categories: true # true or false 主頁是否顯示分類
    tags: true # true or false 主頁是否顯示標籤
    label: false # true or false 顯示描述性文字
  post:
    date_type: both # created or updated or both 文章頁日期是創建日或者更新日或都顯示
    date_format: relative # date/relative 顯示日期還是相對日期
    categories: true # true or false 文章頁是否顯示分類
    tags: true # true or false 文章頁是否顯示標籤
    label: false # true or false 顯示描述性文字
copyright_author: Elysian剑彧
copyright_author_href: https://dcelysia.netlify.app/
copyright_url: https://dcelysia.netlify.app/
copyright_info: 此文章版权归Elysian剑彧所有，如有转载，请注明来自原作者
---



# javaScript

##    变量

在javaScript中，大多数的变量都是弱类型的，一般都是用var进行定义的，而且var存的东西的类型是可以改变的，一会可以是整形，一会可以是字符串

## 运算符

### 比较运算符

其中比较运算符最值得说道说道的是=== 和 ==,\==虽然也能进行比较，但是比较的时候如果两个类型不相同就会进行类型转化再比较，就比如“123”和123在\==比较下就是相同的，而\==\=就是不进行类型转换的比较，如果类型不相同是直接报错的

### 条件判断if

在if中，跟c类似，就是NaN，null,0,undefined,还有空字符串“”都是false，其他都是true

## 数组

### 数组的创建方式

```javascript
//第一种
 var arr=new Array(1,2,3);
//第二种
var arr=[1,2,3,4];
```

在javaScript中，其实数组就是集合，集合就是数组，这里的长度是可以任意改变的，就比如这个数组即使

```javascript
a[10]="123"
```

也是可以的，数组类型不定，长度不定，如果中间没定义的数据全部都是undefined

### 数组的遍历方式

一种是直接for然后用索引去遍历

```javascript
   for (let i = 0; i < arr.length; i++) {
        console.log(arr[i]);
    }
```

另一种是用foreach，相当于增强for循环去遍历

```javascript
    arr.forEach(function(e){
        console.log(e);
    })
```

也可以改成lambda表达式

```javascript
    arr.forEach((e)=>{
        console.log(e);
    })
```

### 数组的push方法

数组的push方法会把push的数据放在数组的尾部

```javascript
    arr.push(12,"123","abc")
    arr.forEach(function(e){
        console.log(e);
    })
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/e6decb6b25e5466c9701a90503f106eb.jpeg)

### 数组的删除

```javascript
arr.slice(2)
```

括号里面是数组的索引，也可以同时删除多个元素，就比如我要删除第2个和第3个

```javascript
//同时删除第二个和第三个
arr.slice(1，2)
```
## 字符串

### 创建方式

也是有两种

```javascript
 //第一种方式
 var str1="你好";
 //第二种方式
 var str2=new String("你好啊");
```

### 常用方法

也是有charAt,indexOf,substring等方法的

#### trim

用于去除字符串左右两边的空格

```javascript
var str2=new String("  abaaab  ");
var str3=str2.trim();
```

打印出来是abaaab,两边的空格被删掉了

## JSON

### javaScript的对象

#### 创建方法

```javascript
     var lei={
         name: "TOM",
         age:  18,
         eat: function(){
             alert("我要用膳了")
         }
     }
     //调用
      alert(lei.name)
```



### JSON字符串

在数据传输中，对象的数据会用JSON的方式进行传递，就是以下格式

用JSON.parse的方法可以把一个JSON的字符串转成一个对象，并正常调用

```javascript
    var jsonStr='{"name":"TOm","age":18}';
    var lei=JSON.parse(jsonStr)
    alert(lei.name)
```

## BOM

在BOM里其实就是浏览器的一些框架

开头是**window.**

其中有用的是alert就是弹出一个框

confirm就是弹出一个选择框

location.herf就是显示和设置这个网址的地址，正常就是自己的网址，设置完可以打开别的网址

## DOM

DOM是文档对象模型，将标记语言的各个组成部分封装成对应的对象

比如:

**Document:** 整个文档对象

**ELement**: 元素对象

**Attribute**： 属性对象

**Text**: 文本对象

**Comment**: 注释对象

DOM也把很多HTML标签变成一个对象，就比如**Image**: <img\>,**Button**: <input type='Button'\>

### DOM获取HTML中的Element对象的方法

可以通过HTML的id属性，标签名字，name属性值和class属性值获取，如果有多个是返回的是Element对象的数组

通过这个返回的Element对象可以去JS官方的文档里查询用法，就可以做到改变HTML的元素

## JS事件监听

### 设置监听函数

已点击事件监听为例子，有两种办法给一个按钮设置事件监听

```javascript
//通过DOM获取ELement对象然后对onclick点击事件进行定义
<input type="button" id="按钮1" value="按钮1" >
//下面是script
document.getElementById("按钮1").onclick=function on1(){
        alert("按钮1被点击了")
  }
```

```javascript
//在定义Element的时候就把onclick和一个函数进行绑定，然后再script里对这个函数进行定义
<input type="button" value="按钮2" onclick="on2()">
    //下面是script
    function on2(){
        alert("按钮2被点击了")
    }
```

### 常见事件监听

#### onload

在加载完成是会触发

```javascript
<body onload="load()">
    
</body>
<script>
    function load(){
        alert("加载完毕")
    }
</script>
```

在浏览器启动的时候会弹出一个加载完毕的提示

#### onblur和onfoucs

成为焦点和失去焦点的时候会触发

```javascript
</head>
     <form action="" style="text-align: center;" onsubmit="submit()">
        <input type="text" name="username" onblur="left()" onfocus="focus2()">
</form>
</body>
<script>
    function left(){
        console.log("失去焦点");
    }
    function focus2(){
        console.log("成为焦点");
    }
</script>
```

焦点就是这个表单上面有光标闪烁就是焦点，没有就不是焦点

#### onkeydown

在按下键盘的时候会触发

```javascript
</head>
     <form action="" style="text-align: center;" onsubmit="submit()">
        <input type="text" name="username" onkeydown="kdown()">
     </form>
    
</body>
<script>
    function kpress(){
        alert("键盘按下")
    }
</script>
```

#### 其他

跟上面的写法都大差不差


**onsubmit**：当表单被提交

**onmouseover**:鼠标光标移除

**onmouseout**：鼠标从某元素移开
