---
title: 利用hexo搭建自己的博客
date: 2017-08-26 23:34:17
tags: [hexo,博客] 
categories:
  - hexo
---
### 介绍
 hexo是一款基于Node.js的静态博客框架,方便、快捷。并且有丰富的主题可供选择，是大多数开发者的首选。
### 环境配置
- 安装node.js
- 安装git，注册github账户
 nodejs可以到[nodejs官网](https://nodejs.org/en/)下载相应的版本，安装也很简单这里不再多说，安装完成后，打开命令行，执行 
~~~~
node -v
~~~~
 出现正确的安装版本，即安装成功。由于nodejs自带的包管理工具npm在国内安装比较慢，建议大家将包地址换为国内淘宝镜像源。执行如下命令：
~~~~
npm config set registry https://registry.npm.taobao.org
~~~~
git是目前比较流行的版本控制系统，这里推荐廖雪峰老师的[git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)，大家可以安装学习，[github](http://note.youdao.com/)的安装注册更不用我再废话了，我们这里主要是利用github提供的gh-pages服务把自己的博客托管到github上去。
## 开始搭建
### 1. 利用npm，来全局安装hexo和hexo脚手架工具,hexo只需要全局安装一次，以后就可以在项目里使用了
 ~~~~
 npm install -g hexo
 npm install -g hexo-cli
 ~~~~
### 2. 开始创建
 ~~~~
 hexo init blogs  //初始化，会在本地生成一个blogs目录
 ~~~~
### 3. 本地浏览
 ~~~~
 cd blogs     //来到新建的目录
 npm install  //安装依赖
 hexo g       //(hexo generate的简写)编译输出静态文件
 hexo s       //(hexo server的简写)开启本地服务
 ~~~~
-  浏览器输入[http://localhost:4000](http://localhost:4000)，如果你本地的4000端口被占用，可能会报错，执行如下命令，来改端口。
 ~~~~
 hexo server -p 5000 //5000可以改为任意你想设定的端口
 ~~~~
### 4. 建立github连接
-  在github上新建一个Repository，命名必须为【your_user_name.github.io】，固定写法，大小写都不能错，比如你的用户名是webblogs，这里新建的Repository命名必须为 webblogs.github.io
- 在刚才新建的blogs目录下，找到配置文件：_config.yml，进行修改
~~~~
 deploy:
    type: git
    repo: https://github.com/liutaochange/liutaochange.github.io.git
    branch: master
~~~~
- 注意把这里的liutaochange换成你自己的用户名 

### 5. 开始部署
 ~~~~
 hexo d  //(hexo deploy 的简写)部署
 ~~~~
-  如果这里有报错 
~~~~
ERROR Deployer not found: git 或者 ERROR Deployer not found: github
~~~~
- 执行命令
~~~~
npm install hexo-deployer-git --save //安装hexo 的 git插件
~~~~
- 出现 【Deploy done: github】表示部署成功
- 打开 [https://liutaochange.github.io](https://liutaochange.github.io/) 就可以访问

---
### 关于主题
 Hexo提供了官网的主题, 初始化Hexo 时也会自动生成一个主题, Hexo还支持个性定制主题, 也可以根据自己的喜好在[themes](https://hexo.io/themes/)里选择。 
1.  启用主题
- 下载主题到你blogs目录下的themes文件夹下，打开配置文件找到
~~~~
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: yilia   //这里改为你需要的主题
~~~~
2. 验证主题
~~~~
hexo clean      //来清除 Hexo 的缓存
hexo s –debug   //debug模式，本地调试
~~~~
启动正常后，你的主题已经修改成功。


