# Git常见问题汇总

<!--more-->
## 生成ssh-key
### 1. 利用ssh生成秘钥(这里以github平台为例)
```bash
ssh-keygen -t rsa -C "youremail@xxx.com" -f ~/.ssh/github_id_rsa
```
{{< admonition >}}
`github_id_rsa`是文件名,可自己修改,大部分默认名为`id_rsa`


如果想要多个平台分开不同秘钥,再次执行上面指令,文件名替换其他,例:`gitlab_id_rsa`
{{< /admonition >}}

### 2. 添加私钥
```bash
ssh-add ~/.ssh/github_id_rsa
```
### 3. 查看公钥内容并复制到对应的git平台
```bash
cat ~/.ssh/os_id_rsa.pub
```
### 4. 测试链接
```bash
ssh -T git@github.com
```
## gitignore忽略文件不生效
1.准备工作: 删除pods文件,然后提交代码
2.缓存导致不生效,按如下解决:
```bash
git rm -r --cached .
git add .
git commit -m 'update gitignore'
```
## github提示账号密码不对
仓库是https拉取,终端提示输入账号密码,怎么输入都不对?
这是因为github不再支持原密码登录,替换为token登录.
{{< admonition tip "解决办法" >}}
打开github设置(settings) -> `Developer settings` -> `Personal access tokens` -> `Tokens`         
选择新建一个如下图所示,得到一个token字符串,登录的时候密码使用此token替换原来的密码进行登录  
{{< /admonition >}}
<center>
{{<image src="https://raw.githubusercontent.com/andy90s/blog-image/master/blog/images/281670218139_.pic.jpg" title="设置github token" width="100%">}}
<div style="color:#717171;font-size:14px;font-weight:normal"> <b> 设置github token </b>  </div>
</center>

