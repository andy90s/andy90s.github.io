# Web学习笔记 - a元素的伪类

<!--more-->
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
            link 一般用来表示没有访问过的链接(正常的链接)
        */
        a:link {
            color: red;
        }
        /* 
            visited 用来表示访问过的链接

            由于隐私的原因,visited这个伪类只能修改链接的颜色.
        */

        a:visited {
            color: yellow;
        }
        /* 
            :hover 用来表示鼠标移入的状态

        */
        a:hover {
            color: aqua;
            font-size: 50px;
        }

        /* 
            :active 用来表示鼠标点击
        */
        a:active {
            color: blue;
        }
    </style>
</head>
<body>
    <!-- 
        1.没有访问过的链接
        2.访问过的链接

     -->
    <a href="https://www.baidu.com">访问过的链接</a>
    <br>
    <a href="https://www.baidu.com123">没有访问过的链接</a>
</body>
</html>
```
