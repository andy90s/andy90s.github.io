# Web学习笔记 - CSS简介

<!--more-->
## 简介
```scss
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>

    <!-- 
        第二种方式(内部样式表)
            - 将样式编写到head中的style标签里
                然后通过css的选择器来选中元素并为其设置各种样式
                可以同时为多个标签设置样式,并且修改时只需修改一处即可全部应用
            - 内部样式表更加方便对样式进行复用
            - 问题:
                我们的内部样式只能对一个网页起作用
                    它里面的样式不能跨页面复用
     -->
     <!-- <style>
         /* 所有的p元素 */
         p{color: red; font-size: 30px;}
     </style> -->

     <!-- 
         第三种方式(外部样式表)
            - 可以将CSS样式编写到一个外部的CSS文件中
                然后通过link标签来引入外部的css文件
            - 外部样式需要通过link标签进行引入
                意味着只要想使用这些样式的网页都可以对其使用,使样式可以再不同页面之间进行复用
            - 将样式编写到外部的css文件中,可以使用到浏览器的缓存机制
                从而加快的网页的加载速度,提高用户的体验
      -->
      <link rel="stylesheet" href="./style.css">
      <!--
        p{
            color: red;
            font-size: 30px;
        }
      -->
</head>
<body>
    <!--  
        网页分成三个部分:
            结构(html)
            表现(css)
            行为(JavaScript)
        CSS
            - 层叠样式表
            - 网页实际上是一个多层的结构,通过css可以分别为网页的每一个层来设置样式
                而最终我们能看到的至少网页的最上边一层
            - 总之一句话,css用来设置网页中元素的样式
     -->

     <!-- 
         使用css来修改元素的样式

         第一种方式(内联样式,或者叫行内样式)
            - 在标签内部通过style属性来设置元素的样式
            - 问题:
                使用内联样式,样式只能对一个标签生效
                    如果希望影响到多个元素必须每个元素复制一遍
                    并且当样式发送变化时,我们必须一个一个修改
            - 注意: 开发时绝对不要使用内联样式
      -->
     <!-- <p style="color: red; font-size: 30px;">少小离家老大回,乡音无改鬓毛衰</p>
     <p style="color: red; font-size: 30px;">今天天气真不错</p> -->

     <p>少小离家老大回,乡音无改鬓毛衰</p>
     <p>今天天气真不错</p>
</body>
</html>
```
## 语法
```scss
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>

    <style>
        /* 
            CSS中的注释,注释中内容会自动被浏览器忽略

            CSS的基本语法:
                选择器 声明块
                选择器,通过选择器可以选中页面中的指定元素
                    比如 p 的作用就是选中页面中所有的 p 元素
                声明块,通过声明块来指定要为元素设置的样式
                    声明块由一个一个的声明组成
                    声明是一个名值对结构
                        一个样式名对应一个样式值,名和值之间以`:`链接,以`;`结尾
                        
        */
        p{
            color: red;
            font-size: 30px;
        }
        h1{
            color: blue;
        }
    </style>
</head>
<body>
    <h1>我是h1</h1>
    <p>今天天气真不错</p>
    <p>今天天气真不错</p>
    <p>今天天气真不错</p>
    <p>今天天气真不错</p>
</body>
</html>
```
