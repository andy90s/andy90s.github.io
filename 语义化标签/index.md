# Web学习笔记 - 语义化标签

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
        在网页中HTML专门用来负责网页的结构
            所以在使用html标签时,应该关注的是标签的语义,而不是样式

            标题标签:
                h1~h6 一共六级标题
                从h1~h6重要性递减,h1最重要,h6最不重要
                h1在网页中的重要性仅次于title标签,一般情况下一个页面只会有一个h1
                一般情况下标题标签只会使用到h1~h3,h4~h6很少用

                标题标签都是块元素

            块元素,在页面中独占一行的元素(block element)
     -->
     <h1>一级标题</h1>
     <h2>二级标题</h2>
     <h3>三级标题</h3>
     <h4>丝级标题</h4>
     <h5>五级标题</h5>
     <h6>六级标题</h6>
     <!-- 
         hgroup 标签用来标题分组,可以将一组相关的标题同时放入hgroup
      -->
     <hgroup>
        <h1>大标题</h1>
        <h2>小标题</h2>
     </hgroup>
     
     <!-- 
         p标签表示页面汇总的一个段落;

         p也是一个块元素
     -->
      <p>段落1</p>
      <p>段落2</p>
      <!-- 
          em标签用于表示语音语调的一个加重

          在页面中不会独占一行的元素成为行内元素(inline element)
       -->

      <p>今天天气<em>真</em>不错!</p>
       <!-- 
           strong 表示强调,重要的内容
        -->
      <p>你今天必须要<strong>完成作业!</strong></p>

      鲁迅说,
      <!-- 
          blockquote 表示一个引用
       -->
      <blockquote>这句话我是从来没有说过</blockquote>
       <!-- 
           q 表示一个短引用
        -->
      子曰<q>学而时学志,乐呵乐呵!</q>
        <!-- 
            br 表示换行
        -->
      <br>
      <br>
      今天天气真不错
</body>
</html>
```

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
        块元素(block element)
            - 在网页中一般通过块元素对页面进行布局
        行内元素(inline element)
            - 行内元素主要用来包裹文字

            - 一般情况下会在块元素中放行内元素,而不会在行内元素中放块元素
            - 块元素中基本什么都能放
            - p元素中不能放任何的块元素
        
            浏览器在解析网页时,会自动对网页中不符合规范的内容进行修整
                比如:
                    标签写在了根元素的外部
                    p元素中嵌套了块元素
                    根元素中出现了除head和body以外的子元素
     -->
</body>
</html>
```

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
        header 表示网页的头部
        main 表示网页的主体部分(一个网页中只会有一个main)
        footer 表示网页的底部
        nav 表示网页的导航
        aside 表示主体相关的其他内容(侧边栏)
        article 文章
        section 表示一个独立的区块,上边的标签都不能表示时使用section

        div 没有语义,就是表示一个区块
        span 行内元素,没有语义,一般用于在网页中选中文字
    -->
    <header></header>
    <main></main>
    <footer></footer>
    <nav></nav>
    <aside></aside>
    <article></article>
    <section></section>
    <div></div>
    <span></span>
</body>
</html>
```

