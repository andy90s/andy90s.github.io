# Hugo常用指令


<!--more-->

## 常用指令

```bash
hugo new posts 文章.md # 发布文章
hugo # 编译(production)
hugo server --disableFastRender # 禁用快速渲染
hugo server -D # 启动本地服务 包含草稿
hugo server # 启动本地服务 不包含草稿
hugo server -environment production # 以生产环境启动
hugo server -e production # 以生产环境启动
```

## 更多指令

```bash
hugo --help # 查看帮助
```
{{< admonition tip "提示">}}
以下内容由AI翻译,可能存在不准确的地方,请以英文为准
{{< /admonition >}}
commands:
```bash
hugo completion # 生成自动补全脚本
hugo config # 打印站点配置
hugo convert # 将内容转换为不同的格式
hugo deploy # 将您的站点部署到云提供商。
hugo env # 打印Hugo版本和环境信息
hugo gen # 一组几个有用的生成器。
hugo help # 关于任何命令的帮助
hugo import # 从其他地方导入您的站点。
hugo list # 列出各种类型的内容
hugo mod # 各种Hugo模块助手。
hugo new # 为您的站点创建新内容
hugo server # 启动本地服务器
hugo version # 打印Hugo版本和环境信息
```

flags:
```bash
-b, --baseURL string            主机名(和路径)到根,例如 https://spf13.com/
-D, --buildDrafts                包括标记为草稿的内容
-E, --buildExpired               包括过期的内容
-F, --buildFuture                包括发布日期在未来的内容
    --cacheDir string            缓存目录的文件系统路径
    --cleanDestinationDir        删除目标中未在静态目录中找到的文件
    --clock string               设置Hugo使用的时钟，例如 --clock 2021-11-06T22:30:00.00+09:00
    --config string              配置文件(默认为hugo.yaml|json|toml)
    --configDir string           配置目录(默认为 "config")
-c, --contentDir string          读取文件的文件系统路径
    --debug                      调试输出
-d, --destination string         写入文件的文件系统路径
    --disableKinds strings       禁用不同种类的页面(主页、RSS等)
    --enableGitInfo              将Git修订、日期、作者和CODEOWNERS信息添加到页面
-e, --environment string         构建环境
    --forceSyncStatic            当静态文件更改时复制所有文件。
    --gc                         在构建后运行一些清理任务(删除未使用的缓存文件)
-h, --help                       帮助hugo
    --ignoreCache                忽略缓存目录
    --ignoreVendorPaths string   忽略与给定Glob模式匹配的模块路径的任何_vendor
-l, --layoutDir string           布局目录的文件系统路径
    --logLevel string            日志级别(debug|info|warn|error)
    --minify                     缩小任何支持的输出格式(HTML、XML等)
    --noBuildLock                不创建.hugo_build.lock文件
    --noChmod                    不同步文件的权限模式
    --noTimes                    不同步文件的修改时间
    --panicOnWarning             在第一个WARNING日志上发生恐慌
    --poll string                设置此为轮询间隔，例如 --poll 700ms，以使用基于轮询的方法监视文件系统更改
    --printI18nWarnings          打印缺少的翻译
    --printMemoryUsage           在屏幕上定期打印内存使用情况
    --printPathWarnings          打印有关重复目标路径等的警告
    --printUnusedTemplates       打印有关未使用模板的警告。
    --quiet                      静默模式下构建
    --renderSegments strings     要呈现的命名段(在段配置中配置)
-M, --renderToMemory             渲染到内存(在运行服务器时最有用)
-s, --source string              读取文件的文件系统路径
    --templateMetrics            显示有关模板执行的指标
    --templateMetricsHints       当与 --templateMetrics 结合使用时，计算一些改进提示
-t, --theme strings              要使用的主题(位于/themes/THEMENAME/)
    --themesDir string           主题目录的文件系统路径
    --trace file                 将跟踪写入文件(通常不实用)
-v, --verbose                    详细输出
-w, --watch                      监视文件系统的更改并根据需要重新创建
```


