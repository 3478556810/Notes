# Java Web

## 1.HTML、CSS

### 1.1定义

HTML：超文本标记语言

超文本:除了文字信息，还可以定义图片、音频、视频等内容。

标记语言：由标签构成的语言

- HTML标签都是预定义好的。
- HTML代码直接在浏览器中运行，HTML标签由浏览器解析。

HTML结构标签：

```html
<html>
    <head>
        <tittle>标题</tittle>
    </head>
    <body>
        
    </body>
</html>
```

特点：

- HTML标签不区分大小写
- HTML标签属性值单双引号都可以
- HTML语法松散

CSS：层叠样式表，用于控制页面的样式

### 1.2 基本标签和样式

#### 1.2.1 HTML标签

- 图片标签：<img>
  - src:指定图像的url（绝对路径/相对路径）
  - width：图像的宽度（像素/相对于父元素的百分比）
  - heigh：图像的高度（像素/相对于父元素的百分比）

- 视频标签：<video>
  - src：规定视频的url
  - controls：显示播放控件
  - width：播放器的宽度
  - heigh：播放器的高度

-  音频标签：<audio>

  - src：规定视频的url
  - controls：显示播放控件

- 段落标签：<p>

- 文本加粗标签：

  <b>

  <strong>(具有强调语义)

- 标题标签：<h1> - <h6>

- 水平线标签：<hr>

- 无语义行内元素标签：<span>

- 表格标签：

  - 定义表格

    <table>

  - 定义表格中的行，一个<tr>表示一行

  - 表示表头单元格，具有加粗居中效果,<th>

  - 表示普通单元格，<td>

完整实例：

![image-20230328162126448](C:\Users\undercurrent\AppData\Roaming\Typora\typora-user-images\image-20230328162126448.png)

```html
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

其中border定义了单元格间分隔线的粗细，cellspacing定义了单元格之间的空隙

- 表单标签

  - 场景：在网页中主要通过表单标签<form>实现数据采集功能，如注册、登录等数据采集

  - 表单项：不同类型的input元素、下拉列表、文本域等

    - 定义表单项，通过type属性控制输入形式

      ![image-20230328182214999](C:\Users\undercurrent\AppData\Roaming\Typora\typora-user-images\image-20230328182214999.png)

      ```html
      <input>
      ```

    - 定义下拉列表，<option>定义列表项

      ```html
      <select>
      ```

    - 定义文本域

      ```html
      <textarea>
      ```

  - 属性

    - action：规定当提交表单时向何处发送表单数据，URL
    - method：规定用于发送表单数据的方式，GET、POST
      - get：表单数据拼接在URL后面，如URL?username=xx,大小有限制
      - post：表单数据在请求体中携带，大小没有限制

#### 1.2.2 CSS样式

1.引入方式：

- 行内样式：写在标签的style属性中（不推荐）

  ```html
  <h1 style="...">
      
  </h1>
  ```

  

- 内嵌样式：写在style标签中（可以写在页面的任何位置，但通常约定写在head标签中）

  ```html
  <head>
      <style>...</style>
  </head>
  ```

  

- 外联样式：写在一个单独的.css文件中（需要通过link标签在网页中引入）

  ```html
   <link rel="stylesheet" href="css/xxx.css">
  ```

2.颜色表示

- 关键字：red、green
- rgb表示法：红蓝黄三原色，如rgb（255，0，0）
- 十六进制：#cccccc（可简写为#ccc）

3.CSS属性

color：设置文本颜色

font-size：字体大小（px）

text-decoration：规定添加到文本的修饰，none表示定义标准的文本

line-heigh：设置行高

text-indent：定义第一个行内容的缩进

text-align：规定元素中的文本的水平对齐方式

border：设置边框的属性，如1px solid #000

padding：内边距

margin：外边距

4.CSS选择器：用来选取需要设置样式的元素（标签）

- 元素选择器 

  标签名{...}![image-20230328111103837](C:\Users\undercurrent\AppData\Roaming\Typora\typora-user-images\image-20230328111103837.png)

- id选择器

  #id属性值{...}

  ![image-20230328111141934](C:\Users\undercurrent\AppData\Roaming\Typora\typora-user-images\image-20230328111141934.png)

- 类选择器

  .class属性值{...}

![image-20230328111330262](C:\Users\undercurrent\AppData\Roaming\Typora\typora-user-images\image-20230328111330262.png)

优先级：id选择器>类选择器>元素选择器

5.超链接

标签：<a>

属性：

href：指定资源访问的url

target：指定在何处打开资源链接

​	_self：默认值，在当前页面打开

​	_blank：在空白页面打开

### 1.2.3 盒子模型

盒子：页面中的所有元素（标签），都可以视为一个盒子，由盒子将页面中的元素包含在一个矩形区域内，通过盒子的视角更方便的进行页面布局

盒子模型组成：内容区域（content）、内边距区域（padding）、边框区域（border）、外边距区域（margin）

![image-20230328155943551](C:\Users\undercurrent\AppData\Roaming\Typora\typora-user-images\image-20230328155943551.png)

布局标签：实际开发网页中，会大量频繁地使用div和span这两个没有语义的布局标签。

特点：

- div标签：
  - 一行只显示一个（独占一行）
  - 宽度默认是父元素的宽度，高度默认由内容撑开
  - 可以设置宽高（width、height）
- span标签
  - 一行可以显示多个
  - 宽度和高度默认由内容撑开
  - 不可以设置宽高（width、height）

## 2.JavaScript

## 2.1定义

JavaScript是一门跨平台、面向对象的脚本语言，是用来控制网页行为的，它能使网页可交互。

## 2.2引入方式

- 内部脚本：将JS代码定义在HTML页面中

  - JavaScript代码必须位于<script></script>标签之间，即script标签不能自闭合
  - 在HTML文档中，可以在任意地方，放置任意数量的<script>
  - 一般会把脚本置于<body>元素的底部，可改善显示速度

  ```html
  <script> 
      alert("hello")
  </script>
  ```

  

- 外部脚本：将JS代码定义在外部JS文件中，然后引入到HTML页面中

  - 外部JS文件中，只包含JS代码，不包含<script>标签

    在HTML引入

    ```html
    <script src="js/demo.js"></script>
    ```

    并在外部JS文件写JS代码

    ```javascript
    alert("hello")
    ```

## 2.3基础语法

### 2.3.1 书写语法

- 区分大小写：与Java一样，变量名、函数名以及其他一切东西都是区分大小写的
- 每行的结尾的分号可有可无
- 输出语句
  - 使用window.alert()写入警告框
  - 使用document.write()写入HTML输出
  - 使用console.log()写入浏览器控制台

### 2.3.2 变量 

- JavaScipt中用var关键字来声明变量
- JavaSript是一门弱类型语言，变量可以存放不同类型的值
- 变量名需要遵循如下规则：
  - 组成字符可以是任何字母、数字、下划线（_）或美元符号（$）
  - 数字不能开头
  - 建议使用驼峰命名
- 关键字var：
  - 由var修饰的变量作用域较大，即全局变量
  - 可以重复声明
- 关键字let：
  - 由let修饰的变量作用域只在所在的代码块内有效，即局部变量，且不允许重复声明
- 关键字const：
  - 用来声明一个只读的常量，一旦声明，常量的值就不能改变



### 2.3.3 数据类型、运算符

JavaScript中分为：原始类型和引用类型。

- 原始类型

  - number：数字（整数、小数、NaN（Not a Number））
  - string：字符串，单双引号皆可
  - boolean：布尔。true，false
  - null：对象为空，使用typeof查看类型会返回object，这是一种沿承的错误
  - undefined：当声明的变量未初始化时，该变量的默认值是undefined

- 运算符

  - 算术运算符

  - 赋值运算符

  - 比较运算符

    - ==与=== 

      - ==会进行类型转换，===不会进行类型转换（全等符只要类型不一样就返回false,当且仅当类型相等且内容相等时返回false）

      ```javascript
          var age = 20;
          var _age = "20";
          var $age = 20;
          
          alert(age == _age);//true
          alert(age === _age);//false
          alert(age === $age);//true
      ```

      

  - 逻辑运算符

  - 三元运算符

- 类型转换

  - 字符串类型转为数字：

    - 将字符串字面值转为数字，如果字面值不是数字，则转为NaN

      ```js
      alert(parseInt("12")); //12
      alert(parseInt("12A45")); //12(遇到第一个字面值不为数字的字符，自动停止转换)
      alert(parseInt("A45"));//NaN
      ```

  - 其他类型转为boolean：

    - Number：0和NaN为false，其他均为true
    - String：空字符串为false，其他均转为true
    - Null和undefined：均转为false

### 2.3.4 函数

- 介绍：函数（方法）是被设计为执行特定任务的代码块。

- 定义：JavaScript函数通过function关键字进行定义，语法为：

  ```javascript
  function functionName(参数1，参数2，...){
      //要执行的代码
  }
  ```

  或者

  ```javascript
  var functionName=function(a,b){
      return a+b;
  }
  ```

  

- 注意：

  - 形式参数不需要类型，因为JavaScript是弱类型语言
  - 返回值也不需要定义类型，可以在函数内部直接使用return返回即可

- 调用：函数名称（实际参数列表）

  ```javascript
  function add(a,b){
      return a+b;
  }
  ```

### 2.3.5 对象

- Array对象：用于定义数组

  - 定义

    var 变量名=new Array(元素列表)； 

    ```javascript
    var arr =new Array(1,2,3,4);
    ```

    或者，var 变量名=[元素列表]

    ```javascript
    var arr =[1,2,3,4]
    ```

  - 访问

    arr[索引]=值；

    ```javascript
    arr[10] ="hello"
    ```

  - 特点：JavaScript中的数组相当于Java中的集合，数组的长度是可变的，而Javascript是弱类型，所以可以存储任意的类型
    - 属性：
      - length：设置或返回数组中元素的数量

  - 方法：

    - forEach()：遍历数组中的每个有值的元素，并调用一次传入的函数（for方法遍历所有元素，包括undefined）

      - 箭头函数简化forEach()，类似于lambda：

      ```javascript
          array.forEach(element => {
              console.log(element)
          });
      ```

      

      - push(新元素...)：将新元素添加到数组的末尾，并返回新的长度
      - splice(起始索引，删除)：从数组中删除元素

- String对象：字符串对象

  - 定义

    var 变量名=new String(“...”)； 

    ```javascript
    var arr =new String("hello");
    ```

    或者，var 变量名="..."

    ```javascript
    var str ="hello"
    ```

  - 属性：
    - length：字符串的长度
  - 方法：
    - charAt()：返回指定位置的字符
    - indexOf()：检索字符串
    - trim()：去除字符串两边的空格
    - substring()：提取字符串中两个指定的索引号之间的字符（含头不含尾）

- JavaScript自定义对象

  - 定义格式：

    var 对象名={

    属性名：属性值，

    函数名称(形参列表){}

    };

    ```js
        var user={
            name:"gala",
            eat(){
              alert(this.name+"五杀了")
            }
        }
    
    user.eat()
    ```

- 调用格式：

  对象名.属性名

  ```js
  console.log(user.name)
  ```

  对象名.函数名()

  ```js
  user.eat();
  ```

- Json

  - 概念：JavaScript Object Notation，JavaScript对象标记法

  - JSON是通过JavaScript对象标记法书写的文本

  - 现多用于数据载体，在网络中进行数据传输

  - 定义：

    var 变量名 ='{"key1":value1}'

    ```js
    var user='{"name":"gala","career":["sdg","dmo","rng"],"obj"={...}}'
    ```

  - JSON字符串转为JS对象

    ```js
    var jsObject = JSON.parse(jsonStr)
    ```

  - JS对象转为JSON字符串

    ```js
    var jsonStr = JSON.stringify(jsObject)
    ```

- BOM对象

  - 概念：Browser Object Model 浏览器对象模型，允许JavaScript与浏览器对话，JavaScript将浏览器的各个组成部分封装成对象

  - 组成：

    - Window：浏览器窗口对象

      - 获取：直接使用window，其中window.可以省略，如

        ```js
        window.alert("Hello Window")//相当于alert()
        ```

      - 属性

        - history：对History对象的只读引用
        - location：用于窗口或框架的Location对象
        - navigator：对Navigator对象的只读引用

      - 方法

        - alert()：显示带有一段消息和一个确认按钮的警告框

        - confirm()：显示带有一段消息以及确认按钮和取消按钮的对话框

        - setInterval()：按照指定的周期（以毫秒计）来调用函数或计算表达式

          ```js
          var i=0
              setInterval(function(){
                  i++
                  console.log(i)
              },1000)//每隔一秒打印自增数
          ```

          

        - setTimeout()：在指定的毫秒数后调用函数或计算表达式

        ```js
         setTimeout(function(){
                alert("hello")
            },3000)//延迟三秒后弹出
        ```

        

    - Navigator：浏览器对象

    - Screen：屏幕对象

    - History：历史记录对象

    - Location：地址栏对象

      - 获取：使用window.location获取，其中window.可以省略，如window.location.属性
      - 属性：
        - href：设置或返回完整的URL location.href ="https://..."

    

- DOM对象

  - 概念：Document Object Model，文档对象模型

  - 将标记语言的各个组成部分封装为对应的对象

    - Document：整个文档对象
    - Element：元素对象
    - Attribute：属性对象
    - Text：文本对象
    - Comment：注释对象

  - JavaScript通过DOM，就能够对HTML进行操作：

    - 改变HTML元素的内容
    - 改变HTML元素的样式（CSS）
    - 对HTML DOM事件作出反应
    - 添加和删除HTML元素

  - HTML中的Element对象可以通过Document对象获取，而Document对象是通过Window对象获取的

  - Document对象中提供了以下获取Element元素对象的函数：

    - 根据id属性值获取，获取单个Element对象

      ```js
      var h1 = document.getElementById('h1')
      ```

    - 根据标签名称获取，返回Element对象数组

      ```js
      var divs = document.getElementsByTagName('div')
      ```

    - 根据name属性值获取，返回Element对象数组

      ```js
      var hobbys = document.getElementsByName('hobby')
      ```

    - 根据class属性值获取，返回Element对象数组

      ```js
      var clss = document.getElementsByClassName('cls')
      ```

- 事件

  - 事件监听

    - 概念：Javascript在事件被检测时执行代码

  - 事件绑定

    - 通过HTML标签中的事件属性进行绑定

      ```html
      <input type='button' onclick='on()' value='btn1'>
          <script>
              function on(){
                  alert('我被点击了')
              }
      </script>
      ```

    - 通过DOM元素属性绑定

      ```html
      <input type='button' id='btn' value='btn2'>
      
      <script>
      document.getElementById('btn').onclick=function(){
          alert('我被点击了')
      }
      </script>
      ```

  - 常见事件

    - onclick：鼠标单击事件
    - onblur：元素失去焦点
    - onfocus：元素获得焦点
    - onload：某个页面或图像被完成加载
    - onsubmit：当表单提交时触发该事件
    - onkeydown：某个键盘的键被按下
    - onmouseover：鼠标被移到某元素之上
    - onmouseout：鼠标从某元素移开

## 3.Vue、Element-UI

## 3.1 Vue

### 3.1.1 概念

- Vue是一套前端框架，免除原生JavaScript中的DOM操作，简化书写
- 基于MVVM（Model-View-ViewModel）思想，实现数据的双向绑定，将编程的关注点放在数据上

![image-20230329221303365](C:\Users\undercurrent\AppData\Roaming\Typora\typora-user-images\image-20230329221303365.png)



### 3.1.2 Vue快速入门

- 新建HTML页面，引入Vue.js文件
- 在JS代码区域，创建Vue核心对象，定义数据模型
- 编写视图

```html
<head>
    <script src="js/vue.js"></script>
</head>
<body>
    <div id="app">
<input type="text" v-model="message">
        <!-- 由于Vue的双向数据绑定特性，输入框输入的数据会同步到视图 -->
{{message}}
    </div>
</body>
<script>
    new Vue({
        el:"#app",
        data:{
            message:"gala五杀了"
        }
    })
</script>
```



![image-20230329222037981](C:\Users\undercurrent\AppData\Roaming\Typora\typora-user-images\image-20230329222037981.png)

### 3.1.3 Vue常用指令

- 指令：HTML标签上带有v-前缀的特殊属性，不同指令具有不同含义。

- 常用指令

  - v-bind：为HTML标签绑定属性值，如设置href、css样式等，可简化为 ‘:’

    ```html
    <a :href="url">链接</a> 
    <!--等同于<a v-bind:href="url">链接</a>-->
    ```

    

  - v-model：在表单元素上创建双向数据绑定

  - v-on：为HTML标签绑定事件，可简化为'@'

    ```html
    <body>
        <div id="app">
    
            <input type="button" value="点我一下" v-on:click="handle()"  >
    
            <input type="button" value="点我一下" @click="handle()">
    
        </div>
    </body>
    <script>
        //定义Vue对象
        new Vue({
            el: "#app", //vue接管区域
            data:{
               
            },
            methods: {
                handle(){
                    alert("我被点击了")
                }
            }
        })
    </script>
    ```

    

  - v-if

  - v-else-if：条件性的渲染某元素，判定为true时渲染，否则不渲染

  - v-else

    ```html
       年龄<input type="text" v-model="age">经判定,为:
            <span  v-if="age<35">年轻人(35及以下)</span>
    <!--这里作了代码优化，只要不满足第一个span，age必然>=35,故第二个span只需判定age<60-->
            <span v-else-if="age<60">中年人(35-60)</span>
            <span v-else>老年人(60及以上)</span>
    ```

    

  - v-show：根据条件展示某元素，区别在于渲染该元素后通过调整display属性的值来决定是否显示

  - v-for：列表渲染，遍历容器的元素或者对象的属性

    ```html
    <body>
        <div id="app">
    <div v-for="(item, index) in arr" :key="index">
       {{index}}: {{item}}
    </div>
    
        </div>
    </body>
    <script>
        //定义Vue对象
        new Vue({
            el: "#app", //vue接管区域
            data:{
               arr:["北京", "上海", "西安", "成都", "深圳"]
            },
            methods: {
                
            }
        })
    </script>
    ```

    ![image-20230329230008901](C:\Users\undercurrent\AppData\Roaming\Typora\typora-user-images\image-20230329230008901.png)

案例：渲染表格

```html
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

            <tr align="center" v-for="(item, index) in users" :key="index">
                <td>{{index+1}}</td>
                <td>{{item.name}}</td>
                <td>{{item.age}}</td>
                <td>
                    <span v-if="item.gender==1">男</span>
                    <span v-if="item.gender==2">女</span>
                </td>
                <td>{{item.score}}</td>
                <td>
                    <span v-if="item.score>=85">优秀</span>
                    <span v-else-if="item.score>=60">及格</span>
                    <span style="color: red; " v-else>不及格</span>
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
```

![image-20230329231407444](C:\Users\undercurrent\AppData\Roaming\Typora\typora-user-images\image-20230329231407444.png)

### 3.1.4 Vue的生命周期

- 生命周期：指一个对象从创建到销毁的整个过程

- 生命周期的八个阶段：每触发一个生命周期事件，会自动执行一个生命周期方法（钩子）

  - beforeCreate：创建前
  - created：创建后
  - beforeMount：挂载前
  - mounted：挂载完成，Vue初始化成功，HTML页面渲染成功。（发送请求到服务端，加载数据）
  - beforeUpdate：更新前
  - updated：更新后
  - beforeDestroy：销毁前
  - destroyed：销毁后

  ![image-20230329231926687](C:\Users\undercurrent\AppData\Roaming\Typora\typora-user-images\image-20230329231926687.png)







