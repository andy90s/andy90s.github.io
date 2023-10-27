# Mac版Chrome浏览器关闭左右滑动

<!--more-->
## 前言
Mac 版 Chrome 自带双指手势前进后退功能,这个功能叫做`Overscroll history navigation`,但是这个功能很不好用，经常会误触，所以需要禁用。<br>
这个功能是根据MAC系统的设置来开启的,在MAC中这个功能叫做`在页面之间轻扫`，默认是开启的。<br>
下面就来介绍两种禁用方法。    
## 系统设置
### 触控板设置   

「系统偏好设置」 → 「触控板」 → 「更多手势」中的「在页面之间轻扫」,取消勾选。
<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image@master/blog/images/触摸板轻扫.png" title="触摸板轻扫" width="80%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 触摸板轻扫 </b>  </div>
</center>

### 鼠标设置

「系统偏好设置」 → 「鼠标」 → 「更多手势」中的「在页面之间轻扫」,取消勾选。   
<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image@master/blog/images/鼠标轻扫.png" title="鼠标轻扫" width="80%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 鼠标轻扫 </b>  </div>
</center>

## Chrome设置
上面系统设置会禁用所有的轻扫手势，如果只想禁用Chrome浏览器的轻扫手势，可以通过以下命令禁用：
### 禁用鼠标手势
```shell
defaults write com.google.Chrome AppleEnableMouseSwipeNavigateWithScrolls -bool false
```
### 禁用触控板手势
```shell
defaults write com.google.Chrome AppleEnableSwipeNavigateWithScrolls -bool false
```
