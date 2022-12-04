# Web学习笔记 - 超链接

<!--more-->
```scss
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <!-- 
        超链接可以让我们从一个页面跳转到其他页面,
            或者是当前页面的其他位置
        
        使用 a 标签来定义超链接
            属性:
                href 指定跳转的目标路径
                    - 值可以是一个外部网站的地址
                    - 值也可以是一个内部网站的地址

        超链接也是一个行内元素,在 a 标签中可以嵌套除它自身外任何元素
     -->
     <a href="https://www.baidu.com">百度</a>
     <br>
     <a href="https://www.baidu.com">百度</a>
     <br>
     <a href="7列表.html">内部页面</a>
</body>
</html>
```

```scss
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <!-- 
        target 属性, 用来指定超链接打开的位置
            可选值:
                _self 默认值,当前页中打开超链接
                _blank 在一个新的页面中打开超链接
     -->
     <a href="https://www.baidu.com" target="_blank">打开百度</a>
     <br>
     <!-- 
         在开发中可以将#作为超链接路径的占位符使用
    
      -->
     <a href="#">这是一个新的超链接</a>
     <br>
     
    <!-- 
        可以使用JavaScript:; 来作为href属性,此时点击这个超链接什么也不会发生
     -->
     <a href="javascript:;">这是一个新的超链接</a>
     <br>

     <a href="bottom">去底部</a>
     <!-- 
         可以直接将超链接的href属性设置为#,这样点击超链接后,
            页面不会发送跳转,而是转到当前页面的顶部的位置

         可以调整到页面的指定位置,只需讲href属性设置 #目标元素的id
         id属性(唯一不重复的)
            - 每一个标签都可以添加一个id属性
            - id属性就是元素的唯一标识,同一个页面中不能出现重复的id属性;
      -->
      <a id="bottom" href="#">回到顶部</a>
      <br>
    </body>
</html>
```
