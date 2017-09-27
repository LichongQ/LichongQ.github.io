---
title: github+hexo搭建个人博客
date: 2017-09-26 21:49:10
---

预置条件
1. 安装git
2. 安装node.js

# 注册github账号并创建仓库


创建仓库时注意：仓库的名字必须是 github-username.github.io，否则使用https://github-username.github.io 不能打开博客，其中github-name为注册github账号时的github用户名。

如：注册github的账号为tianfuzhen,则仓库的名字必须为tianfuzhen.github.io，这样使用https://tianfuzhen.github.io 来访问个人博客。

# hexo部署个人博客到github

<!--more-->


1.下载hexo
hexo的官网地址：https://hexo.io/
下载时，首先在本地创建一个目录，然后进入该目录执行以下命令安装hexo

``` 
	$ npm install hexo-cli -g
```

2.初始化hexo

```
	$ hexo init
```

3.开启并查看本地预览

```
	$ hexo server 
```

浏览器访问：http://localhost:4000 即可看到内容。如下图
![image](https://user-images.githubusercontent.com/31929148/30928995-c335f27e-a3ef-11e7-9dcb-1e7a4111cdc3.png)

4.配置github

修改hexo目录下的_config.yml，建立与github的关联，如下

```
deploy:
   type: git
   repo: https://github.com/tianfuzhen/tianfuzhen.github.io.git
   branch: master
```
然后执行命令

```
	$ npm install hexo-deployer-git --save
```

5.生成网页页面

```
	$ hexo generate 
```

6.部署页面到github上
 
```
	$ hexo deploy
```

部署页面前，需要设置好部署的github地址和分支等信息。部署完成后，浏览器输入：https://tianfuzhen.github.io 即可访问个人博客

# 博客主题的个性化设置

一般第一次初始化hexo时，页面为默认的hexo主题页面，比较丑，不适合作为个人博客的风格。

hexo主题的地址：https://hexo.io/themes/
网站上有很多主题，可以根据个人喜好选择。推荐NexT主题，简单而不失逼格。

NexT主题的github地址：https://github.com/iissnan/hexo-theme-next

NexT的主题切换参考：http://theme-next.iissnan.com/getting-started.html

# 博客源码的备份

由于hexo部署时，上传github的只是包含静态页面的文件，且部署时会替换github仓库原有的文件，所以一旦本地的源码丢失后，博客便无法再继续维护。

## 源码备份思路

参考网上内容，选择按如下方式备份源码

1.在github的仓库上添加新的branch=hexo，然后将源码上传到branch=hexo下面。
2.hexo部署时，将静态页面的文件上传到仓库默认的branch=master分支。

每次更新时，编写源码文件，hexo部署页面到github的master分支，然后上传源码到github的hexo分支。如果本地源码丢失，则可以下载仓库的hexo分支内容，重新用hexo部署。

上传源文件时，注意修改hexo目录下的.gitignore文件，确保除了log文件外其他文件全部上传。

当themes中包含除了默认主题外的其他主题，上传源码时，会出现 themes/next目录下的文件无法提交的问题。

解决方法：删除themes/next目录下的.git目录，然后在hexo目录下执行命令

```
	$ git rm -rf --cached themes/next
	$ git add .
	$ git commit -m "备份源码"
```

然后git push origin hexo提交代码到github上即可




