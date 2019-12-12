---
title: 使用Hexo+GitHubPage+CodingMe搭建一个博客
date: 2019-03-25 22:44:15
categories:
- Hexo
tags:
- Blog
- Hexo
---

* 本文采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh) 协议，转载请注明出处。
* 本人是Java后端开发，想找个地方记录一些有用有意思的东西，自己又比较喜欢搞事情，就在网上找到了这种利用静态文件搭建一个博客的方法。按照网上的教程自己也遇到了一些问题，所以希望文章能够帮助到一些人，如有错误之处请多多指教。

---
# 准备工作

## 安装 [Node.js](https://nodejs.org/en/)
作者主要是在Win10下办公，所以此处就介绍Win10下的安装方法。
页面中间就有下载按钮。下载之后按照向导一步步来即可。
安装完成之后检查PATH环境变量是否配置Node.js：
在CMD中运行返回安装的版本号说明安装成功！
```bash
C:\Users\zm>node --version
v10.15.3
```
## 设置[淘宝](https://npm.taobao.org/)镜像
这里的[淘宝](https://npm.taobao.org/)不是那个淘宝：

> 淘宝 NPM 镜像：这是一个完整 npmjs.org 镜像，你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。

官网上有详细的安装方式，这里就照搬一下。
> 使用说明：
你可以使用我们定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:
```bash
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```
> 或者你直接通过添加 npm 参数 alias 一个新命令:
```bash
alias cnpm="npm --registry=https://registry.npm.taobao.org \
--cache=$HOME/.npm/.cache/cnpm \
--disturl=https://npm.taobao.org/dist \
--userconfig=$HOME/.cnpmrc
# Or alias it in .bashrc or .zshrc
$ echo '\n#alias for cnpm\nalias cnpm="npm --registry=https://registry.npm.taobao.org \
  --cache=$HOME/.npm/.cache/cnpm \
  --disturl=https://npm.taobao.org/dist \
  --userconfig=$HOME/.cnpmrc"' >> ~/.zshrc && source ~/.zshrc
```
安装的话就使用cnpm来代替npm就可以了，速度有明显的提升。

## 安装 [Git](https://git-scm.com/)
Git是用来做项目版本管理的，[下载链接](https://gitforwindows.org/)下载安装之后，以后的命令尽量在GitBash中输入。
<b>如果不把源代码上传，那么后面自己用不到git命令，`hexo deploy`命令会调用git上传到你配置的git仓库。</b>但是如果自己有两台以上电脑，或者需要在本地两个地方存放的话（在配置网址时可能需要国内一个网站，国外一个网站，后面会说），就需要将博客的源代码上传git，保证各工作空间代码一致。

首先我们新建一个仓库如下：
Repository name （仓库名称）: BlogResources
Description （描述，可填）: MyBlogTest
可以选择为Public对所有人可见或者选择Private私有
勾选Initialize this repository with a README，生成一个描述文件
.gitignore是上传时的忽略文件，这个可以后面修改添加，不急
Add a license可以使用MIT License
<img src="../../images/postimages/HexoGuide/1.png" style="zoom:50%">
创建完成之后先放着，后面再用。

###常用Git命令  
这里可能就能用到这么多，还有有关分支、合并的一些操作这里就不展开。
```bash
// 配置全局用户Name和E-mail
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
// 初始化仓库
git init
// 添加文件到Git仓库
git add <file>  //可以使用git add . 添加当前目录
// 提交添加的文件到Git仓库
git commit  //然后会弹出一个vim编辑器输入提交说明
// 或者直接
git commit -m "你要输入的提交说明"
// 查看仓库当前的状态
git status
// 查看历史提交记录
git log
// 或者加上参数查看
git log --pretty=oneline
// 关联Github远程库：
git remote add origin https://github.com/zhaomin6666/MyTest.git
// 推送到远程仓库
git push -u origin master  //注意：第一次提交需要加一个参数-u
// 查看远程库的信息
git remote
```

## 安装 [Hexo](https://hexo.io/zh-cn/)
搭建Hexo环境：
```bash
npm install -g hexo-cli
```
初始化Hexo：在本地某个目录下，新建一个文件夹folder
右击Git Bash Here打开命令行工具，输入：

```bash
hexo init <folder>
cd <folder>
npm install
```
<img src="../../images/postimages/HexoGuide/2.png" style="zoom:50%">
这样就初始化完成了，以后所有的Hexo命令都在这里执行。_config.yml是站点配置文件，theme/下的_config.yml是主题的配置文件。
在路径下，命令行（即Git Bash）输入以下命令启动服务器：
```bash
hexo server //或hexo s
```

浏览器访问网址： [http://localhost:4000/](http://localhost:4000/)

至此你的最基本的博客在本地已经搭建好了。

# 开始搭建博客

## 申请自己的域名（可选）、使用GithubPages+CodingPages搭建博客
1.在github上再新建一个仓库，仓库名为：`<Github账号名称>.github.io`
2.将本地Hexo博客推送到GithubPages
&emsp;2.1 安装hexo-deployer-git插件。在命令行（即Git Bash）运行以下命令即可：

``` 
npm install hexo-deployer-git --save
```
&emsp;2.2 添加SSH key
&emsp;创建一个SSH key。在命令行（即Git Bash）输入以下命令， 回车三下即可：

```
ssh-keygen -t rsa -C "邮箱地址"
```
&emsp;添加到Github。复制密钥文件内容（路径形如`C:\Users\Administrator\.ssh\id_rsa.pub`），粘贴到Github网站Settings --> SSH and GPG keys --> New SSH Key即可。

测试是否添加成功。在命令行（即Git Bash）依次输入以下命令，返回“You’ve successfully authenticated”即成功：
```bash
$ ssh -T git@github.com
$ yes
Hi zhaomin6666! You've successfully authenticated
```
&emsp;2.3 修改_config.yml（在站点目录下）。文件末尾修改为：
```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:<Github账号名称>/<Github账号名称>.github.io.git
  branch: master
```
&emsp;2.4 推送到GithubPages。在命令行（即Git Bash）依次输入以下命令， 返回INFO Deploy done: git即成功推送：
```bash
$ hexo g
$ hexo d
```
等待1分钟左右，浏览器访问网址： `https://<Github账号名称>.github.io`
如果失败了，尝试把_config.yml中的repo即repository改为`repository: https://github.com/zhaomin6666/zhaomin6666.github.io.git`，使用Https再次提交。这是会让你输账号密码，此处的密码在Settings --> Developer Settings --> Personal access tokens中生成。

3.将本地Hexo博客推送到CodingPages
和推送到Github差不多，都是使用托管平台的静态Pages。
&emsp;3.1 创建[Coding](https://dev.tencent.com/)账号（腾讯云开发平台）
&emsp;3.2 创建项目，项目名为：`<项目名称>`
&emsp;3.3 进入项目里 代码 --> Pages服务 中开启，稍等片刻CodingPages即可部署成功。
&emsp;3.4 将本地Hexo博客推送到CodingPages

推送成功之后，会自动生成一个访问地址：`<用户名>.coding.me/<项目名称>`，点击这个地址就可以访问自己的博客了！

此时可能会发现在coding搭建的博客的js路径都是不对的，这时需要修改站点目录下的_config.yml。在使用GithubGages的时候`# URL`这个标签是这样设置的：
```yaml
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://zhaomin6666.github.io/;
root: /
permalink: :category/:title/
permalink_defaults:
```
而在Coding中搭建项目的时候需要如下配置：
```yaml
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://zm666.coding.me/MYBLOG/
root: /MYBLOG
permalink: :category/:title/
permalink_defaults:
```
这样js才能正常显示。

4.申请域名
申请一个属于自己的域名应该不难，各大服务提供商的都有域名服务。申请完自己的域名之后需要做一下域名解析，也就是把你的域名指向你的博客网址。
比如我申请的域名为`www.zm6666.top`，在做域名解析时配置说明如下：
主机记录：也就是你访问的网址，进行跳转。可以把www和@都做解析；
记录类型：因为Codeing已经为我们提供了可以访问的域名所以这里选择CNAME；
解析线路：选择默认即可；
记录值：填写生成的可以访问的地址`https://zm666.coding.me/MYBLOG`；
TTL：默认5分钟即可。
做完了域名解析，在Coding静态页面的配置中也需要增加一下配置：
自定义域名：添加自己申请的域名。
此时，地址栏中敲入你申请的域名，就可以成功访问你的博客了。当然站点目录下的_config.yml又要做修改了，修改网址为新的网址：
```yaml
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://www.zm6666.top/
root: /
permalink: :category/:title/
permalink_defaults:
```



----------
更新日期 2019年12月12日