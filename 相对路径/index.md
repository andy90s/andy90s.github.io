# Web学习笔记 - 相对路径

<!--more-->
```css
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
        当我们需要跳转一个服务器内部的页面时,一般我们都会使用相对路径
            相对路径都会使用.或者..开头
                ./ 表示当前文件所在的目录
                ../ 表示当前文件所在目录的上一级
                
            ./可以省略不写, 如果不写./也不写../则就相当于写./
     -->
    <a href="../7列表.html">内部相对路径跳转</a>
</body>
</html>
```
