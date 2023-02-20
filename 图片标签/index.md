# Web学习笔记 - 图片标签

<!--more-->

{{<image src="https://img0.baidu.com/it/u=1590746422,1746295824&fm=253&fmt=auto&app=120&f=JPEG?w=463&h=454" alt="">}}

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
        图片标签用于向当前页面中引入一个外部图片
        使用img标签来引入外部图片,img标签是一个自结束标签
        img这种元素用于替换元素(基于块和行内元素之间,具有两种元素的特点)
            属性:
                scr 属性指定的是外部图片的路径(路径规则和超链接是一样的)

                alt 属性是图片的描述,这个描述默认情况下不会显示,有些浏览器无法加载时显示
                    搜索引擎会根据alt中的内容来识别图片,如果不写alt属性,则图片不会被搜索引擎收录
                
                width 宽度(单位是像素)
                height 高度
                    - 如果宽度和高度中只修改了一个,另一个会等比例缩放
                注意:
                    一般情况在pc端,不建议修改图片的大小
                    但是在移动端,经常需要对图片进行缩放(主要大图缩小)

        图片的格式:
            jpeg(jpg)
                - 支持的颜色比较丰富,不支持透明效果,不支持动图
                - 一般用来显示照片
            gif
                - 支持的颜色比较少,支持简单透明,支持动图
                - 适合颜色单一的图片,动图
            png
                - 支持的颜色丰富,支持复杂透明,不支持动图
                - 颜色丰富,复杂透明图片(专为网页而生)
            webp
                - 这种格式是谷歌新推出的专门用来表示网页中的图片格式
                - 它具备其他图片格式所有的优点,而且文件大小特别小
                - 缺点,兼容性不好

            base64 (不是图片格式)
                - 将图片使用base64进行编码,这样可以将图片转换为字符,通过字符的形式来引入图片
                - 一般都是一些和网页一起加载的图片才会使用base64
                
            效果一样,用小的
            效果不一样,用效果好的
     -->

    <img src="./下载.jpeg" alt="京东">
    <br>

    <img src="https://img95.699pic.com/photo/50136/1351.jpg_wh300.jpg" alt="美女">
     <br>

</body>
</html>
```
