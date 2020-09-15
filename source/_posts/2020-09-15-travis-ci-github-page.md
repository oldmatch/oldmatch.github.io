---
thumbnail: https://oldmatch-1302978637.cos.ap-guangzhou.myqcloud.com/banner1.jpg
title: 使用travis-ci自动化构建部署GitHub Pages
date: 2020-09-15 16:50
tags:
  - blog
  - hexo
categories:
  - 分享
---

# 使用travis-ci自动化构建部署GitHub Pages

##Travis CI##
目前，自动化构建、持续集成的理念在整个计算行业非常的流行，大家更愿意去使用自动化代替手动，从而提高效率。详细介绍自己去百度谷歌啦，不要像我这么懒。

打开https://travis-ci.com/ 使用github账号登录

此时为了让travis获取调用GitHub Api的权限需要在GitHub上生成一个token。
在github(https://github.com/settings/tokens) Settings/ Developer settings 新生成一个travis专用的token.

##在travis 配置token环境变量##
在你需要构建的仓库的设置里添加GH_TOKEN环境变量。

![](https://oldmatch-1302978637.cos.ap-guangzhou.myqcloud.com/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20200915161753.png)


##创建 配置文件##
在你的github pages 项目里面新建.travis.yml配置文件。

分享下我的

	language: node_js
	node_js:
  	- lts/*

	cache:
	  directories:
		- node_modules

	install:
	  - npm install hexo-cli -g
	  - npm install

	script:
	  - hexo g

	after_script:
	  - cd ./public
	  - git init
	  - git config user.name "oldmatch"
	  - git config user.email "oldmatch24@gmail.com"
	  - git add .
	  - git commit -m "Update blog content by Travis CI"
	  - git push --force --quiet "https://${GH_TOKEN}@github.com/oldmatch/oldmatch.github.io.git" master:master

	branches:
	  only:
		- hexo

##测试##
在github page的项目里面修改一点东西，push到GitHub上去就会在travis上看到触发build。

大概等个2min你会发现你的github pages 已经更新了。
