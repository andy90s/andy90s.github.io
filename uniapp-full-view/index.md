# Uni页面撑满整个view

<!--more-->
## Uni页面撑满整个view
设置了父元素高度100%后,没有撑满整个view,如下图所示:
<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image@master/blog/images/202305081351874.png" title="示例" width="40%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 示例 </b>  </div>
</center>

## 解决方法

增加如下样式代码,**uni-page-body,html,body{height:100%}**:
```css
<style lang="scss" scoped>
uni-page-body,html,body{height:100%}
.page {
	display: flex;
	background-color: #FFFFFF;
	height: 100%;
}
</style>
```

## 效果

<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image@master/blog/images/202305081356147.png" title="效果" width="40%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 效果 </b>  </div>
</center>

