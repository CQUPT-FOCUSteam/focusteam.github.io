---
title: 博客网站开发机迁移及文章发布教程 - 以 LearningYard 为例(原网站已关闭)
date: 2020-03-07 13:02:10
categories: TechArea技术区
tags: [hexo]
thumbnail: https://focusteam-blog-pic.oss-cn-shenzhen.aliyuncs.com/ivrgs.png!thumbnail
---

本篇教程将详细介绍 LearningYard 博客网站开发机的迁移流程(从 0 开始)，以及编写和发布文章的相关须知。

<!-- more -->



## 安装 git

### Windows

主页地址：[https://git-scm.com/](https://git-scm.com/)

双击根据指引猛点下一步安装

### Linux

使用系统自带的包管理工具进行安装(如果速度太慢可以加代理或更换国内镜像源)

执行命令(**Ubuntu**)：

```bash
sudo apt update && sudo apt install -y git
```

执行命令(**CentOS**)：

```bash
sudo yum -y update && sudo yum install -y git
```

### Mac

使用 home-brew 包管理工具安装

执行命令：

```bash
brew update && brew install git
```



## 安装 Node.js 和 npm

### Windows

下载地址：[https://nodejs.org/dist/v12.15.0/node-v12.15.0-x86.msi](https://nodejs.org/dist/v12.15.0/node-v12.15.0-x86.msi)

双击根据指引猛点下一步安装

### Linux(二选一，推荐方法2)

1. 通过源码编译安装

下载地址：[https://nodejs.org/dist/v12.15.0/node-v12.15.0.tar.gz](https://nodejs.org/dist/v12.15.0/node-v12.15.0.tar.gz)

执行命令：

```bash
sudo mkdir -p /usr/local/nodejs && cd ~/Downloads/ && wget https://nodejs.org/dist/v12.15.0/node-v12.15.0.tar.gz && tar -zxvf node-v12.15.0.tar.gz && cd node-v12.15.0 && ./configure --prefix=/usr/local/nodejs/ && sudo make && sudo make install && cd .. && rm -rf node-v12.15.0.tar.gz && rm -rf node-v12.15.0
```

设置环境变量：

```bash
echo 'export NODE_HOME=/usr/local/nodejs/bin' >> ~/.bashrc
echo 'export PATH=$NODE_HOME:$PATH' >> ~/.bashrc
source ~/.bashrc
```

2. 直接安装编译好的二进制包

下载地址：[https://nodejs.org/dist/v12.15.0/node-v12.15.0-linux-x64.tar.xz](https://nodejs.org/dist/v12.15.0/node-v12.15.0-linux-x64.tar.xz)

执行命令：

```bash
cd ~/Downloads/ && wget https://nodejs.org/dist/v12.15.0/node-v12.15.0-linux-x64.tar.xz && sudo tar xvJf node-v12.15.0-linux-x64.tar.xz -C /usr/local/
```

设置环境变量：

```bash
echo 'export NODE_HOME=/usr/local/node-v12.15.0-linux-x64/bin' >> ~/.bashrc
echo 'export PATH=$NODE_HOME:$PATH' >> ~/.bashrc
source ~/.bashrc
```

删除压缩包：

```bash
rm -rf ~/Downloads/node-v12.15.0-linux-x64.tar.xz
```



### Mac

下载地址：[https://nodejs.org/dist/v12.15.0/node-v12.15.0.pkg](https://nodejs.org/dist/v12.15.0/node-v12.15.0.pkg)

双击根据指引安装



## 安装 hexo

执行命令：

```bash
npm install -g hexo-cli
```

Linux 和 Mac 用户如遇到权限问题请额外执行命令：

```bash
sudo chown -R $(whoami) $(npm config get prefix)/{lib/node_modules,bin,share}
```



## 设置 git 全局信息

执行命令：

```bash
git config --global user.name "填你的 github 名字(带上两端的双引号)"
git config --global user.email "填你的 github 关联邮箱(带上了两端的双引号)"
```



## 设置 git 的 ssh 认证公钥

执行命令：

```bash
ssh-keygen -t rsa -C "填你的 github 关联邮箱(带上了两端的双引号)"
```

什么都不需要输入，一路回车到命令执行结束，接着：

Linux 和 Mac 用户请执行命令：

```bash
cat ~/.ssh/id_rsa.pub
```

Windows 用户请到如图所示的红框内的地址中寻找 id_rsa.pub 文件并用记事本打开：

![windows 下生成 ssh key](https://focusteam-blog-pic.oss-cn-shenzhen.aliyuncs.com/e876i.png!origin)

将打印的值完整的复制，以 `ssh-rsa` 开头，以 `你输入的邮箱` 结尾，全部完整的复制，打开你的 `github 主页` ，点击右上角头像，选择 `Settings` 选项，如图：

![选择 "Settings" 选项](https://focusteam-blog-pic.oss-cn-shenzhen.aliyuncs.com/07xi1.png!origin)

进入到用户设置界面后，点击左侧红框内的 `SSH and GPG keys` 按钮，进入到右侧 `SSH keys` 编辑界面，如下图所示：

![选择 "SSH and GPG keys" 选项](https://focusteam-blog-pic.oss-cn-shenzhen.aliyuncs.com/r30k6.png!origin)

点击上图中右上角蓝色框内的 `New SSH key` 按钮，进入到新增 ssh key 的界面，如下图所示：

![新增 ssh key](https://focusteam-blog-pic.oss-cn-shenzhen.aliyuncs.com/bhwvj.png!origin)
在 `Title` 单行文本框内**随意输入一个字符串**，最好用于标示你的计算机，接着，在下方 `Key` 多行文本框内填写刚才复制的 `id_rsa.pub` 文件的内容，最后点击下方的 `Add SSH key` 按钮完成公钥的添加。

执行命令以验证配置结果：

```bash
ssh -T git@github.com
```

如果出现下图所示的输出，则表示配置成功：

![验证 ssh key 配置](https://focusteam-blog-pic.oss-cn-shenzhen.aliyuncs.com/z3c9q.png!origin)



## 向 LearningYard/learningyard.github.io 仓库管理员申请开放合作权限

需要向 LearningYard 仓库管理员提供自己的 **GitHub账号名称** ，仓库管理员操作如下所示：

1. 登陆管理员的 GitHub 账号；
2. 进入该仓库主页，链接：[https://github.com/LearningYard/learningyard.github.io](https://github.com/LearningYard/learningyard.github.io)；
3. 在页面中部找到如下图所示的部分，点击红色框内的 `Settings` 按钮：

![仓库设置按钮](https://focusteam-blog-pic.oss-cn-shenzhen.aliyuncs.com/mul0i.png!origin)

4. 进入仓库设置部分后，点击左边列表的 `Manage access` 选项，进入管理权限设置界面，如下图所示：

![管理权限设置界面](https://focusteam-blog-pic.oss-cn-shenzhen.aliyuncs.com/2m4ki.png!origin)

5. 点击上图右侧红框内的 `Invite a collaborator` 按钮，输入本节开头别人提供的 GitHub 账号进行搜索，确认后添加，如下图所示：

![添加合作者](https://focusteam-blog-pic.oss-cn-shenzhen.aliyuncs.com/tilfw.png!origin)

6. 被邀请者需要去自己 GitHub 绑定的邮箱内查收一封**带有邀请链接**的邮件，并点击链接进行确认，完成合作者的添加，至此便有权限通过自己的 GitHub 账号为 LearningYard 项目提交代码。



## 在一台新计算机上复建 LearningYard 博客网站开发环境和配置

1. 将 LearningYard/learningyard.github.io 仓库克隆到本地：

执行命令：

```bash
git clone git@github.com:LearningYard/learningyard.github.io.git -b dev && cd learningyard.github.io
```

2. 使用 hexo 生成静态文件：

执行命令：

```bash
hexo g
```

3. 使用 hexo 自带服务器模块预览：

执行命令：

```bash
hexo s
```

hexo 将会在本地启动，并监听默认的 `4000` 端口，如下图所示：

![启动 hexo 服务器](https://focusteam-blog-pic.oss-cn-shenzhen.aliyuncs.com/jqrq5.png!origin)

4. 打开浏览器，访问地址 `http://localhost:4000` 将会看到和 `https://www.learningyard.xyz` 网站一样的内容；
5. 若一切看起来正常，则回到命令行界面，按下 `Ctrl + C` 组合键可以停止 hexo 服务器，这也意味着开发环境的复建顺利完成。



## 添加新文章并上传

1. 进入到本地我们克隆的开发目录 `learningyard.github.io` 下；
2. 执行命令：

```bash
hexo new post "你要新建的文章的文件名，即 .md 文件的文件名，最好是英文，例如 tutorial，不需要 .md 后缀"
```

该命令会在 `learningyard.github.io/source/_post/` 路径下生成同名的 markdown 文件，例如本文是这样生成的：

```bash
hexo new post "tutorial"
```

进入到 `learningyard.github.io/source/_post/` 路径下便会发现一个名为 `tutorial.md` 的 markdown 文件；

3. 用 markdown 编辑器打开刚才新建的 .md 文件，这里推荐使用 **[Typora](https://typora.io/)** 。会看到新建的文件里包含一定的信息，如下图所示：

![新建文件内容示例](https://focusteam-blog-pic.oss-cn-shenzhen.aliyuncs.com/xtacl.png!origin)

如上图所示，文件顶部是一个 `YAML 头`，这个头包含了文件的一些基本信息供 hexo 生成静态文件时使用，这里包含了三个字段，分别是 `title` 、`date` 、`tags` 。

首先我们需要将 title 字段的内容改为我们文章真正的标题；

其次，date 字段表示的是文章生成的时间，这个改不改都无所谓，但 hexo 会按照此时间将所有文章排序生成归档，如果没有特殊需求的话一般不做改动；

接着，为了方便读者对文章进行查找，我们需要给文章**归类**以及**添加标签**，这就需要添加和修改 `categories` 和 `tags` 两个字段了，既然是字段名是复数，那单个字段肯定可以有多个值，这里需要注意：

​		1⃣️ **如果单个字段要填入多个值，那么这些值需要用一对方括号 `[]` 包裹，不同值之间通过英文逗号 `,` 间隔**，如 categories: [tutorials,test]；

​		2⃣️ **如果为 categories 字段添加多个值，那么这些值将按顺序逐级下沉，后一个值会成为前一个值的子分类**，如下图所示：

![子分类示例](https://focusteam-blog-pic.oss-cn-shenzhen.aliyuncs.com/8zqhi.png!origin)

​		3⃣️ **如果为 tags 字段添加多个值，那么这些值是同级的**，如下图所示：

![多标签示例](https://focusteam-blog-pic.oss-cn-shenzhen.aliyuncs.com/bt66v.png!origin)

最后，为了美观，我们可能会想给文章加个封面图，这里就需要用到 `thumbnail` 这个字段了，在该字段中填入需要作为封面的图的地址便可以了。

本文的 YAML 头如下图所示，以供参考：

![本文的 YAML 头](https://focusteam-blog-pic.oss-cn-shenzhen.aliyuncs.com/d73uq.png!origin)

4. 头写完了，下面就该写文章主体部分了，这里就不详细介绍了，请参考 markdown 语法。
5. 在写完了文章后，我们希望能在博客中预览整体效果，同样的，请回到本地 learningyard.github.io 这个目录下，执行命令：

```bash
hexo g
```

以将 markdown 文件转变成静态的 html 文件，接着，执行命令：

```bash
hexo s
```

启动 hexo 自带的服务器模块，在浏览器中预览。

6. 在本地预览感觉满意后，我们希望能将文章发布到真正的博客服务器上，供更多的人阅读，这里就需要执行命令：

```bash
hexo d
```

hexo 将利用 git 工具将生成的静态 html 文件和相关资源同步到远端的 GitHub 仓库中，也就是 LearningYard/learningyard.github.io 仓库中，GitHub Page 将根据仓库内容的改变自动更新，稍等几分钟便可以通过访问域名 `https://www.learningyard.xyz` 的方式正式访问到更新的文章资源了。

7. 同步完了静态资源，别忘了把刚刚辛苦编写的 markdown 源文件也同步到仓库中，便于下次复建开发环境，这里我已经在仓库中新建了一个 `dev` 分支，用于存放开发资源，而生成的静态资源会默认提交到仓库的 `master` 分支。静态资源的同步交给 `hexo d` 命令来做，源文件的同步则需要执行下面的命令：

```bash
git add .
git commit -m "这里简单描述一下做了哪些更新，比如：新增了 xxx.md 文件"
git push
```

这样，便完成了源文件和开发环境的同步。



**以上。**

