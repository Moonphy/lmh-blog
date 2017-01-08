---
title: vue项目
subtitle: "vue"
date: 2016-12-18 23:19:19
tags:
    - MVVM
    - js
    - 项目架构
    - vue
cdn: 'header-off'
header-img: 'http://www.codeblocq.com/assets/projects/hexo-theme-clean-blog/img/home-bg.jpg'
---

目前最火的前端MVVM框架应该非Vue莫属了, 

<!-- more -->
很早以前就知道vue, 但是都是辅助性用用, 基本是直接在页面中引入vue的js动态渲染DOM而已, 真正用vue-cli做个完整的项目是没有的.
9月份公司刚好出来个移动端微信项目, 高分专家, 很适合做spa项目, 部门技术选型直接选了vue, 跟着志文哥哥玩了下. 一直想总结下, 今天说说简单的东西

# 环境配置
* node.js与npm
node出到6.2.2了, 更新的太快了, 每过段时间就会发现自己的版本不是新的了, nvm管理node刚好, 可以方便的切换要使用的node版本
Npm使用淘宝镜像会快点.
~~~bash
$ npm config set registry https://registry.npm.taobao.org
~~~

* IDE
一直习惯了用webstorm, 对前端来说这是个神器, 下载了个vue插件就完美兼容vue了, 但是对es6老是报错, 配置了babel文件监听, 还是不大好用, 相对来说sublime-text就要更好用点, 装个Vue syntax highlight, 兼容vue文件高亮, 再装个colorpicker, sublimecodeintel, alignment, emmet就差不多了
所以, 做Vue项目还是用sublime会更顺心一点.

* webpack & Vue & vue-cli & vuex & vue-router
~~~bash
$ npm install webpack -g
~~~
# 搭建项目
~~~bash
$ vue init webpack gfzj-vue
~~~
* build：webpack打包配置文件夹
* config： 同样是打包配置的文件夹，只是职能不同
* src：源码存放文件夹
* static： 静态文件存放位置
* test：测试代码存放位置，展开可以看到单元测试和e2e测试两个目录
* .babelrc ： babel的配置文件
* index.html: 单页应用的html文件
* package.json: npm的配置文件，配置项目信息，需要安装的模块之类
* README.md： 项目说明文档
~~~bash
$ npm install
$ npm run dev
~~~