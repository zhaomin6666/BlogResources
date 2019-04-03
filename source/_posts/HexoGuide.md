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
<b>如果不把源代码上传，那么后面自己用不到git命令，`hexo deploy`命令会调用git上传到你配置的git仓库。</b>但是如果自己有两台以上电脑，或者需要在本地两个地方存放的话（在配置网址时可能需要国内一个网站，国外一个网站），就需要将博客的源代码上传git，保证各工作空间代码一致。

首先我们新建一个仓库如下：
Repository name （仓库名称）: BlogResources
Description （描述，可填）: MyBlogTest
可以选择为Public对所有人可见或者选择Private私有
勾选Initialize this repository with a README，生成一个描述文件
.gitignore是上传时的忽略文件，这个可以后面修改添加，不急
Add a license可以使用MIT License
<img src="https://zm666.coding.me/MYBLOG/images/postimages/HexoGuide/1.png" style="zoom:50%">

```Java
// TODO 常用Git命令
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
这样就初始化完成了，以后所有的Hexo命令都在这里执行。_config.yml是站点配置文件，theme/下的_config.yml是主题的配置文件。
在路径下，命令行（即Git Bash）输入以下命令启动服务器：
```bash
hexo server
```

浏览器访问网址： [http://localhost:4000/](http://localhost:4000/)

至此你的博客在本地已经搭建好了，总算是完成了第一步。
---- 2019年3月26日更新

## 申请自己的域名（可选）

以下是测试图片
<img src="https://zm666.coding.me/MYBLOG/images/my-avatar.jpg" style="zoom:50%">
