# Whistle抓包工具

<!--more-->


## 抓包工具
Windows平台有`finder`   
Mac平台有`Charles`
都是比较常用的,因为我是mac刚开始用的也是**Charles**,但是因为公司路由器等原因,经常性的无法抓包,这里分享另一个开源工具 {{<link href="https://github.com/avwo/whistle" content="【Whistle】">}}

### 1.安装
```zsh
npm install -g whistle
```
### 2.启动
```zsh
w2 start
```
默认是8899端口,如果需要修改端口,可以在启动命令后面加上端口号,比如
```zsh
w2 start -p 8888
```

启动之后可以在浏览器中输入终端提示链接打开抓包工具
<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image@master/blog/images/202304131941846.png" title="启动" width="80%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 启动 </b>  </div>
</center>

### 3.停止
```zsh
w2 stop
```

### 3.手机配置
- 手机连接电脑,打开wifi,选择电脑的wifi
- 手机设置代理,代理地址填写电脑的ip地址,端口号填写`8899`
- 手机用safari输入`rootca.pro`下载证书,安装证书,也可以到网页设置页面扫码安装如下图
- 到手机的设置中,找到`通用`->`关于本机`->`证书信任设置`,找到刚才安装的证书,打开信任开关
- 电脑打开`http://localhost:8899/`点击左侧`network`->`enable`开启抓包

<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image@master/blog/images/202305161441116.png" title="扫码安装证书" width="100%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 扫码安装证书 </b>  </div>
</center>

<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image@master/blog/images/202304131947012.png" title="开启" width="50%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 抓包 </b>  </div>
</center>

{{< admonition tip "">}}
如果证书配置成功,但是抓包不成功,并且网络也正常,但是手机却无法访问网络,这时候可以尝试关闭电脑的wifi,然后再打开,这样手机就可以访问网络了
{{< /admonition >}}

### MOCK数据

