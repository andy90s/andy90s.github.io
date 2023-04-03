# Xcode问题记录

<!--more-->
## 前言
记录Xcode等相关的问题
## Xcode14.3 编译报错
pod第三方库的支持版本改为11.0以上
<center>
{{<image src="https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/202304031423551.png" title="设置版本" width="50%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 设置版本 </b>  </div>
</center>

## Xcode14.3 打包报错`command phasescriptexecution failed with a nonzero exit code`
如图脚本文件增加 `-f`
<center>
{{<image src="https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/202304031451060.png" title="示例" width="100%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 示例 </b>  </div>
</center>

