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

- 标题标签：<h1> - <h6>
- 水平线标签：<hr>
- 无语义行内元素标签：<span>

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

## 2.JavaScript







## 3.Vue、Element-UI

