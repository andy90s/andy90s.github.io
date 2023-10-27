# Mac电脑安装Docker

<!--more-->

## 安装

```zsh
brew install --cask docker
```

## 查看版本

```zsh
docker --version
```

## 安装完毕打开客户端
在应用程序中找到Docker图标，双击打开，会出现如下界面，点击右上角的鲸鱼图标，选择preferences，进入设置界面，选择resources，设置内存大小，推荐设置为4G，然后点击apply & restart，重启docker，这样就可以了。
<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image@master/blog/images/docker.png" title="" width="20%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> Docker </b>  </div>
</center>

<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image@master/blog/images/docker_setting.png" title="Setting" width="90%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 默认的Setting </b>  </div>
</center>

这里我运行开源项目 {{<link href="https://github.com/rafalp/Misago/" content="【Misago】">}}截图

```zsh
docker compose up
```

<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image@master/blog/images/docker_demo.png" title="【Misago】" width="90%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 【Misago】 </b>  </div>
</center>

## 清理
默认docker占用磁盘60G，如果不清理，会占用很多磁盘空间，可以通过如下命令清理
```zsh
docker system prune -a
```

## 参考
{{<link href="https://yeasy.gitbook.io/docker_practice/install/mac" content="【Docker 入门到实践】">}}

