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
缓存导致不生效,按如下解决:
```bash
git rm -r --cached .
git add .
git commit -m 'update gitignore'
```


