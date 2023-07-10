# MAC工作环境

<!--more-->
## 前言
习惯了用MAC电脑进行工作开发,搬砖的辅助生产工具也是必不可少,根据自己的经验分享一些常用的软件或者技巧,提升开发效率.
## 终端
{{<link href="https://iterm2.com/" content="【iterm2】">}}
### iterm2快捷键    
|快捷键组合|说明|
|---|---|
|ctrl + a|到行首|
|ctrl + e|行末|
|ctrl + u|删除一行|
|⌘ + r（ctrl + l）|清屏，其实是滚到新的一屏，并没有清空。|
|ctrl + r|搜索命令历史，这个大家都应该很熟悉了|
|ctrl + d|删除当前字符|
|ctrl + h|删除之前的字符|
|ctrl + w|删除光标前的单词|
|ctrl + k|删除到文本末尾|
|ctrl + t|交换光标处文本|
|⌘ + —/+/0|	调整字体大小|
|ctrl + f/b	|前进后退，相当于左右方向键，但是显然比移开手按方向键更快|
|ctrl + p	|上一条命令，相当于方向键上|
### 窗口操作
- **新建窗口：** shift + command + d（横向）command + d（竖向）   
- **关闭窗口：** shift + command + w    
- **前一个窗口：**  command + `   
- **后一个窗口：**  command + ~   
- **进入窗口 1,2,3：**  option + command + 编号   
### 标签页操作
- **新建标签页:** Command + T
- **关闭标签页:** Command + W
- **前一个标签页:** Command + 左方向键，Shift + Command + [
- **后一个标签页:** Command + 右方向键，Shitf + Command + ]
- **进入标签页1，2，3…:** Command + 标签页编号
- **Expose 标签页:** Option + Command + E（将标签页打撒到全屏，并可以全局搜索所有的标签页）
### 面板操作
- **垂直分割:** Command + D
- **水平分割:** Shift + Command + D
- **前一个面板:** Command + [
- **后一个面板:** Command + ]
- **切换到上/下/左/右面板:** Option + Command + 上下左右方向键      
  
## tree
```ruby
brew install tree
```
<center>
{{<image src="https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/202307070028887.png" title="tree" width="60%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> tree </b>  </div>
</center>

## oh-my-zsh
```ruby
curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
```
打开 .zshrc文件
修改插件配置(自带插件)
```shell
plugins=(colored-man-pages pod git git-flow ruby gem python pip node npm bower sublime)
```
### 新增其他插件,比如zsh-syntax-highlighting高亮插件
```
cd ~/.oh-my-zsh/custom/plugins
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
```
再次修改.zshrc文件插件配置
在sublime后面加上zsh-syntax-highlighting
然后执行
```
source ~/.zshrc 
```
比如执行 `pod install`
<center>
{{<image src="https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/Untitled.gif" title="pod install" width="100%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> pod install </b>  </div>
</center>

## Go2Shell
快捷打开当前文件路径的终端,并且支持`iterm2` {{<link href="https://zipzapmac.com/Go2Shell" content="【官网】">}}   
效果如下:

<center>
{{<image src="https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/go2shell%E6%BC%94%E7%A4%BA2.gif" title="Go2Shell" width="100%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> Go2Shell </b>  </div>
</center>

## Xcodes
{{<link href="https://www.xcodes.app/" content="【Xcodes】">}} 非常方便的管理Xcode的工具,优点: 
- 不必等苹果商店更新
- 可安装测试版本
- 多版本安装
- 下载快,不会卡99%

<center>
{{<image src="https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/202304031139069.png" title="Xcodes" width="100%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> Xcodes </b>  </div>
</center>

## AppCode

{{<link href="https://www.jetbrains.com/objc/" content="【AppCode】">}}

## MAC自带邮箱增加QQ邮箱
首先登录网页端QQ邮箱,然后到设置中生成授权码,在mac中添加QQ邮箱时密码填授权码即可.
<center>
{{<image src="https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/202304041500308.png" title="授权码" width="100%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 授权码 </b>  </div>
</center>

## 超级右键lite
<center>
{{<image src="https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/202304071450741.png" title="超级右键" width="30%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 超级右键 </b>  </div>
</center>

## Snipaste
超好用的截图贴板工具
{{<link href="https://www.snipaste.com/" content="【Snipaste】">}}

<center>
{{<image src="https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/snipaste.gif" title="snipaste" width="50%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> snipaste </b>  </div>
</center>

## 抓包工具
Windows平台有`finder`   
Mac平台有`Charles`
都是比较常用的,因为我是mac刚开始用的也是**Charles**,但是因为公司路由器等原因,经常性的无法抓包,这里分享另一个开源工具 {{<link href="https://github.com/avwo/whistle" content="【Whistle】">}}

### 1.安装
```
npm install -g whistle
```
### 2.启动
```
w2 start
```
启动之后可以在浏览器中输入终端提示链接打开抓包工具
<center>
{{<image src="https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/202304131941846.png" title="启动" width="80%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 启动 </b>  </div>
</center>

### 3.停止
```
w2 stop
```

### 3.手机配置
- 手机连接电脑,打开wifi,选择电脑的wifi
- 手机设置代理,代理地址填写电脑的ip地址,端口号填写`8899`
- 手机用safari输入`rootca.pro`下载证书,安装证书,也可以到网页设置页面扫码安装如下图
- 到手机的设置中,找到`通用`->`关于本机`->`证书信任设置`,找到刚才安装的证书,打开信任开关
- 电脑打开`http://localhost:8899/`点击左侧`network`->`enable`开启抓包

<center>
{{<image src="https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/202305161441116.png" title="扫码安装证书" width="100%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 扫码安装证书 </b>  </div>
</center>

<center>
{{<image src="https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/202304131947012.png" title="开启" width="50%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 抓包 </b>  </div>
</center>
