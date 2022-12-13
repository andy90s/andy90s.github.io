# Web学习笔记 - 选择器

<!--more-->
## 复合选择器
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
            将所有的段落设置为红色(字体)

            元素选择器
                作用,根据标签名来选中指定的元素
                语法:标签名{}
                例子:p{} h1{} dic{}
        */
        /* p{
            color: red;
        }

        h1{
            color: red;
        } */

        /* 
            将`儿童乡间不相识`设置红色

            id选择器
                作用:根据元素的id属性值选中一个元素
                语法:#id属性值{}
                例子:#box{} #red{}
        */
        /* #red{
            color: red;
        } */

        /* 
            将`秋水共长天一色`和`落霞与孤鹜齐飞` 设置为红色

            类选择器
                作用:根据元素的class属性值选中一组元素
                语法: .class属性值
                例子: .bule .red .abc
        */
        /* .red{
            color: red;
        }
        .abc{
            font-size: 15px;
        } */

        /* 
            通配选择器
                作用:选中页面中的所有元素
                语法: *
        */
        *{
            color: red;
        }

    </style>
</head>
<body>
    <h1 class="red">我是标题</h1>
    <p>少小离家老大回</p>
    <p>乡音无改鬓毛衰</p>
    <!-- 
        多个类选择时使用空格隔开;
     -->
    <p class="red abc">儿童相见不相识</p>
    <p>笑问客从何处来</p>
    <!-- 
        class 是一个标签的属性,它和id类似,不同的是class可以重复使用
            可以通过class属性来为元素分组
     -->
    <p class="red">秋水共长天一色</p>
    <p>落霞与孤鹜齐飞</p>
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
    <style>

        /* 将class为red的元素设置为红色(字体) */
        .red{
            color: red;
        }

        /* 将class为red的div字体大小设置为30px */
        /* 
            交集选择器
                作用:选中同时符合多个条件的元素
                语法:选择器1选择器2选择器3...{}
                注意点:交集选择器中如果有元素选择器,必须使用元素选择器开头
        */
        /* 比如下面就是 元素选择器+类选择器 */
        /* 与的关系& */
        div.red{
            font-size: 30px;
        }

        /* 
            选择器分组(并集选择器)
                作用:同时选择多个选择器对应的元素
                语法:选择器1,选择器2...{}
        */
        /* 或的关系|| */
        h1,span{
            color: blue;
        }



    </style>
</head>
<body>
    <div class="red">我的div</div>
    <p class="red">我是p</p>
    <h1 >标题</h1>
    <span >哈哈</span>
</body>
</html>
```
## 关系选择器
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
            为div的子元素span设置一个字体颜色红色
            (为div直接包含的span设置一个字体颜色)

            子元素选择器
                作用: 选中指定父元素的指定子元素
                语法: 父元素 > 子元素
        */
        /* div.box > span {
            color: red;
        } */

        /* 
            后代元素选择器:
                作用: 选中指定元素内的指定后代元素
                语法: 祖先 后代
        */

        /* div span {
            color: blue;
        } */

        /* div > p > span {
            color: orange;
        } */

        /* 
            选中下一个兄弟
                语法: 前一个 + 后一个 (要紧挨着)
            选中下边所有的兄弟
                语法: 兄 ~ 弟
        */
        p + span {
            color: aqua;
        }

    </style>
</head>
<body>
    
    <!-- 
        父元素
            - 直接包含子元素的元素叫做父元素
        子元素
            - 直接被父元素包含的元素叫子元素
        祖先元素
            - 直接或间接包含后代元素的元素叫祖先元素
            - 一个元素的父元素也是它的祖先元素
        后代元素
            - 直接或者间接被祖先元素包含的元素叫做后代元素
            - 子元素也是后代元素
        兄弟元素
            - 拥有相同父元素的元素是兄弟元素
     -->
    <div class="box">我是一个div
        <p>我的div中的p
            <span>
                我是p中的span
            </span>
        </p>
        <span>我的div中的span</span>

    </div>
    <span>
        我是div外的span1
    </span>
    <p>
        我是div外的p
    </p>
    <span>
        我是div外的span2
    </span>
</body>
</html>
```
## 属性选择器
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
            [属性名] 选择含有指定属性的元素
            [属性名=属性值] 选择含有指定属性和属性值的元素
            [属性名^=属性值] 选择属性值以指定值开头的元素
            [属性名$=属性值] 选择属性值以指定值结尾的元素
            [属性名*=属性值] 选择属性值中含有某值的元素
        */
        /* p[title=abc] {
            color: orange;
        } */

        /* p[title^=abc]{
            color: red;
        } */

        /* p[title$=abc]{
            color: orange;
        } */

        p[title*=e]{
            color: aqua;
        }
    </style>
</head>
<body>

    <p title="abc">少小离家老大回</p>
    <p title="abcd">乡音无改鬓毛衰</p>
    <p title="adsiaje">儿童相见不相识</p>
    <p>笑问客从何处来</p>
    <p>秋水共长天一色</p>
    <p>落霞与孤鹜齐飞</p>
</body>
</html>
```
## 伪类选择器
```scss
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        p{
            font-size: 20px;
        }
        /* 
         伪元素,表示页面中一些特殊的并不真实的存在的元素(特殊的位置)
            伪元素使用 :: 开头
            ::first-letter 表示第一个字母
            ::first-line 表示第一行
            ::selection 表示选中的内容
            ::before 表示元素的起始
            ::after 表示元素的结束
                -before atfer 必须结合content属性来使用
        */
        p::first-letter {
            font-size: 50px;
        }
        p::first-line {
            background-color: blue;
        }
        p::selection {
            background-color: yellow;
        }
        div::before {
            content: "abc";
            color: red;
        }
        div::after {
            content: "haha";
            color: greenyellow;
        }
    </style>
</head>
<body>
    <q>
        hello
    </q>
    <p>
        这是一个p标签
    </p>

    <div>hello hello hello</div>
</body>
</html>
```
## 伪元素选择器
```scss
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        p{
            font-size: 20px;
        }
        /* 
         伪元素,表示页面中一些特殊的并不真实的存在的元素(特殊的位置)
            伪元素使用 :: 开头
            ::first-letter 表示第一个字母
            ::first-line 表示第一行
            ::selection 表示选中的内容
            ::before 表示元素的起始
            ::after 表示元素的结束
                -before atfer 必须结合content属性来使用
        */
        p::first-letter {
            font-size: 50px;
        }
        p::first-line {
            background-color: blue;
        }
        p::selection {
            background-color: yellow;
        }
        div::before {
            content: "abc";
            color: red;
        }
        div::after {
            content: "haha";
            color: greenyellow;
        }
    </style>
</head>
<body>
    <q>
        hello
    </q>
    <p>
        这是一个p标签
    </p>

    <div>hello hello hello</div>
</body>
</html>
```
