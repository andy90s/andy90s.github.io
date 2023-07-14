# Xcode14.3问题记录

<!--more-->
## 前言
记录Xcode等相关的问题
## Xcode14.3 报错
### 更新后先重置下pod文件
在podfile文件中增加如下:

```ruby
post_install do |installer|
  installer.generated_projects.each do |project|
    project.targets.each do |target|
            target.build_configurations.each do |config|
                config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'] = '11.0'
            end
        end
    end
end
```

然后依次执行:

```
pod deintegrate
pod install
```
编译看是否报错? 其他报错看下面继续:
### Xcode14.3 编译报错(手动修改为11.0)
pod第三方库的支持版本改为11.0以上
<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image@master/blog/images/202304031423551.png" title="设置版本" width="85%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 设置版本 </b>  </div>
</center>

### Xcode14.3 打包报错`command phasescriptexecution failed with a nonzero exit code`
如图脚本文件增加 `-f`
<center>
{{<image src="https://cdn.jsdelivr.net/gh/andy90s/blog-image@master/blog/images/202304031451060.png" title="示例" width="95%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 示例 </b>  </div>
</center>

更新: 新版本cocoapods已修复此问题, 无需手动修改


### iOS13系统闪退问题
最新发现, 已退回14.2 , 坑!

崩溃日志:
```
Date/Time:           2023-04-06 10:25:29.5699 +0800
Launch Time:         2023-04-06 10:25:29.5246 +0800
OS Version:          iPhone OS 13.7 (17H35)
Release Type:        User
Baseband Version:    7.70.01
Report Version:      104

Exception Type:  EXC_CRASH (SIGABRT)
Exception Codes: 0x0000000000000000, 0x0000000000000000
Exception Note:  EXC_CORPSE_NOTIFY
Termination Description: DYLD, Assertion failed: (gotLocation), function applyFixupsToImage_block_invoke_3, file /Library/Caches/com.apple.xbs/Sources/dyld/dyld-750.4.2/dyld3/Loading.cpp, line 779.
Highlighted by Thread:  0

Backtrace not available

Unknown thread crashed with ARM Thread State (64-bit):
    x0: 0x0000000000000006   x1: 0x0000000000000009   x2: 0x000000016d117e70   x3: 0x0000000000000014
    x4: 0x000000016d117a70   x5: 0x0000000000000000   x6: 0x000000016d1187f0   x7: 0x000000016d118908
    x8: 0x0000000000000020   x9: 0x0000000000000009  x10: 0x2e342e3035372d64  x11: 0x2f33646c79642f32
   x12: 0x2f33646c79642f32  x13: 0x2e676e6964616f4c  x14: 0x6e696c202c707063  x15: 0x000a2e3937372065
   x16: 0x0000000000000209  x17: 0x0000000000000000  x18: 0x0000000000000000  x19: 0x0000000000000000
   x20: 0x000000016d117a70  x21: 0x0000000000000014  x22: 0x000000016d117e70  x23: 0x0000000000000009
   x24: 0x0000000000000006  x25: 0x0000000000000392  x26: 0x000000000000030a  x27: 0x000000010758ac70
   x28: 0x0000000000117cf0   fp: 0x000000016d117a40   lr: 0x000000010784fee8
    sp: 0x000000016d117a00   pc: 0x0000000107848f68 cpsr: 0x40000000
   esr: 0x00000000  Address size fault

Binary images description not available

Error Formulating Crash Report:
Failed to create CSSymbolicatorRef - corpse still valid ¯\_(ツ)_/¯

EOF
```

