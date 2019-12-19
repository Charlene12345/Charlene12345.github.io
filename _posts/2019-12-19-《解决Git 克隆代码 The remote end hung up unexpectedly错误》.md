---
layout:     post   				    # 使用的布局（不需要改）
title:      前端资料 				# 标题 
subtitle:   《解决Git 克隆代码 The remote end hung up unexpectedly错误》 #副标题
date:       2019-12-19 				# 时间
author:     Charlene 						# 作者
header-img: img/5.jpg 	#这篇文章标题背景图片
catalog: false 						# 是否归档
tags:								#标签
    - 错误记录
---

![The remote end hung up unexpectedly错误](https://raw.githubusercontent.com/Charlene12345/img-repo/master/QQ%E6%88%AA%E5%9B%BE20191219221819.png)

解决方法：
配置git的最低速度和最低速度时间：
```
git config --global http.lowSpeedLimit 0
git config --global http.lowSpeedTime 999999  单位 秒
```




