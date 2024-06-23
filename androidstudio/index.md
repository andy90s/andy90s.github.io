# AndroidStudio 代理设置提示报错 You have JVM property https.proxyHost set to '127.0.0.1'

<!--more-->

## 报错信息
```
You have JVM property https.proxyHost set to '127.0.0.1'.
This may lead to incorrect behaviour.
Proxy settings are applied to all network connections made by Android Studio.
```
## 当前环境
```
Android Studio Koala | 2024.1.1
Build #AI-241.15989.150.2411.11948838, built on June 11, 2024
Runtime version: 21.0.2+13-b375.1 aarch64
VM: OpenJDK 64-Bit Server VM by JetBrains s.r.o.
macOS 14.5
GC: G1 Young Generation, G1 Concurrent GC, G1 Old Generation
Memory: 4096M
Cores: 16
Metal Rendering is ON
```

## 解决方法
这是由于本地的代理服务导致的,解决办法如下:

1.help(桌面顶部工具栏) -> Edit Custom VM Options<br>

2.添加如下配置:
```
-Dhttp.proxyHost
-Dhttp.proxyPort
-Dhttps.proxyHost
-Dhttps.proxyPort
-DsocksProxyHost
-DsocksProxyPort
```
3.重启Android Studio即可

