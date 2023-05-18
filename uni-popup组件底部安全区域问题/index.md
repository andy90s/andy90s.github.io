# Uni Popup组件底部安全区域问题

<!--more-->
## uni-popup 带圆角的情况下，底部安全区域问题
官方自带了一个popup组件，但是在ios手机上，如果popup组件带圆角，会出现底部安全区域问题，如下图所示：
<center>
{{<image src="https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/202305181545761.png" title="底部安全区域问题" width="50%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 底部安全区域问题 </b>  </div>
</center>

## 解决方案 `:safeArea=false`
```js
<uni-popup type="bottom" :safeArea=false>
    <view class="content">
        <view>标题</view>
        <view>内容</view>
    </view>
</uni-popup>

.content {
    background-color: #fff;
    border-radius: 20upx 20upx 0 0;
    /*兼容 IOS<11.2*/
		padding-bottom: constant(safe-area-inset-bottom);
		/*兼容 IOS>11.2*/
		padding-bottom: env(safe-area-inset-bottom);
}
```
