# Hugo 引入Giscus评论系统

<!--more-->

## 选择评论系统

`DoIt`主题支持的评论功能:

- disqus
- utterances
- giscus
- gitalk
- valine
- waline
- telegram
- ...

{{< admonition tip "">}}

utterances、gitalk 是基于 GitHub Issue 的评论系统, disqus 是基于 GitHub Discussions, 如果你的网站是基于 GitHub Pages 部署的, 那么这三个评论系统都是不错的选择.

由于我使用的是`DoIt`主题,是支持了多个评论系统的,因为`Giscus`在更新,并且有回复功能,管理评论功能,所以我选择了`Giscus`作为评论系统.

utterances、gitalk 是可以一键迁移到 Giscus 的,所以如果你之前使用的是这两个评论系统,可以考虑迁移到 Giscus.

{{< /admonition >}}

## Giscus简介

{{<link href="https://giscus.app/zh-CN" content="【Giscus中文官网】">}}

- 开源
- 无跟踪,无广告,永久免费
- 无需数据库。所有数据均储存在 GitHub Discussions 中
- 支持自定义主题
- 支持多种语言
- 高可配置性
- 自动从GitHub 拉取新评论与编辑
- 可自建服务

## 怎么用

### 选择仓库开启`GitHub Discussions`功能

以我仓库为例,我博客是部署在github pages上的,所以我需要在我的仓库中开启`GitHub Discussions`功能.

{{< admonition tip "提示">}}

也可以新建一个其他仓库,用来存放评论数据,这样可以更好的管理评论数据.
{{< /admonition >}}

比如我的仓库是`andy90s/andy90s.github.io`,我需要在这个仓库的设置中开启`GitHub Discussions`功能.

{{<image src="discus_repo_setting.webp" caption="GitHub Discussions" src_s="" src_l="" width="100%">}}

找到`Discussions`选项,点击勾选按钮,开启`GitHub Discussions`功能.

{{<image src="discus_repo_setting2.webp" caption="GitHub Discussions" src_s="" src_l="" width="100%">}}

### 安装Giscus

点击 {{<link href="https://github.com/apps/giscus" content="【安装】">}}

安装提示选择的仓库,有两个选项

- All repositories
- Only select repositories

选择`Only select repositories`,然后选择你的仓库,点击`Install`按钮.

{{<image src="giscus_install.webp" caption="Giscus安装" src_s="" src_l="" width="100%">}}


### 配置Giscus

打开{{<link href="https://giscus.app/zh-CN" content="【Giscus配置】">}}

设置仓库:

{{<image src="giscus_setting.webp" caption="设置仓库" src_s="" src_l="" width="50%">}}

选择分类:

{{<image src="giscus_setting2.webp" caption="选择分类" src_s="" src_l="" width="70%">}}

其他保持默认即可,往下拉查看生成的配置:

{{<image src="giscus_setting3.webp" caption="生成的配置" src_s="" src_l="" width="80%">}}

### Hugo配置

在`config.toml`中添加配置:

```toml
[page]
  [page.comment]
    enable = true
    [page.comment.giscus]
      enable = true
      # owner/repo
      dataRepo = "你的仓库示例:andy90s/andy90s.github.io"
      dataRepoId = "上面配置生成的"
      dataCategory = "Announcements"
      dataCategoryId = "上面配置生成的"
      dataMapping = "pathname"
      dataStrict = "0"
      dataReactionsEnabled = "1"
      dataEmitMetadata = "0"
      dataInputPosition = "bottom"
      lightTheme = "light"
      darkTheme = "dark"
      dataLang = "zh-CN"
```

{{< admonition tip "多语言">}}
如果你的网站是多语言的, 可以根据语言配置不同的评论系统. 参考`DoIt`主题的demo网站配置. config.toml多语言配置.
{{< /admonition >}}

### 本地测试

在本地启动hugo服务,查看是否正常显示评论系统.

```bash
hugo server -e production
```

{{< admonition tip "注意">}}
默认开发环境是没有评论的, 所以要强制开启生产环境.
{{< /admonition >}}
