---
layout: post
title: offline-storage
date: 2016-10-19 14:42:49
tags:
- storage
categories:
- Notes
---

## 什么是离线存储
离线存储，就是将指定的网页文件（例如css、js）保存到本地，当用户没有网络时，依旧可以通过浏览器使用这些文件。
## 为何要使用离线存储
从网站所有者的角度来说，增加离线存储功能，能够让用户更好的使用网站。
## 离线存储的设置步骤
* 配置manifest文件
* 通过Js进行缓存的控制

**manifest文件的配置**
### 添加manifest属性
将需要离线存储的文件罗列下来，存储于后缀名为manifest的文件当中，在HTML文件中引入manifest文件
```html
<html manifest="test.manifest">
```
### manifest文件的基本组成
- 基本语法：
```
CACHE MANIFEST
# 该符号后面的内容为注释信息，第一行的CACHE MANIFEST不能少
CACHE:
# 需要缓存的文件
NETWORK:
# 不需要缓存的文件
FALLBACK:
# 当页面无法访问时的回退页面/重定向
```
- 代码示例
```
CACHE MANIFEST
CACHE:
../test.html
../css/reset.css
../css/test.css
# 需要注意的是：manifest文件中书写的路径，是以该文件的路径为基础，而非以html所在文件夹为基础
NETWORK:
../images/logo.jpg
FALLBACK
```
## 离线存储工作流程
![离线存储工作流程](/images/storage-01.png)
## 使用Javascript控制缓存
缓存的各类属性和事件，均绑定在"applicationCache"上
### 涉及缓存的属性
applicationCache.status
用于便是当前的缓存状态，取值范围为0-5.
- 0代表未缓存，通常是这些页面没有运用离线缓存技术，就是这个状态。
- 1代表空闲，当缓存是最新的时候为1，不需要做什么操作。
- 2代表检查中，即浏览器在检查manifest文件是否为最新。
- 3代表下载中，当前有更新，并且正在下载。
- 4代表更新就绪，代表当前有更新，并且已经下载完毕。等待下次载入页面时候，进行更新。
- 5代表缓存过期，即找不到正确的缓存文件时候，会变成5.
### 涉及缓存的事件
- update 用于主动更新缓存。
- updateready 当有新的缓存，并更新完毕之后，会触发此事件。
- progress 进度事件，当进行缓存的下载时，会不断的触发该事件。
progress中的event对象包含：loaded。loaded代表当前已经加载完毕的文件，total总共需要更新的文件数。
- checking 正在检查
- downloading 正在下载。
- obsolete 缓存过期。
- cached 空闲，缓存为最新状态。
- error 报错。
- noupdate 检查更新结束，不需要更新。
部分事件使用案例代码：
```
// update事件使用案例
applicationCache.update();

// updateready事件使用案例
application.addEventListener('updateready', function(){
    console.log('已完成缓存的更新');
}, false);

// progress事件使用案例
application.addEventListener('progress', function(){
    console.log(applicationCache.status);
}, false);
```
