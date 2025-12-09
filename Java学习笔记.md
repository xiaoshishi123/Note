

# JavaWeb

课程：https://www.bilibili.com/video/BV1m84y1w7Tb?p=5&vd_source=d9ebacf85c69289ce6e270a1c64762bb

项目源码地址：https://github.com/faithtaker/Note-JavaWeb

下载下来放在：D:\JavaPractice

## 											24年8月15日  HTML+CSS学习

#### 1，CSS设置标题

CSS设置标题样式

1，内嵌  2，外联  

设置颜色的方法（有些复杂）

对于h1 h2这种标签 直接通过内嵌样式 给每一级标签定义即可

```css
<style>
        h1 {
            /* color: red; */
            /* color: rgb(0, 0, 255); */
            color: #4D4F53;
        }
    </style>
```

对于<hr>这种单纯的标题，就需要用CSS选择器来定义这个标题

ID唯一  类选择器可以多个

###### **1.id选择器:**

- 选择器的名字前面需要加上#
- 作用：选择器中的样式会作用于指定id的标签上，而且`有且只有一个标签`（由于id是唯一的）

例子如下：

~~~css
#id {
    color: blue;
    font-size: 18px; /* 设置字体大小 */
}
~~~



###### **2.类选择器：**

- 选择器的名字前面需要加上 .
- 作用：选择器中的样式会作用于所有class的属性值和该名字一样的标签上，可以是多个

例子如下：

~~~css
.cls{
     color: green;
    font-size: 18px; /* 设置字体大小 */
 }
~~~

#### 2 添加超连接

让某些标签或者文字可以点

- 标签: &lt;a href="..." target="...">这是一个超链接</a>
- 属性:
  - href: 指定资源访问的url
  - target: 指定在何处打开资源链接
    - _self: 默认值，在当前页面打开
    - _blank: 在空白页面打开

```css
<span id="time">2023年03月02日 21:50</span>  
<span> <a href="https://news.cctv.com/2023/03/02/ARTIUCKFf9kE9eXgYE46ugx3230302.shtml" target="_blank">这是一个超链接</a></span>
```

重点：

1，超链接的标签就是<a> 所以给他设置样式也是 直接用a开头 类似给h1标题设样式

```css 
a {
            color: black;
            text-decoration: none;  
        }

        a:hover {
            color: blue; /* 这表示鼠标移动上去之后标签的样式 */
            text-decoration: underline;
        }
```

2， text-decoration: none;是对这个超链接的文本设置，例如加下划线，不加等

#### 3，正文排版

视频和音频  controls标签的意义是视频的播放暂停等功能 封装好了 必须加上

```css
    <!-- 正文 -->
    <!-- 视频 -->
    <video src="video/1.mp4" controls width="950px"></video>

    <!-- 音频 -->
    <audio src="audio/1.mp3" controls></audio>
```

加文字 有手就行 不懂临时看就够了

```css
<p>
    <strong>央视网消息</strong> （焦点访谈）：党的十八大以来，以习近平同志为核心的党中央始终把解决粮食安全问题作为治国理政的头等大事，重农抓粮一系列政策举措有力有效，我国粮食产量站稳1.3万亿斤台阶，实现谷物基本自给、口粮绝对安全。我们把饭碗牢牢端在自己手中，为保障经济社会发展提供了坚实支撑，为应对各种风险挑战赢得了主动。连续八年1.3万亿斤，这个沉甸甸的数据是如何取得的呢？
    </p>
```

#### 4，正文布局

```
div标签用于创建页面结构和布局，通常包含较大块内容。
```

**`<span>`**: 用于包装小块内容或部分文本，以便应用特定样式

1，用div标签把正文包起来，取个名叫center

```css
<div id="center">
        <!-- 标题 -->
        <img src="img/news_logo.png"> <a href="http://gov.sina.com.cn/" target="_self">新浪政务</a>  > 正文

        <h1>焦点访谈：中国底气 新思想夯实大国粮仓</h1>

        <!-- 正文 -->
        <!-- 视频 -->
        <video src="video/1.mp4" controls width="950px"></video>

        <!-- 音频 -->
        <!-- <audio src="audio/1.mp3" controls></audio> -->

        <p>
        <strong>央视网消息</strong> （焦点访谈）：党的十八大以来，以习近平同志为核心的党中央始终把解决粮食安全问题作为治国理政的头等大事。
        </p>
        <img src="img/1.jpg">
    </div>
```

2，给center定义样式即可 具体的问chatgpt

```css
#center {
            width: 65%;
            /* margin: 0% 17.5% 0% 17.5% ; */ /* 外边距, 上 右 下 左 */
            margin: 0 auto;
        }
```

#### 5，表格

```css
<table>：定义表格
<tr>：定义表格中的行，一个 <tr> 表示一行
<th>：表示表头单元格，具有加粗居中效果
<td>：表示普通单元格
```

```css
<table border="1px" cellspacing="0"  width="600px">
        <tr>
            <th>序号</th>
            <th>品牌Logo</th>
            <th>品牌名称</th>
            <th>企业名称</th>
        </tr>
        <tr>
            <td>1</td>
            <td> <img src="img/huawei.jpg" width="100px"> </td>
            <td>华为</td>
            <td>华为技术有限公司</td>
        </tr>
    </table>
```

#### 6，表单*

写更复杂的输入 采集信息  认真看看

在一个表单中，可以存在很多的表单项，而虽然表单项的形式各式各样，但是表单项的标签其实就只有三个，分别是：

- &lt;input>: 表单项 , 通过type属性控制输入形式。

  | type取值                 | **描述**                             |
  | ------------------------ | ------------------------------------ |
  | text                     | 默认值，定义单行的输入字段           |
  | password                 | 定义密码字段                         |
  | radio                    | 定义单选按钮                         |
  | checkbox                 | 定义复选框                           |
  | file                     | 定义文件上传按钮                     |
  | date/time/datetime-local | 定义日期/时间/日期时间               |
  | number                   | 定义数字输入框                       |
  | email                    | 定义邮件输入框                       |
  | hidden                   | 定义隐藏域                           |
  | submit / reset / button  | 定义提交按钮 / 重置按钮 / 可点击按钮 |

- &lt;select>: 定义下拉列表, &lt;option> 定义列表项

- &lt;textarea>: 文本域

这里不同的标签或者数据类型有不同的用法 简单但繁杂  下面的注释举了一些例子 

需要的时候结合下面的图片和源码  删除更改  这里不深究了

```css
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML-表单项标签</title>
</head>
<body>

<!-- value: 表单项提交的值 -->
<form action="" method="post">
     姓名: <input type="text" name="name"> <br><br>
     ## password类型再输入时不会显示出来
     密码: <input type="password" name="password"> <br><br> 
	## radio表示单选 只能选男或者女
     性别: <label><input type="radio" name="gender" value="1"> 男</label>
          <label><input type="radio" name="gender" value="2"> 女 </label> <br><br>
	## checkbox是多选 可以有多个爱好
     爱好: <label><input type="checkbox" name="hobby" value="java"> java </label>
          <label><input type="checkbox" name="hobby" value="game"> game </label>
          <label><input type="checkbox" name="hobby" value="sing"> sing </label> <br><br>
    ## 这个file允许我们从本地上传文件
     图像: <input type="file" name="image">  <br><br>
     生日: <input type="date" name="birthday"> <br><br>
     时间: <input type="time" name="time"> <br><br>
     日期时间: <input type="datetime-local" name="datetime"> <br><br>
     邮箱: <input type="email" name="email"> <br><br>
     年龄: <input type="number" name="age"> <br><br>
     学历: <select name="degree">
               <option value="">----------- 请选择 -----------</option>
               <option value="1">大专</option>
               <option value="2">本科</option>
               <option value="3">硕士</option>
               <option value="4">博士</option>
          </select>  <br><br>
     描述: <textarea name="description" cols="30" rows="10"></textarea>  <br><br>
     <input type="hidden" name="id" value="1">
	 	
     <!-- 表单常见按钮 -->
     <input type="button" value="按钮">
     <input type="reset" value="重置"> 
     <input type="submit" value="提交">   
     <br>
</form>

</body>
</html>
```

![1_1](picture/1_1.PNG)

## 												24年8月16日 JavaScript学习

#### 1，JavaScript介绍

JavaScript的目的是让网页可以交互 也就是实现一些功能

#### 2，JavaScript的引入方式

js代码也是书写在html中的，那么html中如何引入js代码呢？主要通过下面的2种引入方式：

**第一种方式：**内部脚本，将JS代码定义在HTML页面中

- JavaScript代码必须位于&lt;script&gt;&lt;/script&gt;标签之间
- 在HTML文档中，可以在任意地方，放置任意数量的&lt;script&gt;
- 一般会把脚本置于&lt;body&gt;元素的底部，可改善显示速度

例子：

~~~html
<script>
    alert("Hello JavaScript")
</script>
~~~



**第二种方式：**外部脚本，单独拉一个文本写JavaScript代码，然后引入到 HTML页面中

- 外部JS文件中，只包含JS代码，不包含&ltscript&gt;标签
- 引入外部js的&lt;script&gt;标签，必须是双标签

例子：

~~~html
<script src="js/demo.js"></script>
~~~

注意：demo.js中只有js代码，没有&lt;script&gt;标签

#### 3，JavaScript的语法和数组，字符串等

和Java本身没什么区别  略 都看得懂

#### 4，JSON对象（C++结构体 Java类  python字典 核心是键值对）

##### 自定义对象

在 JavaScript 中自定义对象特别简单，其语法格式如下：

~~~js
var 对象名 = {
    属性名1: 属性值1, 
    属性名2: 属性值2,
    属性名3: 属性值3,
    函数名称: function(形参列表){}
};

~~~

我们可以通过如下语法调用属性：

~~~js
对象名.属性名
~~~

通过如下语法调用函数：

~~~js
对象名.函数名()
~~~



##### json对象

JSON对象：**J**ava**S**cript **O**bject **N**otation，JavaScript对象标记法。是通过JavaScript标记法书写的文本。其格式如下：

~~~js
{
    "key":value,
    "key":value,
    "key":value
}
~~~

其中，**key必须使用引号并且是双引号标记，value可以是任意数据类型。**

例如我们可以直接百度搜索“json在线解析”，随便挑一个进入，然后编写内容如下：

~~~js
{
	"name": "李传播"
}
~~~

##### json字符串

json字符串和json对象之间需要转化   json字符串是不好输出的  json对象才可以

```JavaScript
    // //定义json
    var jsonstr = '{"name":"Tom", "age":18, "addr":["北京","上海","西安"]}';
    //alert(jsonstr.name);

    // //json字符串--js对象
    var obj = JSON.parse(jsonstr);
    //alert(obj.name);

    // //js对象--json字符串
    alert(JSON.stringify(obj));
```

#### 5 ，BOM对象和DOM对象（了解概念 具体靠GPT就行）

##### 1，BOM对象

BOM的全称是Browser Object Model,翻译过来是浏览器对象模型。也就是JavaScript将浏览器的各个组成部分封装成了对象。我们要操作浏览器的部分功能，可以通过操作BOM对象的相关属性或者函数来完成。例如：我们想要将浏览器的地址改为`http://www.baidu.com`,我们就可以通过BOM中提供的location对象的href属性来完成，代码如下：`location.href='http://www.baidu.com'`

BOM中提供了如下5个对象：

| 对象名称  | 描述           |
| :-------- | :------------- |
| Window    | 浏览器窗口对象 |
| Navigator | 浏览器对象     |
| Screen    | 屏幕对象       |
| History   | 历史记录对象   |
| Location  | d地址栏对象    |

上述5个对象与浏览器各组成对应的关系如下图所示：

![2_1](picture/2_1.png)

###### 主要看Windows对象和location对象



下面代码已经添加注释 有不懂的看就行

| 函数          | 描述                                               |
| ------------- | -------------------------------------------------- |
| alert()       | 显示带有一段消息和一个确认按钮的警告框。           |
| comfirm()     | 显示带有一段消息以及确认按钮和取消按钮的对话框。   |
| setInterval() | 按照指定的周期（以毫秒计）来调用函数或计算表达式。 |
| setTimeout()  | 在指定的毫秒数后调用函数或计算表达式。             |

```JavaScript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JS-对象-BOM</title>
</head>
<body>
    
</body>
<script>
    //获取  这两行就是弹出一个窗口打印
    // window.alert("Hello BOM");
    // alert("Hello BOM Window");

    //方法     弹出一个可以点击的选择窗口
     这个代码其实很有意思 封装的很好 确认返回1  取消返回0 是一个可以交互的代码段
    //confirm - 对话框 -- 确认: true , 取消: false
    // var flag = confirm("您确认删除该记录吗?");
    // alert(flag);

    //定时器 - setInterval -- 周期性的执行某一个函数
    // var i = 0;
    // setInterval(function(){
    //     i++;
    //     console.log("定时器执行了"+i+"次");
    // },2000);

    //定时器 - setTimeout -- 延迟指定时间执行一次 
    // setTimeout(function(){
    //     alert("JS");
    // },3000);




    //location
    //打印当前的网站地址
    alert(location.href);
    //跳转到这个地址！
    location.href = "https://www.itcast.cn";

</script>
</html>
```

##### 2，DOM对象

网站写的太啰嗦，结合案例分析，DOM对象的目的就是在网站上添加按钮来修改网站内容！

代码自己看看就行 看得懂 核心是如何通过id,class等等方式来选中元素并进行修改值的操作

![2_2](picture/2_2.PNG)

```JavaScript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JS-对象-DOM演示</title>
</head>

<body>
    <div style="font-size: 30px; text-align: center;" id="tb1">课程表</div>
    <table width="800px" border="1" cellspacing="0" align="center">
        <tr>
            <th>学号</th>
            <th>姓名</th>
            <th>分数</th>
            <th>评语</th>
        </tr>
        <tr align="center" class="data">
            <td>001</td>
            <td>张三</td>
            <td>90</td>
            <td>很优秀</td>
        </tr>
        <tr align="center" class="data">
            <td>002</td>
            <td>李四</td>
            <td>92</td>
            <td>优秀</td>
        </tr>
    </table>
    <br>
    <div style="text-align: center;">
        <input id="b1" type="button" value="改变标题内容" onclick="fn1()">
        <input id="b2" type="button" value="改变标题颜色" onclick="fn2()">
        <input id="b3" type="button" value="删除最后一个" onclick="fn3()">
    </div>
</body>

<script>
    function fn1(){
        document.getElementById('tb1').innerHTML="学员成绩表";
    }
    
    function fn2(){
        document.getElementById('tb1').style="font-size: 30px; text-align: center; color: red;"
    }

    function fn3(){
       var trs = document.getElementsByClassName('data');
       trs[trs.length-1].remove();
    }
</script>

</html>
```

#### 6， 案例

接下来我们需要通过案例来加强对于上述DOM知识的掌握。需求如下3个：

- 点亮灯泡
- 将所有的div标签的标签体内容后面加上：very good
- 使所有的复选框呈现被选中的状态

![2_3](picture/2_3.png)

代码已经看懂且能做到自行更改 看看就行  核心就两部

1，选中需要操作的元素

2，找到对应的操作函数 调用  或者直接赋值

```JavaScript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JS-对象-DOM-案例</title>
</head>

<body>
    <img id="h1" src="img/off.gif">  <br><br>

    <div class="cls">传智教育</div>   <br>
    <div class="cls">黑马程序员</div>  <br>

    <input type="checkbox" name="hobby"> 电影
    <input type="checkbox" name="hobby"> 旅游
    <input type="checkbox" name="hobby"> 游戏
</body>

<script>
    //1. 点亮灯泡 : src 属性值
    var img = document.getElementById('h1');
    img.src = "img/on.gif";


    //2. 将所有div标签的内容后面加上: very good (红色字体) -- <font color='red'></font>
    var divs = document.getElementsByTagName('div');
    for (let i = 0; i < divs.length; i++) {
        const div = divs[i];
        div.innerHTML += "<font color='red'>very good</font>"; 
    }


    // //3. 使所有的复选框呈现选中状态
    var ins = document.getElementsByName('hobby');
    for (let i = 0; i < ins.length; i++) {
        const check = ins[i];
        check.checked = true;//选中
    }

</script>
</html>
```

#### 7， JavaScript事件（代码实现有些困惑 但是GPT辅助即可）

什么是事件呢？HTML事件是发生在HTML元素上的 “事情”，例如：

- 按钮被点击
- 鼠标移到元素上
- 输入框失去焦点
- ........

而我们可以给这些事件绑定函数，当事件触发时，可以自动的完成对应的功能。这就是`事件监听`。

##### 1，事件绑定（两种方式）

方式1：通过html标签中的事件属性进行绑定

例如一个按钮，我们对于按钮可以绑定单机事件，可以借助标签的onclick属性，属性值指向一个函数。

方式2：通过DOM中Element元素的事件属性进行绑定

依据我们学习过得DOM的知识点，我们知道html中的标签被加载成element对象，所以我们也可以通过element对象的属性来操作标签的属性。此时我们再次添加一个按钮，

```JavaScript
<!DOCTYPE html>
<html lang="en">,
<body>
    <input type="button" id="btn1" value="事件绑定1" onclick="on()">
    <input type="button" id="btn2" value="事件绑定2">
</body>

<script>
    function on(){
        alert("按钮1被点击了...");
    }

    document.getElementById('btn2').onclick = function(){
        alert("按钮2被点击了...");
    }

</script>
</html>
```

##### 2，常见事件

onclick是一个例子，还有一些常用的事件

| 事件属性名  | 说明                     |
| ----------- | ------------------------ |
| onclick     | 鼠标单击事件             |
| onblur      | 元素失去焦点             |
| onfocus     | 元素获得焦点             |
| onload      | 某个页面或图像被完成加载 |
| onsubmit    | 当表单提交时触发该事件   |
| onmouseover | 鼠标被移到某元素之上     |
| onmouseout  | 鼠标从某元素移开         |

onfocus:获取焦点事件，鼠标点击输入框，输入框中光标一闪一闪的，就是输入框获取焦点了



这里看似很复杂 其实和鼠标点击一模一样，每种操作都封装好了事件名称

```Java
事件属性名 = "函数名()"
```

例如  然后后面再自己写函数就行  至于第二种外联绑定 也是一样的

```JavaScript
<input type="text" name="username" onblur="bfn()" onfocus="ffn()" onkeydown="kfn()">
```

#### 8 案例

接下来我们通过案例来加强所学js知识点的掌握。

需求如下3个：

1. 点击 “点亮”按钮 点亮灯泡，点击“熄灭”按钮 熄灭灯泡
2. 输入框鼠标聚焦后，展示小写；鼠标离焦后，展示大写。
3. 点击 “全选”按钮使所有的复选框呈现被选中的状态，点击 “反选”按钮使所有的复选框呈现取消勾选的状态。

![2_4](picture/2_4.png)

已经看得懂了 还自己把第三个功能从内部绑定改成了外部的前后端分离

```JavaScript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JS-事件-案例</title>
</head>
<body>

    <img id="light" src="img/off.gif"> <br>

    <input type="button" value="点亮" onclick="on()"> 
    <input type="button"  value="熄灭" onclick="off()">

    <br> <br>

    <input type="text" id="name" value="ITCAST" onfocus="lower()" onblur="upper()">
    <br> <br>

    <input type="checkbox" name="hobby"> 电影
    <input type="checkbox" name="hobby"> 旅游
    <input type="checkbox" name="hobby"> 游戏
    <br>

    <input type="button" id="allselect" value="全选"> 
    <input type="button" value="反选" onclick="reverse()">

</body>

<script>
       //这里是自己改的 自己加了个id
    document.getElementById('allselect').onclick = function(){
        var hobbys = document.getElementsByName("hobby");

        //b. 设置选中状态
        for (let i = 0; i < hobbys.length; i++) {
            const element = hobbys[i];
            element.checked = true;
        }
    }

    //1. 点击 "点亮" 按钮, 点亮灯泡; 点击 "熄灭" 按钮, 熄灭灯泡; -- onclick
    function on(){
        //a. 获取img元素对象
        var img = document.getElementById("light");

        //b. 设置src属性
        img.src = "img/on.gif";
    }

    function off(){
        //a. 获取img元素对象
        var img = document.getElementById("light");

        //b. 设置src属性
        img.src = "img/off.gif";
    }



    //2. 输入框聚焦后, 展示小写; 输入框离焦后, 展示大写; -- onfocus , onblur
    function lower(){//小写
        //a. 获取输入框元素对象
        var input = document.getElementById("name");

        //b. 将值转为小写
        input.value = input.value.toLowerCase();
    }

    function upper(){//大写
        //a. 获取输入框元素对象
        var input = document.getElementById("name");

        //b. 将值转为大写
        input.value = input.value.toUpperCase();
    }



    //3. 点击 "全选" 按钮使所有的复选框呈现选中状态 ; 点击 "反选" 按钮使所有的复选框呈现取消勾选的状态 ; -- onclick
    function checkAll(){
        //a. 获取所有复选框元素对象
        var hobbys = document.getElementsByName("hobby");

        //b. 设置选中状态
        for (let i = 0; i < hobbys.length; i++) {
            const element = hobbys[i];
            element.checked = true;
        }

    }
    
    function reverse(){
        //a. 获取所有复选框元素对象
        var hobbys = document.getElementsByName("hobby");

        //b. 设置未选中状态
        for (let i = 0; i < hobbys.length; i++) {
            const element = hobbys[i];
            element.checked = false;
        }
    }



</script>
</html>
```

##                                               VUE框架(VUE3快速上手文档参考尚硅谷)

#### 1 VUE概述

通过我们学习的html+css+js已经能够开发美观的页面了，但是开发的效率还有待提高，那么如何提高呢？我们先来分析下页面的组成。一个完整的html页面包括了视图和数据，数据是通过请求 从后台获取的，那么意味着我们需要将后台获取到的数据呈现到页面上，很明显， 这就需要我们使用DOM操作。正因为这种开发流程，所以我们引入了一种叫做**MVVM(Model-View-ViewModel)的前端开发思想**，即让我们开发者更加关注数据，而非数据绑定到视图这种机械化的操作。那么具体什么是MVVM思想呢？

MVVM:其实是Model-View-ViewModel的缩写，有3个单词，具体释义如下：

- Model: 数据模型，特指前端中通过请求从后台获取的数据
- View: 视图，用于展示数据的页面，可以理解成我们的html+css搭建的页面，但是没有数据
- ViewModel: 数据绑定到视图，负责将数据（Model）通过JavaScript的DOM技术，将数据展示到视图（View）上

![3_1](picture/3_1.png)

简单来说，VUE框架让我们的输入输出让我们的前端输入没有写死，而是可以通过指令，更方便的集成在一起输出

官话：`让前端开发更灵活、更动态地处理数据，而不是将输入和输出"写死"在页面中。通过使用指令和双向数据绑定，Vue.js 提供了更方便的方式来处理和展示数据。`

#### 2， VUE常用指令

**指令：**HTML 标签上带有 v- 前缀的特殊属性，不同指令具有不同含义。例如：v-if，v-for…

在vue中，通过大量的指令来实现数据绑定到视图的，所以接下来我们需要学习vue的常用指令，如下表所示：

| **指令**  | **作用**                                            |
| --------- | --------------------------------------------------- |
| v-bind    | 为HTML标签绑定属性值，如设置  href , css样式等      |
| v-model   | 在表单元素上创建双向数据绑定                        |
| v-on      | 为HTML标签绑定事件                                  |
| v-if      | 条件性的渲染某元素，判定为true时渲染,否则不渲染     |
| v-else    |                                                     |
| v-else-if |                                                     |
| v-show    | 根据条件展示某元素，区别在于切换的是display属性的值 |
| v-for     | 列表渲染，遍历容器的元素或者对象的属性              |

##### 2.1 v-blind和v-model

1，v-blind很简单，这里就是设置超链接

2，v-model`**实现双向绑定：可以获取表单的数据的值，然后提交给服务器**`

简单来说，这里在输入栏里改变输入，超链接的值也发生变化，跳到你输入的那个链接了

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue-指令-v-bind</title>
    <script src="js/vue.js"></script>
</head>
<body>
    <div id="app">

        <a v-bind:href="url">链接1</a>
        <a :href="url">链接2</a>

        <input type="text" v-model="url">

    </div>
</body>
<script>
    //定义Vue对象
    new Vue({
        el: "#app", //vue接管区域
        data:{
           url: "https://www.baidu.com"
        }
    })
</script>
</html>
```

##### 2.2 v-on

绑定函数，执行点击，鼠标放置等操作用的

- v-on语法给标签的事件绑定的函数，必须是vue对象种声明的函数

- v-on语法绑定事件时，事件名相比较js中的事件名，没有on

  例如：在js中，事件绑定demo函数

  ~~~html
  <input onclick="demo()">
  ~~~

  vue中，事件绑定demo函数

  ~~~html
  <input v-on:click="demo()">
  ~~~

接下来我们通过代码演示。

在vue对象的methods属性中定义事件绑定时需要的handle()函数，代码如下：

~~~js
 methods: {
        handle: function(){
           alert("你点我了一下...");
        }
}
~~~

然后我们给第一个按钮，通过v-on指令绑定单击事件，代码如下：

~~~html
 <input type="button" value="点我一下" v-on:click="handle()">
~~~

同样，v-on也存在简写方式，即v-on: 可以替换成@，所以第二个按钮绑定单击事件的代码如下：

~~~html
<input type="button" value="点我一下" @click="handle()">
~~~



完整代码如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue-指令-v-on</title>
    <script src="js/vue.js"></script>
</head>
<body>
    <div id="app">

        <input type="button" value="点我一下" v-on:click="handle()">

        <input type="button" value="点我一下" @click="handle1()">

    </div>
</body>
<script>
    //定义Vue对象
    new Vue({
        el: "#app", //vue接管区域
        data:{
           
        },
        methods: {
            handle: function(){
                alert("你点我了一下...");
            },
            handle1: function(){
                alert("你又一次点我了一下...");
            }
        }
    })
</script>
</html>
```

##### 2.3 v-if和v-show（作用相同 底层原理不同）

就是给各种功能加一个条件 满足才执行

| 指令      | 描述                                                |
| --------- | --------------------------------------------------- |
| v-if      | 条件性的渲染某元素，判定为true时渲染,否则不渲染     |
| v-if-else |                                                     |
| v-else    |                                                     |
| v-show    | 根据条件展示某元素，区别在于切换的是display属性的值 |

例如， span标签本来直接展示 这里就是

年轻人直接展示

中年人和老年人则条件判断

```html
<div id="app">
        
        年龄<input type="text" v-model="age">经判定,为:
        <span>年轻人(35及以下)</span>
        <span v-if="age > 35 && age < 60">中年人(35-60)</span>
        <span v-else-if="age>=60">老年人(60及以上)</span>

        <br><br>


    </div>
```



##### 2.4 v-for

v-for: 从名字我们就能看出，这个指令是用来遍历的。其语法格式如下：

`下面输入的两者都在双引号“”中间！`

~~~html
<标签 v-for="变量名 in 集合模型数据">
    {{变量名}}
</标签>
~~~

需要注意的是：需要循环那个标签，v-for 指令就写在那个标签上。

有时我们遍历时需要使用索引，那么v-for指令遍历的语法格式如下：

~~~html
<标签 v-for="(变量名,索引变量) in 集合模型数据">
    <!--索引变量是从0开始，所以要表示序号的话，需要手动的加1-->
   {{索引变量 + 1}} {{变量名}}
</标签>
~~~

稍微有点抽象 给出代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue-指令-v-for</title>
    <script src="js/vue.js"></script>
</head>
<body>
    <div id="app">

        <div v-for="addr in addrs">{{addr}}</div>

        <hr>

        <div v-for="(addr,index) in addrs">{{index + 1}} : {{addr}}</div>

    </div>
</body>
<script>
    //定义Vue对象
    new Vue({
        el: "#app", //vue接管区域
        data:{
           addrs:["北京", "上海", "西安", "成都", "深圳"]
        },
        methods: {
            
        }
    })
</script>
</html>
```

##### 2.5 实际案例（复习就看这个，很值得看）

![3-2](picture/3-2.png)

- 如上图所示，我们提供好了数据模型，users是数组集合，提供了多个用户信息。然后我们需要将数据以表格的形式，展示到页面上，其中，性别需要转换成中文男女，等级需要将分数数值转换成对应的等级。

- 分析：

  首先我们肯定需要遍历数组的，所以需要使用v-for标签；然后我们每一条数据对应一行，所以v-for需要添加在tr标签上；其次我们需要将编号，所以需要使用索引的遍历语法；然后我们要将数据展示到表格的单元格中，所以我们需要使用{{}}插值表达式；最后，我们需要转换内容，所以我们需要使用v-if指令，进行条件判断和内容的转换

- 步骤：

  - 使用v-for的带索引方式添加到表格的&lt;tr&gt;标签上
  - 使用{{}}插值表达式展示内容到单元格
  - 使用索引+1来作为编号
  - 使用v-if来判断，改变性别和等级这2列的值

如果不用vue标签，那么代码会是：

```html
<table border="1" cellspacing="0" width="60%">
            <tr>
                <th>编号</th>
                <th>姓名</th>
                <th>年龄</th>
                <th>性别</th>
                <th>成绩</th>
                <th>等级</th>
            </tr>

            <!-- 手动填充数据 -->
            <tr align="center">
                <td>1</td>
                <td>Tom</td>
                <td>20</td>
                <td>男</td>
                <td>78</td>
                <td>及格</td>
            </tr>
    ......
```

其完整代码如下：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue-指令-案例</title>
    <script src="js/vue.js"></script>
</head>
<body>
    
    <div id="app">
        
        <table border="1" cellspacing="0" width="60%">
            <tr>
                <th>编号</th>
                <th>姓名</th>
                <th>年龄</th>
                <th>性别</th>
                <th>成绩</th>
                <th>等级</th>
            </tr>

            <tr align="center" v-for="(user,index) in users">
                <td>{{index + 1}}</td>
                <td>{{user.name}}</td>
                <td>{{user.age}}</td>
                <td>
                    <span v-if="user.gender == 1">男</span>
                    <span v-if="user.gender == 2">女</span>
                </td>
                <td>{{user.score}}</td>
                <td>
                    <span v-if="user.score >= 85">优秀</span>
                    <span v-else-if="user.score >= 60">及格</span>
                    <span style="color: red;" v-else>不及格</span>
                </td>
            </tr>
        </table>

    </div>

</body>

<script>
    new Vue({
        el: "#app",
        data: {
            users: [{
                name: "Tom",
                age: 20,
                gender: 1,
                score: 78
            },{
                name: "Rose",
                age: 18,
                gender: 2,
                score: 86
            },{
                name: "Jerry",
                age: 26,
                gender: 1,
                score: 90
            },{
                name: "Tony",
                age: 30,
                gender: 1,
                score: 52
            }]
        },
        methods: {
            
        },
    })
</script>
</html>
~~~

#### 3 Vue生命周期

ue的生命周期：指的是vue对象从创建到销毁的过程。

vue的生命周期包含8个阶段：`每触发一个生命周期事件，会自动执行一个生命周期方法，这些生命周期方法也被称为钩子方法`。其完整的生命周期如下图所示：

其中我们需要重点关注的是**mounted,**其他的我们了解即可。

mounted：挂载完成，Vue初始化成功，HTML页面渲染成功。**以后我们一般用于页面初始化自动的ajax请求后台数据**

| 状态          | 阶段周期   |
| ------------- | ---------- |
| beforeCreate  | 创建前     |
| created       | 创建后     |
| beforeMount   | 挂载前     |
| `mounted`     | `挂载完成` |
| beforeUpdate  | 更新前     |
| updated       | 更新后     |
| beforeDestroy | 销毁前     |
| destroyed     | 销毁后     |

![3-3](picture/3-3.png)



~~~html
<script>
    //定义Vue对象
    new Vue({
        el: "#app", //vue接管区域
        data:{
           
        },
        methods: {
            
        },
        mounted () {
            alert("vue挂载完成,发送请求到服务端")
        }
    })
</script>
~~~

在mounted这个阶段，可以执行一些功能

## 														24年8月19日  前后端工程化

#### 1，Ajax介绍

Ajax: 全称Asynchronous JavaScript And XML，异步的JavaScript和XML。其作用有如下2点：

- 与服务器进行数据交换：通过Ajax可以给服务器发送请求，并获取服务器响应的数据。
- 异步交互：可以在**不重新加载整个页面**的情况下，与服务器交换数据并**更新部分网页**的技术，如：搜索联想、用户名是否可用的校验等等。





在后续框架中 这个都是封装好的 所以这里略过 简单来说就是向服务器发起请求获取数据





- 第一步：首先我们再VS Code中创建AJAX的文件夹，并且创建名为01. Ajax-原生方式.html的文件，提供如下代码，主要是按钮绑定单击事件，我们希望点击按钮，来发送ajax请求

  ~~~html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>原生Ajax</title>
  </head>
  <body>
      
      <input type="button" value="获取数据" onclick="getData()">
  
      <div id="div1"></div>
      
  </body>
  <script>
      function getData(){
       
      }
  </script>
  </html>
  ~~~

  第二步：创建XMLHttpRequest对象，用于和服务器交换数据，也是原生Ajax请求的核心对象，提供了各种方法。代码如下：

  ~~~js
  //1. 创建XMLHttpRequest 
  var xmlHttpRequest  = new XMLHttpRequest();
  ~~~

  第三步：调用对象的open()方法设置请求的参数信息，例如请求地址，请求方式。然后调用send()方法向服务器发送请求，代码如下：

  ~~~js
  //2. 发送异步请求
  xmlHttpRequest.open('GET','http://yapi.smart-xwork.cn/mock/169327/emp/list');
  xmlHttpRequest.send();//发送请求
  ~~~

  第四步：我们通过绑定事件的方式，来获取服务器响应的数据。

  ~~~js
  //3. 获取服务响应数据
  xmlHttpRequest.onreadystatechange = function(){
      //此处判断 4表示浏览器已经完全接受到Ajax请求得到的响应， 200表示这是一个正确的Http请求，没有错误
      if(xmlHttpRequest.readyState == 4 && xmlHttpRequest.status == 200){
          document.getElementById('div1').innerHTML = xmlHttpRequest.responseText;
      }
  }
  ~~~



  最后我们通过浏览器打开页面，请求点击按钮，发送Ajax请求，最终显示结果如下图所示：

  ![1669106705778](../BaiduNetdiskDownload/day03-Vue-Element/day03-Vue-Element/讲义/assets/1669106850383.png) 

  并且通过浏览器的f12抓包，我们点击网络中的XHR请求，发现可以抓包到我们发送的Ajax请求。XHR代表的就是异步请求







#### 2， Axios（能看懂案例就行，加注释）

上述原生的Ajax请求的代码编写起来还是比较繁琐的，所以接下来我们学习一门更加简单的发送Ajax请求的技术Axios 。Axios是对原生的AJAX进行封装，简化书写。Axios官网是：`https://www.axios-http.cn`



直接上案例 

需求：基于Vue及Axios完成数据的动态加载展示，如下图所示

![4-1](picture/4-1.png) 

其中数据是来自于后台程序的，地址是：https://mock.apifox.cn/m1/3128855-0-default/emp/list



步骤：

1. 首先创建文件，提前准备基础代码，包括表格以及vue.js和axios.js文件的引入
2. 我们需要在vue的mounted钩子函数中发送ajax请求，获取数据
3. 拿到数据，数据需要绑定给vue的data属性
4. 在&lt;tr&gt;标签上通过v-for指令遍历数据，展示数据

完整代码如下：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ajax-Axios-案例</title>
    <script src="js/axios-0.18.0.js"></script>
    <script src="js/vue.js"></script>
</head>
<body>
    <div id="app">
		//定义了一个表格
        <table border="1" cellspacing="0" width="60%">
            <tr>
                <th>编号</th>
                <th>姓名</th>
                <th>图像</th>
                <th>性别</th>
                <th>职位</th>
                <th>入职日期</th>
                <th>最后操作时间</th>
            </tr>
			//向表格里填内容  这里emp在script框架里面定义了 然后通过axios来请求服务器返回值
            <tr align="center" v-for="(emp,index) in emps">
                <td>{{index + 1}}</td>
                <td>{{emp.name}}</td>
                <td>
                    <img :src="emp.image" width="70px" height="50px">
                </td>
                <td>
                    <span v-if="emp.gender == 1">男</span>
                    <span v-if="emp.gender == 2">女</span>
                </td>
                <td>{{emp.job}}</td>
                <td>{{emp.entrydate}}</td>
                <td>{{emp.updatetime}}</td>
            </tr>
        </table>
    </div>
</body>
<script>
    //这里定义了emps  然后通过mounted钩子函数来发送了异步请求 加载了服务器的值
    new Vue({
       el: "#app",
       data: {
         emps:[]
       },
       mounted () {
          //发送异步请求,加载数据
          axios.get("http://yapi.smart-xwork.cn/mock/169327/emp/list").then(result => {
            console.log(result.data);
            this.emps = result.data.data;
          })
       }
    });
</script>
</html>
~~~

#### 3， 前后端分离开发

前端开发有2种方式：**前后台混合开发**和**前后台分离开发**。

混合开发耦合度太高了，不便于管理和分工

所以我们目前基本都是采用的前后台分离开发方式，如下图所示：

![4-2](picture/4-2.png)

我们将原先的工程分为前端工程和后端工程这2个工程，然后前端工程交给专业的前端人员开发，后端工程交给专业的后端人员开发。前端页面需要数据，可以通过发送异步请求，从后台工程获取。但是，我们前后台是分开来开发的，那么前端人员怎么知道后台返回数据的格式呢？后端人员开发，怎么知道前端人员需要的数据格式呢？所以针对这个问题，我们前后台统一指定一套规范！我们前后台开发人员都需要遵循这套规范开发，这就是我们的**接口文档**。接口文档有离线版和在线版本，接口文档示可以查询今天提供**资料/接口文档示例**里面的资料。那么接口文档的内容怎么来的呢？是我们后台开发者根据产品经理提供的产品原型和需求文档所撰写出来的，产品原型示例可以参考今天提供**资料/页面原型**里面的资料。

那么基于前后台分离开发的模式下，我们后台开发者开发一个功能的具体流程如何呢？如下图所示：

 ![4-3](picture/4-3.png)

1. 需求分析：首先我们需要阅读需求文档，分析需求，理解需求。
2. 接口定义：查询接口文档中关于需求的接口的定义，包括地址，参数，响应数据类型等等
3. 前后台并行开发：各自按照接口文档进行开发，实现需求
4. 测试：前后台开发完了，各自按照接口文档进行测试
5. 前后段联调测试：前段工程请求后端工程，测试功能



这里，接口文档的定义就非常重要了，下面给出一个接口文档的示例。

```mysql
地址：D:\文本文件记录\接口文档-示例
```

#### 4，YAPI

前后台分离开发中，我们前后台开发人员都需要遵循接口文档，所以接下来我们介绍一款撰写接口文档的平台。

YApi 是高效、易用、功能强大的 api 管理平台，旨在为开发、产品、测试人员提供更优雅的接口管理服务。

其官网地址：http://yapi.smart-xwork.cn/

YApi主要提供了2个功能：

- API接口管理：根据需求撰写接口，包括接口的地址，参数，响应等等信息。
- Mock服务：模拟真实接口，生成接口的模拟测试数据，用于前端的测试。

`但是由于网络原因，我登不上去这个网站，反正是前后端之间沟通用的 。`

## 																前端工程化

我们目前的前端开发中，当我们需要使用一些资源时，`例如：vue.js，和axios.js文件，都是直接再工程中导入的`，如下图所示：

![5-1](picture/5-1.png)

但是上述开发模式存在如下问题：

- 每次开发都是从零开始，比较麻烦
- 多个页面中的组件共用性不好
- js、图片等资源没有规范化的存储目录，没有统一的标准，不方便维护



所以现在企业开发中更加讲究前端工程化方式的开发，主要包括如下4个特点

- 模块化：将js和css等，做成一个个可复用模块
- 组件化：我们将UI组件，css样式，js行为封装成一个个的组件，便于管理
- 规范化：我们提供一套标准的规范的目录接口和编码规范，所有开发人员遵循这套规范
- 自动化：项目的构建，测试，部署全部都是自动完成

所以对于前端工程化，说白了，就是在企业级的前端项目开发中，把前端开发所需要的工具、技术、流程、经验进行规范化和标准化。从而提升开发效率，降低开发难度等等。接下来我们就需要学习vue的官方提供的脚手架帮我们完成前端的工程化。

#### 1，前端工程化入门  安装NodeJs(图太多了复制，有需求去看源文档)

这里安装的版本和教学不一样 安装的20.16.0但是应该没影响

简述一下，

这里按照视频教程  安装了NodeJs  但是安装的是20的版本， vue cld正常安装

vue ui 正常运行  并创建了基于VUE3.X的项目 vue-project

```
D:\JavaPractice\vue_practice\vue-project
```

我们通过VS Code打开之前创建的vue文件夹，打开之后，呈现如下图所示页面：

![1669302718419](../BaiduNetdiskDownload/day03-Vue-Element/day03-Vue-Element/讲义/assets/1669302718419.png)

vue项目的标准目录结构以及目录对应的解释如下图所示:

![1669302973198](../BaiduNetdiskDownload/day03-Vue-Element/day03-Vue-Element/讲义/assets/1669302973198.png)

其中我们平时开发代码就是在**src目录**下



#### 2， 运行vue项目

那么vue项目开发好了，我们应该怎么运行vue项目呢？主要提供了2种方式

- 第一种方式：通过VS Code提供的图形化界面 ，如下图所示：（注意：NPM脚本窗口默认不显示，可以参考本节的最后调试出来）

  ![1669303687468](../BaiduNetdiskDownload/day03-Vue-Element/day03-Vue-Element/讲义/assets/1669303687468.png)

  点击之后，我们等待片刻，即可运行，在终端界面中，我们发现项目是运行在本地服务的8080端口，我们直接通过浏览器打开地址

  ![1669303846100](../BaiduNetdiskDownload/day03-Vue-Element/day03-Vue-Element/讲义/assets/1669303846100.png) 

  最终浏览器打开后，呈现如下界面，表示项目运行成功

  ![1669304009602](../BaiduNetdiskDownload/day03-Vue-Element/day03-Vue-Element/讲义/assets/1669304009602.png)

  其实此时访问的是 **src/App.vue**这个根组件，此时我们打开这个组件，修改代码：添加内容Vue

  ![1669304267724](../BaiduNetdiskDownload/day03-Vue-Element/day03-Vue-Element/讲义/assets/1669304267724.png)

  只要我们保存更新的代码，我们直接打开浏览器，不需要做任何刷新，发现页面呈现内容发生了变化，如下图所示：

  ![1669304385826](../BaiduNetdiskDownload/day03-Vue-Element/day03-Vue-Element/讲义/assets/1669304385826.png)

  这就是我们vue项目的热更新功能 

  对于8080端口，经常被占用，所以我们可以去修改默认的8080端口。我们修改vue.config.js文件的内容，添加如下代码：

  ~~~json
  devServer:{
      port:7000
  }
  ~~~

  如下图所示，然后我们关闭服务器，并且重新启动，

  ![1669305444633](../BaiduNetdiskDownload/day03-Vue-Element/day03-Vue-Element/讲义/assets/1669305444633.png)

​       重新启动如下图所示：

​	![1669305570022](../BaiduNetdiskDownload/day03-Vue-Element/day03-Vue-Element/讲义/assets/1669305570022.png) 

​	端口更改成功，可以通过浏览器访问7000端口来访问我们之前的项目

- 第二种方式：命令行方式

  直接基于cmd命令窗口，在vue目录下，执行输入命令`npm run serve`即可，如下图所示：

  ![1669304694076](../BaiduNetdiskDownload/day03-Vue-Element/day03-Vue-Element/讲义/assets/1669304694076.png) 



补充：NPM脚本窗口调试出来

第一步：通过**设置/用户设置/扩展/MPM**更改NPM默认配置，如下图所示

![1669304930336](../BaiduNetdiskDownload/day03-Vue-Element/day03-Vue-Element/讲义/assets/1669304930336.png)

然后重启VS Code，并且**双击打开package.json文件**，然后点击**资源管理器处的3个小点**，**勾选npm脚本选项**，如图所示

![1669305068434](../BaiduNetdiskDownload/day03-Vue-Element/day03-Vue-Element/讲义/assets/1669305068434.png) 

然后就能都显示NPM脚本小窗口了。

#### 3， Vue项目开发流程

那么我们访问的首页是index.html，但是我们找到public/index.html文件，打开之后发现，里面没有什么代码，但是能够呈现内容丰富的首页：如下图所示：

![1669308098856](../BaiduNetdiskDownload/day03-Vue-Element/day03-Vue-Element/讲义/assets/1669308098856.png) 

我们自习观察发现，index.html的代码很简洁，但是浏览器所呈现的index.html内容却很丰富，代码和内容不匹配，所以vue是如何做到的呢？接下来我们学习一下vue项目的开发流程。

对于vue项目，index.html文件默认是引入了入口函数main.js文件，我们找到**src/main.js**文件，其代码如下：

~~~js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'

createApp(App).use(router).mount('#app')
~~~

上述代码中，包括如下几个关键点：

- import: 导入指定文件，并且重新起名。例如上述代码`import App from './App.vue'`导入当前目录下得App.vue并且起名为App
- new Vue(): 创建vue对象
- `$mount('#app');将vue对象创建的dom对象挂在到id=app的这个标签区域中，作用和之前学习的vue对象的le属性一致`。
- router:  路由，详细在后面的小节讲解
- render: 主要使用视图的渲染的。



来到**public/index.html**中，我们**删除div的id=app属性**，打开浏览器，发现之前访问的首页一片空白，如下图所示，这样就证明了，我们main.js中通过代码挂在到index.html的id=app的标签区域的。



此时我们知道了vue创建的dom对象挂在到id=app的标签区域，但是我们还是没有解决最开始的问题：首页内容如何呈现的？这就涉及到render中的App了，如下图所示：

![1669313364004](../BaiduNetdiskDownload/day03-Vue-Element/day03-Vue-Element/讲义/assets/1669313364004.png) 

那么这个App对象怎么回事呢，我们打开App.vue,注意的是.vue结尾的都是vue组件。而vue的组件文件包含3个部分：

- `1 template: 模板部分，主要是HTML代码，用来展示页面主体结构的`
- `2 script: js代码区域，这里主要通过js代码来进行行为 返回值 给template部分展示`
- 3 style: css样式部分，主要通过css样式控制模板的页面效果（可以不看 耦合性很低）

如下图所示就是一个vue组件的小案例：

![1669313699186](../BaiduNetdiskDownload/day03-Vue-Element/day03-Vue-Element/讲义/assets/1669313699186.png)



此时我们可以打开App.vue，观察App.vue的代码，其中可以发现，App.vue组件的template部分内容，和我们浏览器访问的首页内容是一致的，如下图所示：

![1669313894258](../BaiduNetdiskDownload/day03-Vue-Element/day03-Vue-Element/讲义/assets/1669313894258.png)

接下来我们可以简化模板部分内容，添加script部分的数据模型，删除css样式，完整代码如下：

~~~html
<template>
  <div id="app">
    {{message}}
  </div>
</template>

<script>
export default {
  data(){
    return {
      "message":"hello world"
    }
  }
}
</script>
<style>

</style>
~~~



保存直接，回到浏览器，我们发现首页展示效果发生了变化，如下图所示：

![1669314115641](../BaiduNetdiskDownload/day03-Vue-Element/day03-Vue-Element/讲义/assets/1669314115641.png)

## 													Vue组件库---ElementPlus

Element：是饿了么公司前端开发团队提供的一套基于 Vue 的网站组件库，用于快速构建网页。

Element 提供了很多组件（组成网页的部件）供我们使用。例如 超链接、按钮、图片、表格等等。如下图所示就是我们开发的页面和ElementUI提供的效果对比：可以发现ElementUI提供的各式各样好看的按钮

![1669357961971](../BaiduNetdiskDownload/day03-Vue-Element/day03-Vue-Element/讲义/assets/1669357961971.png) 

ElementUI的学习方式和我们之前的学习方式不太一样，对于ElementUI，我们作为一个后台开发者，只需要**学会如何从ElementUI的官网拷贝组件到我们自己的页面中，并且做一些修改即可**。其官网地址：https://element.eleme.cn/#/zh-CN，我们主要学习的是ElementUI中提供的常用组件，至于其他组件同学们可以通过我们这几个组件的学习掌握到ElementUI的学习技巧，然后课后自行学习。



勉强调通了测试代码

```html
<template>
    <div class="mb-4">
      <el-button>Default</el-button>
      <el-button type="primary">Primary</el-button>
      <el-button type="success">Success</el-button>
      <el-button type="info">Info</el-button>
      <el-button type="warning">Warning</el-button>
      <el-button type="danger">Danger</el-button>
    </div>
  </template>
  
  <script>
  export default{

  }
  </script>

<style>

</style>
```

```js
import { createApp } from 'vue';
import App from './App.vue';
import router from './router';

// 引入 Element Plus 组件库
import ElementPlus from 'element-plus';
import 'element-plus/dist/index.css';

// 创建应用实例
const app = createApp(App);

// 注册 Element Plus 插件
app.use(ElementPlus);

// 注册路由
app.use(router);

// 挂载应用到 DOM
app.mount('#app');

```

这里看不太懂  详细给出gpt的解释

##### 1. 创建应用实例

```
javascript
复制代码
const app = createApp(App);
```

**解释**:

- 这一步通过 `createApp(App)` 创建了一个 Vue 应用实例，并将其赋值给变量 `app`。
- `App` 是你定义的根组件，代表整个应用的结构和逻辑。
- **想象**: 这是在准备一个空的“展示柜”，你还没有放任何东西进去，但这个“展示柜”已经准备好了，它有一个基础的框架（`App` 组件）。

##### 2. 注册 Element Plus 插件

```
javascript
复制代码
app.use(ElementPlus);
```

**解释**:

- 这一步使用 `app.use(ElementPlus)` 注册了 Element Plus 插件，使这个组件库中的所有组件在整个应用中可用。
- **想象**: 就像是给你的“展示柜”加了一些特殊的功能，比如灯光、旋转平台等。Element Plus 是一套功能强大的 UI 组件库，类似于提供了一些现成的、好看的展示道具。

##### 3. 注册路由

```
javascript
复制代码
app.use(router);
```

**解释**:

- 这一步使用 `app.use(router)` 注册了 Vue Router 插件，这样你的应用就可以处理页面之间的导航（比如从主页到关于页的跳转）。
- **路由**: 路由负责定义用户可以访问的不同页面（或视图），并在用户点击链接或按钮时切换显示不同的内容。
- **想象**: 在你的“展示柜”中，添加了一个功能，允许观众通过按钮或菜单来切换展示的不同模型。每个模型对应一个不同的“页面”，而“路由”就是管理这些页面切换的功能。

##### 4. 挂载应用到 DOM

```
javascript
复制代码
app.mount('#app');
```

**解释**:

- 这一步将你创建的 Vue 应用实例（包括它的所有功能、组件、路由等）挂载到 HTML 页面中的某个具体的元素上，这里指定的是 `id="app"` 的元素。
- **挂载**: 挂载的过程就是将 Vue 应用的内容（由 Vue 组件生成的 DOM 结构）实际渲染到页面上的某个位置，使其可见和可交互。
- **想象**: 最后，你将这个装饰了各种功能的“展示柜”放到博物馆的展厅中（即页面的某个具体位置）。从这一刻起，观众就能看到并与展示柜中的内容互动了。

##### 综合想象

1. **创建应用实例**: 准备了一个空的“展示柜”。
2. **注册 Element Plus 插件**: 给展示柜添加了一些现成的展示道具和功能。
3. **注册路由**: 让观众可以通过按钮或菜单来切换不同的展示内容。
4. **挂载应用到 DOM**: 把展示柜放到博物馆（页面）中，让观众可以看到和互动。

##### 总结

- **创建应用实例**: 初始化 Vue 应用，准备好基本的框架。
- **注册插件**: 增加一些全局可用的功能（如 UI 组件库）。
- **注册路由**: 定义页面的导航逻辑，管理不同页面之间的切换。
- **挂载应用到 DOM**: 将应用实际渲染到页面上，让用户可以看到和操作



## 												ElementPlus应用实例

这里用这个组件弄了一个前端页面 基本就是cv工程师  核心在于

1， 函数的定义

2， 2.0 3.0vue的区别  

3， axios发请求

前端的大部分东西能cv就行了 不用仔细看

#### 1，实际展示效果(这里代码没有参考价值，实际肯定是用插槽或者总分架构的)

![6-1](picture/6-1.PNG)

##### 全部代码EmpVies.vue：

```vue
<template>
    <div class="common-layout">
        <el-container style="height: 700px; border: 1px solid #eee;">
            <el-header style="font-size: 40px; background-color:darkgrey;">
                Wisdri视频播放平台
            </el-header>
            <el-container>
                <el-aside width="200px">
                    <el-scrollbar>
                        <el-menu @select="handleMenuSelect">
                            <el-sub-menu index="1">
                                <template #title>
                                    <i class="el-icon-message"></i>系统信息管理
                                </template>
                                <el-menu-item index="1-1">部门管理</el-menu-item>
                                <el-menu-item index="1-2">员工管理</el-menu-item>
                            </el-sub-menu>
                        </el-menu>
                    </el-scrollbar>
                </el-aside>

                <el-main>
                    <!-- 使用 v-if 控制内容的显示 -->
                    <div v-if="activeMenu === '1-2'">
                        <!-- 这里是员工管理的内容 -->
                        <!-- 表单 -->
                        <el-form :inline="true" :model="searchForm" class="demo-form-inline">
                            <el-form-item label="姓名">
                                <el-input v-model="searchForm.name" placeholder="姓名"></el-input>
                            </el-form-item>
                            <el-form-item label="性别">
                                <el-select v-model="searchForm.gender" placeholder="性别">
                                    <el-option label="男" value="1"></el-option>
                                    <el-option label="女" value="2"></el-option>
                                </el-select>
                            </el-form-item>

                            <!-- 日期选择器 -->
                            <el-form-item label="入职日期">
                                <el-date-picker v-model="searchForm.entrydate" type="daterange" range-separator="至"
                                    start-placeholder="开始日期" end-placeholder="结束日期">
                                </el-date-picker>
                            </el-form-item>

                            <el-form-item>
                                <el-button type="primary" @click="onSubmit">查询</el-button>
                            </el-form-item>
                        </el-form>
                        <!-- 表格 -->
                        <el-scrollbar>
                            <el-table :data="tableData">
                                <el-table-column prop="name" label="姓名" width="180"></el-table-column>
                                <el-table-column prop="image" label="图像" width="180">
                                    <template v-slot="scope">
                                        <img :src="scope.row.image" width="100px" height="70px">
                                    </template>
                                </el-table-column>
                                <el-table-column prop="gender" label="性别" width="140">
                                    <template v-slot="scope">
                                        {{ scope.row.gender == 1 ? "男" : "女" }}
                                    </template>
                                </el-table-column>
                                <el-table-column prop="job" label="职位" width="140"></el-table-column>
                                <el-table-column prop="entrydate" label="入职日期" width="180"></el-table-column>
                                <el-table-column prop="updatetime" label="最后操作时间" width="230"></el-table-column>
                                <el-table-column label="操作">
                                    <el-button type="primary" size="small">编辑</el-button>
                                    <el-button type="danger" size="small">删除</el-button>
                                </el-table-column>
                            </el-table>
                        </el-scrollbar>

                        <!-- Pagination分页 -->
                        <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" background
                            layout="sizes,prev, pager, next,jumper,total" :total="1000">
                        </el-pagination>
                    </div>
                </el-main>
            </el-container>
        </el-container>
    </div>
</template>

<script>
import { ref, reactive, onMounted } from 'vue'
import axios from 'axios'

export default {
    setup() {
        const tableData = ref([]);
        const searchForm = reactive({
            name: '',
            gender: '',
            entrydate: [],
        });

        const activeMenu = ref('');

        // 在组件加载时立即请求数据
        onMounted(() => {
            axios.get("https://mock.apifox.cn/m1/3128855-0-default/emp/list")
                .then(resp => {
                    tableData.value = resp.data.data;
                });
        });

        function handleMenuSelect(index) {
            activeMenu.value = index;
        }

        function handleSizeChange(val) {
            console.log(`每页 ${val} 条`);
        }

        function handleCurrentChange(val) {
            console.log(`当前页: ${val}`);
        }

        function onSubmit() {
            console.log(searchForm);
        }

        return {
            tableData,
            searchForm,
            activeMenu,
            handleMenuSelect,
            onSubmit,
            handleSizeChange,
            handleCurrentChange,
        };
    }
}
</script>

<style></style>

```

##### 全部代码DeptView.vue

```vue
<template>
    <div class="common-layout">
        <el-container style="height: 700px; border: 1px solid #eee;">
            <el-header style="font-size: 40px; background-color:darkgrey;">
                Wisdri视频播放平台
            </el-header>
            <el-container>
                <el-aside width="200px">
                    <el-scrollbar>
                        <el-menu @select="handleMenuSelect">
                            <el-sub-menu index="1">
                                <template #title>
                                    <i class="el-icon-message"></i>系统信息管理
                                </template>
                                <el-menu-item index="1-1">
                                    <router-link to="/dept">部门管理</router-link>
                                </el-menu-item>
                                <el-menu-item index="1-2">
                                    <router-link to="/emp">员工管理</router-link>
                                </el-menu-item>
                            </el-sub-menu>
                        </el-menu>
                    </el-scrollbar>
                </el-aside>

                <el-main>
                    <!-- 使用 v-if 控制部门管理表格的显示 -->
                    <div v-if="activeMenu === '1-1'">
                        <el-scrollbar>
                            <el-table :data="departmentData">
                                <el-table-column prop="name" label="部门名称" width="200"></el-table-column>
                                <el-table-column prop="updatetime" label="最后操作时间" width="250"></el-table-column>
                                <el-table-column label="操作">
                                    <template v-slot="scope">
                                        <!-- 使用 scope.row.name 和 scope.row.updatetime 来填充数据 -->
                                        <el-button type="primary" size="small" @click="editDepartment(scope.row)">编辑</el-button>
                                        <el-button type="danger" size="small" @click="deleteDepartment(scope.row)">删除</el-button>
                                    </template>
                                </el-table-column>
                            </el-table>
                        </el-scrollbar>

                        <!-- Pagination分页 -->
                        <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" background
                            layout="sizes, prev, pager, next, jumper, total" :total="totalItems">
                        </el-pagination>
                    </div>
                </el-main>
            </el-container>
        </el-container>
    </div>
</template>

<script>
import { ref } from 'vue';

export default {
    setup() {
        const departmentData = ref([
            {
                name: '研发部',
                updatetime: '2024-08-01 10:23:45',
            },
            {
                name: '市场部',
                updatetime: '2024-08-02 14:12:34',
            },
        ]);
        const totalItems = ref(2);
        const activeMenu = ref('');

        function handleMenuSelect(index) {
            activeMenu.value = index;
        }

        function handleSizeChange(val) {
            console.log(`每页 ${val} 条`);
        }

        function handleCurrentChange(val) {
            console.log(`当前页: ${val}`);
        }

        return {
            departmentData,
            totalItems,
            activeMenu,
            handleMenuSelect,
            handleSizeChange,
            handleCurrentChange,
        };
    }
};
</script>

<style></style>

```

#### 2， Nginx--将代码打包部署到服务器上

##### 1， 打包  用npm代码打包即可

##### 2， 用nginx部署 

大致步骤是把打包的文件放在nginx文件夹中 然后启动就可以了

将我们之前打包的前端工程dist目录下得内容拷贝到nginx的html目录下，如下图所示：

![6-2](picture/6-2.png)

然后我们通过双击nginx下得nginx.exe文件来启动nginx，如下图所示：



![6-3](picture/6-3.png)



nginx服务器的端口号是80，所以启动成功之后，我们浏览器直接访问http://localhost:80 即可，其中80端口可以省略，其浏览器展示效果如图所示：

![6-4](picture/6-4.png)

![6-5](picture/6-5.png)

到此，我们的前端工程发布成功。



`PS: 如果80端口被占用，我们需要通过**conf/nginx.conf**配置文件来修改端口号。如下图所示：`



## 													Maven（重点复习）

这里不对maven做详细介绍了  只用一句话就够了

Maven是Apache旗下的一个开源项目，是一款用于管理和构建java项目的工具。

官网：https://maven.apache.org/

> Apache 软件基金会，成立于1999年7月，是目前世界上最大的最受欢迎的开源软件基金会，也是一个专门为支持开源项目而生的非盈利性组织。
>
> 开源项目：https://www.apache.org/index.html#projects-list



#### 1.  Maven的作用

使用Maven能够做什么呢？

1. 依赖管理

   在pom文件中修改就行了

2. 统一项目结构：不同开发工具搞出来的目录结构都通用

   > 目录说明： 
   >
   > - src/main/java: java源代码目录
   > - src/main/resources:  配置文件信息
   > - src/test/java: 测试代码
   > - src/test/resources: 测试配置文件信息

3. 项目构建

如上图所示我们开发了一套系统，代码需要进行编译、测试、打包、发布，这些操作如果需要反复进行就显得特别麻烦，而Maven提供了一套简单的命令来完成项目构建。

![7-1](picture/7-1.png)

综上所述，可以得到一个结论：**Maven是一款管理和构建java项目的工具**

#### 2. maven概述

解压缩后的目录结构如下：

![image-20220616100529868](../BaiduNetdiskDownload/day04-Maven-SpringBootWeb入门/day04-Maven-SpringBootWeb入门/讲义/01. Maven/assets/image-20220616100529868-1669794069698.png) 

* bin目录 ： 存放的是可执行命令。（mvn 命令重点关注）
* conf目录 ：存放Maven的配置文件。（settings.xml配置文件后期需要修改）
* lib目录 ：存放Maven依赖的jar包。（Maven也是使用java开发的，所以它也依赖其他的jar包）

#### 3. maven项目结构

> - Maven项目的目录结构:
>
>   maven-project01
>   	|---  src  (源代码目录和测试代码目录)
>   		    |---  main (源代码目录)
>   			           |--- java (源代码java文件目录)
>   			           |--- resources (源代码配置文件目录)
>   		    |---  test (测试代码目录)
>   			           |--- java (测试代码java目录)
>   			           |--- resources (测试代码配置文件目录)
>   	|--- target (编译、打包生成文件存放目录)

创建过程由于版本不一致 省略，没什么大问题

##### 3.1 POM配置详解

POM (Project Object Model) ：指的是项目对象模型，用来描述当前的maven项目。

- 使用pom.xml文件来实现

pom.xml文件：

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <!-- POM模型版本 -->
    <modelVersion>4.0.0</modelVersion>

    <!-- 当前项目坐标 -->
    <groupId>com.itheima</groupId>
    <artifactId>maven_project1</artifactId>
    <version>1.0-SNAPSHOT</version>
    
    <!-- 打包方式 -->
    <packaging>jar</packaging>
 
</project>
~~~

pom文件详解：

- <project> ：pom文件的根标签，表示当前maven项目
- <modelVersion> ：声明项目描述遵循哪一个POM模型版本
  - 虽然模型本身的版本很少改变，但它仍然是必不可少的。目前POM模型版本是4.0.0
- 坐标 ：<groupId>、<artifactId>、<version>
  - 定位项目在本地仓库中的位置，由以上三个标签组成一个坐标
- <packaging> ：maven项目的打包方式，通常设置为jar或war（默认值：jar）



##### 3.2 Maven坐标详解

什么是坐标？

* Maven中的坐标是==**资源的唯一标识== , 通过该坐标可以唯一定位资源位置**
* 使用坐标来定义项目或引入项目中需要的依赖

Maven坐标主要组成

* groupId：定义当前Maven项目隶属组织名称（通常是域名反写，例如：com.itheima）
* artifactId：定义当前Maven项目名称（通常是模块名称，例如 order-service、goods-service）
* version：定义当前项目版本号

`这些坐标创建的时候没改的话 直接在文件中手动修改就行`





#### 4 maven依赖配置

直接在pom.xml文件中调用即可

1. 在pom.xml中编写<dependencies>标签
2. 在<dependencies>标签中使用<dependency>引入坐标

3. 定义坐标的 groupId、artifactId、version

```xml
<dependencies>
    <!-- 第1个依赖 : logback -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.2.11</version>
    </dependency>
    <!-- 第2个依赖 : junit -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>
</dependencies>
```

4. 点击刷新按钮，引入最新加入的坐标
   - 刷新依赖：保证每一次引入新的依赖，或者修改现有的依赖配置，都可以加入最新的坐标

![image-20221130184402805](../BaiduNetdiskDownload/day04-Maven-SpringBootWeb入门/day04-Maven-SpringBootWeb入门/讲义/01. Maven/assets/image-20221130184402805.png)  

> 注意事项：
>
> 1. 如果引入的依赖，在本地仓库中不存在，将会连接远程仓库 / 中央仓库，然后下载依赖（这个过程会比较耗时，耐心等待）
> 2. 如果不知道依赖的坐标信息，可以到mvn的中央仓库（https://mvnrepository.com/）中搜索





#### 5 maven依赖传递 引入父依赖就够了

我们现在使用了maven，当项目中需要使用logback-classic时，只需要在pom.xml配置文件中，添加logback-classic的依赖坐标即可。

![8-1](picture/8-1.png)

在pom.xml文件中只添加了logback-classic依赖，但由于maven的依赖具有传递性，所以会自动把所依赖的其他jar包也一起导入。



依赖传递可以分为：

1. 直接依赖：在当前项目中通过依赖配置建立的依赖关系

2. 间接依赖：被依赖的资源如果依赖其他资源，当前项目间接依赖其他资源

![8-2](picture/8-2.png)

比如以上图中：

- projectA依赖了projectB。对于projectA 来说，projectB 就是直接依赖。
- 而projectB依赖了projectC及其他jar包。 那么此时，在projectA中也会将projectC的依赖传递下来。对于projectA 来说，projectC就是间接依赖。

![image-20221201115801806](../BaiduNetdiskDownload/day04-Maven-SpringBootWeb入门/day04-Maven-SpringBootWeb入门/讲义/01. Maven/assets/image-20221201115801806.png)





#### 6. 排除依赖

问题：之前我们讲了依赖具有传递性。那么A依赖B，B依赖C，如果A不想将C依赖进来，是否可以做到？ 

答案：**在maven项目中，我们可以通过排除依赖来实现**。



什么是排除依赖？

- 排除依赖：指主动断开依赖的资源。（被排除的资源无需指定版本）

```xml
<dependency>
    <groupId>com.itheima</groupId>
    <artifactId>maven-projectB</artifactId>
    <version>1.0-SNAPSHOT</version>
   
    <!--排除依赖, 主动断开依赖的资源-->
    <exclusions>
    	<exclusion>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

##### 6.1 通过排除依赖解决冲突

假设你有一个项目 `A`，它依赖于库 `B` 和 `C`，而 `B` 又依赖于 `X-1.0` 版本，`C` 依赖于 `X-2.0` 版本。这样就可能会发生版本冲突，因为 `X` 的两个不同版本被引入了你的项目。



你可以通过在 `pom.xml` 文件中排除一个不想要的版本来解决这个问题。示例：

```xml
<dependencies>
    <dependency>
        <groupId>com.example</groupId>
        <artifactId>B</artifactId>
        <version>1.0</version>
        <exclusions>
            <exclusion>
                <groupId>com.example</groupId>
                <artifactId>X</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <dependency>
        <groupId>com.example</groupId>
        <artifactId>C</artifactId>
        <version>1.0</version>
    </dependency>
</dependencies>
```

在这个例子中，依赖 `B` 的时候，我们排除了 `B` 所依赖的 `X` 库。这样，Maven 将只会使用 `C` 依赖的 `X-2.0` 版本，从而避免了版本冲突。

##### 6.2 其他用途

除了防止版本冲突之外，排除依赖还可以减少不必要的依赖，减小项目体积，或者解决安全性问题（排除存在已知漏洞的依赖版本）。





#### 7. maven依赖范围

在项目中导入依赖的jar包后，**`默认情况下，可以在任何地方使用。`**

如果希望限制依赖的使用范围，可以通过<scope>标签设置其作用范围。

 ![8-4](picture/8-4.png)

作用范围：

1. 主程序范围有效（main文件夹范围内）

2. 测试程序范围有效（test文件夹范围内）

3. 是否参与打包运行（package指令范围内）

![8-5](picture/8-5.png)

如上图所示，给junit依赖通过scope标签指定依赖的作用范围。 那么这个依赖就只能作用在测试环境，其他环境下不能使用。

scope标签的取值范围：

| **scope**值     | **主程序** | **测试程序** | **打包（运行）** | **范例**    |
| --------------- | ---------- | ------------ | ---------------- | ----------- |
| compile（默认） | Y          | Y            | Y                | log4j       |
| test            | -          | Y            | -                | junit       |
| provided        | Y          | Y            | -                | servlet-api |
| runtime         | -          | Y            | Y                | jdbc驱动    |



#### 8. maven生命周期

Maven的生命周期就是为了对所有的构建过程进行抽象和统一。 描述了一次项目构建，经历哪些阶段。

在Maven出现之前，项目构建的生命周期就已经存在，软件开发人员每天都在对项目进行清理，编译，测试及部署。虽然大家都在不停地做构建工作，但公司和公司间、项目和项目间，往往使用不同的方式做类似的工作。

Maven从大量项目和构建工具中学习和反思，然后总结了一套高度完美的，易扩展的项目构建生命周期。这个生命周期包含了项目的清理，初始化，编译，测试，打包，集成测试，验证，部署和站点生成等几乎所有构建步骤。

Maven对项目构建的生命周期划分为3套（相互独立）：

![8-6](picture/8-6.png)

我们看到这三套生命周期，里面有很多很多的阶段，这么多生命周期阶段，其实我们常用的并不多，主要关注以下几个：

• clean：移除上一次构建生成的文件

• compile：**编译项目源代码**

• test：**使用合适的单元测试框架运行测试(junit)**

• package：将编译后的文件打包，如：jar、war等

• install：安装项目到本地仓库



##### 8.1 执行方式一  IDEA鼠标点击执行

- 选择对应的生命周期，双击执行

![8-7](picture/8-7.png) 

##### 8.2 命令行执行

在项目的目录下打开命令行 用指令即可

```java
mvn clean
```



## 																													Spring（超级重点）

#### 1 springboot项目搭建

用idea完成即可

重点：idea版本过高 需要先创建高版本 再手动降低

**1 java    在pom文件中降低**

**2 jdk    用快捷键 ctrl+shift+alt+s   打开调整页面降低**

**3 springboot  在pom文件中降低**

三个东西的版本



#### 2. http超文本传输协议

有个大概认识就行了   是浏览器向服务器请求数据    服务器返回数据的一种协议

之前已经背过八股文了



#### 3. 注解 

由于后面的开发全是注解   这里有必要复习一下注解的内容

##### 1 认识注解

`就是一个标注  自定义注解的作用和提示没啥区别`

定义了两个注解  `MyTest1和MyTest2`  

里面有自定义的属性 然后右边调用的时候就是放在这里而已

![注解1](picture/注解1.PNG)

`注解本质是一个接口` 继承了annotation接口 `里面的属性都是抽象方法`

![注解2](picture/注解2.PNG)

##### 2 元注解

元注解是  修饰注解的注解

常用的注解如下图

1 target  可以看到 下面注解用错地方了会报错

![注解3](picture/注解3.PNG)

2Retention 声明注解的保留周期 重点是第三个

![注解4](picture//注解4.PNG)

##### 3 注解的解析  复习的时候直接看这个就行

判断是否存在注解  并把注解里的内容给解析出来  

这里就和反射结合起来了

![注解5](picture/注解5.PNG)

这里  `右边定义了一个注解 中间定义了一个类并用注解标注  上方则通过反射成功解析了注解` 能看懂这个就懂注解了

下面的打印成功了 也不需要看

![注解6](picture/注解6.PNG)

解析方法上的注解也是一样的 观察下代码的区别 这里是对这个m解析 其他的都一样

```java
Method m = c.getDeclaredMethod("test1");
```

可以看到打印的是牛魔王 也就是`方法的注解信息`

![注解7](picture/注解7.PNG)

##### 4 注解的使用场景（必须看！！！！  注解+反射）

这里直接给出实例

通过注解的解析  来让加了注解的方法自动执行

`这里有反射的知识！！！`  需要整体复习 能看懂就够了

1 定义自定义注解（简单 注意语法，另外这里的注解规定了运行时间和作用范围）

2 定义类和方法 其中部分方法添加了该自定义注解

3 在main函数中  先`通过反射得到class对象 再得到类中的方法（反射）`

4  然后`对每一个方法解析注解 有注解就执行方法（注解+反射invoke(这个是反射执行成员方法的函数)）`

![注解8](picture//注解8.PNG)





#### 4. 反射

##### 1 认识反射  获取类

反射就是-----加载类，并允许以编程方式访问已加载类的字各种成分（成员变量 构造方法 构造器等等）

反射学什么？

学习获取类的信息 并操作他们



![反射1](picture/反射1.PNG)



![反射2](picture/反射2.PNG)



```java 
//第一步  获取到了全类名  并且这里获取反射的方式是第一种
com.itheima.d2_reflect.Student
//第二步  获取了简单的simplename 对应代码理解
Student
    
//第二种获取反射的方式 通过调用class提供的方法
true
//Object提供的方法  这里是需要存在实际对象的 可以看到全部相等
true
```



##### 2 获取类的构造器

自行理解下列图片 有注释应该问题不大  下面打印的是构造器（构造函数的参数个数 可以看到是对应的 0和2）

![反射3](picture/反射3.PNG)

其他具体函数的用法忽略   区别只是`能否获取private  能否获取多个还是单个的区别` 不重要（具体看右下角 看不清自己查）

![反射4](F:/硕士文件汇总/CS资料/java面经/图片/反射4.PNG)

##### 3 获取类构造器的作用：初始化对象返回

`反射是会破坏封装性的！！！！  通过反射来访问构造器 可以有办法访问private的构造器`

首先  两种通过构造器来构造对象的方法  得到了两个构造器

![反射5](picture/反射5.PNG)

然后可以看到 `对于private的无参数构造器 通过暴力反射仍然可以构造出对象`

![反射6](picture/反射6.PNG)



##### 4 反射获取类的成员变量

如何获取就不说了 和构造器一模一样  先得到类 然后通过函数获取即可

![反射7](picture/反射7.PNG)

获取之后的作用？？---------`赋值和取值`  （和构造器一样 获取就是为了调用）

同理  这里也有私有访问的冲突

但是`反射可以破坏封装性！！！` 设置`fname暴力反射`就行

```java
fName.setAccessible(true);
```

![反射8](picture/反射8.PNG)

##### 5 反射获取类的成员方法

怎么获取不看了 和前面一模一样  直接看作用---`执行成员方法`

同样 这里访问private成员方法 需要暴力反射 

可以看到invoke执行了方法  并且因为是void 所以rs接收到的结果是null 合理

![反射9](picture/反射9.PNG)

##### 6 反射的应用场景（复习的时候直接看这个 能看懂就行）

![反射10](picture/反射10.PNG)

![反射11](picture/反射11.PNG)



![反射12](picture/反射12.PNG)



![反射13](picture/反射13.PNG)

#### 5. tomcat 

springboot已经内嵌了  不用安装



## 													SpringBoot 响应

 基于SpringBoot的方式开发一个web应用，浏览器发起请求 /hello 后 ，给浏览器返回字符串 “Hello World ~”。



其实呢，是`我们在浏览器发起请求，请求了我们的后端web服务器(也就是内置的Tomcat)`。而我们在开发web程序时呢，定义了一个控制器类Controller，请求会被部署在Tomcat中的Controller接收，然后Controller再给浏览器一个响应，响应一个字符串 “Hello World”。 而在请求响应的过程中是遵循HTTP协议的。

但是呢，这里要告诉大家的时，其实**在Tomcat这类Web服务器中，是不识别我们自己定义的Controller**的。但是我们前面讲到过Tomcat是一个Servlet容器，是支持Serlvet规范的，所以呢，在tomcat中是可以识别 Servlet程序的。 

那我们所编写的XxxController 是如何处理请求的，又与Servlet之间有什么联系呢？

其实呢，在SpringBoot进行web程序开发时，它**内置了一个核心的Servlet程序 DispatcherServlet，称之为 核心控制器**。 DispatcherServlet 负责接收页面发送的请求，然后根据执行的规则，将请求再转发给后面的请求处理器Controller，请求处理器处理完请求之后，最终再由DispatcherServlet给浏览器响应数据。

![9-1](picture/9-1.png)

那将来浏览器发送请求，会携带请求数据，包括：请求行、请求头；请求到达tomcat之后，tomcat会负责解析这些请求数据，然后呢将解析后的请求数据会传递给Servlet程序的HttpServletRequest对象，那也就意味着 HttpServletRequest 对象就可以获取到请求数据。 而Tomcat，还给Servlet程序传递了一个参数 HttpServletResponse，通过这个对象，我们就可以给浏览器设置响应数据 。

![9-2](picture/9-2.png) 

那上述所描述的这种浏览器/服务器的架构模式呢，我们称之为：BS架构。

![9-3](picture/9-3.png) 

• BS架构：Browser/Server，浏览器/服务器架构模式。客户端只需要浏览器，应用程序的逻辑和数据都存储在服务端。

那今天呢，我们的课程内容主要就围绕着：**请求、响应进行**。 

> - 请求
> - 响应
> - 分层解耦





#### 1. Postman 解决接口测试需求

![4-2](picture/4-2.png)

在这种**前后端分离模式**下，前端技术人员基于"接口文档"，开发前端程序；后端技术人员也基于"接口文档"，开发后端程序。

由于前后端分离，对我们后端技术人员来讲，在开发过程中，是没有前端页面的，那我们怎么测试自己所开发的程序呢？

方式1：像之前SpringBoot入门案例中一样，直接使用浏览器。在浏览器中输入地址，测试后端程序。

- 弊端：在浏览器地址栏中输入地址这种方式都是GET请求，如何我们要用到POST请求怎么办呢？
  - 要解决POST请求，需要程序员自己编写前端代码（比较麻烦）

方式2：**使用专业的接口测试工具（课程中我们使用Postman工具）**

具体使用省略





##### 1.1 原始方式发送请求

在原始的Web程序当中，需要通过Servlet中提供的API：HttpServletRequest（请求对象），获取请求的相关信息。比如获取请求参数：

> Tomcat接收到http请求时：把请求的相关信息封装到HttpServletRequest对象中

在Controller中，我们要想获取Request对象，可以直接在方法的形参中声明 HttpServletRequest 对象。然后就可以通过该对象来获取请求信息：

```json
//根据指定的参数名获取请求参数的数据值
String  request.getParameter("参数名")
```

```java
@RestController
public class RequestController {
    //原始方式
    @RequestMapping("/simpleParam")
    public String simpleParam(HttpServletRequest request){
        // http://localhost:8080/simpleParam?name=Tom&age=10
        // 请求参数： name=Tom&age=10   （有2个请求参数）
        // 第1个请求参数： name=Tom   参数名:name，参数值:Tom
        // 第2个请求参数： age=10     参数名:age , 参数值:10

        String name = request.getParameter("name");//name就是请求参数名
        String ageStr = request.getParameter("age");//age就是请求参数名

        int age = Integer.parseInt(ageStr);//需要手动进行类型转换
        System.out.println(name+"  :  "+age);
        return "OK";
    }
}
```

> 以上这种方式，我们仅做了解。（在以后的开发中不会使用到）



##### 1.2 SpringBoot方式

在Springboot的环境中，对原始的API进行了封装，接收参数的形式更加简单。 如果是简单参数，参数名与形参变量名相同，定义同名的形参即可接收参数。

~~~java
@RestController
public class RequestController {
    // http://localhost:8080/simpleParam?name=Tom&age=10
    // 第1个请求参数： name=Tom   参数名:name，参数值:Tom
    // 第2个请求参数： age=10     参数名:age , 参数值:10
    
    //springboot方式
    @RequestMapping("/simpleParam")
    public String simpleParam(String name , Integer age ){//形参名和请求参数名保持一致
        System.out.println(name+"  :  "+age);
        return "OK";
    }
}
~~~

**postman测试( GET 请求)：**

![9-4](picture/9-4.png)

**postman测试( POST请求 )：**

![9-4](picture/9-5.png)

> **结论：不论是GET请求还是POST请求，对于简单参数来讲，只要保证==请求参数名和Controller方法中的形参名保持一致==，就可以获取到请求参数中的数据值。**

##### 1.3 参数名不一致

如果方法形参名称与请求参数名称不一致，controller方法中的形参还能接收到请求参数值吗？

~~~java
@RestController
public class RequestController {
    // http://localhost:8080/simpleParam?name=Tom&age=20
    // 请求参数名：name

    //springboot方式
    @RequestMapping("/simpleParam")
    public String simpleParam(String username , Integer age ){//请求参数名和形参名不相同
        System.out.println(username+"  :  "+age);
        return "OK";
    }
}
~~~

答案：运行没有报错。 controller方法中的username值为：null，age值为20

- 结论：对于简单参数来讲，请求参数名和controller方法中的形参名不一致时，无法接收到请求数据

那么如果我们开发中，遇到了这种请求参数名和controller方法中的形参名不相同，怎么办？

解决方案：可以使用Spring提供的@RequestParam注解完成映射

在方法形参前面加上 @RequestParam 然后通过value属性执行请求参数名，从而完成映射。代码如下：

```java
@RestController
public class RequestController {
    // http://localhost:8080/simpleParam?name=Tom&age=20
    // 请求参数名：name

    //springboot方式
    @RequestMapping("/simpleParam")
    public String simpleParam(@RequestParam("name") String username , Integer age ){
        System.out.println(username+"  :  "+age);
        return "OK";
    }
}
```

> **注意事项：**
>
> @RequestParam中的required属性默认为true（默认值也是true），代表该请求参数必须传递，如果不传递将报错

##### 1.4 复杂实体对象

复杂实体对象指的是，在实体类中有一个或多个属性，也是实体对象类型的。如下：

- User类中有一个Address类型的属性（Address是一个实体类）

![9-6](picture/9-6.png)

复杂实体对象的封装，需要遵守如下规则：

- **请求参数名与形参对象属性名相同，按照对象层次结构关系即可接收嵌套实体类属性参数。**

定义POJO实体类：

- Address实体类

```java
public class Address {
    private String province;
    private String city;

    public String getProvince() {
        return province;
    }

    public void setProvince(String province) {
        this.province = province;
    }

    public String getCity() {
        return city;
    }

    public void setCity(String city) {
        this.city = city;
    }

    @Override
    public String toString() {
        return "Address{" +
                "province='" + province + '\'' +
                ", city='" + city + '\'' +
                '}';
    }
}
```

- User实体类

```java
public class User {
    private String name;
    private Integer age;
    private Address address; //地址对象

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", address=" + address +
                '}';
    }
}
```

Controller方法：

```java
@RestController
public class RequestController {
    //实体参数：复杂实体对象
    @RequestMapping("/complexPojo")
    public String complexPojo(User user){
        System.out.println(user);
        return "OK";
    }
}
```

Postman测试：

![9-7](picture/9-7.png)



##### 1.5 数组集合函数

数组集合参数的使用场景：在HTML的表单中，有一个表单项是支持多选的(复选框)，可以提交选择的多个值。

也就是对于一些统计的表单 例如你的爱好 

![9-8](picture/9-8.png)

![image-20240828104618509](C:/Users/14088/AppData/Roaming/Typora/typora-user-images/image-20240828104618509.png)



后端程序接收上述多个值的方式有两种：

1. 数组

   ```java
   @RestController
   public class RequestController {
       //数组集合参数
       @RequestMapping("/arrayParam")
       public String arrayParam(String[] hobby){
           System.out.println(Arrays.toString(hobby));
           return "OK";
       }
   }
   ```

   

2. 集合

```java
@RestController
public class RequestController {
    //数组集合参数
    @RequestMapping("/listParam")
    public String listParam(@RequestParam List<String> hobby){
        System.out.println(hobby);
        return "OK";
    }
}
```



##### 1.6 日期参数

上述演示的都是一些普通的参数，在一些特殊的需求中，可能会涉及到日期类型数据的封装。

因为日期的格式多种多样（如：2022-12-12 10:05:45 、2022/12/12 10:05:45），那么对于日期类型的参数在进行封装的时候，需要通过@DateTimeFormat注解，以及其pattern属性来设置日期的格式。

![9-9](picture/9-9.png)

- @DateTimeFormat注解的pattern属性中指定了哪种日期格式，前端的日期参数就必须按照指定的格式传递。
- 后端controller方法中，需要使用Date类型或LocalDateTime类型，来封装传递的参数。

Controller方法：

```java
@RestController
public class RequestController {
    //日期时间参数
   @RequestMapping("/dateParam")
    public String dateParam(@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss") LocalDateTime updateTime){
        System.out.println(updateTime);
        return "OK";
    }
}
```

##### 1.7 json参数

服务端Controller方法接收JSON格式数据：

- 传递json格式的参数，在Controller中会使用实体类进行封装。 

- 封装规则：**JSON数据键名与形参对象属性名相同，定义POJO类型形参即可接收参数。需要使用 @RequestBody标识。**

- **跟处理复杂实体比起来，区别就是加了@RequestBody标识，这样就会以json的形式来解析**。

  ![9-10](picture/9-10.png)

实体类：Address和user

```java
public class Address {
    private String province;
    private String city;
    
	//省略GET , SET 方法
}

public class User {
    private String name;
    private Integer age;
    private Address address;
    
    //省略GET , SET 方法
}  
```

Controller方法：

```java
@RestController
public class RequestController {
    //JSON参数
    @RequestMapping("/jsonParam")
    public String jsonParam(@RequestBody User user){
        System.out.println(user);
        return "OK";
    }
}

//这个是没有json 复杂实体形式接受
@RestController
public class RequestController {
    //实体参数：复杂实体对象
    @RequestMapping("/complexPojo")
    public String complexPojo(User user){
        System.out.println(user);
        return "OK";
    }
}
```



##### 1.8  路径参数

看图就懂

![9-11](picture/9-11.png)

Controller方法：

~~~java
@RestController
public class RequestController {
    //路径参数
    @RequestMapping("/path/{id}/{name}")
    public String pathParam2(@PathVariable Integer id, @PathVariable String name){
        System.out.println(id+ " : " +name);
        return "OK";
    }
}
~~~



#### 2 响应

##### 2.1 @RestController注解

controller方法中的return的结果，怎么就可以响应给浏览器呢？

答案：使用@ResponseBody注解

**@ResponseBody注解：**

- 类型：方法注解、类注解
- 位置：书写在Controller方法上或类上
- 作用：将方法返回值直接响应给浏览器
  - 如果返回值类型是实体对象/集合，将会转换为JSON格式后在响应给浏览器

但是在我们所书写的Controller中，只在类上添加了@RestController注解、方法添加了@RequestMapping注解，并没有使用@ResponseBody注解，怎么给浏览器响应呢？

~~~java
@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String hello(){
        System.out.println("Hello World ~");
        return "Hello World ~";
    }
}
~~~

原因**`：在类上添加的@RestController注解，是一个组合注解。**`

- @RestController = @Controller + @ResponseBody 

@RestController源码：

~~~java
@Target({ElementType.TYPE})   //元注解（修饰注解的注解）
@Retention(RetentionPolicy.RUNTIME)  //元注解
@Documented    //元注解
@Controller   
@ResponseBody 
public @interface RestController {
    @AliasFor(
        annotation = Controller.class
    )
    String value() default "";
}
~~~

**结论：在类上添加@RestController就相当于添加了@ResponseBody注解。**

- 类上有@RestController注解或@ResponseBody注解时：表示当前类下所有的方法返回值做为响应数据
  - 方法的返回值，如果是一个POJO对象或集合时，会先转换为JSON格式，在响应给浏览器

##### 2.2 统一响应结果

如果我们开发一个大型项目，项目中controller方法将成千上万，使用上述方式将造成整个项目难以维护。那在真实的项目开发中是什么样子的呢？

在真实的项目开发中，无论是哪种方法，我们都会定义一个统一的返回结果。方案如下：

![9-12](picture/9-12.png)

> 前端：只需要按照统一格式的返回结果进行解析(仅一种解析方案)，就可以拿到数据。

统一的返回结果使用类来描述，在这个结果中包含：

- 响应状态码：当前请求是成功，还是失败

- 状态码信息：给页面的提示信息

- 返回的数据：给前端响应的数据（字符串、对象、集合）

定义在一个实体类Result来包含以上信息。代码如下：

```java
public class Result {
    private Integer code;//响应码，1 代表成功; 0 代表失败
    private String msg;  //响应码 描述字符串
    private Object data; //返回的数据

    public Result() { }
    public Result(Integer code, String msg, Object data) {
        this.code = code;
        this.msg = msg;
        this.data = data;
    }

    public Integer getCode() {
        return code;
    }

    public void setCode(Integer code) {
        this.code = code;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }

    public Object getData() {
        return data;
    }

    public void setData(Object data) {
        this.data = data;
    }

    //增删改 成功响应(不需要给前端返回数据)
    public static Result success(){
        return new Result(1,"success",null);
    }
    //查询 成功响应(把查询结果做为返回数据响应给前端)
    public static Result success(Object data){
        return new Result(1,"success",data);
    }
    //失败响应
    public static Result error(String msg){
        return new Result(0,msg,null);
    }
}
```

改造Controller：

~~~java
@RestController
public class ResponseController { 
    //响应统一格式的结果
    @RequestMapping("/hello")
    public Result hello(){
        System.out.println("Hello World ~");
        //return new Result(1,"success","Hello World ~");
        return Result.success("Hello World ~");
    }

    //响应统一格式的结果
    @RequestMapping("/getAddr")
    public Result getAddr(){
        Address addr = new Address();
        addr.setProvince("广东");
        addr.setCity("深圳");
        return Result.success(addr);
    }

    //响应统一格式的结果
    @RequestMapping("/listAddr")
    public Result listAddr(){
        List<Address> list = new ArrayList<>();

        Address addr = new Address();
        addr.setProvince("广东");
        addr.setCity("深圳");

        Address addr2 = new Address();
        addr2.setProvince("陕西");
        addr2.setCity("西安");

        list.add(addr);
        list.add(addr2);
        return Result.success(list);
    }
}
~~~

##### 2.3 案例 从本地读取表格数据并提交

这个案例展示了一个 Spring Boot 控制器类（`ResponseController`），它用来处理来自客户端的 HTTP 请求，并返回统一格式的响应数据。简单来说，程序做了以下几件事：

1. **处理请求**：客户端发送请求到服务器，比如访问 `/hello` 或 `/getAddr` 等路径。
2. **生成数据**：服务器根据请求，在代码里创建一些简单的对象，比如 "Hello World" 的消息，或者包含地址信息的对象。
3. **返回响应**：服务器把这些数据包装成一个统一格式的响应对象，并返回给客户端。

总的来说，这个程序负责接收请求，生成数据，然后把数据以特定格式返回给客户端。

Contriller代码：

```java
@RestController
public class EmpController {
    @RequestMapping("/listEmp")
    public Result list(){
        //1. 加载并解析emp.xml
        String file = this.getClass().getClassLoader().getResource("emp.xml").getFile();
        //System.out.println(file);
        List<Emp> empList = XmlParserUtils.parse(file, Emp.class);

        //2. 对数据进行转换处理 - gender, job
        empList.stream().forEach(emp -> {
            //处理 gender 1: 男, 2: 女
            String gender = emp.getGender();
            if("1".equals(gender)){
                emp.setGender("男");
            }else if("2".equals(gender)){
                emp.setGender("女");
            }

            //处理job - 1: 讲师, 2: 班主任 , 3: 就业指导
            String job = emp.getJob();
            if("1".equals(job)){
                emp.setJob("讲师");
            }else if("2".equals(job)){
                emp.setJob("班主任");
            }else if("3".equals(job)){
                emp.setJob("就业指导");
            }
        });
        //3. 响应数据
        return Result.success(empList);
    }
}
```

统一返回结果实体类：



#### 3. 分层解耦

上述案例的功能，我们虽然已经实现，但是呢，我们会发现案例中：解析XML数据，获取数据的代码，处理数据的逻辑的代码，给页面响应的代码全部都堆积在一起了，全部都写在controller方法中了。

![9-13](picture/9-13.png)

当前程序的这个业务逻辑还是比较简单的，如果业务逻辑再稍微复杂一点，我们会看到Controller方法的代码量就很大了。

- 当我们要修改操作数据部分的代码，需要改动Controller

- 当我们要完善逻辑处理部分的代码，需要改动Controller

- 当我们需要修改数据响应的代码，还是需要改动Controller



分层解耦的核心，就是把上述完整的代码 **拆分到三层架构中**

![9-14](picture/9-14.png)



![9-16](picture/9-16.png)



- **Controller**：控制层。接收前端发送的请求，对请求进行处理，并响应数据。
- **Service**：业务逻辑层。处理具体的业务逻辑。
- **Dao：**数据访问层(Data Access Object)，也称为持久层。负责数据访问操作，包括数据的增、删、改、查。





##### 3.1 代码拆分

总结来说就是将上面的代码分别按照颜色，塞进三层中

其中，**每一层都需要分别创建接口，并通过函数（上述实际代码）来实现接口完成功能**

![9-16-5](picture/9-16-5.png)

![9-17](picture/9-17.png)

##### 3.2 个人理解

这里直接给出我对于三层框架的理解

本质上就是将**完整代码拆分变成三步**，

根据功能分别塞进controller,service,dao三层架构中去

其中**每层架构除了controller，其他两个dao和service都是接口+实现**



此外，**下一层需要引入上一层创建的接口**，这样上一步完成的功能下一步才能继续下去

例如，dao层已经完成了数据的访问，需要service层进行处理，因此:

**下面实际实现类中便创建了一个empDao,用他里面的函数来完成完整代码中的第一步，然后第二步才能继续写**

```java 
//业务逻辑接口（制定业务标准）
public interface EmpService {
    //获取员工列表
    public List<Emp> listEmp();
}


//业务逻辑实现类（按照业务标准实现）
public class EmpServiceA implements EmpService {
    //dao层对象
    private EmpDao empDao = new EmpDaoA();

    @Override
    public List<Emp> listEmp() {
        //1. 调用dao, 获取数据
        List<Emp> empList = empDao.listEmp();

        //2. 对数据进行转换处理 - gender, job
        empList.stream().forEach(emp -> {
            //处理 gender 1: 男, 2: 女
            String gender = emp.getGender();
            if("1".equals(gender)){
                emp.setGender("男");
            }else if("2".equals(gender)){
                emp.setGender("女");
            }

            //处理job - 1: 讲师, 2: 班主任 , 3: 就业指导
            String job = emp.getJob();
            if("1".equals(job)){
                emp.setJob("讲师");
            }else if("2".equals(job)){
                emp.setJob("班主任");
            }else if("3".equals(job)){
                emp.setJob("就业指导");
            }
        });
        return empList;
    }
}
```

controller中引用service的结果同理：

**创建一个service接口的返回结果empService**

```java
//业务逻辑接口（制定业务标准）
public interface EmpService {
    //获取员工列表
    public List<Emp> listEmp();
}


@RestController
public class EmpController {
    //业务层对象
    private EmpService empService = new EmpServiceA();

    @RequestMapping("/listEmp")
    public Result list(){
        //1. 调用service层, 获取数据
        List<Emp> empList = empService.listEmp();

        //3. 响应数据
        return Result.success(empList);
    }
}
```



##### 补充：接口创建实例语法的问题

**为什么使用接口作为数据类型？**

1. **解耦合**：

   - **面向接口编程**：接口定义了程序的功能契约，而具体的实现由实现类提供。通过依赖接口而不是具体的实现类，代码的依赖性被降低。当你需要更换实现时，只需替换实现类，而不需要修改使用该接口的代码。
   - 例如，你的代码 `private EmpService empService = new EmpServiceA();` 中，`EmpService` 是接口，而 `EmpServiceA` 是具体的实现类。这意味着你可以随时替换 `EmpServiceA` 为其他实现（例如 `EmpServiceB`），而不需要修改其他代码。

2. **灵活性和可扩展性**：

   - 使用接口作为数据类型使得代码更具灵活性。如果将来有新的 `EmpService` 实现类（例如 `EmpServiceB`），你可以很容易地将其替换进来，而无需更改接口的使用代码。

   - 例如，如果你有另一个 EmpServiceB实现类，你可以简单地将 EmpServiceA替换为EmpServiceB：

     ```java
     private EmpService empService = new EmpServiceB(); // 只需这一处修改
     ```

3. **支持多态**：

   - 接口允许你利用多态性。你可以在不改变接口使用代码的情况下，通过不同的实现类来实现不同的行为。这在实际应用中非常有用，尤其是在需要动态替换实现时。



#### 4.  IOC控制反转---进一步解耦

对于IOC和DI  最重要的是弄清楚为什么要引入 具体实现反而简单

##### 4.1 为什么要引入控制反转IOC和依赖注入DI

首先需要了解软件开发涉及到的两个概念：内聚和耦合。

- 内聚：软件中各个功能模块内部的功能联系。

- 耦合：衡量软件中各个层/模块之间的依赖、关联的程度。

**软件设计原则：高内聚低耦合。**

> 高内聚指的是：一个模块中各个元素之间的联系的紧密程度，如果各个元素(语句、程序段)之间的联系程度越高，则内聚性越高，即 "高内聚"。
>
> 低耦合指的是：软件中各个层、模块之间的依赖关联程序越低越好。

例如：对于右边的代码，全都是员工处理逻辑，没有其他无关逻辑，内聚性很高



![9-17](picture/9-17.png)



**耦合：**

把业务类变为EmpServiceB时，**service层和controller层中的代码都要变！**

![10-1](picture/10-1.png)

**高内聚、低耦合**的目的是使程序模块的可重用性、移植性大大增强。

![10-2](picture/10-2.png)

##### 4.2 解耦思路

之前需要什么对象，就直接new一个。 **这种做法呢，层与层之间代码就耦合了，当service层的实现变了之后， 我们还需要修改controller层的代码。**



![10-1](picture/10-1.png)



那应该怎么解耦呢？

- 首先**不能在EmpController中使用new对象**。代码如下：

![10-3](picture/10-3.png)

- 此时，就存在另一个问题了，不能new，就意味着没有业务层对象（程序运行就报错），怎么办呢？
  - 我们的解决思路是：
    - 提供一个容器，容器中存储一些对象(例：EmpService对象)
    - controller程序从容器中获取EmpService类型的对象

我们想要实现上述解耦操作，就涉及到Spring中的两个核心概念：

- **控制反转：** Inversion Of Control，简称IOC。对象的创建控制权由程序自身转移到外部（容器），这种思想称为控制反转。

  > 对象的创建权由程序员主动创建转移到容器(由容器创建、管理对象)。这个容器称为：IOC容器或Spring容器

- **依赖注入：** Dependency Injection，简称DI。容器为应用程序提供运行时，所依赖的资源，称之为依赖注入。

  > 程序运行时需要某个资源，此时容器就为其提供这个资源。
  >
  > 例：EmpController程序运行时需要EmpService对象，Spring容器就为其提供并注入EmpService对象

IOC容器中创建、管理的对象，称之为：**bean对象**





##### 4.3 ioc具体实现--@component与@Autowired

第1步：**删除Controller层、Service层中new对象的代码**

![10-4](picture/10-4.png)

第2步：Service层及Dao层的实现类，用**@Component注解**表示，交给IOC容器管理

- 使用Spring提供的注解：@Component ，就可以实现类交给IOC容器管理

![10-5](picture/10-5.png)



第3步：为Controller及Service注入运行时依赖的对象---**用@Autowired**

- 使用Spring提供的注解：@Autowired ，就可以实现程序运行时IOC容器自动注入需要的依赖对象

![10-6](picture/10-6.png)





这样的话，**后面如果要把serviceA改成serviceB，也只需要把serviceA上面标注的@component删掉，改到serviceB上，就能自动注入了**







##### 4.4 IOC控制反转详解

###### 4.4.1 bean声明精细化

前面我们提到IOC控制反转，就是将对象的控制权交给Spring的IOC容器，由IOC容器创建及管理对象。IOC容器创建的对象称为bean对象。

在之前的入门案例中，要把某个对象交给IOC容器管理，需要在类上添加一个注解：@Component 

而Spring框架为了更好的标识web应用程序开发当中，**bean对象到底归属于哪一层，又提供了@Component的衍生注解：**

- @Controller    （标注在Controller类上）
- @Service          （标注在Service类上）
- @Repository    （标注在Dao类上）



例如：

- **Controller层：**这里@RestController是组合注解 已经包括了@controller 所以不用再多标注

~~~java
@RestController  //@RestController = @Controller + @ResponseBody
public class EmpController {

    @Autowired //运行时,从IOC容器中获取该类型对象,赋值给该变量
    private EmpService empService ;

    @RequestMapping("/listEmp")
    public Result list(){
        //1. 调用service, 获取数据
        List<Emp> empList = empService.listEmp();
        //3. 响应数据
        return Result.success(empList);
    }
}
~~~

- **Service层：**这个替换成了@service  而不是@component

~~~java
@Service
public class EmpServiceA implements EmpService {

    @Autowired //运行时,从IOC容器中获取该类型对象,赋值给该变量
    private EmpDao empDao ;

    @Override
    public List<Emp> listEmp() {
     
        });
        return empList;
    }
}
~~~

**Dao层：**

~~~java
@Repository
public class EmpDaoA implements EmpDao {
    @Override
    public List<Emp> listEmp() {
        //1. 加载并解析emp.xml
        String file = this.getClass().getClassLoader().getResource("emp.xml").getFile();
        System.out.println(file);
        List<Emp> empList = XmlParserUtils.parse(file, Emp.class);
        return empList;
    }
}
~~~



要把某个对象交给IOC容器管理，需要在对应的类上加上如下注解之一：

| 注解        | 说明                 | 位置                                            |
| :---------- | -------------------- | ----------------------------------------------- |
| @Controller | @Component的衍生注解 | 标注在控制器类上                                |
| @Service    | @Component的衍生注解 | 标注在业务类上                                  |
| @Repository | @Component的衍生注解 | 标注在数据访问类上（由于与mybatis整合，用的少） |
| @Component  | 声明bean的基础注解   | 不属于以上三类时，用此注解                      |



在IOC容器中，**每一个Bean都有一个属于自己的名字**，可以通过注解的value属性指定bean的名字。**如果没有指定，默认为类名首字母小写**。

![10-7](picture/10-7.png)

###### 4.4.2 组件扫描

问题：使用前面学习的四个注解声明的bean，一定会生效吗？

答案：**不一定。（原因：bean想要生效，还需要被组件扫描）**

 下面我们通过修改项目工程的目录结构，来测试bean对象是否生效：

![10-8](picture/10-8.png)

这样肯定是不行的，得按照springboot规定的结构来。

但是，从底层的角度，也可以让他能够扫描到

@ComponentScan注解虽然没有显式配置，但是实际上已经包含在了引导类声明注解 @SpringBootApplication 中，==**默认扫描的范围是SpringBoot启动类所在包及其子包**==。

![10-9](picture/10-9.png)



**解决方案：手动添加@ComponentScan注解，指定要扫描的包   （==仅做了解，不推荐==）**

![10-10](picture/10-10.png)



##### 4.5 DI依赖注入详解 @Autowired

上一小节我们讲解了控制反转IOC的细节，接下来呢，我们学习依赖注解DI的细节。

依赖注入，是指IOC容器要为应用程序去提供运行时所依赖的资源，而资源指的就是对象。

在入门程序案例中，我们使用了@Autowired这个注解，完成了依赖注入的操作，而这个Autowired翻译过来叫：自动装配。

@Autowired注解，默认是按照**类型**进行自动装配的（去IOC容器中找某个类型的对象，然后完成注入操作）

> 入门程序举例：在EmpController运行的时候，就要到IOC容器当中去查找EmpService这个类型的对象，而我们的IOC容器中刚好有一个EmpService这个类型的对象，所以就找到了这个类型的对象完成注入操作。



那如果在IOC容器中，存在多个相同类型的bean对象，会出现什么情况呢？

![10-11](picture/10-11.png)

显然，他就不知道应该注入哪一个bean了

如何解决上述问题呢？Spring提供了以下几种解决方案：

- @Primary

- @Qualifier

- @Resource

![10-12](picture/10-12.PNG)

> 面试题 ： @Autowird 与 @Resource的区别
>
> - **@Autowired 是spring框架提供的注解，而@Resource是JDK提供的注解**
> - @Autowired 默认是按照类型注入，而@Resource是按照名称注入







##  													MySQL数据库介绍

#### 1 数据库和Mysql概述

像我们日常访问的电商网站京东，企业内部的管理系统OA、ERP、CRM这类的系统数据其实都是存储在数据库中的。

**最终这些数据，只是在浏览器或app中展示出来而已，最终数据的存储和管理都是数据库负责的**

后端负责从数据库中取数据，而真正操控数据就是通过Mysql来实现

#### 2 mysql安装和登录

安装就是解压和配置环境变量 略

##### 2.1 初始化数据库

==以管理员身份，运行命令行窗口：==

在刚才的命令行中，输入如下的指令： 

```
mysqld --initialize-insecure
```

然后注册mysql服务

```
mysqld -install
```

现在你的计算机上已经安装好了MySQL服务了。

##### 2.2 启动mysql服务

在黑框里敲入`net start mysql`，回车。

```java
net start mysql  // 启动mysql服务
    
net stop mysql  // 停止mysql服务
```

##### 2.3 修改密码

在黑框里敲入`mysqladmin -u root password 1234`，这里的`1234`就是指默认管理员(即root账户)的密码，可以自行修改成你喜欢的。

```
mysqladmin -u root password 1234
```

然后登录

```
mysql -uroot -p1234

mysql -u用户名 -p密码 -h要连接的mysql服务器的ip地址(默认127.0.0.1) -P端口号(默认3306)
```

#####  2.4 具体使用

```
net start mysql  // 启动mysql服务

mysql -uroot -p1234 //登录

//我的指令

exit

net stop mysql  // 停止mysql服务
```

#### 3. mysql分类

SQL语句根据其功能被分为四大类：DDL、DML、DQL、DCL 

| **分类** | **全称**                    | **说明**                                                   |
| -------- | --------------------------- | ---------------------------------------------------------- |
| DDL      | Data Definition  Language   | 数据定义语言，用来**定义数据库对象(数据库，表，字段)**     |
| DML      | Data Manipulation  Language | 数据操作语言，用来对数据库表中的数据进行**增删改**         |
| DQL      | Data Query Language         | 数据查询语言，用来**查询**数据库中表的记录                 |
| DCL      | Data Control  Language      | 数据控制语言，用来**创建数据库用户、控制数据库的访问权限** |

![11-1](picture/11-1.png)  

#### 4. ！！！数据库设计！！！

在学习之前先给大家介绍一下我们要开发一个项目，整个开发流程是什么样的，以及在流程当中哪些环节会涉及到数据库。

需求文档：

- 在我们开发一个项目或者项目当中的某个模块之前，会先会拿到产品经理给我们提供的页面原型及需求文档。

![11-2](picture/11-2.png)

1 设计：

- 拿到产品原型和需求文档之后，我们首先要做的不是编码，而是要**先进行项目的设计**，其中就包括概要设计、详细设计、接口设计、数据库设计等等。
- 数据库设计根据产品原型以及需求文档，要**分析各个模块涉及到的表结构以及表结构之间的关系**，以及表结构的详细信息。最终我们需要将数据库以及数据库当中的表结构设计创建出来。

2 开发/测试：

- 参照页面原型和需求进行编码，实现业务功能。在这个过程当中，我们就需要来操作设计出来的数据库表结构，来完成业务的增删改查操作等。

3 部署上线：

- 在项目的功能开发测试完成之后，项目就可以上线运行了，后期如果项目遇到性能瓶颈，还需要对项目进行优化。优化很重要的一个部分就是数据库的优化，包括数据库当中索引的建立、SQL 的优化、分库分表等操作。



在上述的流程当中，针对于数据库来说，主要包括三个阶段：

1. 数据库设计阶段
   - 参照页面原型以及需求文档**定义数据库**
2. 数据库操作阶段
   - 根据业务功能的实现，**编写SQL语句对数据表中的数据进行增删改查操作**
3. 数据库优化阶段
   - 通过数据库的优化来提高数据库的访问性能。优化手段：索引、SQL优化、分库分表等







#### 5. 数据库操作--DDL

**1.查询所有数据库：**

```mysql
show databases;
```

**查询当前数据库：**

```mysql
select database();
```



> 我们要操作某一个数据库，必须要切换到对应的数据库中。 
>
> 通过指令：select  database() ，就可以查询到当前所处的数据库 



**2.创建数据库**

```mysql
create database [ if not exists ] 数据库名;
```

案例： 创建一个itcast数据库。

```mysql
create database itcast;
```

**3.使用数据库**

```mysql
use 数据库名 ;
```

> 我们要操作某一个数据库下的表时，就需要通过该指令，切换到对应的数据库下，否则不能操作。

案例：切换到itcast数据

```mysql
use itcast;
```



**4.删除数据库：**

```mysql
drop database [ if exists ] 数据库名 ;
```

> 如果删除一个不存在的数据库，将会报错。
>
> 可以加上参数 if exists ，如果数据库存在，再执行删除，否则不执行删除。

案例：删除itcast数据库

~~~mysql
drop database if exists itcast; -- itcast数据库存在时删除
~~~





#### 6. 数据库图形化工具---idea

如何用idea连接数据库并且输入sql语句  没必要记笔记

有需要就去百度吧 就这么回事 很简单

![11-3](picture/11-3.png)





## 															mysql实际操作

#### 1.  创建表结构

直接给出代码

```mysql
create table  表名(
	字段1  字段1类型 [约束]  [comment  字段1注释 ],
	字段2  字段2类型 [约束]  [comment  字段2注释 ],
	......
	字段n  字段n类型 [约束]  [comment  字段n注释 ] 
) [ comment  表注释 ] ;



create table tb_user (
                         id int comment 'ID,唯一标识',   # id是一行数据的唯一标识（不能重复）
                         username varchar(20) comment '用户名',
                         name varchar(10) comment '姓名',
                         age int comment '年龄',
                         gender char(1) comment '性别'
) comment '用户表';
```

创建完直接在可视化界面加数据就行  +添加 箭头提交



![11-4](picture/11-4.PNG)

**约束**

概念：所谓约束就是作用在表中字段上的规则，用于限制存储在表中的数据。



在MySQL数据库当中，提供了以下5种约束：

| **约束** | **描述**                                         | **关键字**  |
| -------- | ------------------------------------------------ | ----------- |
| 非空约束 | 限制该字段值不能为null                           | not null    |
| 唯一约束 | 保证字段的所有数据都是唯一、不重复的             | unique      |
| 主键约束 | 主键是一行数据的唯一标识，要求非空且唯一         | primary key |
| 默认约束 | 保存数据时，如果未指定该字段值，则采用默认值     | default     |
| 外键约束 | 让两张表的数据建立连接，保证数据的一致性和完整性 | foreign key |

> 注意：约束是作用于表中字段上的，可以在创建表/修改表的时候添加约束。
>
> 在上述的表结构中:
>
> - id 是一行数据的唯一标识
>
> - username 用户名字段是非空且唯一的
>
> - name 姓名字段是不允许存储空值的
>
> - gender 性别字段是有默认值，默认为男



- 建表语句： 

```mysql
create table tb_user (
    id int primary key comment 'ID,唯一标识', 
    username varchar(20) not null unique comment '用户名',
    name varchar(10) not null comment '姓名',
    age int comment '年龄',
    gender char(1) default '男' comment '性别'
) comment '用户表';
```

#### 2. 直接上案例 创建表结构

这个表结构是通过idea可视化实现的 但是直接用代码也不复杂

都有个了解即可

```mysql
create table tb_emp
(
    id          int auto_increment comment '主键ID'
        primary key,
    username    varchar(25)                  not null comment '用户名',
    password    varchar(32) default '123456' null comment '密码',
    name        varchar(10)                  not null comment '姓名',
    gender      tinyint unsigned             not null comment '性别',
    image       varchar(300)                 null comment '图像url',
    job         tinyint unsigned             null comment '职位,1班主任2讲师3工程师',
    entrydate   date                         null comment '入职日期',
    create_time datetime                     not null comment '创建时间',
    update_time datetime                     not null comment '修改时间',
    constraint tb_emp_pk_2
        unique (username)
)
    comment '员工表'
    

给这个表加新的字段：
ALTER TABLE tb_emp
ADD COLUMN email VARCHAR(50) NULL COMMENT '电子邮件';

```



> 说明：
>
> - create_time：记录的是当前这条数据插入的时间。 
>
> - update_time：记录当前这条数据最后更新的时间。



#### 3.  查询和修改表结构（都很简单 现场查）

一般都直接用图形化工具查询的 下面还是给出语句

图形化太简单了 真不需要记



**1.查询指定表的建表语句**

```mysql
show create table 表名 ;
```

**2.添加字段**

```sql
alter table 表名 add  字段名  类型(长度)  [comment 注释]  [约束];
```

案例： 为tb_emp表添加字段qq，字段类型为 varchar(11)

```sql
alter table tb_emp add  qq  varchar(11) comment 'QQ号码';
```





**3.修改数据类型**

```mysql
alter table 表名 modify  字段名  新数据类型(长度);
alter table 表名 change  旧字段名  新字段名  类型(长度)  [comment 注释]  [约束];
```

案例：修改qq字段的字段类型，将其长度由11修改为13   修改qq字段名为 qq_num，字段类型varchar(13)

```sql
alter table tb_emp modify qq varchar(13) comment 'QQ号码';
alter table tb_emp change qq qq_num varchar(13) comment 'QQ号码';
```

**4.删除字段**

```sql
alter table 表名 drop 字段名;
```

案例：删除tb_emp表中的qq_num字段

```sql
alter table tb_emp drop qq_num;
```



**5.修改表名**

```sql
rename table 表名 to  新表名;
```





#### 4. 添加数据DML

DML英文全称是Data Manipulation Language(数据操作语言)，用来**对数据库中表的数据记录进行增、删、改**操作。

- 添加数据（INSERT）
- 修改数据（UPDATE）
- 删除数据（DELETE） 

##### 4.1 添加数据 Insert

案例1：向tb_emp表的username、name、gender字段插入数据

~~~mysql
-- 因为设计表时create_time, update_time两个字段不能为NULL，所以也做为要插入的字段
insert into tb_emp(username, name, gender, create_time, update_time)
values ('wuji', '张无忌', 1, now(), now());
~~~

案例2：向tb_emp表的所有字段插入数据

~~~mysql
insert into tb_emp(id, username, password, name, gender, image, job, entrydate, create_time, update_time)
values (null, 'zhirou', '123', '周芷若', 2, '1.jpg', 1, '2010-01-01', now(), now());
~~~

案例3：批量向tb_emp表的username、name、gender字段插入数据

~~~mysql
insert into tb_emp(username, name, gender, create_time, update_time)
values ('weifuwang', '韦一笑', 1, now(), now()),
       ('fengzi', '张三疯', 1, now(), now());
~~~



图形化直接鼠标点就行了

##### 4.2 修改数据 update

update语法：

```sql
update 表名 set 字段名1 = 值1 , 字段名2 = 值2 , .... [where 条件] ;
```

案例1：将tb_emp表中id为1的员工，姓名name字段更新为'张三'

```sql
update tb_emp set name='张三',update_time=now() where id=1;
```

案例2：将tb_emp表的所有员工入职日期更新为'2010-01-01'

```sql
update tb_emp set entrydate='2010-01-01',update_time=now();
```

![11-5](picture/11-5.png)

> 注意事项:
>
> 1. 修改语句的条件可以有，也可以没有，如果没有条件，则会修改整张表的所有数据。
>
> 2. 在修改数据时，一般需要同时修改公共字段update_time，将其修改为当前操作时间。



##### 4.3 删除数据 Delete

delete语法：

```SQL
delete from 表名  [where  条件] ;
```

案例1：删除tb_emp表中id为1的员工

```sql
delete from tb_emp where id = 1;
```

案例2：删除tb_emp表中所有员工

```sql
delete from tb_emp;
```

> 注意事项:
>
> ​	• DELETE 语句的条件可以有，也可以没有，如果没有条件，则会删除整张表的所有数据。
>
> ​	• DELETE 语句不能删除某一个字段的值(可以使用UPDATE，将该字段值置为NULL即可)。
>
> ​	• 当进行删除全部数据操作时，会提示询问是否确认删除所有数据，直接点击Execute即可。



#### 5. 数据查询操作 DQL（看见下面的能想起来分别是什么意思就行）

##### 5.1 语法

DQL查询语句，语法结构如下：

```mysql
SELECT
	字段列表
FROM
	表名列表
WHERE
	条件列表
GROUP  BY
	分组字段列表
HAVING
	分组后条件列表
ORDER BY
	排序字段列表
LIMIT
	分页参数
```

将上面的完整语法拆分为以下几个部分学习：

- 基本查询（不带任何条件）
- 条件查询（where）
- 分组查询（group by）
- 排序查询（order by）
- 分页查询（limit）

##### 5.2 基本查询 （不带任何条件）

直接省略 给出两个稍复杂点的

- 设置别名

  ~~~mysql
  select 字段1 [ as 别名1 ] , 字段2 [ as 别名2 ]  from  表名;
  ~~~

  ![12-1](picture/12-1.png)

- 去除重复记录

  ~~~mysql
  select distinct 字段列表 from  表名;
  ~~~



##### 5.3 条件查询

**语法：**

```sql
select  字段列表  from   表名   where   条件列表 ; -- 条件列表：意味着可以有多个条件
```

学习条件查询就是学习条件的构建方式，而在SQL语句当中构造条件的运算符分为两类：

- 比较运算符
- 逻辑运算符

常用的比较运算符如下: 

| **比较运算符**       | **功能**                                 |
| -------------------- | ---------------------------------------- |
| >                    | 大于                                     |
| >=                   | 大于等于                                 |
| <                    | 小于                                     |
| <=                   | 小于等于                                 |
| =                    | 等于                                     |
| <> 或 !=             | 不等于                                   |
| between ...  and ... | 在某个范围之内(含最小、最大值)           |
| in(...)              | 在in之后的列表中的值，多选一             |
| like 占位符          | 模糊匹配(_匹配单个字符, %匹配任意个字符) |
| is null              | 是null                                   |

常用的逻辑运算符如下:

| **逻辑运算符** | **功能**                    |
| -------------- | --------------------------- |
| and 或 &&      | 并且 (多个条件同时成立)     |
| or 或 \|\|     | 或者 (多个条件任意一个成立) |
| not 或 !       | 非 , 不是                   |

案例2：查询 有分配职位 的员工信息

~~~mysql
select id, username, password, name, gender, image, job, entrydate, create_time, update_time
from tb_emp
where job is not null ;
~~~

案例6：查询 入职日期 在 '2000-01-01' (包含) 到 '2010-01-01'(包含) 之间的员工信息

~~~mysql
-- 方式1：
select id, username, password, name, gender, image, job, entrydate, create_time, update_time
from tb_emp
where entrydate>='2000-01-01' and entrydate<='2010-01-01';
-- 方式2： between...and
select id, username, password, name, gender, image, job, entrydate, create_time, update_time
from tb_emp
where entrydate between '2000-01-01' and '2010-01-01';
~~~

案例8：查询 职位是 2 (讲师), 3 (学工主管), 4 (教研主管) 的员工信息

~~~mysql
-- 方式1：使用or连接多个条件
select id, username, password, name, gender, image, job, entrydate, create_time, update_time
from tb_emp
where job=2 or job=3 or job=4;
-- 方式2：in关键字
select id, username, password, name, gender, image, job, entrydate, create_time, update_time
from tb_emp
where job in (2,3,4);
~~~



案例10：查询 姓 '张' 的员工信息

~~~mysql
select id, username, password, name, gender, image, job, entrydate, create_time, update_time
from tb_emp
where name like '张%'; # 通配符 "%" 代表任意个字符（0个 ~ 多个）
~~~







##### 5.4 聚合函数

之前我们做的查询都是横向查询，就是根据条件一行一行的进行判断，而使用聚合函数查询就是纵向查询，它是对一列的值进行计算，然后返回一个结果值。（将一列数据作为一个整体，进行纵向计算）

语法：

~~~mysql
select  聚合函数(字段列表)  from  表名 ;
~~~

> 注意 : 聚合函数会忽略空值，对NULL值不作为统计。



常用聚合函数：

| **函数** | **功能** |
| -------- | -------- |
| count    | 统计数量 |
| max      | 最大值   |
| min      | 最小值   |
| avg      | 平均值   |
| sum      | 求和     |

> count ：按照列去统计有多少行数据。
>
> - 在根据指定的列统计的时候，如果这一列中有null的行，该行不会被统计在其中。
>
> sum ：计算指定列的数值和，如果不是数值类型，那么计算结果为0
>
> max ：计算指定列的最大值
>
> min ：计算指定列的最小值
>
> avg ：计算指定列的平均值











案例1：统计该企业员工数量

~~~mysql
# count(字段)
select count(id) from tb_emp;-- 结果：29
select count(job) from tb_emp;-- 结果：28 （聚合函数对NULL值不做计算）

# count(常量)
select count(0) from tb_emp;
select count('A') from tb_emp;

# count(*)  推荐此写法（MySQL底层进行了优化）
select count(*) from tb_emp;
~~~



案例2：统计该企业最早入职的员工

~~~mysql
select min(entrydate) from tb_emp;
~~~





案例4：统计该企业员工 ID 的平均值

~~~mysql
select avg(id) from tb_emp;
~~~



##### 5.5 分组查询

分组： 按照某一列或者某几列，把相同的数据进行合并输出。

> 分组其实就是按列进行分类(指定列下相同的数据归为一类)，然后可以对分类完的数据进行合并计算。
>
> 分组查询通常会使用聚合函数进行计算。

语法：

~~~mysql
select  字段列表  from  表名  [where 条件]  group by 分组字段名  [having 分组后过滤条件];
~~~

案例1：根据性别分组 , 统计男性和女性员工的数量

~~~mysql
select gender, count(*)
from tb_emp
group by gender; -- 按照gender字段进行分组（gender字段下相同的数据归为一组）
~~~



案例2：查询入职时间在 '2015-01-01' (包含) 以前的员工 , 并对结果根据职位分组 , 获取员工数量大于等于2的职位

~~~mysql
select job, count(*)
from tb_emp
where entrydate <= '2015-01-01'   -- 分组前条件
group by job                      -- 按照job字段分组
having count(*) >= 2;             -- 分组后条件
~~~

![12-2](picture/12-2.png)

> 注意事项:
>
> ​	• 分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义
>
> ​	• 执行顺序：where > 聚合函数 > having 



**where与having区别（面试题）**

- 执行时机不同：where是分组之前进行过滤，不满足where条件，不参与分组；而having是分组之后对结果进行过滤。
- 判断条件不同：where不能对聚合函数进行判断，而having可以。





##### 5.6 排序查询

语法：

```mysql
select  字段列表  
from   表名   
[where  条件列表] 
[group by  分组字段 ] 
order  by  字段1  排序方式1 , 字段2  排序方式2 … ;
```

- 排序方式：

  - ASC ：升序（默认值）

  - DESC：降序

案例3：根据入职时间对公司的员工进行升序排序，入职时间相同，再按照更新时间进行降序排序

~~~mysql
select id, username, password, name, gender, image, job, entrydate, create_time, update_time
from tb_emp
order by entrydate ASC , update_time DESC;
~~~



##### 5.7 分页查询

这个很简单了

分页查询语法：

```sql
select  字段列表  from   表名  limit  起始索引, 查询记录数 ;
```

案例4：查询 第3页 员工数据, 每页展示5条记录

~~~mysql
select id, username, password, name, gender, image, job, entrydate, create_time, update_time
from tb_emp
limit 10 , 5; -- 从索引10开始，向后取5条记录
~~~

![12-3](picture/12-3.png)

> 注意事项:
>
> 1. 起始索引从0开始。    计算公式 ：   **起始索引 = （查询页码 - 1）* 每页显示记录数**
>
> 2. 分页查询是数据库的方言，不同的数据库有不同的实现，MySQL中是LIMIT
>
> 3. 如果查询的是第一页数据，起始索引可以省略，直接简写为 limit  条数



##### 5.8 实际案例一

> 分析：根据输入的条件，查询第1页数据
>
> 1. 在员工管理的列表上方有一些查询条件：员工姓名、员工性别，员工入职时间(开始时间~结束时间)
>    - 姓名：张
>    - 性别：男
>    - 入职时间：2000-01-01  ~  2015-12-31
>
> 2. 除了查询条件外，在列表的下面还有一个分页条，这就涉及到了分页查询
>    - 查询第1页数据（每页显示10条数据）
> 3. 基于查询的结果，按照修改时间进行降序排序
>
> 结论：条件查询 + 分页查询 + 排序查询

SQL语句代码：

~~~mysql
-- 根据输入条件查询第1页数据（每页展示10条记录）
-- 输入条件：
   -- 姓名：张 （模糊查询）
   -- 性别：男
   -- 入职时间：2000-01-01 ~ 2015-12-31
-- 分页： 0 , 10
-- 排序： 修改时间  DESC
select id, username, password, name, gender, image, job, entrydate, create_time, update_time
from tb_emp
where name like '张%' and gender = 1 and entrydate between '2000-01-01' and '2015-12-31'
order by update_time desc
limit 0 , 10;
~~~

![12-4](picture/12-4.png)

##### 5.9 实际案例二

![12-5](picture/12-5.png)

这里的数量显然是用了count

员工性别统计：

~~~mysql
select gender AS 性别, count(*) AS 人数
from tb_emp
group by gender;


-- if(条件表达式, true取值 , false取值) 这个是让性别里的1和2是中文 不影响逻辑
select if(gender=1,'男性员工','女性员工') AS 性别, count(*) AS 人数
from tb_emp
group by gender;
~~~

员工职位统计：（同理 这里的case只是让工作显示为对应汉字  不影响逻辑）

~~~mysql
-- case 表达式 when 值1 then 结果1  when 值2  then  结果2 ...  else  result  end
select (case job
             when 1 then '班主任'
             when 2 then '讲师'
             when 3 then '学工主管'
             when 4 then '教研主管'
             else '未分配职位'
        end) AS 职位 ,
       count(*) AS 人数
from tb_emp
group by job;
~~~

![12-6](picture/12-6.png)





#### 6. 多表结构

项目开发中，在进行数据库表结构设计时，会根据业务需求及业务模块之间的关系，分析并设计表结构，由于业务之间相互关联，所以各个表结构之间也存在着各种联系，基本上分为三种：

- **一对多(多对一)**

- **多对多**

- **一对一**

例如，每个员工都有所属部门 

![12-7](picture/12-7.png)

> **一对多关系实现：在数据库表中多的一方，添加字段，来关联属于一这方的主键。**





##### 6.1 一对多

###### 问题：

如果我把部门1删了  但属于部门1的员工 其数据行还是会显示部门1    **就出现数据的不完整、不一致了。** 

###### 解决：引入外键约束

> 外键约束：让两张表的数据建立连接，保证数据的一致性和完整性。  
>
> 对应的关键字：foreign key

外键约束的语法：

```mysql
-- 创建表时指定
create table 表名(
	字段名    数据类型,
	...
	[constraint]   [外键名称]  foreign  key (外键字段名)   references   主表 (主表列名)	
);


-- 建完表后，添加外键
alter table  表名  add constraint  外键名称  foreign key(外键字段名) references 主表(主表列名);
```

那接下来，我们就为员工表的dept_id 建立外键约束，来关联部门表的主键。



方式1：通过SQL语句操作

```mysql
create table tb_dept
(
    id int unsigned primary key auto_increment comment '主键ID',
    name varchar(10) not null unique  comment '部门名称',
    create_time datetime not null comment '创建时间',
    update_time datetime not null comment '修改时间'
) comment '部门表';

-- 员工表
create table tb_emp
(
    id          int unsigned primary key auto_increment comment 'ID',
    username    varchar(20)      not null unique comment '用户名',
    password    varchar(32) default '123456' comment '密码',
    name        varchar(10)      not null comment '姓名',
    gender      tinyint unsigned not null comment '性别, 说明: 1 男, 2 女',
    image       varchar(300) comment '图像',
    job         tinyint unsigned comment '职位, 说明: 1 班主任,2 讲师, 3 学工主管, 4 教研主管',
    entrydate   date comment '入职时间',
    
    dept_id     int unsigned comment '部门ID', -- 员工的归属部门
    
    create_time datetime         not null comment '创建时间',
    update_time datetime         not null comment '修改时间'
) comment '员工表';

-- 修改表： 添加外键约束
alter table tb_emp  
add  constraint  fk_dept_id  foreign key (dept_id)  references  tb_dept(id);
```



方式2：图形化界面操作

这个略了 有需要自己查



> 当我们添加外键约束时，我们得保证当前数据库表中的数据是完整的。 所以，我们需要将之前删除掉的数据再添加回来。

> 当我们添加了外键之后，再删除ID为1的部门，就会发现，**此时数据库报错了，不允许删除。**



**物理外键和逻辑外键**

- 物理外键
  - 概念：使用foreign key定义外键关联另外一张表。
  - 缺点：
    - 影响增、删、改的效率（需要检查外键关系）。
    - 仅用于单节点数据库，不适用与分布式、集群场景。
    - 容易引发数据库的死锁问题，消耗性能。

- 逻辑外键
  - 概念：在**业务层逻辑（service层）中，解决外键关联**。
  - 通过逻辑外键，就可以很方便的解决上述问题。

> **在现在的企业开发中，很少会使用物理外键，都是使用逻辑外键。 甚至在一些数据库开发规范中，会明确指出禁止使用物理外键 foreign key **



##### 6.2 一对一

一对一关系表在实际开发中应用起来比较简单，通常是用来做单表的拆分，也就是将一张大表拆分成两张小表，将大表中的一些基础字段放在一张表当中，将其他的字段放在另外一张表当中，以此来提高数据的操作效率。

> 一对一的应用场景： 用户表(基本信息+身份信息)
>
> ![12-8](picture/12-8.png)
>
> - 基本信息：用户的ID、姓名、性别、手机号、学历
> - 身份信息：民族、生日、身份证号、身份证签发机关，身份证的有效期(开始时间、结束时间)
>
> 如果在业务系统当中，对用户的基本信息查询频率特别的高，但是对于用户的身份信息查询频率很低，此时出于提高查询效率的考虑，我就可以将这张大表拆分成两张小表，第一张表存放的是用户的基本信息，而第二张表存放的就是用户的身份信息。他们两者之间一对一的关系，一个用户只能对应一个身份证，而一个身份证也只能关联一个用户。

那么在数据库层面怎么去体现上述两者之间是一对一的关系呢？

其实一对一我们可以看成一种特殊的一对多。一对多我们是怎么设计表关系的？是不是在多的一方添加外键。同样我们也可以通过外键来体现一对一之间的关系，我们只需要在任意一方来添加一个外键就可以了。

![12-9](picture/12-9.png)

> 一对一 ：**在任意一方加入外键，关联另外一方的主键，并且设置外键为唯一的(UNIQUE)**



SQL脚本：

~~~mysql
-- 用户基本信息表
create table tb_user(
    id int unsigned  primary key auto_increment comment 'ID',
    name varchar(10) not null comment '姓名',
    gender tinyint unsigned not null comment '性别, 1 男  2 女',
    phone char(11) comment '手机号',
    degree varchar(10) comment '学历'
) comment '用户基本信息表';
-- 测试数据
insert into tb_user values (1,'白眉鹰王',1,'18812340001','初中'),
                        (2,'青翼蝠王',1,'18812340002','大专'),
                        (3,'金毛狮王',1,'18812340003','初中'),
                        (4,'紫衫龙王',2,'18812340004','硕士');

-- 用户身份信息表
create table tb_user_card(
    id int unsigned  primary key auto_increment comment 'ID',
    nationality varchar(10) not null comment '民族',
    birthday date not null comment '生日',
    idcard char(18) not null comment '身份证号',
    issued varchar(20) not null comment '签发机关',
    expire_begin date not null comment '有效期限-开始',
    expire_end date comment '有效期限-结束',
    user_id int unsigned not null unique comment '用户ID',
    constraint fk_user_id foreign key (user_id) references tb_user(id)
) comment '用户身份信息表';
-- 测试数据
insert into tb_user_card values (1,'汉','1960-11-06','100000100000100001','朝阳区公安局','2000-06-10',null,1),
        (2,'汉','1971-11-06','100000100000100002','静安区公安局','2005-06-10','2025-06-10',2),
        (3,'汉','1963-11-06','100000100000100003','昌平区公安局','2006-06-10',null,3),
        (4,'回','1980-11-06','100000100000100004','海淀区公安局','2008-06-10','2028-06-10',4);
~~~



##### 6.3 多对多

多对多的关系在开发中属于也比较常见的。比如：学生和老师的关系，一个学生可以有多个授课老师，一个授课老师也可以有多个学生。在比如：学生和课程的关系，一个学生可以选修多门课程，一个课程也可以供多个学生选修。

案例：学生与课程的关系

- 关系：一个学生可以选修多门课程，一门课程也可以供多个学生选择

- 实现关系：**建立第三张中间表，中间表至少包含两个外键，分别关联两方主键**



解释：这里的学生表和课程表只存储了单方面的信息 没有联系

而第三章表中，**主键是额外的id 用于存储学生和课程之间的键值对** 因此 才能实现多对多 而不出现主键重复的问题！

![12-10](picture/12-10.png)

SQL脚本：没什么看的 ，**建立了两个外键而已 重点在于解决思路**

~~~mysql
-- 学生表
create table tb_student(
    id int auto_increment primary key comment '主键ID',
    name varchar(10) comment '姓名',
    no varchar(10) comment '学号'
) comment '学生表';
-- 学生表测试数据
insert into tb_student(name, no) values ('黛绮丝', '2000100101'),('谢逊', '2000100102'),('殷天正', '2000100103'),('韦一笑', '2000100104');

-- 课程表
create table tb_course(
   id int auto_increment primary key comment '主键ID',
   name varchar(10) comment '课程名称'
) comment '课程表';
-- 课程表测试数据
insert into tb_course (name) values ('Java'), ('PHP'), ('MySQL') , ('Hadoop');

-- 学生课程表（中间表）
create table tb_student_course(
   id int auto_increment comment '主键' primary key,
   student_id int not null comment '学生ID',
   course_id  int not null comment '课程ID',
   constraint fk_courseid foreign key (course_id) references tb_course (id),
   constraint fk_studentid foreign key (student_id) references tb_student (id)
)comment '学生课程中间表';
-- 学生课程表测试数据
insert into tb_student_course(student_id, course_id) values (1,1),(1,2),(1,3),(2,2),(2,3),(3,4);
~~~

##### 6.4 实际案例--苍穹外卖管理后台

![12-11](picture/12-11.png)



能看懂就可以了  实际代码没有建立物理外键 都是逻辑外键



所以 这里只是建了表格而已。。





#### 7. 多表查询

多表查询：查询时从多张表中获取所需数据

> 单表查询的SQL语句：select  字段列表  from  表名;
>
> 那么要执行多表查询，只需要使用逗号分隔多张表即可，如： select   字段列表  from  表1, 表2;

查询用户表和部门表中的数据：

~~~mysql
select * from  tb_emp , tb_dept;
~~~

但是 这样的结果是**笛卡尔积**  重复的匹配太多了

> 在多表查询时，需要消除无效的笛卡尔积，只保留表关联部分的数据

![13-1](picture/13-1.png)

在SQL语句中，如何去除无效的笛卡尔积呢？**只需要给多表查询加上连接查询的条件即可。**

~~~mysql
select * from tb_emp , tb_dept where tb_emp.dept_id = tb_dept.id ;
~~~

![13-2](picture/13-2.png)

> 由于id为17的员工，没有dept_id字段值，所以在多表查询时，根据连接查询的条件并没有查询到。





多表查询可以分为：

1. 连接查询

   - 内连接：相当于查询A、B交集部分数据

   ![image-20221207165446062](../BaiduNetdiskDownload/day08-MySQL-Mybatis入门/day08-MySQL-Mybatis入门/讲义/01-MySQL/assets/image-20221207165446062.png) 

2. 外连接

   - 左外连接：查询左表所有数据(包括两张表交集部分数据)

   - 右外连接：查询右表所有数据(包括两张表交集部分数据)

3. 子查询

##### 7.1 内连接

内连接查询：查询两表或多表中交集部分数据。

内连接从语法上可以分为：

- 隐式内连接

- 显式内连接

隐式内连接语法：

``` mysql
select  字段列表   from   表1 , 表2   where  条件 ... ;
```

显式内连接语法：

``` mysql
select  字段列表   from   表1  [ inner ]  join 表2  on  连接条件 ... ;
```



案例：查询员工的姓名及所属的部门名称

- 隐式内连接实现

~~~mysql
select tb_emp.name , tb_dept.name -- 分别查询两张表中的数据
from tb_emp , tb_dept -- 关联两张表
where tb_emp.dept_id = tb_dept.id; -- 消除笛卡尔积
~~~

- 显式内连接实现

~~~mysql
select tb_emp.name , tb_dept.name
from tb_emp inner join tb_dept
on tb_emp.dept_id = tb_dept.id;
~~~



##### 7.2 外连接

外连接分为两种：左外连接 和 右外连接。

左外连接语法结构：

```mysql
select  字段列表   from   表1  left  [ outer ]  join 表2  on  连接条件 ... ;
```

> 左外连接相当于查询表1(左表)的所有数据，当然也包含表1和表2交集部分的数据。

右外连接语法结构：

```mysql
select  字段列表   from   表1  right  [ outer ]  join 表2  on  连接条件 ... ;
```

> 右外连接相当于查询表2(右表)的所有数据，当然也包含表1和表2交集部分的数据。



案例：查询员工表中所有员工的姓名, 和对应的部门名称

~~~mysql
-- 左外连接：以left join关键字左边的表为主表，查询主表中所有数据，以及和主表匹配的右边表中的数据
select emp.name , dept.name
from tb_emp AS emp left join tb_dept AS dept 
     on emp.dept_id = dept.id;
~~~

![13-3](picture/13-3.PNG)

案例：查询部门表中所有部门的名称, 和对应的员工名称 

~~~mysql
-- 右外连接
select dept.name , emp.name
from tb_emp AS emp right join  tb_dept AS dept
     on emp.dept_id = dept.id;
~~~

![13-4](picture/13-4.PNG)



> 注意事项：
>
> 左外连接和右外连接是可以相互替换的，只需要调整连接查询时SQL语句中表的先后顺序就可以了。而我们在日常开发使用时，更偏向于左外连接。



##### 7.3 子查询

SQL语句中嵌套select语句，称为嵌套查询，又称子查询。

###### 7.3.1 标量子查询（=,<,>）

**子查询返回的结果是单个值(数字、字符串、日期等)，最简单的形式**，这种子查询称为标量子查询。

常用的操作符： =   <>   >    >=    <   <=   

```sql
SELECT  *  FROM   t1   WHERE  column1 =  ( SELECT  column1  FROM  t2 ... );
```

> 子查询外部的语句可以是insert / update / delete / select 的任何一个，最常见的是 select。

案例2：查询在 "方东白" 入职之后的员工信息

> 可以将需求分解为两步：
>
> 1. 查询 方东白 的入职日期
> 2. 查询 指定入职日期之后入职的员工信息

```mysql
-- 1.查询"方东白"的入职日期
select entrydate from tb_emp where name = '方东白';     #查询结果：2012-11-01
-- 2.查询指定入职日期之后入职的员工信息
select * from tb_emp where entrydate > '2012-11-01';

-- 合并以上两条SQL语句
select * from tb_emp where entrydate > (select entrydate from tb_emp where name = '方东白');
```

###### 7.3.2 列子查询（in, not in）

子查询返回的结果是一列(可以是多行)，这种子查询称为列子查询。

**就是子查询语句的查询结果只有一列**

常用的操作符：

| **操作符** | **描述**                     |
| ---------- | ---------------------------- |
| IN         | 在指定的集合范围之内，多选一 |
| NOT IN     | 不在指定的集合范围之内       |

案例：查询"教研部"和"咨询部"的所有员工信息

> 分解为以下两步：
>
> 1. 查询 "销售部" 和 "市场部" 的部门ID
> 2. 根据部门ID, 查询员工信息

```mysql
-- 1.查询"销售部"和"市场部"的部门ID
select id from tb_dept where name = '教研部' or name = '咨询部';    #查询结果：3,2
-- 2.根据部门ID, 查询员工信息
select * from tb_emp where dept_id in (3,2);

-- 合并以上两条SQL语句
select * from tb_emp where dept_id in (select id from tb_dept where name = '教研部' or name = '咨询部');
```

###### 7.3.3 行子查询

子查询返回的结果是一行(可以是多列)，这种子查询称为行子查询。

**子查询语句的查询属性不止一列**

常用的操作符：= 、<> 、IN 、NOT IN

案例：查询与"韦一笑"的入职日期及职位都相同的员工信息 

> 可以拆解为两步进行： 
>
> 1. 查询 "韦一笑" 的入职日期 及 职位
> 2. 查询与"韦一笑"的入职日期及职位相同的员工信息 

```mysql
-- 查询"韦一笑"的入职日期 及 职位
select entrydate , job from tb_emp where name = '韦一笑';  #查询结果： 2007-01-01 , 2
-- 查询与"韦一笑"的入职日期及职位相同的员工信息
select * from tb_emp where (entrydate,job) = ('2007-01-01',2);

-- 合并以上两条SQL语句
select * from tb_emp where (entrydate,job) = (select entrydate , job from tb_emp where name = '韦一笑');
```



###### 7.3.4 表子查询

子查询返回的结果是多行多列，常作为临时表，这种子查询称为表子查询。



案例：查询入职日期是 "2006-01-01" 之后的员工信息 , 及其部门信息

个人理解：

1 首先排除条件 查询员工信息和部门信息 **可以想到要用左外连接，员工是主表**

```sql
select e.*, d.* from e left join dept d on e.dept_id = d.id ;
```

2 然后加日期这个条件

```sql
select e.*, d.* from (select * from emp where entrydate > '2006-01-01') e left join dept d on e.dept_id = d.id ;
```

在这个语句中，子查询用于首先从 `emp` 表中选择 `entrydate` 大于 '2006-01-01' 的所有记录，然后这个结果集被命名为 `e` 并与 `dept` 表进行左连接。注意，`WHERE` 子句是不需要的，因为过滤条件已经在子查询中指定了





> 分解为两步执行：
>
> 1. 查询入职日期是 "2006-01-01" 之后的员工信息
> 2. 基于查询到的员工信息，在查询对应的部门信息

~~~mysql
select * from emp where entrydate > '2006-01-01';

select e.*, d.* from (select * from emp where entrydate > '2006-01-01') e left join dept d on e.dept_id = d.id ;
~~~

![13-5](picture/13-5.png)



##### 7.4 案例

直接去看一遍视频 

https://www.bilibili.com/video/BV1m84y1w7Tb?p=109&vd_source=d9ebacf85c69289ce6e270a1c64762bb

P109-P110  截图语句没用 关键是写代码的思路





## 															事务

场景：学工部整个部门解散了，该部门及部门下的员工都需要删除了。

- 操作：

  ```sql
  -- 删除学工部
  delete from dept where id = 1;  -- 删除成功
  
  -- 删除学工部的员工
  delete from emp where dept_id = 1; -- 删除失败（操作过程中出现错误：造成删除没有成功）
  ```

- 问题：**如果删除部门成功了，而删除该部门的员工时失败了，此时就造成了数据的不一致。**

​	要解决上述的问题，就需要通过数据库中的事务来解决。

#### 1 事务的介绍

事务是一组操作的集合，它是一个不可分割的工作单位。事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些操作要么同时成功，要么同时失败。

**事务作用：保证在一个事务中多次操作数据库表中数据时，要么全都成功,要么全都失败。**

#### 2 操作

MYSQL中有两种方式进行事务的操作：

1. 自动提交事务：**即执行一条sql语句提交一次事务。（默认MySQL的事务是自动提交）**
2. 手动提交事务：先开启，再提交 

事务操作有关的SQL语句：

| SQL语句                        | 描述             |
| ------------------------------ | ---------------- |
| start transaction;  /  begin ; | 开启手动控制事务 |
| commit;                        | 提交事务         |
| rollback;                      | 回滚事务         |

> 手动提交事务使用步骤：
>
> - 第1种情况：开启事务  =>  执行SQL语句   =>  成功  =>  提交事务
> - 第2种情况：开启事务  =>  执行SQL语句   =>  失败  =>  回滚事务



使用事务控制删除部门和删除该部门下的员工的操作：

```sql
-- 开启事务
start transaction ;

-- 删除学工部
delete from tb_dept where id = 1;

-- 删除学工部的员工
delete from tb_emp where dept_id = 1;
```

**如果不执行下面的提交或者回滚，那么上面的代码就不会实际执行**

- 上述的这组SQL语句，如果如果执行成功，则提交事务

```sql
-- 提交事务 (成功时执行)
commit ;
```

- 上述的这组SQL语句，如果如果执行失败，则回滚事务

```sql
-- 回滚事务 (出错时执行)
rollback ;
```



#### 3 四大特性

ACID 

> 事务的四大特性简称为：ACID

- **原子性（Atomicity）** ：原子性是指事务包装的一组sql是一个不可分割的工作单元，事务中的操作要么全部成功，要么全部失败。

- **一致性（Consistency）**：一个事务完成之后数据都必须处于一致性状态。

​		如果事务成功的完成，那么数据库的所有变化将生效。

​		如果事务执行出现错误，那么数据库的所有变化将会被回滚(撤销)，返回到原始状态。

- **隔离性（Isolation）**：多个用户并发的访问数据库时，一个用户的事务不能被其他用户的事务干扰，多个并发的事务之间要相互隔离。

​		一个事务的成功或者失败对于其他的事务是没有影响。

- **持久性（Durability）**：一个事务一旦被提交或回滚，它对数据库的改变将是永久性的，哪怕数据库发生异常，重启之后数据亦然存在。



## 																	索引

提高效率

优点：

1. **提高数据查询的效率**，降低数据库的IO成本。
2. 通过索引列对数据进行排序，降低数据排序的成本，降低CPU消耗。

缺点：

1. 索引会占用存储空间。
2. **索引大大提高了查询效率，同时却也降低了insert、update、delete的效率。**

#### 1 结构 B+树

思考：采用二叉搜索树或者是红黑树来作为索引的结构有什么问题？

<details>
    <summary>答案</summary>
    最大的问题就是在数据量大的情况下，树的层级比较深，会影响检索速度。因为不管是二叉搜索数还是红黑数，一个节点下面只能有两个子节点。此时在数据量大的情况下，就会造成数的高度比较高，树的高度一旦高了，检索速度就会降低。
</details>



> 说明：如果数据结构是红黑树，那么查询1000万条数据，根据计算树的高度大概是23左右，这样确实比之前的方式快了很多，但是如果高并发访问，那么一个用户有可能需要23次磁盘IO，那么100万用户，那么会造成效率极其低下。所以**为了减少红黑树的高度，那么就得增加树的宽度，就是不再像红黑树一样每个节点只能保存一个数据，**可以引入另外一种数据结构，一个节点可以保存多个数据，这样宽度就会增加从而降低树的高度。这种数据结构例如BTree就满足。

下面我们来看看B+Tree(多路平衡搜索树)结构中如何避免这个问题：

![13-6](picture/13-6.png)

B+Tree结构：

- 每一个节点，可以存储多个key（有n个key，就有n个指针）
- 节点分为：叶子节点、非叶子节点
  - 叶子节点，就是最后一层子节点，所有的数据都存储在叶子节点上
  - 非叶子节点，不是树结构最下面的节点，用于索引数据，存储的的是：key+指针
- 为了提高范围查询效率，叶子节点形成了一个双向链表，便于数据的排序及区间范围查询



> **拓展：**
>
> 非叶子节点都是由key+指针域组成的，一个key占8字节，一个指针占6字节，而一个节点总共容量是16KB，那么可以计算出一个节点可以存储的元素个数：16*1024字节 / (8+6)=1170个元素。
>
> - 查看mysql索引节点大小：show global status like 'innodb_page_size';    -- 节点大小：16384
>
> 当根节点中可以存储1170个元素，那么根据每个元素的地址值又会找到下面的子节点，每个子节点也会存储1170个元素，那么第二层即第二次IO的时候就会找到数据大概是：1170*1170=135W。也就是说B+Tree数据结构中只需要经历两次磁盘IO就可以找到135W条数据。
>
> 对于第二层每个元素有指针，那么会找到第三层，第三层由key+数据组成，假设key+数据总大小是1KB，而每个节点一共能存储16KB，所以一个第三层一个节点大概可以存储16个元素(即16条记录)。那么结合第二层每个元素通过指针域找到第三层的节点，第二层一共是135W个元素，那么第三层总元素大小就是：135W*16结果就是2000W+的元素个数。
>
> 结合上述分析B+Tree有如下优点：
>
> - 千万条数据，B+Tree可以控制在小于等于3的高度
> - 所有的数据都存储在叶子节点上，并且底层已经实现了按照索引进行排序，还可以支持范围查询，叶子节点是一个双向链表，支持从小到大或者从大到小查找



#### 2 语法

这都有手就行 现场查询都可以

**创建索引**

~~~mysql
create  [ unique ]  index 索引名 on  表名 (字段名,... ) ;
~~~

案例：为tb_emp表的name字段建立一个索引

~~~mysql
create index idx_emp_name on tb_emp(name);
~~~





> 在创建表时，如果添加了主键和唯一约束，就会默认创建：主键索引、唯一约束
>
> ![13-7](picture/13-7.png)



**查看索引**

~~~mysql
show  index  from  表名;
~~~

案例：查询 tb_emp 表的索引信息

~~~mysql
show  index  from  tb_emp;
~~~



##  											Mybatis--web后端中操控数据库

在前面我们学习MySQL数据库时，都是**利用图形化客户端工具(如：idea、datagrip)**，来操作数据库的。

> 在客户端工具中，编写增删改查的SQL语句，发给MySQL数据库管理系统，由数据库管理系统执行SQL语句并返回执行结果。
>
> 增删改操作：返回受影响行数
>
> 查询操作：返回结果集(查询的结果)

我们**作为后端程序开发人员，通常会使用Java程序来完成对数据库的操作。Java程序操作数据库，现在主流的方式是：Mybatis。**

什么是MyBatis?

- MyBatis是一款优秀的 **持久层** **框架**，用于简化JDBC的开发。



在上面我们提到了两个词：一个是持久层，另一个是框架。

- 持久层：指的是就是数据访问层(dao)，是用来操作数据库的。

![14-1](picture/14-1.png)

- 框架：是一个半成品软件，是一套可重用的、通用的、软件基础代码模型。在框架的基础上进行软件开发更加高效、规范、通用、可拓展。



#### 1. 简单实现

这个从头到尾过一遍



Mybatis操作数据库的步骤：

##### 1.1  准备工作(创建springboot工程、数据库表user、实体类User)

 创建工程省略，User表额外用图像化创建即可，实体类写在pojo类下面

##### 1.2  引入Mybatis的相关依赖，配置Mybatis(数据库连接信息)

在pom文件中添加依赖 注意这里的版本冲突 

配置数据库连接则在appropties中编写

```properties
#驱动类名称
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
#数据库连接的url
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis
#连接数据库的用户名
spring.datasource.username=root
#连接数据库的密码
spring.datasource.password=1234
```



##### 1.3  编写SQL语句(注解/XML)

在创建出来的springboot工程中，**在引导类所在包下，在创建一个包 mapper。在mapper包下创建一个接口 UserMapper ，这是一个持久层接口**

（Mybatis的持久层接口规范一般都叫 XxxMapper）。

UserMapper：

~~~java
import com.itheima.pojo.User;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;
import java.util.List;

@Mapper
public interface UserMapper {
    
    //查询所有用户数据
    @Select("select id, name, age, gender, phone from user")
    public List<User> list();
    
}
~~~

> @Mapper注解：表示是mybatis中的Mapper接口
>
> - 程序运行时：框架会自动生成接口的实现类对象(代理对象)，并给交Spring的IOC容器管理
>
>  @Select注解：代表的就是select查询，用于书写select查询语句



##### 1.4 测试

在创建出来的SpringBoot工程中，在src下的test目录下，已经自动帮我们创建好了测试类 ，并且在测试类上已经添加了注解 @SpringBootTest，代表该测试类已经与SpringBoot整合。 

**该测试类在运行时，会自动通过引导类加载Spring的环境（IOC容器）。我们要测试那个bean对象，就可以直接通过@Autowired注解直接将其注入进行**，然后就可以测试了。 

这里要注意，本来是不能直接new一个接口userMapper的  但是由于mapper注解，这个接口已经被自动声明为了bean对象，所以通过autowired注入可以这样做



测试类代码如下： 

```java
@SpringBootTest
public class MybatisQuickstartApplicationTests {
	
    @Autowired
    private UserMapper userMapper;
	
    @Test
    public void testList(){
        List<User> userList = userMapper.list();
        for (User user : userList) {
            System.out.println(user);
        }
    }

}
```

> 运行结果：
>
> ~~~
> User{id=1, name='白眉鹰王', age=55, gender=1, phone='18800000000'}
> User{id=2, name='金毛狮王', age=45, gender=1, phone='18800000001'}
> User{id=3, name='青翼蝠王', age=38, gender=1, phone='18800000002'}
> User{id=4, name='紫衫龙王', age=42, gender=2, phone='18800000003'}
> User{id=5, name='光明左使', age=37, gender=1, phone='18800000004'}
> User{id=6, name='光明右使', age=48, gender=1, phone='18800000005'}
> ~~~





#### 2 JDBC  mybayis的底层（我感觉最大的问题是会频繁创建连接）



> Mybatis框架，就是对原始的JDBC程序的封装。 

那到底什么是JDBC呢，接下来，我们就来介绍一下。

JDBC： ( Java DataBase Connectivity )，就是使用Java语言操作关系型数据库的一套API。

![image-20221210144811961](../BaiduNetdiskDownload/day08-MySQL-Mybatis入门/day08-MySQL-Mybatis入门/讲义/02-Mybatis入门/assets/image-20221210144811961.png)



下面我们看看原始的JDBC程序是如何操作数据库的。操作步骤如下：

1. 注册驱动
2. 获取连接对象
3. 执行SQL语句，返回执行结果
4. 处理执行结果
5. 释放资源

> 在pom.xml文件中已引入MySQL驱动依赖，我们直接编写JDBC代码即可

JDBC具体代码实现：

```java
import com.itheima.pojo.User;
import org.junit.jupiter.api.Test;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;

public class JdbcTest {
    @Test
    public void testJdbc() throws Exception {
        //1. 注册驱动
        Class.forName("com.mysql.cj.jdbc.Driver");

        //2. 获取数据库连接
        String url="jdbc:mysql://127.0.0.1:3306/mybatis";
        String username = "root";
        String password = "1234";
        Connection connection = DriverManager.getConnection(url, username, password);

        //3. 执行SQL
        Statement statement = connection.createStatement(); //操作SQL的对象
        String sql="select id,name,age,gender,phone from user";
        ResultSet rs = statement.executeQuery(sql);//SQL查询结果会封装在ResultSet对象中

        List<User> userList = new ArrayList<>();//集合对象（用于存储User对象）
        //4. 处理SQL执行结果
        while (rs.next()){
            //取出一行记录中id、name、age、gender、phone下的数据
            int id = rs.getInt("id");
            String name = rs.getString("name");
            short age = rs.getShort("age");
            short gender = rs.getShort("gender");
            String phone = rs.getString("phone");
            //把一行记录中的数据，封装到User对象中
            User user = new User(id,name,age,gender,phone);
            userList.add(user);//User对象添加到集合
        }
        //5. 释放资源
        statement.close();
        connection.close();
        rs.close();

        //遍历集合
        for (User user : userList) {
            System.out.println(user);
        }
    }
}
```

> DriverManager(类)：数据库驱动管理类。
>
> - 作用：
>
>   1. 注册驱动
>
>   2. 创建java代码和数据库之间的连接，即获取Connection对象
>
> Connection(接口)：建立数据库连接的对象
>
> - 作用：用于建立java程序和数据库之间的连接
>
> Statement(接口)： 数据库操作对象(执行SQL语句的对象)。
>
> - 作用：用于向数据库发送sql语句
>
> ResultSet(接口)：结果集对象（一张虚拟表）
>
> - 作用：sql查询语句的执行结果会封装在ResultSet中

通过上述代码，**我们看到直接基于JDBC程序来操作数据库，代码实现非常繁琐，所以在项目开发中，我们很少使用。**  





##### 问题分析

原始的JDBC程序，存在以下几点问题：

1. 数据库链接的四要素(驱动、链接、用户名、密码)全部`硬编码`在java代码中
2. 查询结果的`解析及封装非常繁琐`
3. `每一次查询数据库都需要获取连接`,操作完毕后释放连接, 资源浪费, 性能降低



分析了JDBC的缺点之后，我们再来看一下在mybatis中，是如何解决这些问题的：

1. 数据库连接四要素(驱动、链接、用户名、密码)，**都配置在springboot默认的配置文件 application.properties中**

2. 查询结果的解析及封装，**由mybatis自动完成映射封装**，我们无需关注

3. 在mybatis中使用了**数据库连接池技术，**从而避免了频繁的创建连接、销毁连接而带来的资源浪费。



##### 数据库连接池

在前面我们所讲解的mybatis中，使用了数据库连接池技术，避免频繁的创建连接、销毁连接而带来的资源浪费。

![14-2](picture/14-2.png)

数据库连接池是个容器，负责分配、管理数据库连接(Connection)

- 程序在启动时，会在数据库连接池(容器)中，创建一定数量的Connection对象

允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个

- 客户端在执行SQL时，先从连接池中获取一个Connection对象，然后在执行SQL语句，SQL语句执行完之后，释放Connection时就会把Connection对象归还给连接池（Connection对象可以复用）

释放空闲时间超过最大空闲时间的连接，来避免因为没有释放连接而引起的数据库连接遗漏

- 客户端获取到Connection对象了，但是Connection对象并没有去访问数据库(处于空闲)，数据库连接池发现Connection对象的空闲时间 > 连接池中预设的最大空闲时间，此时数据库连接池就会自动释放掉这个连接对象

数据库连接池的好处：

1. 资源重用
2. 提升系统响应速度
3. 避免数据库连接遗漏





常见的数据库连接池：

* C3P0
* DBCP
* Druid
* Hikari (springboot默认)

现在使用更多的是：Hikari、Druid  （性能更优越）

- Hikari（追光者） [默认的连接池] 

![14-3](picture/14-3.PNG)

如果我们想把默认的数据库连接池切换为Druid数据库连接池，只需要完成以下两步操作即可：

> 参考官方地址：https://github.com/alibaba/druid/tree/master/druid-spring-boot-starter

1. 在pom.xml文件中引入依赖

```xml
<dependency>
    <!-- Druid连接池依赖 -->
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.8</version>
</dependency>
```

2. 在application.properties中引入数据库连接配置

方式1：

~~~properties
spring.datasource.druid.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.druid.url=jdbc:mysql://localhost:3306/mybatis
spring.datasource.druid.username=root
spring.datasource.druid.password=1234
~~~

方式2：

~~~properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis
spring.datasource.username=root
spring.datasource.password=1234
~~~



#### 3 lombok

Lombok是一个实用的Java类库，可以通过简单的注解来简化和消除一些必须有但显得很臃肿的Java代码。

> 通过注解的形式自动生成构造器、getter/setter、equals、hashcode、toString等方法，并可以自动化生成日志变量，简化java开发、提高效率。

| **注解**            | **作用**                                                     |
| ------------------- | ------------------------------------------------------------ |
| @Getter/@Setter     | 为所有的属性提供get/set方法                                  |
| @ToString           | 会给类自动生成易阅读的  toString 方法                        |
| @EqualsAndHashCode  | 根据类所拥有的非静态字段自动重写 equals 方法和  hashCode 方法 |
| @Data               | 提供了更综合的生成代码功能（@Getter  + @Setter + @ToString + @EqualsAndHashCode） |
| @NoArgsConstructor  | 为实体类生成无参的构造器方法                                 |
| @AllArgsConstructor | 为实体类生成除了static修饰的字段之外带有各参数的构造器方法。 |



第1步：在pom.xml文件中引入依赖

```xml
<!-- 在springboot的父工程中，已经集成了lombok并指定了版本号，故当前引入依赖时不需要指定version -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
```

第2步：在实体类上添加注解

```java
import lombok.Data;

@Data
public class User {
    private Integer id;
    private String name;
    private Short age;
    private Short gender;
    private String phone;
}
```

> 在实体类上添加了@Data注解，那么这个类在编译时期，就会生成getter/setter、equals、hashcode、toString等方法

Lombok的注意事项：

- Lombok会在编译时，会自动生成对应的java代码
- 在使用lombok时，还需要安装一个lombok的插件（新版本的IDEA中自带）



## 													Mybatis--实现数据增删改查（注解实现和XML实现）

这一章就不实践了 以看视频为主  因为基础的操作和进阶的操作 并不存在配置上的区别

而mysql语句的使用 前面已经试过了  没有较大问题

有不懂的随时去看完整版的typora文档即可



#### 1 sql语句的预编译

预编译SQL有两个优势：

1. **性能更高**
2. **更安全(防止SQL注入)**

![15-1](picture/15-1.png)

> 性能更高：预编译SQL，**编译一次之后会将编译后的SQL语句缓存起来，后面再次执行这条语句时，不会再次编译**。（只是输入的参数不同）
>
> 更安全(防止SQL注入)：将敏感字进行转义，保障SQL的安全性。





##### **1.1  如何实现预编译：通过添加占位符 mybatis就会自动用预编译的方式执行语句**

在Mybatis中提供的参数占位符有两种：${...} 、#{...}

- #{...}
  - 执行SQL时，会将#{…}替换为?，生成预编译SQL，会自动设置参数值
  - 使用时机：参数传递，都使用#{…}

- ${...}
  - 拼接SQL。直接将参数拼接在SQL语句中，存在SQL注入问题
  - 使用时机：如果对表名、列表进行动态设置时使用

> 注意事项：在项目开发中，**建议使用#{...}，生成预编译SQL**，防止SQL注入安全。



##### 1.2  sql注入问题：

SQL注入：是通过操作输入的数据来修改事先定义好的SQL语句，以达到执行代码对服务器进行攻击的方法。

> 由于没有对用户输入进行充分检查，而SQL又是拼接而成，在用户输入参数时，在参数中添加一些SQL关键字，达到改变SQL运行结果的目的，也可以完成恶意攻击。



![15-2](picture/15-2.png)

为什么这个可以登录成功呢？

因为这个密码的分号  让sql语句中出现了一个1=1的恒等式 返回值变成了1  骗过了系统

这就是：**在用户输入参数时，在参数中添加一些SQL关键字，达到改变SQL运行结果的目的，完成恶意攻击。**



##### 1.3 为什么预编译能解决sql注入

因为数据库会将输入看做一个整体参数值，而不是 SQL 代码的一部分，因此不会执行恶意的 SQL 命令。



#### 2 插入数据/主键返回



SQL语句：

```sql
insert into emp(username, name, gender, image, job, entrydate, dept_id, create_time, update_time) values ('songyuanqiao','宋远桥',1,'1.jpg',2,'2012-10-09',2,'2022-10-01 10:00:00','2022-10-01 10:00:00');
```

接口方法：

```java
@Mapper
public interface EmpMapper {

    @Insert("insert into emp(username, name, gender, image, job, entrydate, dept_id, create_time, update_time) values (#{username}, #{name}, #{gender}, #{image}, #{job}, #{entrydate}, #{deptId}, #{createTime}, #{updateTime})")
    public void insert(Emp emp);

}
```

> 说明：#{...} 里面写的名称是对象的属性名



有时候 添加完成后，还需要**顺便获取插入数据库数据的主键--通过注解完成**

那要如何实现在插入数据之后返回所插入行的主键值呢？

- 默认情况下，执行插入操作时，是不会主键值返回的。如果我们想要拿到主键值，需要在Mapper接口中的方法上添加一个Options注解，并在注解中指定属性useGeneratedKeys=true和keyProperty="实体类属性名"



主键返回代码实现：

~~~java
@Mapper
public interface EmpMapper {
    
    //会自动将生成的主键值，赋值给emp对象的id属性
    @Options(useGeneratedKeys = true,keyProperty = "id")
    @Insert("insert into emp(username, name, gender, image, job, entrydate, dept_id, create_time, update_time) values (#{username}, #{name}, #{gender}, #{image}, #{job}, #{entrydate}, #{deptId}, #{createTime}, #{updateTime})")
    public void insert(Emp emp);

}
~~~

测试：

~~~java
@SpringBootTest
class SpringbootMybatisCrudApplicationTests {
    @Autowired
    private EmpMapper empMapper;

    @Test
    public void testInsert(){
        //创建员工对象
        emp.setDeptId(1);
        
        //调用添加方法
        empMapper.insert(emp);
		// 这一行可以直接获取主键
        System.out.println(emp.getId());
    }
}
~~~



#### 3. 查询

SQL语句：

~~~mysql
select id, username, password, name, gender, image, job, entrydate, dept_id, create_time, update_time from emp;
~~~

接口方法：

~~~java
@Mapper
public interface EmpMapper {
    @Select("select id, username, password, name, gender, image, job, entrydate, dept_id, create_time, update_time from emp where id=#{id}")
    public Emp getById(Integer id);
}
~~~

测试类：

~~~java
@SpringBootTest
class SpringbootMybatisCrudApplicationTests {
    @Autowired
    private EmpMapper empMapper;

    @Test
    public void testGetById(){
        Emp emp = empMapper.getById(1);
        System.out.println(emp);
    }
}
~~~

> 执行结果：
>
> ![15-3](picture/15-3.png)
>
> 而在测试的过程中，我们会发现有几个字段(deptId、createTime、updateTime)是没有数据值的





##### 3.1  原因与数据封装：

原因如下： 

- 实体类属性名和数据库表查询返回的字段名一致，mybatis会自动封装。
- 如果**实体类属性名和数据库表查询返回的字段名不一致**，不能自动封装。





![15-4](picture/15-4.png)



##### 3.2  解决方案：三种

第一种是起别名 

第二种是用注解实现的起别名   

第三种是自动映射但条件苛刻

![15-5](picture/15-5.PNG)



##### 3.3 条件查询

通过页面原型以及需求描述我们要实现的查询：

- 姓名：要求支持模糊匹配
- 性别：要求精确匹配
- 入职时间：要求进行范围查询
- 根据最后修改时间进行降序排序



SQL语句：

```sql
select id, username, password, name, gender, image, job, entrydate, dept_id, create_time, update_time 
from emp 
where name like '%张%' 
      and gender = 1 
      and entrydate between '2010-01-01' and '2020-01-01 ' 
order by update_time desc;
```

**很显然，这里的问题在于 如何将模糊匹配，范围查询这些东西以参数的形式交给Java**



- 方式一（有sql注入风险 不看）

```java
@Mapper
public interface EmpMapper {
    @Select("select * from emp " +
            "where name like '%${name}%' " +
            "and gender = #{gender} " +
            "and entrydate between #{begin} and #{end} " +
            "order by update_time desc")
    public List<Emp> list(String name, Short gender, LocalDate begin, LocalDate end);
}
```

> ![15-6](picture/15-6.png)
>
> 以上方式注意事项：
>
> 1. 方法中的形参名和SQL语句中的参数占位符名保持一致
>
> 2. 模糊查询使用${...}进行字符串拼接，这种方式呢，由于是字符串拼接，并不是预编译的形式，所以效率不高、且存在sql注入风险。



- 方式二（解决SQL注入风险）
  - **使用MySQL提供的字符串拼接函数：concat('%' , '关键字' , '%')**

~~~java
@Mapper
public interface EmpMapper {

    @Select("select * from emp " +
            "where name like concat('%',#{name},'%') " +
            "and gender = #{gender} " +
            "and entrydate between #{begin} and #{end} " +
            "order by update_time desc")
    public List<Emp> list(String name, Short gender, LocalDate begin, LocalDate end);

}

~~~

> 执行结果：生成的SQL都是预编译的SQL语句（性能高、安全）
>
> ![image-20221212120006242](../BaiduNetdiskDownload/day09-Mybatis/day09-Mybatis/讲义/assets/image-20221212120006242.png)





#### 4. XML配置文件

Mybatis的开发有两种方式：

1. 注解
2. XML

使用Mybatis的注解方式，主要是来完成一些简单的增删改查功能。**如果需要实现复杂的SQL功能，建议使用XML来配置映射语句**，也就是将SQL语句写在XML配置文件中。



##### 4.1 xml配置文件实现

这里过程太繁琐 放一张一图流 其他图片源没更改

有需求去翻day9的源文档即可

在Mybatis中使用XML映射文件方式开发，需要符合一定的规范：

1. XML映射文件的名称与Mapper接口名称一致，并且将XML映射文件和Mapper接口放置在相同包下（同包同名）

2. XML映射文件的namespace属性为Mapper接口全限定名一致

3. XML映射文件中sql语句的id与Mapper接口中的方法名一致，并保持返回类型一致。

![15-7](picture/15-7.png)

> \<select>标签：就是用于编写select查询语句的。
>
> - resultType属性，指的是查询返回的单条记录所封装的类型。

第1步：创建XML映射文件

![image-20221212154908306](../BaiduNetdiskDownload/day09-Mybatis/day09-Mybatis/讲义/assets/image-20221212154908306.png)

![image-20221212155304635](../BaiduNetdiskDownload/day09-Mybatis/day09-Mybatis/讲义/assets/image-20221212155304635.png)

![image-20221212155544404](../BaiduNetdiskDownload/day09-Mybatis/day09-Mybatis/讲义/assets/image-20221212155544404.png)



第2步：编写XML映射文件

> xml映射文件中的dtd约束，直接从mybatis官网复制即可

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="">
 
</mapper>
~~~



配置：XML映射文件的namespace属性为Mapper接口全限定名

![image-20221212160316644](../BaiduNetdiskDownload/day09-Mybatis/day09-Mybatis/讲义/assets/image-20221212160316644.png)

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.mapper.EmpMapper">

</mapper>
~~~



配置：XML映射文件中sql语句的id与Mapper接口中的方法名一致，并保持返回类型一致

![image-20221212163528787](../BaiduNetdiskDownload/day09-Mybatis/day09-Mybatis/讲义/assets/image-20221212163528787.png)

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.mapper.EmpMapper">

    <!--查询操作-->
    <select id="list" resultType="com.itheima.pojo.Emp">
        select * from emp
        where name like concat('%',#{name},'%')
              and gender = #{gender}
              and entrydate between #{begin} and #{end}
        order by update_time desc
    </select>
</mapper>
~~~

> 运行测试类，执行结果：
>
> ![image-20221212163719534](../BaiduNetdiskDownload/day09-Mybatis/day09-Mybatis/讲义/assets/image-20221212163719534.png)

##### 4.2  mybatisX插件

MybatisX是一款基于IDEA的快速开发Mybatis的插件，为效率而生。

**xml注解的关键在于实现xml文件和实际类的绑定，这个插件能快速定位其绑定关系**

![15-8](picture/15-8.png)

##### 4.3 结论

使用Mybatis的注解，主要是来完成一些简单的增删改查功能。如果需要实现复杂的SQL功能，建议使用XML来配置映射语句。





#### 5. 动态SQL（重要）

##### 5.1 概念

在页面原型中，列表上方的条件是动态的，是可以不传递的，也可以只传递其中的1个或者2个或者全部。

![image-20220901173203491](../BaiduNetdiskDownload/day09-Mybatis/day09-Mybatis/讲义/assets/image-20220901173203491.png)

而在我们刚才编写的SQL语句中，

**我们会看到，我们将三个条件直接写死了。 如果页面只传递了参数姓名name 字段，其他两个字段 性别 和 入职时间没有传递，那么这两个参数的值就是null。**

此时，执行的SQL语句为：

![image-20220901173431554](../BaiduNetdiskDownload/day09-Mybatis/day09-Mybatis/讲义/assets/image-20220901173431554.png) 

这个查询结果是不正确的。正确的做法应该是：传递了参数，再组装这个查询条件；如果没有传递参数，就不应该组装这个查询条件。



比如：如果姓名输入了"张", 对应的SQL为:

```sql
select *  from emp where name like '%张%' order by update_time desc;
```



如果姓名输入了"张",，性别选择了"男"，则对应的SQL为:

```sql
select *  from emp where name like '%张%' and gender = 1 order by update_time desc;
```

SQL语句会随着用户的输入或外部条件的变化而变化，我们称为：**动态SQL**。



##### 5.2 动态SQL--if查询

就只是在sql语句中添加一些判断而已

`<if>`：用于判断条件是否成立。使用test属性进行条件判断，如果条件为true，则拼接SQL。

~~~xml
<if test="条件表达式">
   要拼接的sql语句
</if>
~~~

接下来，我们就通过`<if>`标签来改造之前条件查询的案例，使用`<where>`标签代替SQL语句中的where关键字

- `<where>`只会在子元素有内容的情况下才插入where子句，而且会自动去除子句的开头的AND或OR

~~~xml
<select id="list" resultType="com.itheima.pojo.Emp">
        select * from emp
        <where>
             <!-- if做为where标签的子元素 -->
             <if test="name != null">
                 and name like concat('%',#{name},'%')
             </if>
             <if test="gender != null">
                 and gender = #{gender}
             </if>
             <if test="begin != null and end != null">
                 and entrydate between #{begin} and #{end}
             </if>
        </where>
        order by update_time desc
</select>
~~~

测试方法：

~~~java
@Test
public void testList(){
    //只有性别
    List<Emp> list = empMapper.list(null, (short)1, null, null);
    for(Emp emp : list){
        System.out.println(emp);
    }
}
~~~

##### 5.3  动态SQL--if更新

案例：完善更新员工功能，修改为动态更新员工数据信息

- 动态更新员工信息，如果更新时传递有值，则更新；如果更新时没有传递值，则不更新
- 解决方案：动态SQL

修改Mapper接口：

~~~java
@Mapper
public interface EmpMapper {
    //删除@Update注解编写的SQL语句
    //update操作的SQL语句编写在Mapper映射文件中
    public void update(Emp emp);
}
~~~

修改Mapper映射文件：

- `<set>`：动态的在SQL语句中插入set关键字，并会删掉额外的逗号。（用于update语句中）

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.mapper.EmpMapper">

    <!--更新操作-->
    <update id="update">
        update emp
        <!-- 使用set标签，代替update语句中的set关键字 -->
        <set>
            <if test="username != null">
                username=#{username},
            </if>
            <if test="name != null">
                name=#{name},
            </if>
        
            <if test="updateTime != null">
                update_time=#{updateTime}
            </if>
        </set>
        where id=#{id}
    </update>
</mapper>
~~~

**小结**

- `<if>`

  - 用于判断条件是否成立，如果条件为true，则拼接SQL

  - 形式：

    ~~~xml
    <if test="name != null"> … </if>
    ~~~

- `<where>`

  - where元素只会在子元素有内容的情况下才插入where子句，而且会自动去除子句的开头的AND或OR

- `<set>`

  - 动态地在行首插入 SET 关键字，并会删掉额外的逗号。（用在update语句中）



##### 5.4 动态SQL--foreach

案例：员工删除功能（既支持删除单条记录，又支持批量删除）



就是删除的时候有个多选项 一次删除几个那种



SQL语句：

~~~mysql
delete from emp where id in (1,2,3);
~~~

Mapper接口：

~~~java
@Mapper
public interface EmpMapper {
    //批量删除
    public void deleteByIds(List<Integer> ids);
}
~~~

XML映射文件：

- 使用`<foreach>`遍历deleteByIds方法中传递的参数ids集合

~~~xml
<foreach collection="集合名称" item="集合遍历出来的元素/项" separator="每一次遍历使用的分隔符" 
         open="遍历开始前拼接的片段" close="遍历结束后拼接的片段">
</foreach>
~~~

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.mapper.EmpMapper">
    <!--删除操作-->
    <delete id="deleteByIds">
        delete from emp where id in
        <foreach collection="ids" item="id" separator="," open="(" close=")">
            #{id}
        </foreach>
    </delete>
</mapper> 
~~~

> ![15-11](picture/15-11.png)

> 执行的SQL语句：
>
> ![15-12](picture/15-12.png)



##### 5.5 动态SQL--sql&include

问题分析：

- 在xml映射文件中配置的SQL，有时可能会存在很多重复的片段，此时就会存在很多冗余的代码

我们可以对重复的代码片段进行抽取，将其通过`<sql>`标签封装到一个SQL片段，然后再通过`<include>`标签进行引用。

**其实就是把select这一段用一个标签定义在xml文件中，这样在mapper中可以直接调用**

- `<sql>`：定义可重用的SQL片段

- `<include>`：通过属性refid，指定包含的SQL片段

![15-13](picture/15-13.png)



SQL片段： 抽取重复的代码

```xml
<sql id="commonSelect">
 	select id, username, password, name, gender, image, job, entrydate, dept_id, create_time, update_time from emp
</sql>
```

然后通过`<include>` 标签在原来抽取的地方进行引用。操作如下：

```xml
<select id="list" resultType="com.itheima.pojo.Emp">
    <include refid="commonSelect"/>
    <where>
        <if test="name != null">
            name like concat('%',#{name},'%')
        </if>
        <if test="gender != null">
            and gender = #{gender}
        </if>
        <if test="begin != null and end != null">
            and entrydate between #{begin} and #{end}
        </if>
    </where>
    order by update_time desc
</select>
```





## 																实际案例

大部分功能选择自己敲，但是一些理论性的东西还是可以记录下

### 一，登录校验功能的实现方法

#### 1. Cookie

cookie 是客户端会话跟踪技术，它是存储在客户端浏览器的，我们使用 cookie 来跟踪会话，我们就可以在浏览器第一次发起请求来请求服务器的时候，我们在服务器端来设置一个cookie。



比如第一次请求了登录接口，登录接口执行完成之后，我们就可以设置一个cookie，**在 cookie 当中我们就可以来存储用户相关的一些数据信息。比如我可以在 cookie 当中来存储当前登录用户的用户名，用户的ID。**





##### **1.1 过程描述：**

- 第一次登录时，**客户端不会有 Cookie**，登录成功后，服务端生成并发送 `Cookie` 给客户端。

1 客户端发送登录请求（例如提供用户名和密码）。

2 服务端验证客户端提供的登录凭证（例如用户名和密码）。

3 如果验证成功，**服务端会生成一个 `Cookie`**（通常是一个会话 ID 或者令牌，用来标识用户的会话状态）。

4 服务端在响应头中通过 `Set-Cookie` 字段将这个 `Cookie` 发送给客户端。



例如

```http
HTTP/1.1 200 OK
Set-Cookie: sessionId=abc123; Path=/; HttpOnly
```





- **后续每次请求**，客户端都会自动携带之前存储的 `Cookie`，服务端根据 `Cookie` 来识别用户。

后续操作就是--客户端（浏览器）带Cookie当钥匙，服务端验证







##### 1.2 Cookie生命周期

如果 `Cookie` 是会话 `Cookie`，那么当浏览器关闭时，`Cookie` 会被删除。

如果 `Cookie` 是持久 `Cookie`，则会按照指定的 `Expires` 或 `Max-Age` 设置的时间存储，直到时间到了才失效。





##### **1.3 为什么这一切都是自动化进行的？**

是因为 cookie 它是 HTP 协议当中所支持的技术，而各大浏览器厂商都支持了这一标准。在 HTTP 协议官方给我们提供了一个响应头和请求头：

- 响应头 Set-Cookie ：设置Cookie数据的

- 请求头 Cookie：携带Cookie数据的



![16-1](picture/16-1.png)

##### 1.4 **代码测试**



```java
@Slf4j
@RestController
public class SessionController {

    //设置Cookie
    @GetMapping("/c1")
    public Result cookie1(HttpServletResponse response){
        response.addCookie(new Cookie("login_username","itheima")); //设置Cookie/响应Cookie
        return Result.success();
    }
	
    //获取Cookie
    @GetMapping("/c2")
    public Result cookie2(HttpServletRequest request){
        Cookie[] cookies = request.getCookies();
        for (Cookie cookie : cookies) {
            if(cookie.getName().equals("login_username")){
                System.out.println("login_username: "+cookie.getValue()); //输出name为login_username的cookie
            }
        }
        return Result.success();
    }
}    
```
##### 

**优缺点**

- 优点：HTTP协议中支持的技术（像Set-Cookie 响应头的解析以及 Cookie 请求头数据的携带，都是浏览器自动进行的，是无需我们手动操作的）
- 缺点：
  - **移动端APP(Android、IOS)中无法使用Cookie**
  - 不安全，用户可以自己禁用Cookie
  - **Cookie不能跨域**

> 跨域介绍：
>
> ​	 ![16-2](picture/16-2.png) 
>
> - 现在的项目，大部分都是前后端分离的，前后端最终也会分开部署，前端部署在服务器 192.168.150.200 上，端口 80，后端部署在 192.168.150.100上，端口 8080
> - 我们打开浏览器直接访问前端工程，访问url：http://192.168.150.200/login.html
> - 然后在该页面发起请求到服务端，而服务端所在地址不再是localhost，而是服务器的IP地址192.168.150.100，假设访问接口地址为：http://192.168.150.100:8080/login
> - 那此时就存在跨域操作了，因为我们是在 http://192.168.150.200/login.html 这个页面上访问了http://192.168.150.100:8080/login 接口
> - 此时如果服务器设置了一个Cookie，这个Cookie是不能使用的，因为Cookie无法跨域
>
> 
>
> 区分跨域的维度：
>
> - 协议
> - IP/协议
> - 端口
>
> 只要上述的三个维度有任何一个维度不同，那就是跨域操作
>
> 
>
> 举例：
>
> ​	http://192.168.150.200/login.html ----------> https://192.168.150.200/login   		[协议不同，跨域]
>
> ​	http://192.168.150.200/login.html ----------> http://192.168.150.100/login     		[IP不同，跨域]
>
> ​	http://192.168.150.200/login.html ----------> http://192.168.150.200:8080/login   [端口不同，跨域]
>
> ​    http://192.168.150.200/login.html ----------> http://192.168.150.200/login    		 [不跨域]
>
> 另外举例：
>
> **不算跨域**：`https://www.example.com/employee` 和 `https://www.example.com/department`（只是路径不同，域名相同）
>
> **算跨域**：`https://www.example.com/employee` 和 `https://api.example.com/department`（域名不同）



#### 2. session

我们提到Session，它是服务器端会话跟踪技术，所以它是存储在服务器端的。而 **Session 的底层其实就是基于我们刚才所介绍的 Cookie 来实现的。**



##### 2.1 具体原理：

1. **首次请求**：
   - 当客户端（浏览器）第一次访问服务器时，服务器会为这个客户端创建一个 **Session**（`通常会有一个唯一的 Session ID 标识这个会话`）。
   - 服务器会将这个 Session ID 存储在服务器端，同时通过 HTTP 响应中的 **Set-Cookie** 头将这个 **Session ID** 作为一个 Cookie 返回给客户端。
2. **后续请求**：
   - 在客户端后续的请求中，浏览器会自动携带之前收到的 Cookie，其中包含的 **Session ID** 。
   - 服务器接收到请求时，通过 Cookie 中的 **Session ID** 来识别是哪一个客户端发来的请求，并根据该 **Session ID** 找到对应的服务器端的 **Session** 数据，确定用户的状态或数据。

##### 2.2 举例说明：

1. **第一次访问**：用户首次登录时，服务器创建一个 Session，并通过 Cookie 将 **Session ID** 发送给客户端，比如：

   ```http
   Set-Cookie: JSESSIONID=1234567890; Path=/; HttpOnly
   ```

   这里的 `JSESSIONID=1234567890` 就是服务器生成的 **Session ID**。

2. **后续请求**：浏览器在每次请求时，都会自动将这个 **Session ID** 通过 Cookie 发送给服务器：

   ```http
   Cookie: JSESSIONID=1234567890
   ```

   服务器通过这个 Session ID，找到对应的用户 Session 数据，从而保持用户的登录状态或其他信息。



#### 3 cookie/session 区别

Session 和 Cookie 的主要区别可以简单归纳为以下几点：

1. **存储位置不同：**
   - **Cookie** 是存储在**客户端（浏览器）**上的小数据文件，浏览器会自动在每次请求时发送给服务器。
   - **Session** 是存储在**服务器端**的，客户端只是保存一个唯一的 Session ID，通过这个 ID 来访问服务器上存储的数据。
2. **安全性不同：**
   - **Cookie** 是存储在客户端的，所以数据容易被用户或攻击者篡改，不适合存放敏感信息。
   - **Session** 存在服务器端，客户端只传递 Session ID，相对更安全，尤其是涉及敏感数据时。
3. **生命周期不同：**
   - **Cookie** 可以设置过期时间，可能会保存在客户端较长时间，甚至浏览器关闭后仍然存在（持久 Cookie）。
   - **Session** 通常在浏览器关闭时或会话结束后就失效，服务器上会定期清理过期的 Session。
4. **传输方式：**
   - **Cookie** 会随每个请求自动发送给服务器，占用一定带宽。
   - **Session** 只需要发送一个小小的 Session ID，而实际的数据保存在服务器，不占用额外的传输带宽。

简单来说，**Cookie 适合存储一些不太敏感的、客户端可以操作的数据**，而 **Session 更适合存储需要安全管理的会话数据**，比如用户的登录状态。





如果只看登录校验这个功能：

**Cookie 校验**：服务器`不需要存储用户的登录状态`，登录信息或 Token 直接存在客户端 Cookie 中，服务器通过验证 Cookie 中的信息来识别用户。

**Session 校验**：`服务器维护用户的登录状态`，客户端通过 Session ID 与服务器关联，服务器从 Session 中读取用户状态。







#### 4. 令牌技术JWT

这里我们所提到的令牌，其实它就是一个用户身份的标识，看似很高大上，很神秘，其实本质就是一个字符串。



**优缺点**

- 优点：
  - 支持PC端、移动端
  - 解决集群环境下的认证问题
  - 减轻服务器的存储压力（无需在服务器端存储）
- `**缺点：需要自己实现（包括令牌的生成、令牌的传递、令牌的校验）**`





JWT全称：JSON Web Token  （官网：https://jwt.io/）

- 定义了一种简洁的、自包含的格式，用于在通信双方以json数据格式安全的传输信息。由于数字签名的存在，这些信息是可靠的。

  > 简洁：是指**jwt就是一个简单的字符串。可以在请求参数或者是请求头当中直接传递。**
  >
  > 自包含：指的是jwt令牌，看似是一个随机的字符串，但是我们是可以根据自身的需求在jwt令牌中存储自定义的数据内容。如：可以直接在jwt令牌中存储用户的相关信息。
  >
  > 简单来讲，jwt就是将原始的json数据格式进行了安全的封装，这样就可以直接基于jwt在通信双方安全的进行信息传输了。



##### 4.1 生成和校验

首先我们先来实现JWT令牌的生成。要想使用JWT令牌，需要先引入JWT的依赖：

~~~xml
<!-- JWT依赖-->
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.9.1</version>
</dependency>
~~~

> 在引入完JWT来赖后，就可以调用工具包中提供的API来完成JWT令牌的生成和校验
>
> 工具类：Jwts



生成JWT代码实现：

~~~java
@Test
public void genJwt(){
    Map<String,Object> claims = new HashMap<>();
    claims.put("id",1);
    claims.put("username","Tom");
    
    String jwt = Jwts.builder()
        .setClaims(claims) //自定义内容(载荷)          
        .signWith(SignatureAlgorithm.HS256, "itheima") //签名算法        
        .setExpiration(new Date(System.currentTimeMillis() + 24*3600*1000)) //有效期   
        .compact();
    
    System.out.println(jwt);
}
~~~

运行测试方法：

~~~
eyJhbGciOiJIUzI1NiJ9.eyJpZCI6MSwiZXhwIjoxNjcyNzI5NzMwfQ.fHi0Ub8npbyt71UqLXDdLyipptLgxBUg_mSuGJtXtBk
~~~

实现了JWT令牌的生成，下面我们接着使用Java代码来校验JWT令牌(解析生成的令牌)：

~~~java
@Test
public void parseJwt(){
    Claims claims = Jwts.parser()
        .setSigningKey("itheima")//指定签名密钥（必须保证和生成令牌时使用相同的签名密钥）  
	    .parseClaimsJws("eyJhbGciOiJIUzI1NiJ9.eyJpZCI6MSwiZXhwIjoxNjcyNzI5NzMwfQ.fHi0Ub8npbyt71UqLXDdLyipptLgxBUg_mSuGJtXtBk")
        .getBody();

    System.out.println(claims);
}
~~~

运行测试方法：

~~~
{id=1, exp=1672729730}
~~~

> 令牌解析后，我们可以看到id和过期时间，如果在解析的过程当中没有报错，就说明解析成功了。





如果过期和篡改令牌，都是解析不出来的



##### 4.2 登录下发令牌

JWT令牌的生成和校验的基本操作我们已经学习完了，接下来我们就需要在案例当中通过JWT令牌技术来跟踪会话。具体的思路我们前面已经分析过了，主要就是两步操作：

1. **生成令牌**
   - 在登录成功之后来生成一个JWT令牌，并且把这个令牌直接返回给前端
2. **校验令牌**
   - 拦截前端请求，从请求中获取到令牌，对令牌进行解析校验



JWT令牌怎么返回给前端呢？此时我们就需要再来看一下接口文档当中关于登录接口的描述（主要看响应数据）：

- 响应数据

  参数格式：application/json

  参数说明：

  | 名称 | 类型   | 是否必须 | 默认值 | 备注                     | 其他信息 |
  | ---- | ------ | -------- | ------ | ------------------------ | -------- |
  | code | number | 必须     |        | 响应码, 1 成功 ; 0  失败 |          |
  | msg  | string | 非必须   |        | 提示信息                 |          |
  | data | string | 必须     |        | 返回的数据 , jwt令牌     |          |

  响应数据样例：

  ~~~json
  {
    "code": 1,
    "msg": "success",
    "data": "eyJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoi6YeR5bq4IiwiaWQiOjEsInVzZXJuYW1lIjoiamlueW9uZyIsImV4cCI6MTY2MjIwNzA0OH0.KkUc_CXJZJ8Dd063eImx4H9Ojfrr6XMJ-yVzaWCVZCo"
  }
  ~~~



**实现步骤：**

1. 引入JWT工具类
   - 在项目工程下创建com.itheima.utils包，并把提供JWT工具类复制到该包下
2. 登录完成后，调用工具类生成JWT令牌并返回



**JWT工具类**

~~~java
public class JwtUtils {

    private static String signKey = "itheima";//签名密钥
    private static Long expire = 43200000L; //有效时间

    /**
     * 生成JWT令牌
     * @param claims JWT第二部分负载 payload 中存储的内容
     * @return
     */
    public static String generateJwt(Map<String, Object> claims){
        String jwt = Jwts.builder()
                .addClaims(claims)//自定义信息（有效载荷）
                .signWith(SignatureAlgorithm.HS256, signKey)//签名算法（头部）
                .setExpiration(new Date(System.currentTimeMillis() + expire))//过期时间
                .compact();
        return jwt;
    }

    /**
     * 解析JWT令牌
     * @param jwt JWT令牌
     * @return JWT第二部分负载 payload 中存储的内容
     */
    public static Claims parseJWT(String jwt){
        Claims claims = Jwts.parser()
                .setSigningKey(signKey)//指定签名密钥
                .parseClaimsJws(jwt)//指定令牌Token
                .getBody();
        return claims;
    }
}

~~~



**登录成功，生成JWT令牌并返回**

~~~java
@RestController
@Slf4j
public class LoginController {
    //依赖业务层对象
    @Autowired
    private EmpService empService;

    @PostMapping("/login")
    public Result login(@RequestBody Emp emp) {
        //调用业务层：登录功能
        Emp loginEmp = empService.login(emp);

        //判断：登录用户是否存在
        if(loginEmp !=null ){
            //自定义信息
            Map<String , Object> claims = new HashMap<>();
            claims.put("id", loginEmp.getId());
            claims.put("username",loginEmp.getUsername());
            claims.put("name",loginEmp.getName());

            //使用JWT工具类，生成身份令牌
            String token = JwtUtils.generateJwt(claims);
            return Result.success(token);
        }
        return Result.error("用户名或密码错误");
    }
}
~~~



到这里已经实现了令牌的生成 

在每次发起请求的时候，前端页面已经有一个token了

但是还没有拦截



#### 5. 过滤器filter

那怎么样来统一拦截到所有的请求校验令牌的有效性呢？这里我们会学习两种解决方案：

1. Filter过滤器
2. Interceptor拦截器

我们首先来学习过滤器Filter。



什么是Filter？

- Filter表示过滤器，是 JavaWeb三大组件(Servlet、Filter、Listener)之一。
- 过滤器可以把对资源的请求拦截下来，从而实现一些特殊的功能
  - 使用了过滤器之后，要想访问web服务器上的资源，必须先经过滤器，过滤器处理完毕之后，才可以访问对应的资源。
- 过滤器一般完成一些通用的操作，比如：登录校验、统一编码处理、敏感字符处理等。

![16-3](picture/16-3.png) 



下面我们通过Filter快速入门程序掌握过滤器的基本使用操作：

- 第1步，定义过滤器 ：1.定义一个类，实现 Filter 接口，并重写其所有方法。
- 第2步，配置过滤器：Filter类上加 @WebFilter 注解，配置拦截资源的路径。引导类上加 @ServletComponentScan 开启Servlet组件支持



> - init方法：过滤器的初始化方法。在web服务器启动的时候会自动的创建Filter过滤器对象，在创建过滤器对象的时候会自动调用init初始化方法，这个方法只会被调用一次。
>
> - doFilter方法：这个方法是在每一次拦截到请求之后都会被调用，所以这个方法是会被调用多次的，每拦截到一次请求就会调用一次doFilter()方法。
>
> - destroy方法： 是销毁的方法。当我们关闭服务器的时候，它会自动的调用销毁方法destroy，而这个销毁方法也只会被调用一次。



在定义完Filter之后，Filter其实并不会生效，还需要完成Filter的配置，Filter的配置非常简单，只需要在Filter类上添加一个注解：@WebFilter，并指定属性urlPatterns，通过这个属性指定过滤器要拦截哪些请求

~~~java
@WebFilter(urlPatterns = "/*") //配置过滤器要拦截的请求路径（ /* 表示拦截浏览器的所有请求 ）
public class DemoFilter implements Filter {
    @Override //初始化方法, 只调用一次
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("init 初始化方法执行了");
    }

    @Override //拦截到请求之后调用, 调用多次
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        System.out.println("Demo 拦截到了请求...放行前逻辑");
        //放行
        chain.doFilter(request,response);
    }

    @Override //销毁方法, 只调用一次
    public void destroy() {
        System.out.println("destroy 销毁方法执行了");
    }
}
~~~

当我们在Filter类上面加了@WebFilter注解之后，接下来我们还需要在启动类上面加上一个注解@ServletComponentScan，通过这个@ServletComponentScan注解来开启SpringBoot项目对于Servlet组件的支持。

~~~java
@ServletComponentScan
@SpringBootApplication
public class TliasWebManagementApplication {

    public static void main(String[] args) {
        SpringApplication.run(TliasWebManagementApplication.class, args);
    }

}
~~~







##### 5.1 执行流程



![16-1](picture/16-1.png)



过滤器当中我们拦截到了请求之后，如果希望继续访问后面的web资源，就要执行放行操作，

**放行就是调用 FilterChain对象当中的doFilter()方法**

~~~java
@WebFilter(urlPatterns = "/*") 
public class DemoFilter implements Filter {
    
    @Override //初始化方法, 只调用一次
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("init 初始化方法执行了");
    }
    
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        
        System.out.println("DemoFilter   放行前逻辑.....");

        //放行请求
        filterChain.doFilter(servletRequest,servletResponse);

        System.out.println("DemoFilter   放行后逻辑.....");
        
    }

    @Override //销毁方法, 只调用一次
    public void destroy() {
        System.out.println("destroy 销毁方法执行了");
    }
}
~~~

##### 5.2 拦截路径

执行流程我们搞清楚之后，接下来再来介绍一下过滤器的拦截路径，Filter可以根据需求，配置不同的拦截资源路径：

| 拦截路径     | urlPatterns值 | 含义                               |
| ------------ | ------------- | ---------------------------------- |
| 拦截具体路径 | /login        | 只有访问 /login 路径时，才会被拦截 |
| 目录拦截     | /emps/*       | 访问/emps下的所有资源，都会被拦截  |
| 拦截所有     | /*            | 访问所有资源，都会被拦截           |

下面我们来测试"拦截具体路径"：

~~~java
@WebFilter(urlPatterns = "/login")  //拦截/login具体路径
public class DemoFilter implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("DemoFilter   放行前逻辑.....");

        //放行请求
        filterChain.doFilter(servletRequest,servletResponse);

        System.out.println("DemoFilter   放行后逻辑.....");
    }


    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        Filter.super.init(filterConfig);
    }

    @Override
    public void destroy() {
        Filter.super.destroy();
    }
}
~~~



其他的照猫画虎即可 已经自行编程测试了  很好掌握



##### 5.3 过滤器链条

所谓过滤器链指的是在一个web应用程序当中，可以配置多个过滤器，多个过滤器就形成了一个过滤器链。

![16-6](picture/16-6.png)

现在，在filter文件夹下新建一个Abcfilter

```

```

![16-7](picture/16-7.png)



通过控制台日志的输出，**发现AbcFilter先执行DemoFilter后执行，**这是为什么呢？



其实就是名称的问题，A在D前面

假如我们想让DemoFilter先执行，怎么办呢？答案就是修改类名。测试：修改AbcFilter类名为XbcFilter，运行程序查看控制台日志

这里就是Demo先执行了





#### 6. 登录校验Filter

大概清楚了在Filter过滤器的实现步骤了，那在正式开发登录校验过滤器之前，我们思考两个问题：

1. 所有的请求，拦截到了之后，都需要校验令牌吗？
   - 答案：**登录请求例外**

2. 拦截到请求后，什么情况下才可以放行，执行业务操作？
   - 答案：**有令牌，且令牌校验通过(合法)；否则都返回未登录错误结果**



![16-8](picture/16-8.png)



基于上面的业务流程，我们分析出具体的操作步骤：

1. 获取请求url
2. 判断请求url中是否包含login，如果包含，说明是登录操作，放行
3. 获取请求头中的令牌（token）
4. 判断令牌是否存在，如果不存在，返回错误结果（未登录）
5. 解析token，如果解析失败，返回错误结果（未登录）
6. 放行

**登录校验过滤器：LoginCheckFilter**

~~~java
@Slf4j
@WebFilter(urlPatterns = "/*") //拦截所有请求
public class LoginCheckFilter implements Filter {

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain chain) throws IOException, ServletException {
        //前置：强制转换为http协议的请求对象、响应对象 （转换原因：要使用子类中特有方法）
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        HttpServletResponse response = (HttpServletResponse) servletResponse;

        //1.获取请求url
        String url = request.getRequestURL().toString();
        log.info("请求路径：{}", url); //请求路径：http://localhost:8080/login


        //2.判断请求url中是否包含login，如果包含，说明是登录操作，放行
        if(url.contains("/login")){
            chain.doFilter(request, response);//放行请求
            return;//结束当前方法的执行
        }


        //3.获取请求头中的令牌（token）
        String token = request.getHeader("token");
        log.info("从请求头中获取的令牌：{}",token);


        //4.判断令牌是否存在，如果不存在，返回错误结果（未登录）
        if(!StringUtils.hasLength(token)){
            log.info("Token不存在");

            Result responseResult = Result.error("NOT_LOGIN");
            //把Result对象转换为JSON格式字符串 (fastjson是阿里巴巴提供的用于实现对象和json的转换工具类)
            String json = JSONObject.toJSONString(responseResult);
            response.setContentType("application/json;charset=utf-8");
            //响应
            response.getWriter().write(json);

            return;
        }

        //5.解析token，如果解析失败，返回错误结果（未登录）
        try {
            JwtUtils.parseJWT(token);
        }catch (Exception e){
            log.info("令牌解析失败!");

            Result responseResult = Result.error("NOT_LOGIN");
            //把Result对象转换为JSON格式字符串 (fastjson是阿里巴巴提供的用于实现对象和json的转换工具类)
            String json = JSONObject.toJSONString(responseResult);
            response.setContentType("application/json;charset=utf-8");
            //响应
            response.getWriter().write(json);

            return;
        }


        //6.放行
        chain.doFilter(request, response);

    }
}
~~~

在上述过滤器的功能实现中，我们使用到了一个第三方json处理的工具包fastjson。我们要想使用，需要引入如下依赖：

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.76</version>
</dependency>
```



登录校验的过滤器我们编写完成了，接下来我们就可以重新启动服务来做一个测试：

> 测试前先把之前所编写的测试使用的过滤器，暂时注释掉。直接将@WebFilter注解给注释掉即可。





#### 7. 拦截器Interceptor

大部分和filter差不多  注册+自定义 

就不多加赘述了

**自定义拦截器：**实现HandlerInterceptor接口，并重写其所有方法

~~~java
//自定义拦截器
@Component
public class LoginCheckInterceptor implements HandlerInterceptor {
    //目标资源方法执行前执行。 返回true：放行    返回false：不放行
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle .... ");
        
        return true; //true表示放行
    }

    //目标资源方法执行后执行
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle ... ");
    }

    //视图渲染完毕后执行，最后执行
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion .... ");
    }
}
~~~



> 注意：
>
> ​	preHandle方法：目标资源方法执行前执行。 返回true：放行    返回false：不放行
>
> ​	postHandle方法：目标资源方法执行后执行
>
> ​	afterCompletion方法：视图渲染完毕后执行，最后执行



**注册配置拦截器**：实现WebMvcConfigurer接口，并重写addInterceptors方法

~~~java
@Configuration  
public class WebConfig implements WebMvcConfigurer {

    //自定义的拦截器对象
    @Autowired
    private LoginCheckInterceptor loginCheckInterceptor;

    
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
       //注册自定义拦截器对象
        registry.addInterceptor(loginCheckInterceptor).addPathPatterns("/**");//设置拦截器拦截的请求路径（ /** 表示拦截所有请求）
    }
}
~~~



**执行流程**

![16-9](picture/16-9.png)



- ### **拦截器 vs 过滤器的区别**：

  | 方面         | 拦截器（Interceptor）                        | 过滤器（Filter）                          |
  | ------------ | -------------------------------------------- | ----------------------------------------- |
  | **所在层次** | 基于 Spring MVC 层次，作用于控制器之前和之后 | 基于 Servlet 层次，作用于请求链的早期阶段 |
  | **依赖性**   | 依赖于 Spring 框架                           | 不依赖框架，依赖于 Servlet 规范           |
  | **拦截范围** | 只拦截控制器的请求路径                       | 可以拦截所有类型的请求，包括静态资源      |
  | **使用场景** | 业务逻辑相关的任务，如登录校验、权限控制     | 全局性任务，如编码设置、跨域处理          |
  | **执行顺序** | 在过滤器之后执行                             | 在拦截器之前执行                          |
  | **配置方式** | Spring 配置或注解                            | web.xml 或 Spring 的配置                  |

##### **在登录校验上用拦截器还是过滤器？**

对于 **登录校验**，一般推荐使用 **拦截器（Interceptor）**。理由如下：

1. **与业务逻辑的紧密结合**：
   - 登录校验是一个业务相关的操作，拦截器可以在请求到达控制器之前进行处理，比较符合 MVC 模式中的处理逻辑。它能获取到和控制器相关的更多信息（如 `Handler` 对象）。
2. **灵活的路径控制**：
   - 拦截器可以精确地控制哪些 URL 需要登录校验，哪些 URL 可以放行。这对于处理需要细粒度权限控制的应用非常重要。你可以为不同的 URL 配置不同的拦截器，而过滤器一般用于全局性操作，难以精确区分路径。
3. **Spring MVC 的支持**：
   - 拦截器是 Spring MVC 框架中的组件，它与 Spring 框架深度集成。登录校验往往需要用到 Spring 管理的 Bean（如用户服务、权限服务），拦截器可以直接获取这些 Bean 并使用。而过滤器由于在更底层，无法直接访问 Spring 容器中的 Bean。



#### 8.  异常处理器（让返回给前端的信息标准化）

当我们没有做任何的异常处理时，我们三层架构处理异常的方案：

- Mapper接口在操作数据库的时候出错了，此时异常会往上抛(谁调用Mapper就抛给谁)，会抛给service。 
- service 中也存在异常了，会抛给controller。
- 而在controller当中，我们也没有做任何的异常处理，所以最终异常会再往上抛。最终抛给框架之后，框架就会返回一个JSON格式的数据，里面封装的就是错误的信息，但是框架返回的JSON格式的数据并不符合我们的开发规范。





那么在三层构架项目中，出现了异常，该如何处理?

- 方案一：在所有Controller的所有方法中进行try…catch处理
  - 缺点：代码臃肿（不推荐）
- **方案二：全局异常处理器**
  - **好处：简单、优雅（推荐）**



我们该怎么样定义全局异常处理器？

- 定义全局异常处理器非常简单，就是定义一个类，在类上加上一个注解@RestControllerAdvice，加上这个注解就代表我们定义了一个全局异常处理器。
- 在全局异常处理器当中，需要定义一个方法来捕获异常，在这个方法上需要加上注解@ExceptionHandler。通过@ExceptionHandler注解当中的value属性来指定我们要捕获的是哪一类型的异常。



新建一个exception文件夹：

~~~java
@RestControllerAdvice
public class GlobalExceptionHandler {

    //处理异常
    @ExceptionHandler(Exception.class) //指定能够处理的异常类型
    public Result ex(Exception e){
        e.printStackTrace();//打印堆栈中的异常信息

        //捕获到异常之后，响应一个标准的Result
        return Result.error("对不起,操作失败,请联系管理员");
    }
}
~~~

> @RestControllerAdvice = @ControllerAdvice + @ResponseBody
>
> 处理异常的方法返回值会转换为json后再响应给前端



重新启动SpringBoot服务，打开浏览器，再来测试一下添加部门这个操作，我们依然添加已存在的 "就业部" 这个部门：



此时，我们可以看到，出现异常之后，异常已经被全局异常处理器捕获了。然后返回的错误信息，被前端程序正常解析，然后提示出了对应的错误提示信息。





以上就是全局异常处理器的使用，主要涉及到两个注解：

- @RestControllerAdvice  //表示当前类为全局异常处理器
- @ExceptionHandler  //指定可以捕获哪种类型的异常进行处理









## 															AOP事务



#### 1 事务管理

在数据库阶段我们已学习过事务了，我们讲到：

**事务**是一组操作的集合，它是一个不可分割的工作单位。事务会把所有的操作作为一个整体，一起向数据库提交或者是撤销操作请求。所以这组操作要么同时成功，要么同时失败。



怎么样来控制这组操作，让这组操作同时成功或同时失败呢？此时就要涉及到事务的具体操作了。

事务的操作主要有三步：

1. 开启事务（一组操作开始前，开启事务）：start transaction / begin ;
2. 提交事务（这组操作全部成功后，提交事务）：commit ;
3. 回滚事务（中间任何一个操作出现异常，回滚事务）：rollback ;



##### 1.1 spring事务管理

需求：当部门解散了不仅需要把部门信息删除了，还需要把该部门下的员工数据也删除了。

步骤：

- 根据ID删除部门数据
- 根据部门ID删除该部门下的员工

代码实现：

1. **DeptServiceImpl  加点异常进去**

~~~java
@Slf4j
@Service
public class DeptServiceImpl implements DeptService {
    @Autowired
    private DeptMapper deptMapper;

    @Autowired
    private EmpMapper empMapper;


    //根据部门id，删除部门信息及部门下的所有员工
    @Override
    public void delete(Integer id){
        //根据部门id删除部门信息
        deptMapper.deleteById(id);
        
        //模拟：异常发生
        int i = 1/0;

        //删除部门下的所有员工信息
        empMapper.deleteByDeptId(id);   
    }
}
~~~

很显然，这里的两个mapper操作没有联系到一起，由于添加了一个无法执行的操作，所以后续删除员工信息的操作无法执行



##### 1.2 解决办法--transactional注解

原因：

- 先执行根据id删除部门的操作，这步执行完毕，数据库表 dept 中的数据就已经删除了。
- 执行 1/0 操作，抛出异常
- 抛出异常之前，下面所有的代码都不会执行了，根据部门ID删除该部门下的员工，这个操作也不会执行 。

此时就出现问题了，部门删除了，部门下的员工还在，业务操作前后数据不一致。

通过事务来实现，因为一个事务中的多个业务操作，要么全部成功，要么全部失败。



此时，我们就需要在delete删除业务功能中添加事务。



> @Transactional作用：就是在当前这个方法执行开始之前来开启事务，方法执行完毕之后提交事务。如果在这个方法执行的过程当中出现了异常，就会进行事务的回滚操作。
>
> @Transactional注解：我们一般会在业务层当中来控制事务，因为在业务层当中，一个业务功能可能会包含多个数据访问的操作。在业务层来控制事务，我们就可以将多个数据访问操作控制在一个事务范围内。



@Transactional注解书写位置：

- 方法
  - 当前方法交给spring进行事务管理
- 类
  - 当前类中所有的方法都交由spring进行事务管理
- 接口
  - 接口下所有的实现类当中所有的方法都交给spring 进行事务管理



接下来，我们就可以在业务方法delete上加上 @Transactional 来控制事务 。

```java
@Slf4j
@Service
public class DeptServiceImpl implements DeptService {
    @Autowired
    private DeptMapper deptMapper;

    @Autowired
    private EmpMapper empMapper;

    
    @Override
    @Transactional  //当前方法添加了事务管理
    public void delete(Integer id){
        //根据部门id删除部门信息
        deptMapper.deleteById(id);
        
        //模拟：异常发生
        int i = 1/0;

        //删除部门下的所有员工信息
        empMapper.deleteByDeptId(id);   
    }
}
```



#### 2. rollbackFor---扩充捕获异常类型

默认情况下，只有出现RuntimeException(运行时异常)才会回滚事务。

假如我们想让所有的异常都回滚，**需要来配置@Transactional注解当中的rollbackFor属性，通过rollbackFor这个属性可以指定出现何种异常类型回滚事务。**

~~~java
@Slf4j
@Service
public class DeptServiceImpl implements DeptService {
    @Autowired
    private DeptMapper deptMapper;

    @Autowired
    private EmpMapper empMapper;

    
    @Override
    @Transactional(rollbackFor=Exception.class)
    public void delete(Integer id){
        //根据部门id删除部门信息
        deptMapper.deleteById(id);
        
        //模拟：异常发生
        int num = id/0;

        //删除部门下的所有员工信息
        empMapper.deleteByDeptId(id);   
    }
}
~~~

就只是加了这一行而已





> 结论：
>
> - 在Spring的事务管理中，默认只有运行时异常 RuntimeException才会回滚。
> - 如果还需要回滚指定类型的异常，可以通过rollbackFor属性来指定。





#### 3. propagation

我们接着继续学习@Transactional注解当中的第二个属性propagation，这个属性是用来配置事务的传播行为的。

什么是事务的传播行为呢？

- 就是当一个事务方法被另一个事务方法调用时，这个事务方法应该如何进行事务控制。



例如：两个事务方法，一个A方法，一个B方法。在这两个方法上都添加了@Transactional注解，就代表这两个方法都具有事务，而在A方法当中又去调用了B方法。

![16-10](picture/16-10.png)

所谓事务的传播行为，指的就是在A方法运行的时候，首先会开启一个事务，在A方法当中又调用了B方法， B方法自身也具有事务，那么B方法在运行的时候，到底是加入到A方法的事务当中来，还是B方法在运行的时候新建一个事务？这个就涉及到了事务的传播行为。

我们要想控制事务的传播行为，在@Transactional注解的后面指定一个属性propagation，通过 propagation 属性来指定传播行为。接下来我们就来介绍一下常见的事务传播行为。

| **属性值**    | **含义**                                                     |
| ------------- | ------------------------------------------------------------ |
| REQUIRED      | 【默认值】需要事务，有则加入，无则创建新事务                 |
| REQUIRES_NEW  | 需要新事务，无论有无，总是创建新事务                         |
| SUPPORTS      | 支持事务，有则加入，无则在无事务状态中运行                   |
| NOT_SUPPORTED | 不支持事务，在无事务状态下运行,如果当前存在已有事务,则挂起当前事务 |
| MANDATORY     | 必须有事务，否则抛异常                                       |
| NEVER         | 必须没事务，否则抛异常                                       |
| …             |                                                              |

> 对于这些事务传播行为，我们只需要关注以下两个就可以了：
>
> 1. REQUIRED（默认值）
> 2. REQUIRES_NEW------这个方法的应用场景是：A方法失败的时候B也得运行，就需要创建 新事务了，不然B会随着A一起失败



举例：对于删除功能（A），往往需要记录操作日志（B）。

这里就必须用REQUIRES_NEW了



## 															AOP基础

#### 1.  AOP概述

什么是AOP？

- AOP英文全称：Aspect Oriented Programming（面向切面编程、面向方面编程），其实说白了，面向切面编程就是面向特定方法编程。 

然而有一些业务功能执行效率比较低，执行耗时较长，我们需要针对于这些业务方法进行优化。 那首先第一步就需要定位出执行耗时比较长的业务方法，再针对于业务方法再来进行优化。

此时我们就需要统计当前这个项目当中每一个业务方法的执行耗时。那么统计每一个业务方法的执行耗时该怎么实现？

可能多数人首先想到的就是在每一个业务方法运行之前，记录这个方法运行的开始时间。在这个方法运行完毕之后，再来记录这个方法运行的结束时间。拿结束时间减去开始时间，不就是这个方法的执行耗时吗？



![17-1](picture/17-1.png)



以上分析的实现方式是可以解决需求问题的。但是对于一个项目来讲，里面会包含很多的业务模块，每个业务模块又包含很多增删改查的方法，**如果我们要在每一个模块下的业务方法中，添加记录开始时间、结束时间、计算执行耗时的代码，就会让程序员的工作变得非常繁琐。**





而AOP面向方法编程，就可以做到在不改动这些原始方法的基础上，针对特定的方法进行功能的增强。

> AOP的作用：在程序运行期间在不修改源代码的基础上对已有方法进行增强（无侵入性: 解耦）

我们要想完成统计各个业务方法执行耗时的需求，我们只需要定义一个模板方法，将记录方法执行耗时这一部分公共的逻辑代码，定义在模板方法当中，在这个方法开始运行之前，来记录这个方法运行的开始时间，在方法结束运行的时候，再来记录方法运行的结束时间，中间就来运行原始的业务方法。



AOP的优势：

1. 减少重复代码
2. 提高开发效率
3. 维护方便



#### 2. AOP快速入门

在了解了什么是AOP后，我们下面通过一个快速入门程序，体验下AOP的开发，并掌握Spring中AOP的开发步骤。

**需求：**统计各个业务层方法执行耗时。

**实现步骤：**

1. 导入依赖：在pom.xml中导入AOP的依赖
2. 编写AOP程序：针对于特定方法根据业务需要进行编程

> 为演示方便，可以自建新项目或导入提供的`springboot-aop-quickstart`项目工程



**pom.xml**

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
~~~



**AOP程序：TimeAspect**

注意，这个一定要放在com.itheima下面的文件夹下 不然扫描不到

~~~java
@Component
@Aspect //当前类为切面类
@Slf4j
public class TimeAspect {

    @Around("execution(* com.itheima.service.*.*(..))") 
    public Object recordTime(ProceedingJoinPoint pjp) throws Throwable {
        //记录方法执行开始时间
        long begin = System.currentTimeMillis();

        //执行原始方法
        Object result = pjp.proceed();

        //记录方法执行结束时间
        long end = System.currentTimeMillis();

        //计算方法执行耗时
        log.info(pjp.getSignature()+"执行耗时: {}毫秒",end-begin);

        return result;
    }
}
~~~

#### 3. 核心概念

通过SpringAOP的快速入门，感受了一下AOP面向切面编程的开发方式。下面我们再来学习AOP当中涉及到的一些核心概念。



**1. 连接点：JoinPoint**，可以被AOP控制的方法（暗含方法执行时的相关信息）

​	连接点指的是可以被aop控制的方法。例如：入门程序当中所有的业务方法都是可以被aop控制的方法。

​	![17-2](picture/17-2.png)

​	在SpringAOP提供的JoinPoint当中，封装了连接点方法在执行时的相关信息。（后面会有具体的讲解）



**2. 通知：Advice**，指哪些重复的逻辑，也就是共性功能（最终体现为一个方法）

​	在入门程序中是需要统计各个业务方法的执行耗时的，此时我们就需要在这些业务方法运行开始之前，先记录这个方法运行的开始时间，在每一个业务方法运行结束的时候，再来记录这个方法运行的结束时间。

​	但是在AOP面向切面编程当中，我们只需要将这部分重复的代码逻辑抽取出来单独定义。抽取出来的这一部分重复的逻辑，也就是共性的功能。

​	![17-3](picture/17-3.png)

​	

**3. 切入点：PointCut**，匹配连接点的条件，通知仅会在切入点方法执行时被应用

​	在通知当中，我们所定义的共性功能到底要应用在哪些方法上？此时就涉及到了切入点pointcut概念。切入点指的是匹配连接点的条件。通知仅会在切入点方法运行时才会被应用。

​	在aop的开发当中，我们通常会通过一个切入点表达式来描述切入点(后面会有详解)。

​	 ![17-4](picture/17-4.png)

​	假如：切入点表达式改为DeptServiceImpl.list()，此时就代表仅仅只有list这一个方法是切入点。只有list()方法在运行的时候才会应用通知。

​	

**4. 切面：Aspect**，描述通知与切入点的对应关系（通知+切入点）

​	当通知和切入点结合在一起，就形成了一个切面。通过切面就能够描述当前aop程序需要针对于哪个原始方法，在什么时候执行什么样的操作。

​	 ![17-5](picture/17-5.png)

​	切面所在的类，我们一般称为**切面类**（被@Aspect注解标识的类）

​	

**5. 目标对象：Target**，通知所应用的对象

​	目标对象指的就是通知所应用的对象，我们就称之为目标对象。

​	![17-6](picture/17-6.png) 





AOP的核心概念我们介绍完毕之后，接下来我们再来分析一下我们所定义的通知是如何与目标对象结合在一起，对目标对象当中的方法进行功能增强的。

![17-7](picture/17-7.png) 

Spring的AOP底层是基于动态代理技术来实现的，也就是说在程序运行的时候，会自动的基于动态代理技术为目标对象生成一个对应的代理对象。在代理对象当中就会对目标对象当中的原始方法进行功能的增强。



#### 4. AOP进阶

AOP的基础知识学习完之后，下面我们对AOP当中的各个细节进行详细的学习。主要分为4个部分：

1. 通知类型
2. 通知顺序
3. 切入点表达式
4. 连接点

我们先来学习第一部分通知类型。





##### 4.1 通知类型---看一下备注就懂了

在入门程序当中，我们已经使用了一种功能最为强大的通知类型：Around环绕通知。

~~~java
@Around("execution(* com.itheima.service.*.*(..))")
public Object recordTime(ProceedingJoinPoint pjp) throws Throwable {
    //记录方法执行开始时间
    long begin = System.currentTimeMillis();
    //执行原始方法
    Object result = pjp.proceed();
    //记录方法执行结束时间
    long end = System.currentTimeMillis();
    //计算方法执行耗时
    log.info(pjp.getSignature()+"执行耗时: {}毫秒",end-begin);
    return result;
}
~~~

> 只要我们在通知方法上加上了@Around注解，就代表当前通知是一个环绕通知。



Spring中AOP的通知类型：

- @Around：环绕通知，此注解标注的通知方法在目标方法前、后都被执行
- @Before：前置通知，此注解标注的通知方法在目标方法前被执行
- @After ：后置通知，此注解标注的通知方法在目标方法后被执行，无论是否有异常都会执行
- @AfterReturning ： 返回后通知，此注解标注的通知方法在目标方法后被执行，有异常不会执行
- @AfterThrowing ： 异常后通知，此注解标注的通知方法发生异常后执行



下面我们通过代码演示，来加深对于不同通知类型的理解：

~~~java
@Slf4j
@Component
@Aspect
public class MyAspect1 {
    //前置通知
    @Before("execution(* com.itheima.service.*.*(..))")
    public void before(JoinPoint joinPoint){
        log.info("before ...");

    }

    //环绕通知
    @Around("execution(* com.itheima.service.*.*(..))")
    public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        log.info("around before ...");

        //调用目标对象的原始方法执行
        Object result = proceedingJoinPoint.proceed();
        
        //原始方法如果执行时有异常，环绕通知中的后置代码不会在执行了
        
        log.info("around after ...");
        return result;
    }

    //后置通知
    @After("execution(* com.itheima.service.*.*(..))")
    public void after(JoinPoint joinPoint){
        log.info("after ...");
    }

    //返回后通知（程序在正常执行的情况下，会执行的后置通知）
    @AfterReturning("execution(* com.itheima.service.*.*(..))")
    public void afterReturning(JoinPoint joinPoint){
        log.info("afterReturning ...");
    }

    //异常通知（程序在出现异常的情况下，执行的后置通知）
    @AfterThrowing("execution(* com.itheima.service.*.*(..))")
    public void afterThrowing(JoinPoint joinPoint){
        log.info("afterThrowing ...");
    }
}

~~~

五种常见的通知类型，我们已经测试完毕了，此时我们再来看一下刚才所编写的代码，有什么问题吗？

~~~java
//前置通知
@Before("execution(* com.itheima.service.*.*(..))")

//环绕通知
@Around("execution(* com.itheima.service.*.*(..))")
  
//后置通知
@After("execution(* com.itheima.service.*.*(..))")

//返回后通知（程序在正常执行的情况下，会执行的后置通知）
@AfterReturning("execution(* com.itheima.service.*.*(..))")

//异常通知（程序在出现异常的情况下，执行的后置通知）
@AfterThrowing("execution(* com.itheima.service.*.*(..))")
~~~

我们发现啊，每一个注解里面都指定了切入点表达式，而且这些切入点表达式都一模一样。此时我们的代码当中就存在了大量的重复性的切入点表达式，假如此时切入点表达式需要变动，就需要将所有的切入点表达式一个一个的来改动，就变得非常繁琐了。



怎么来解决这个切入点表达式重复的问题？ 答案就是：**抽取**



Spring提供了@PointCut注解，该注解的作用是将公共的切入点表达式抽取出来，需要用到时引用该切入点表达式即可。

~~~java
@Slf4j
@Component
@Aspect
public class MyAspect1 {

    //切入点方法（公共的切入点表达式）
    @Pointcut("execution(* com.itheima.service.*.*(..))")
    private void pt(){

    }

    //前置通知（引用切入点）
    @Before("pt()")
    public void before(JoinPoint joinPoint){
        log.info("before ...");

    }

    //环绕通知
    @Around("pt()")
    public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        log.info("around before ...");

        //调用目标对象的原始方法执行
        Object result = proceedingJoinPoint.proceed();
        //原始方法在执行时：发生异常
        //后续代码不在执行

        log.info("around after ...");
        return result;
    }

    //后置通知
    @After("pt()")
    public void after(JoinPoint joinPoint){
        log.info("after ...");
    }

    //返回后通知（程序在正常执行的情况下，会执行的后置通知）
    @AfterReturning("pt()")
    public void afterReturning(JoinPoint joinPoint){
        log.info("afterReturning ...");
    }

    //异常通知（程序在出现异常的情况下，执行的后置通知）
    @AfterThrowing("pt()")
    public void afterThrowing(JoinPoint joinPoint){
        log.info("afterThrowing ...");
    }
}
~~~

##### 4.2 通知顺序

就是用多个切片类中的多个切入点来匹配同一个方法，谁先执行？

直接放答案，**根据这个切片类的名称来**

- 目标方法前的通知方法：字母排名靠前的先执行
- 目标方法后的通知方法：字母排名靠前的后执行



![17-8](picture/17-8.png)





那么，如果想控制呢？

1. 修改切面类的类名（这种方式非常繁琐、而且不便管理）
2. 使用Spring提供的@Order注解

使用@Order注解，控制通知的执行顺序：

~~~java
@Slf4j
@Component
@Aspect
@Order(2)  //切面类的执行顺序（前置通知：数字越小先执行; 后置通知：数字越小越后执行）
public class MyAspect2 {
    //前置通知
    @Before("execution(* com.itheima.service.*.*(..))")
    public void before(){
        log.info("MyAspect2 -> before ...");
    }

    //后置通知 
    @After("execution(* com.itheima.service.*.*(..))")
    public void after(){
        log.info("MyAspect2 -> after ...");
    }
}
~~~

~~~java
@Slf4j
@Component
@Aspect
@Order(3)  //切面类的执行顺序（前置通知：数字越小先执行; 后置通知：数字越小越后执行）
public class MyAspect3 {
    //前置通知
    @Before("execution(* com.itheima.service.*.*(..))")
    public void before(){
        log.info("MyAspect3 -> before ...");
    }

    //后置通知
    @After("execution(* com.itheima.service.*.*(..))")
    public void after(){
        log.info("MyAspect3 ->  after ...");
    }
}
~~~

~~~java
@Slf4j
@Component
@Aspect
@Order(1) //切面类的执行顺序（前置通知：数字越小先执行; 后置通知：数字越小越后执行）
public class MyAspect4 {
    //前置通知
    @Before("execution(* com.itheima.service.*.*(..))")
    public void before(){
        log.info("MyAspect4 -> before ...");
    }

    //后置通知
    @After("execution(* com.itheima.service.*.*(..))")
    public void after(){
        log.info("MyAspect4 -> after ...");
    }
}
~~~



![17-9](picture/17-9.png)



> 通知的执行顺序大家主要知道两点即可：
>
> 1. 不同的切面类当中，默认情况下通知的执行顺序是与切面类的类名字母排序是有关系的
> 2. 可以在切面类上面加上@Order注解，来控制不同的切面类通知的执行顺序







#### 5. 切入点表达式

就是用来标注哪些方法需要加入通知



##### 5.1 execution 根据方法的签名来匹配

execution主要根据方法的返回值、包名、类名、方法名、方法参数等信息来匹配，语法为：

~~~
execution(访问修饰符?  返回值  包名.类名.?方法名(方法参数) throws 异常?)
~~~

其中带`?`的表示可以省略的部分

- 访问修饰符：可省略（比如: public、protected）

- 包名.类名： 可省略

- throws 异常：可省略（注意是方法上声明抛出的异常，不是实际抛出的异常）

示例：

~~~java
@Before("execution(void com.itheima.service.impl.DeptServiceImpl.delete(java.lang.Integer))")
~~~



可以使用通配符描述切入点

- `*` ：单个独立的任意符号，可以通配任意返回值、包名、类名、方法名、任意类型的一个参数，也可以通配包、类、方法名的一部分

- `..` ：多个连续的任意符号，可以通配任意层级的包，或任意类型、任意个数的参数



对于一些具体的语法实在没必要看了 靠大模型就行了





##### 5.2 @annotation-----根据注解来识别

对于一些比较复杂的切入点表达式 用execution太麻烦了

因此可以通过自定义注解来标识切入点



实现步骤：

1. 编写自定义注解

2. 在业务类要做为连接点的方法上添加自定义注解

   

**自定义注解**：MyLog

下面的两个注解

@Target表明这个注解是用来标注方法的   @Retention标识这个注解什么时候生效（运行时生效）

~~~java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface MyLog {
}
~~~



**业务类**：DeptServiceImpl

~~~java
@Slf4j
@Service
public class DeptServiceImpl implements DeptService {
    @Autowired
    private DeptMapper deptMapper;

    @Override
    @MyLog //自定义注解（表示：当前方法属于目标方法）
    public List<Dept> list() {
        List<Dept> deptList = deptMapper.list();
        //模拟异常
        //int num = 10/0;
        return deptList;
    }

    @Override
    @MyLog  //自定义注解（表示：当前方法属于目标方法）
    public void delete(Integer id) {
        //1. 删除部门
        deptMapper.delete(id);
    }


    @Override
    public void save(Dept dept) {
        dept.setCreateTime(LocalDateTime.now());
        dept.setUpdateTime(LocalDateTime.now());
        deptMapper.save(dept);
    }

    @Override
    public Dept getById(Integer id) {
        return deptMapper.getById(id);
    }

    @Override
    public void update(Dept dept) {
        dept.setUpdateTime(LocalDateTime.now());
        deptMapper.update(dept);
    }
}
~~~



**切面类**

~~~java
@Slf4j
@Component
@Aspect
public class MyAspect6 {
    //针对list方法、delete方法进行前置通知和后置通知

    //前置通知
    @Before("@annotation(com.itheima.anno.MyLog)")
    public void before(){
        log.info("MyAspect6 -> before ...");
    }

    //后置通知
    @After("@annotation(com.itheima.anno.MyLog)")
    public void after(){
        log.info("MyAspect6 -> after ...");
    }
}
~~~









#### 6. 连接点--被AOP控制的方法

在Spring中用JoinPoint抽象了连接点，用它可以获得方法执行时的相关信息，如目标类名、方法名、方法参数等。

- 对于**@Around通知，获取连接点信息只能使用ProceedingJoinPoint类型**

- 对于**其他四种通知，获取连接点信息只能使用JoinPoint**，它是ProceedingJoinPoint的父类型



**JoinPoint**：是Spring AOP中的一个抽象接口，用来表示一个AOP切面可以控制的那些方法。在通知（Advice）中，我们可以通过JoinPoint获取到目标方法的一些相关信息，比如类名、方法名、参数等。

**ProceedingJoinPoint**：这是JoinPoint的子接口，它用于`@Around`通知中，除了能获取到方法的基本信息外，它还提供了一个非常重要的功能——控制目标方法的执行。通过调用`proceed()`方法，`ProceedingJoinPoint`允许你手动决定是否执行目标方法、何时执行以及是否修改返回值。



示例代码：

~~~java
@Before("execution(* com.example.MyService.*(..))")
public void beforeAdvice(JoinPoint joinPoint) {
    System.out.println("方法名: " + joinPoint.getSignature().getName());
    System.out.println("目标类名: " + joinPoint.getTarget().getClass().getSimpleName());
}


@Around("execution(* com.example.MyService.*(..))")
public Object aroundAdvice(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
    System.out.println("方法名: " + proceedingJoinPoint.getSignature().getName());
    Object result = proceedingJoinPoint.proceed(); // 执行目标方法
    System.out.println("返回值: " + result);
    return result; // 可以修改返回值
}
~~~

总结来说，不同通知类型需要选择相应的类型来获取信息和控制目标方法的执行，`ProceedingJoinPoint`是`@Around`通知中才会用到的，因为它能提供额外的控制能力。





#### 7. 实际案例--给每次修改数据库操作添加日志

这里把步骤都复制进来 还是可以理解的  看看理解次序 然后用GPT补充细节

这里的日志其实就是把操作人，操作对象，操作时间等存进数据库里面

##### 7.1 准备工作

1. AOP起步依赖

~~~xml
<!--AOP起步依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
~~~

2. 导入资料中准备好的数据库表结构，并引入对应的实体类

数据表

~~~mysql
-- 操作日志表
create table operate_log(
    id int unsigned primary key auto_increment comment 'ID',
    operate_user int unsigned comment '操作人',
    operate_time datetime comment '操作时间',
    class_name varchar(100) comment '操作的类名',
    method_name varchar(100) comment '操作的方法名',
    method_params varchar(1000) comment '方法参数',
    return_value varchar(2000) comment '返回值',
    cost_time bigint comment '方法执行耗时, 单位:ms'
) comment '操作日志表';
~~~

3 实体类

~~~java
//操作日志实体类
@Data
@NoArgsConstructor
@AllArgsConstructor
public class OperateLog {
    private Integer id; //主键ID
    private Integer operateUser; //操作人ID
    private LocalDateTime operateTime; //操作时间
    private String className; //操作类名
    private String methodName; //操作方法名
    private String methodParams; //操作方法参数
    private String returnValue; //操作方法返回值
    private Long costTime; //操作耗时
}
~~~

4   Mapper接口-----**这里的日志其实就是把操作人，操作对象，操作时间等存进数据库里面**

~~~java
@Mapper
public interface OperateLogMapper {

    //插入日志数据
    @Insert("insert into operate_log (operate_user, operate_time, class_name, method_name, method_params, return_value, cost_time) " +
            "values (#{operateUser}, #{operateTime}, #{className}, #{methodName}, #{methodParams}, #{returnValue}, #{costTime});")
    public void insert(OperateLog log);

}
~~~

##### 7.2 编码实现

- 自定义注解@Log

在com.itheima.anno文件夹下新建接口即可

~~~java
/**
 * 自定义Log注解
 */
@Target({ElementType.METHOD})
@Documented
@Retention(RetentionPolicy.RUNTIME)
public @interface Log {
}
~~~



- 修改业务实现类，在增删改业务方法上添加@Log注解

这里只添加了@Log注解 用于标识切面

~~~java
@Slf4j
@Service
public class EmpServiceImpl implements EmpService {
    @Autowired
    private EmpMapper empMapper;

    @Override
    @Log
    public void update(Emp emp) {
        emp.setUpdateTime(LocalDateTime.now()); //更新修改时间为当前时间

        empMapper.update(emp);
    }

    @Override
    @Log
    public void save(Emp emp) {
        //补全数据
        emp.setCreateTime(LocalDateTime.now());
        emp.setUpdateTime(LocalDateTime.now());
        //调用添加方法
        empMapper.insert(emp);
    }

    @Override
    @Log
    public void delete(List<Integer> ids) {
        empMapper.delete(ids);
    }

    //省略其他代码...
}
~~~





- 定义切面类，完成记录操作日志的逻辑

放在com.itheima.aop文件夹下 作为需要执行功能的实现

下面的注释已经写的很清楚了 就是分别获取需要记录的数据





其中：

```Java
String className = joinPoint.getTarget().getClass().getName();
```

每个`.`的作用如下：

1. `joinPoint.getTarget()`：`JoinPoint`对象的`getTarget()`方法，用来获取目标对象（即被AOP切面代理的那个原始对象）。这个对象是你正在拦截的业务类的实例。
2. `getClass()`：这是所有Java对象都有的方法，用于返回这个对象的`Class`对象，它代表了这个对象的类型信息。`Class`对象是Java反射机制的基础，通过它可以获取类型的各种信息（如类名、方法、字段等）。
3. `getName()`：`Class`对象的`getName()`方法，用来获取这个类的全限定名（即包含包路径的类名，如`com.example.MyClass`）。

~~~java
@Slf4j
@Component
@Aspect //切面类
public class LogAspect {

    @Autowired
    private HttpServletRequest request;

    @Autowired
    private OperateLogMapper operateLogMapper;

    @Around("@annotation(com.itheima.anno.Log)")
    public Object recordLog(ProceedingJoinPoint joinPoint) throws Throwable {
        //操作人ID - 当前登录员工ID
        //获取请求头中的jwt令牌, 解析令牌
        String jwt = request.getHeader("token");
        Claims claims = JwtUtils.parseJWT(jwt);
        Integer operateUser = (Integer) claims.get("id");

        //操作时间
        LocalDateTime operateTime = LocalDateTime.now();

        //操作类名
        String className = joinPoint.getTarget().getClass().getName();

        //操作方法名
        String methodName = joinPoint.getSignature().getName();

        //操作方法参数
        Object[] args = joinPoint.getArgs();
        String methodParams = Arrays.toString(args);

        long begin = System.currentTimeMillis();
        //调用原始目标方法运行
        Object result = joinPoint.proceed();
        long end = System.currentTimeMillis();

        //方法返回值
        String returnValue = JSONObject.toJSONString(result);

        //操作耗时
        Long costTime = end - begin;


        //记录操作日志
        OperateLog operateLog = new OperateLog(null,operateUser,operateTime,className,methodName,methodParams,returnValue,costTime);
        operateLogMapper.insert(operateLog);

        log.info("AOP记录操作日志: {}" , operateLog);

        return result;
    }

}
~~~

> 代码实现细节： 获取request对象，从请求头中获取到jwt令牌，解析令牌获取出当前用户的id。





重启SpringBoot服务，测试操作日志记录功能：

- 添加一个新的部门

![17-10](picture/17-10.png)

- 数据表

![17-11](picture/17-11.PNG)



可以看到这里已经写进去了







## 													Springboot原理

#### 1 配置优先级--没什么用

在SpringBoot项目当中，常见的属性配置方式有5种， 3种配置文件，加上2种外部属性的配置(**Java系统属性、命令行参数**)。

通过以上的测试，我们也得出了优先级**(从低到高)**：

- application.yaml（忽略）
- application.yml
- application.properties
- java系统属性（-Dxxx=xxx）
- 命令行参数（--xxx=xxx）





后面那俩是在idea中鼠标点点设置的 有需要自己去看





思考：如果项目已经打包上线了，这个时候我们又如何来设置Java系统属性和命令行参数呢？

答案：运行的时候加上去就行

~~~shell
java -Dserver.port=9000 -jar XXXXX.jar --server.port=10010
~~~

下面我们来演示下打包程序运行时指定Java系统属性和命令行参数：

1. 执行maven打包指令package，把项目打成jar文件
2. 使用命令：java -jar 方式运行jar文件程序



#### 2 bean

##### 2.1 获取bean

SpringBoot项目在启动的时候会自动的创建IOC容器(也称为Spring容器)，并且在启动的过程当中会自动的将bean对象都创建好，存放在IOC容器当中。应用程序在运行时需要依赖什么bean对象，就直接进行依赖注入就可以了。

而在Spring容器中提供了一些方法，可以主动从IOC容器中获取到bean对象，下面介绍3种常用方式：

1. 根据name获取bean

   ~~~java
   Object getBean(String name)
   ~~~

2. 根据类型获取bean

   ~~~java
   <T> T getBean(Class<T> requiredType)
   ~~~

3. 根据name获取bean（带类型转换）

   ~~~java
   <T> T getBean(String name, Class<T> requiredType)
   ~~~

测试类：

~~~java
@SpringBootTest
class SpringbootWebConfig2ApplicationTests {

    @Autowired
    private ApplicationContext applicationContext; //IOC容器对象

    //获取bean对象
    @Test
    public void testGetBean(){
        //根据bean的名称获取
        DeptController bean1 = (DeptController) applicationContext.getBean("deptController");
        System.out.println(bean1);

        //根据bean的类型获取
        DeptController bean2 = applicationContext.getBean(DeptController.class);
        System.out.println(bean2);

        //根据bean的名称 及 类型获取
        DeptController bean3 = applicationContext.getBean("deptController", DeptController.class);
        System.out.println(bean3);
    }
}
~~~

程序运行后控制台日志：

![18-1](picture/18-1.PNG)

> 问题：输出的bean对象地址值是一样的，说明IOC容器当中的bean对象有几个？
>
> 答案：**只有一个。        （默认情况下，IOC中的bean对象是单例）**
>
> 
>
> 那么能不能将bean对象设置为非单例的(每次获取的bean都是一个新对象)？
>
> 可以，在下一个知识点(bean作用域)中讲解。

注意事项：

- 上述所说的 【Spring项目启动时，会把其中的bean都创建好】还会受到作用域及延迟初始化影响，这里主要针对于默认的单例非延迟加载的bean而言。



##### 2.2 Bean作用域（单例/非单例）

在前面我们提到的IOC容器当中，默认bean对象是单例模式(只有一个实例对象)。那么如何设置bean对象为非单例呢？需要设置bean的作用域。

在Spring中支持五种作用域，后三种在web环境才生效：

| **作用域**  | **说明**                                        |
| ----------- | ----------------------------------------------- |
| singleton   | 容器内同名称的bean只有一个实例（单例）（默认）  |
| prototype   | 每次使用该bean时会创建新的实例（非单例）        |
| request     | 每个请求范围内会创建新的实例（web环境中，了解） |
| session     | 每个会话范围内会创建新的实例（web环境中，了解） |
| application | 每个应用范围内会创建新的实例（web环境中，了解） |

知道了bean的5种作用域了，我们要怎么去设置一个bean的作用域呢？

- 可以借助Spring中的@Scope注解来进行配置作用域

修改控制器DeptController代码：

~~~java
@Scope("prototype") //bean作用域为非单例
@Lazy //延迟加载
@RestController
@RequestMapping("/depts")
public class DeptController {

    @Autowired
    private DeptService deptService;

    public DeptController(){
        System.out.println("DeptController constructor ....");
    }

    //省略其他代码...
}
~~~

重启SpringBoot服务，再次执行测试方法，查看控制吧打印的日志：

![image-20240910162633489](C:/Users/14088/AppData/Roaming/Typora/typora-user-images/image-20240910162633489.png)



> 注意事项：
>
> - prototype的bean，每一次使用该bean的时候都会创建一个新的实例
> - 实际开发当中，绝大部分的Bean是单例的，也就是说绝大部分Bean不需要配置scope属性



##### 2.3 第三方bean如何标识

之前我们所配置的bean，像controller、service，dao三层体系下编写的类，这些类都是我们在项目当中自己定义的类(自定义类)。当我们要声明这些bean，也非常简单，我们只需要在类上加上@Component以及它的这三个衍生注解（@Controller、@Service、@Repository），就可以来声明这个bean对象了。
但是在我们项目开发当中，还有一种情况就是这个类它不是我们自己编写的，而是我们引入的第三方依赖当中提供的。



例如，通过xml文件中引入一个读取文件的类：SAXReader

![18-3](picture/18-3.png)

这里源代码是只读的，无法添加**@Component注解**



###### 解决方法：在config文件夹下新建一个commonconfig类

- 如果需要定义第三方Bean时， 通常会单独定义一个配置类

~~~java
@Configuration //配置类  (在配置类当中对第三方bean进行集中的配置管理)
public class CommonConfig {

    //声明第三方bean
    @Bean //将当前方法的返回值对象交给IOC容器管理, 成为IOC容器bean
          //通过@Bean注解的name/value属性指定bean名称, 如果未指定, 默认是方法名
    public SAXReader reader(DeptService deptService){
        System.out.println(deptService);
        return new SAXReader();
    }

}

~~~

在方法上加上一个@Bean注解，Spring 容器在启动的时候，它会自动的调用这个方法，并将方法的返回值声明为Spring容器当中的Bean对象。



> 注意事项 ：
>
> - 通过@Bean注解的name或value属性可以声明bean的名称，如果不指定，默认bean的名称就是方法名。
>
> - 
>
> - 
>
> - 如果第三方bean需要依赖其它bean对象，直接在bean定义方法中设置形参即可，容器会根据类型自动装配。







##### 2.4 使用Component还是bean注解



关于Bean大家只需要保持一个原则：

- 如果是**在项目当中我们自己定义的类**，想将这些类交给IOC容器管理，我们直接**使用@Componen**t以及它的衍生注解来声明就可以。
- 如果这个类它不是我们自己定义的，而是**引入的第三方依赖当中提供的类**，而且我们还想将这个类交给IOC容器管理。此时我们就需要在配置类中定义一个方法，**在方法上加上一个@Bean注解**，通过这种方式来声明第三方的bean对象。



#### 3. Springboot底层

解析自动配置的原理就是分析在 SpringBoot项目当中，我们引入对应的依赖之后，是如何将依赖jar包当中所提供的bean以及配置类直接加载到当前项目的SpringIOC容器当中的。





思考：引入进来的第三方依赖当中的bean以及配置类为什么没有生效？

- 原因在我们之前讲解IOC的时候有提到过，在类上添加@Component注解来声明bean对象时，还需要保证@Component注解能被Spring的组件扫描到。
- SpringBoot项目中的@SpringBootApplication注解，具有包扫描的作用，但是它只会扫描启动类所在的当前包以及子包。 
- **当前包：com.itheima， 第三方依赖中提供的包：com.example（扫描不到）**



那么如何解决以上问题的呢？

- 方案1：@ComponentScan 组件扫描**（太臃肿 不用了）**

@ComponentScan组件扫描

~~~java
@SpringBootApplication
@ComponentScan({"com.itheima","com.example"}) //指定要扫描的包
public class SpringbootWebConfig2Application {
    public static void main(String[] args) {
        SpringApplication.run(SpringbootWebConfig2Application.class, args);
    }
}

~~~



- 方案2：**@Import 导入（使用@Import导入的类会被Spring加载到IOC容器中）**

@Import导入

- 导入形式主要有以下几种：

  1. 导入普通类---直接把tokenparser的字节码放进来

  ~~~java
  @Import(TokenParser.class) //导入的类会被Spring加载到IOC容器中
  @SpringBootApplication
  public class SpringbootWebConfig2Application {
      public static void main(String[] args) {
          SpringApplication.run(SpringbootWebConfig2Application.class, args);
      }
  }
  ~~~

  2.导入配置类（**配置类**（使用 `@Configuration` 注解的类就是**把多个普通类放一起管理**。它的作用是集中定义和管理多个 Spring Bean，而不是在项目中分散地创建这些 Bean。）

  - 配置类---里面放了多个普通类并注册为Bean

  ~~~java
  @Configuration
  public class HeaderConfig {
      @Bean
      public HeaderParser headerParser(){
          return new HeaderParser();
      }
  
      @Bean
      public HeaderGenerator headerGenerator(){
          return new HeaderGenerator();
      }
  }
  ~~~

  - 启动类---这里就只用@import导入配置类就可以了

  ~~~java
  @Import(HeaderConfig.class) //导入配置类
  @SpringBootApplication
  public class SpringbootWebConfig2Application {
      public static void main(String[] args) {
          SpringApplication.run(SpringbootWebConfig2Application.class, args);
      }
  }
  ~~~

  - 测试类

  ~~~java
  @SpringBootTest
  public class AutoConfigurationTests {
      @Autowired
      private ApplicationContext applicationContext;
  
      @Test
      public void testHeaderParser(){
          System.out.println(applicationContext.getBean(HeaderParser.class));
      }
  
      @Test
      public void testHeaderGenerator(){
          System.out.println(applicationContext.getBean(HeaderGenerator.class));
      }
      
      //省略其他代码...
  }
  ~~~

  

  3.导入ImportSelector接口实现类--**上一步的更进阶 用return string的形式来返回配置类**

- ImportSelector接口实现类

~~~java
public class MyImportSelector implements ImportSelector {
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {
        //返回值字符串数组（数组中封装了全限定名称的类）
        return new String[]{"com.example.HeaderConfig"};
    }
}
~~~

- 启动类

~~~java
@Import(MyImportSelector.class) //导入ImportSelector接口实现类
@SpringBootApplication
public class SpringbootWebConfig2Application {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootWebConfig2Application.class, args);
    }
}

~~~





思考：如果基于以上方式完成自动配置，当要引入一个第三方依赖时，是不是还要知道第三方依赖中有哪些配置类和哪些Bean对象？

- 答案：是的。 （对程序员来讲，很不友好，而且比较繁琐）



思考：当我们要使用第三方依赖，依赖中到底有哪些bean和配置类，谁最清楚？

- 答案：第三方依赖自身最清楚。



> **结论：我们不用自己指定要导入哪些bean对象和配置类了，让第三方依赖它自己来指定。**



怎么让第三方依赖自己指定bean对象和配置类？

- 比较常见的方案就是第三方依赖给我们提供一个注解，这个注解一般都以@EnableXxxx开头的注解，注解中封装的就是@Import注解

  4). 使用第三方依赖提供的 @EnableXxxxx注解

  - 第三方依赖中提供的注解---这里导入了我的MyImportSelector

  ~~~java
  @Retention(RetentionPolicy.RUNTIME)
  @Target(ElementType.TYPE)
  @Import(MyImportSelector.class)//指定要导入哪些bean对象或配置类
  public @interface EnableHeaderConfig { 
  }
  ~~~

  - 在使用时只需在启动类上加上@EnableXxxxx注解即可

  ~~~java
  @EnableHeaderConfig  //使用第三方依赖提供的Enable开头的注解
  @SpringBootApplication
  public class SpringbootWebConfig2Application {
      public static void main(String[] args) {
          SpringApplication.run(SpringbootWebConfig2Application.class, args);
      }
  }
  
  ~~~

  

#### 4. 源码跟踪

要搞清楚SpringBoot的自动配置原理，要从SpringBoot启动类上使用的核心注解@SpringBootApplication开始分析：

![19-1](picture/19-1.png)



在@SpringBootApplication注解中包含了：

- 元注解（不再解释，修饰注解的注解）

- @SpringBootConfiguration

- @EnableAutoConfiguration

- @ComponentScan

  

  我们先来看第一个注解：@SpringBootConfiguration

> @SpringBootConfiguration注解上使用了@Configuration，表明SpringBoot启动类就是一个配置类。
>
> @Indexed注解，是用来加速应用启动的（不用关心）。



接下来再先看@ComponentScan注解：

![19-3](picture/19-3.png)

> @ComponentScan注解是用来进行组件扫描的，扫描启动类所在的包及其子包下所有被@Component及其衍生注解声明的类。
>
> SpringBoot启动类，之所以具备扫描包功能，就是因为包含了@ComponentScan注解。



最后我们来看看@EnableAutoConfiguration注解（**自动配置核心注解**）：

![19-2](picture/19-2.png)



> 使用@Import注解，导入了实现ImportSelector接口的实现类----**AutoConfigurationImportSelector。**
>
> ![19-4](picture/19-4.png)



`AutoConfigurationImportSelector` 是 Spring Boot 自动配置的核心类。它根据项目的依赖和配置，从 `spring.factories（以前版本是META-INF）` 文件中选择合适的配置类，并将这些类自动导入到 Spring 容器中





在前面在给大家演示自动配置的时候，我们直接在测试类当中注入了一个叫gson的bean对象，进行JSON格式转换。虽然我们没有配置bean对象，但是我们是可以直接注入使用的。

**原因就是因为在自动配置类当中做了自动配置。**到底是在哪个自动配置类当中做的自动配置呢？我们通过搜索来查询一下。

**在META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports配置文件中指定了第三方依赖Gson的配置类：GsonAutoConfiguration**





**自动配置源码小结**

自动配置原理源码入口就是@SpringBootApplication注解，在这个注解中封装了3个注解，分别是：

- @SpringBootConfiguration
  - 声明当前类是一个配置类
- @ComponentScan
  - 进行组件扫描（SpringBoot中默认扫描的是启动类所在的当前包及其子包）
- @EnableAutoConfiguration
  - 封装了@Import注解（Import注解中指定了一个ImportSelector接口的实现类）
    - 在实现类重写的selectImports()方法，读取当前项目下所有依赖jar包中META-INF/spring.factories、META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports两个文件里面定义的配置类（配置类中定义了@Bean注解标识的方法）。



当SpringBoot程序启动时，就会加载配置文件当中所定义的配置类，并将这些配置类信息(类的全限定名)封装到String类型的数组中，最终通过@Import注解将这些配置类全部加载到Spring的IOC容器中，交给IOC容器管理。



> 最后呢给大家抛出一个问题：在META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports文件中定义的配置类非常多，而且每个配置类中又可以定义很多的bean，那这些bean都会注册到Spring的IOC容器中吗？
>
> 答案：并不是。 在声明bean对象时，上面有加一个以@Conditional开头的注解，这种注解的作用就是按照条件进行装配，只有满足条件之后，才会将bean注册到Spring的IOC容器中（下面会详细来讲解）



#### 5. @conditional

我们在跟踪SpringBoot自动配置的源码的时候，在自动配置类声明bean的时候，除了在方法上加了一个@Bean注解以外，还会经常用到一个注解，就是以Conditional开头的这一类的注解。以Conditional开头的这些注解都是条件装配的注解。下面我们就来介绍下条件装配注解。

@Conditional注解：

- 作用：按照一定的条件进行判断，在满足给定条件后才会注册对应的bean对象到Spring的IOC容器中。

- 位置：方法、类

- @Conditional本身是一个父注解，派生出大量的子注解：

  1  @ConditionalOnClass：判断环境中有对应字节码文件，才注册bean到IOC容器。

  只有当**类路径**中存在某个特定的类时，Spring 才会注册对应的 Bean。

  **例子**：

  ```java 
  @ConditionalOnClass(name = "com.example.MyClass")
  @Bean
  public MyService myService() {
      return new MyServiceImpl();
  }
  ```

  - 如果类路径中存在 `com.example.MyClass` 这个类（比如你引入了对应的依赖），Spring 才会注册 `myService` 这个 Bean。
  - 如果没有这个类，`myService` 就不会被注册。

  

  2  @ConditionalOnMissingBean：判断环境中没有对应的bean(类型或名称)，才注册bean到IOC容器。

  只有当 Spring 容器中**没有**某个特定的 Bean 时，Spring 才会注册这个 Bean。

  它检查当前的 Spring 容器里是否已经存在某个 Bean，如果没有的话，才会注册当前的 Bean。

  这**通常用于提供默认配置，当你没有手动配置某个 Bean 时，它会自动提供一个默认的。**

  

  

  3  @ConditionalOnProperty：判断配置文件中有对应属性和值，才注册bean到IOC容器。

​		只有当配置文件（比如 `application.properties` 或 `application.yml`）中手动设置了预设值，Spring 才会注册对应的 Bean。



```java 
@ConditionalOnProperty(name = "my.feature.enabled", havingValue = "true")
@Bean
public MyFeature myFeature() {
    return new MyFeature();
}
```

- 只有当 `application.properties` 或 `application.yml` 文件中存在 `my.feature.enabled=true` 时，`myFeature` Bean 才会被注册。
- 如果属性不存在，或者值不为 `true`，则不会创建这个 Bean。









最后再给大家梳理一下自动配置原理：



![19-5](picture/19-5.png)



> 自动配置的核心就在@SpringBootApplication注解上，SpringBootApplication这个注解底层包含了3个注解，分别是：
>
> - @SpringBootConfiguration
>
> - @ComponentScan
>
> - @EnableAutoConfiguration
>
> 
>
> @EnableAutoConfiguration这个注解才是自动配置的核心。
>
> - 它封装了一个@Import注解，Import注解里面指定了一个ImportSelector接口的实现类。
> - 在这个实现类中，重写了ImportSelector接口中的selectImports()方法。
> - 而selectImports()方法中会去读取两份配置文件，并将配置文件中定义的配置类做为selectImports()方法的返回值返回，返回值代表的就是需要将哪些类交给Spring的IOC容器进行管理。
> - 那么所有自动配置类的中声明的bean都会加载到Spring的IOC容器中吗? 其实并不会，因为这些配置类中在声明bean时，通常都会添加@Conditional开头的注解，这个注解就是进行条件装配。而Spring会根据Conditional注解有选择性的进行bean的创建。
> - @Enable 开头的注解底层，它就封装了一个注解 import 注解，它里面指定了一个类，是 ImportSelector 接口的实现类。在实现类当中，我们需要去实现 ImportSelector  接口当中的一个方法 selectImports 这个方法。这个方法的返回值代表的就是我需要将哪些类交给 spring 的 IOC容器进行管理。
> - 此时它会去读取两份配置文件，一份儿是 spring.factories，另外一份儿是 autoConfiguration.imports。而在  autoConfiguration.imports 这份儿文件当中，它就会去配置大量的自动配置的类。
> - 而前面我们也提到过这些所有的自动配置类当中，所有的 bean都会加载到 spring 的 IOC 容器当中吗？其实并不会，因为这些配置类当中，在声明 bean 的时候，通常会加上这么一类@Conditional 开头的注解。这个注解就是进行条件装配。所以SpringBoot非常的智能，它会根据 @Conditional 注解来进行条件装配。只有条件成立，它才会声明这个bean，才会将这个 bean 交给 IOC 容器管理。



## 															Maven高级

#### 1 分模块开发

##### 1.1 分析

所谓分模块设计，顾名思义指的就是我们在设计一个 Java 项目的时候，将一个 Java 项目拆分成多个模块进行开发。

**单体主要两点问题：不方便项目的维护和管理、项目中的通用组件难以复用。**

![image-20230113094045299](../BaiduNetdiskDownload/day15-maven高级/day15-maven高级/讲义/assets/image-20230113094045299.png)



##### 1.2 实践：

我们可以看到在这个项目当中，除了我们所开发的部门管理以及员工管理、登录认证等相关业务功能以外，我们是不是也定义了一些实体类，也就是pojo包下存放的一些类，像分页结果的封装类PageBean、 统一响应结果Result，我们还定义了一些通用的工具类，像Jwts、阿里云OSS操作的工具类等等。

如果在当前公司的其他项目组当中，也想使用我们所封装的这些公共的组件，该怎么办？大家可以思考一下。

- 方案一：直接依赖我们当前项目 tlias-web-management ，但是存在两大缺点：

  - 这个项目当中包含所有的业务功能代码，而想共享的资源，仅仅是pojo下的实体类，以及 utils 下的工具类。如果全部都依赖进来，项目在启动时将会把所有的类都加载进来，会**影响性能**。
  - 如果直接把这个项目都依赖进来了，那也就意味着我们所有的业务代码都对外公开了，这个是非常**不安全**的。

- 方案二：分模块设计

  - 将pojo包下的实体类，抽取到一个maven模块中 tlias-pojo
  - 将utils包下的工具类，抽取到一个maven模块中 tlias-utils
  - 其他的业务代码，放在tlias-web-management这个模块中，在该模块中需要用到实体类pojo、工具类utils，直接引入对应的依赖即可。

-  ![21-2](picture/21-2.png)

​	

> ​	**注意：分模块开发需要先针对模块功能进行设计，再进行编码。不会先将工程开发完毕，然后进行拆分。**
>
> ​	PS：当前我们是为了演示分模块开发，所以是基于我们前面开发的案例项目进行拆分的，实际中都是分模块设计，然后再开发的。



##### 1.3 实现

**1. 创建maven模块 tlias-pojo，存放实体类**



A. 创建一个正常的Maven模块，模块名tlias-pojo

![21-3](picture/21-3.png)

B. 然后在tlias-pojo中创建一个包 com.itheima.pojo (和原来案例项目中的pojo包名一致)

C. 将原来案例项目 tlias-web-management 中的pojo包下的实体类，复制到tlias-pojo模块中

![21-4](picture/21-4.png)



D. 在 tlias-pojo 模块的pom.xml文件中引入用到了的依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.24</version>
    </dependency>
</dependencies>
```



E. 删除原有案例项目tlias-web-management的pojo包【直接删除不要犹豫，我们已经将该模块拆分出去了】，然后在pom.xml中引入 tlias-pojo的依赖

```xml
<dependency>
    <groupId>com.itheima</groupId>
    <artifactId>tlias-pojo</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```



就搞定了 非常简单

简单来说，创建一个新的module,将utiles代码挪进来，再在原有的项目中引入这个依赖 完事





#### 2. 继承关系

巨简单 放几张图看得懂就行

核心就是简化依赖的配置

所有的springboot项目都有一个统一的父工程，就是spring-boot-starter-parent。 与java语言类似，Maven不支持多继承，一个maven项目只能继承一个父工程，如果继承了spring-boot-starter-parent，就没法继承我们自己定义的父工程 tlias-parent了。

那我们怎么来解决这个问题呢？

那此时，大家可以想一下，Java虽然不支持多继承，但是可以支持**多重继承，**比如：A 继承 B， B 继承C。 那在Maven中也是支持多重继承的，所以呢，我们就可以让 **我们自己创建的三个模块，都继承tlias-parent，而tlias-parent 再继承 spring-boot-starter-parent，**就可以了。



![21-5](picture/21-5.PNG)





![](picture/21-7.PNG)



![21-8](picture/21-8.PNG)





![21-9](picture/21-9.PNG)





> **工程结构说明：**
>
> - 我们当前的项目结构为左边那种平级的 
>
>   因为我们是项目开发完毕之后，给大家基于现有项目拆分的各个模块，tlias-web-management已经存在了，然后再创建各个模块与父工程，所以父工程与模块之间是平级的。
>
> 
>
> - 而实际项目中，可能还会见到右边那种 父工程包含子工程的 
>
>   而在真实的企业开发中，都是先设计好模块之后，再开始创建模块，开发项目。 那此时呢，一般都会先创建父工程 tlias-parent，然后将创建的各个子模块，都放在父工程parent下面。 这样层级结构会更加清晰一些。 
>
>   ​	
>
>   **PS：上面两种工程结构，都是可以正常使用的，没有一点问题。 只不过，第二种结构，看起来，父子工程结构更加清晰、更加直观。**



#### 3. 版本锁定

如果项目中各个模块中都公共的这部分依赖，我们可以直接定义在父工程中，从而简化子工程的配置。 然而在项目开发中，还有一部分依赖，并不是各个模块都共有的，可能只是其中的一小部分模块中使用到了这个依赖。

比如：在tlias-web-management、tlias-web-system、tlias-web-report这三个子工程中，都使用到了jwt的依赖。 但是 tlias-pojo、tlias-utils中并不需要这个依赖，那此时，这个依赖，我们不会直接配置在父工程 tlias-parent中，而是哪个模块需要，就在哪个模块中配置。

而由于是一个项目中的多个模块，那多个模块中，我们要使用的同一个依赖的版本要一致，这样便于项目依赖的统一管理。比如：这个jwt依赖，我们都使用的是 0.9.1 这个版本。

![21-10](picture/21-10.png)



那假如说，我们项目要升级，要使用到jwt最新版本 0.9.2 中的一个新功能，那此时需要将依赖的版本升级到0.9.2，那此时该怎么做呢 ？

第一步：去找当前项目中所有的模块的pom.xml配置文件，看哪些模块用到了jwt的依赖。

第二步：找到这个依赖之后，将其版本version，更换为 0.9.2。



**问题：如果项目拆分的模块比较多，每一次更换版本，我们都得找到这个项目中的每一个模块，一个一个的更改。 很容易就会出现，遗漏掉一个模块，忘记更换版本的情况。**



那我们又该如何来解决这个问题，如何来统一管理各个依赖的版本呢？ 

答案：**Maven的版本锁定功能。**

在maven中，可以在父工程的pom文件中通过 `<dependencyManagement>` 来统一管理依赖版本。

父工程：

```xml
<!--统一管理依赖版本-->
<dependencyManagement>
    <dependencies>
        <!--JWT令牌-->
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>0.9.1</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```

子工程：

```xml
<dependencies>
    <!--JWT令牌-->
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt</artifactId>
    </dependency>
</dependencies>
```



> 注意：
>
> - 在父工程中所配置的 `<dependencyManagement>` 只能统一管理依赖版本，并不会将这个依赖直接引入进来。 这点和 `<dependencies>` 是不同的。
>
> - 子工程要使用这个依赖，还是需要引入的，只是此时就无需指定 `<version>` 版本号了，父工程统一管理。变更依赖版本，只需在父工程中统一变更。





##### 属性配置：

进一步的，当依赖比较多的时候 可以将所有版本信息放在一个<properties>中

1). 自定义属性

```xml
<properties>
	<lombok.version>1.18.24</lombok.version>
</properties>
```



2). 引用属性

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>${lombok.version}</version>
</dependency>
```



接下来，我们就可以在父工程中，将所有的版本号，都集中管理维护起来。

```xml
<properties>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>

    <lombok.version>1.18.24</lombok.version>
    <jjwt.version>0.9.1</jjwt.version>
    <aliyun.oss.version>3.15.1</aliyun.oss.version>
    <jaxb.version>2.3.1</jaxb.version>
    <activation.version>1.1.1</activation.version>
    <jaxb.runtime.version>2.3.3</jaxb.runtime.version>
</properties>


<dependencies>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>${lombok.version}</version>
    </dependency>
</dependencies>

<!--统一管理依赖版本-->
<dependencyManagement>
    <dependencies>
        <!--JWT令牌-->
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>${jjwt.version}</version>
        </dependency>

        <!--阿里云OSS-->
        <dependency>
            <groupId>com.aliyun.oss</groupId>
            <artifactId>aliyun-sdk-oss</artifactId>
            <version>${aliyun.oss.version}</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```

版本集中管理之后，我们要想修改依赖的版本，就只需要在父工程中自定义属性的位置，修改对应的属性值即可。



> **面试题：`<dependencyManagement>` 与 `<dependencies>` 的区别是什么?**
>
> - `<dependencies>` 是直接依赖，在父工程配置了依赖，子工程会直接继承下来。 
> - `<dependencyManagement>` 是统一管理依赖版本，不会直接依赖，还需要在子工程中引入所需依赖(无需指定版本)



#### 4. 聚合

不聚合的话 打包非常麻烦 要用maven一直install  

![21-11](picture/21-11.PNG)



在maven中，我们可以在聚合工程中通过 `<moudules>` 设置当前聚合工程所包含的子模块的名称。我们可以在 tlias-parent中，添加如下配置，来指定当前聚合工程，需要聚合的模块：

```xml
<!--聚合其他模块-->
<modules>
    <module>../tlias-pojo</module>
    <module>../tlias-utils</module>
    <module>../tlias-web-management</module>
</modules>
```



那此时，我们要进行编译、打包、安装操作，就无需在每一个模块上操作了。只需要在聚合工程上，统一进行操作就可以了。

**测试：**执行在聚合工程 tlias-parent 中执行 package 打包指令

![21-12](picture/21-12.png) 

那 tlias-parent 中所聚合的其他模块全部都会执行 package 指令，这就是通过聚合实现项目的一键构建（一键清理clean、一键编译compile、一键测试test、一键打包package、一键安装install等）。

#### 5. 继承与聚合对比



- **作用**

  - 聚合用于快速构建项目

  - 继承用于简化依赖配置、统一管理依赖

- **相同点：**

  - 聚合与继承的pom.xml文件打包方式均为pom，通常将两种关系制作到同一个pom文件中

  - 聚合与继承均属于设计型模块，并无实际的模块内容

- **不同点：**
  - 聚合是在聚合工程中配置关系，聚合可以感知到参与聚合的模块有哪些

  - 继承是在子模块中配置关系，父模块无法感知哪些子模块继承了自己



#### 6. 私服（公司服务器）

私服其实就是架设在公司局域网内部的一台服务器，用来让各个员工共享数据和文件

**误区：**中央仓库指全球只有一个的springboot官方仓库，用来下载Springboot依赖的那个 所以是不可能让私人上传文件的



![21-13](picture/21-13.png)



在这个正常的架构中

我们在项目中需要使用其他第三方提供的依赖，

如果本地仓库没有，也会自动连接私服下载，

如果私服没有，私服此时会自动连接中央仓库，去中央仓库中下载依赖，然后将下载的依赖存储在私服仓库及本地仓库中。

资源上传与下载，我们需要做三步配置，执行一条指令。



##### 第一步配置：

在maven的配置文件中配置访问私服的用户名、密码。（在自己maven安装目录下的conf/settings.xml中的servers中配置）

```xml
<server>
    <id>maven-releases</id>
    <username>admin</username>
    <password>admin</password>
</server>
    
<server>
    <id>maven-snapshots</id>
    <username>admin</username>
    <password>admin</password>
</server>
```

##### 第二步配置

在maven的配置文件中配置连接私服的地址(url地址)。

```xml
<mirror>
    <id>maven-public</id>
    <mirrorOf>*</mirrorOf>
    <url>http://192.168.150.101:8081/repository/maven-public/</url>
</mirror>
```

```xml
<profile>
    <id>allow-snapshots</id>
        <activation>
        	<activeByDefault>true</activeByDefault>
        </activation>
    <repositories>
        <repository>
            <id>maven-public</id>
            <url>http://192.168.150.101:8081/repository/maven-public/</url>
            <releases>
            	<enabled>true</enabled>
            </releases>
            <snapshots>
            	<enabled>true</enabled>
            </snapshots>
        </repository>
    </repositories>
</profile>
```

##### 第三步配置：

在项目的pom.xml文件中配置上传资源的位置(url地址)。

```xml
<distributionManagement>
    <!-- release版本的发布地址 -->
    <repository>
        <id>maven-releases</id>
        <url>http://192.168.150.101:8081/repository/maven-releases/</url>
    </repository>

    <!-- snapshot版本的发布地址 -->
    <snapshotRepository>
        <id>maven-snapshots</id>
        <url>http://192.168.150.101:8081/repository/maven-snapshots/</url>
    </snapshotRepository>
</distributionManagement>
```



配置好了上述三步之后，要上传资源到私服仓库，就执行执行maven生命周期：deploy(在右边maven目录install的下面 截图没接进去)。

![21-15](picture/21-15.PNG)



> 私服仓库说明：
>
> - RELEASE：存储自己开发的RELEASE发布版本的资源。
> - SNAPSHOT：存储自己开发的SNAPSHOT发布版本的资源。
> - Central：存储的是从中央仓库下载下来的依赖。

> 项目版本说明：
>
> - RELEASE(发布版本)：功能趋于稳定、当前更新停止，可以用于发行的版本，存储在私服中的RELEASE仓库中。
> - SNAPSHOT(快照版本)：功能不稳定、尚处于开发中的版本，即快照版本，存储在私服的SNAPSHOT仓库中。



















# 															Web开发总结

web后端开发现在基本上都是基于标准的三层架构进行开发的，在三层架构当中，Controller控制器层负责接收请求响应数据，Service业务层负责具体的业务逻辑处理，而Dao数据访问层也叫持久层，就是用来处理数据访问操作的，来完成数据库当中数据的增删改查操作。

![20-1](picture/20-1.png)

> 在三层架构当中，前端发起请求首先会到达Controller(不进行逻辑处理)，然后Controller会直接调用Service 进行逻辑处理， Service再调用Dao完成数据访问操作。



如果我们在执行具体的业务处理之前，需要去做一些通用的业务处理，比如：我们要进行统一的登录校验，我们要进行统一的字符编码等这些操作时，我们就可以借助于Javaweb当中三大组件之一的过滤器Filter或者是Spring当中提供的拦截器Interceptor来实现。

![20-2](picture/20-2.png)



而为了实现三层架构层与层之间的解耦，我们学习了Spring框架当中的第一大核心：IOC控制反转与DI依赖注入。

> 所谓控制反转，指的是将对象创建的控制权由应用程序自身交给外部容器，这个容器就是我们常说的IOC容器或Spring容器。
>
> 而DI依赖注入指的是容器为程序提供运行时所需要的资源。



除了IOC与DI我们还讲到了AOP面向切面编程，还有Spring中的事务管理、全局异常处理器，以及传递会话技术Cookie、Session以及新的会话跟踪解决方案JWT令牌，阿里云OSS对象存储服务，以及通过Mybatis持久层架构操作数据库等技术。

![20-3](picture/20-3.png)



我们在学习这些web后端开发技术的时候，我们都是基于主流的SpringBoot进行整合使用的。而SpringBoot又是用来简化开发，提高开发效率的。像过滤器、拦截器、IOC、DI、AOP、事务管理等这些技术到底是哪个框架提供的核心功能？

![20-4](picture/20-4.png)

> Filter过滤器、Cookie、 Session这些都是传统的JavaWeb提供的技术。
>
> JWT令牌、阿里云OSS对象存储服务，是现在企业项目中常见的一些解决方案。
>
> IOC控制反转、DI依赖注入、AOP面向切面编程、事务管理、全局异常处理、拦截器等，这些技术都是 Spring Framework框架当中提供的核心功能。
>
> Mybatis就是一个持久层的框架，是用来操作数据库的。



在Spring框架的生态中，对web程序开发提供了很好的支持，如：全局异常处理器、拦截器这些都是Spring框架中web开发模块所提供的功能，而Spring框架的web开发模块，我们也称为：SpringMVC

![20-5](picture/20-5.png)

> SpringMVC不是一个单独的框架，它是Spring框架的一部分，是Spring框架中的web开发模块，是用来简化原始的Servlet程序开发的。



外界俗称的SSM，就是由：SpringMVC、Spring Framework、Mybatis三块组成。

基于传统的SSM框架进行整合开发项目会比较繁琐，而且效率也比较低，所以在现在的企业项目开发当中，基本上都是直接基于SpringBoot整合SSM进行项目开发的。

到此我们web后端开发的内容就已经全部讲解结束了。























# 结尾分割线

