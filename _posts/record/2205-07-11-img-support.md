---
title: 利用Github+PicGo+jsDelivr+Imagine创建免费的图床
date: 2025-07-11 23:20:00 +0800
categories: [网络服务]
tags: [图床, PicGo]
description: 图床是一个在网络上存储图片的地方，通过URL就可以对图片进行查看，目的是为了节省本地服务器空间，加快图片的打开速度，以及解决跨平台的图片存储问题。本文利用Github+Pingo创建一个免费的图床，将图片转为一个URL并且可以嵌入到Markdown文档中，更方便地在Markdown中插入图片。
---

> 图床是一个在网络上存储图片的地方，通过URL就可以对图片进行查看，目的是为了节省本地服务器空间，加快图片的打开速度，以及解决跨平台的图片存储问题。本文利用Github+Pingo创建一个免费的图床，将图片转为一个URL并且可以嵌入到Markdown文档中，更方便地在Markdown中插入图片。

## 一、平台选择

- Github：利用Github的仓库存储功能，实现图片的存放
- PicGo：一个用于快速上传图片并获取图片URL链接的工具
- jsDelivr：一款开源的免费公共CDN，利用它加速对Github仓库中图片资源的访问
- Imagine：一个用于压缩PNG以及JPEG的桌面端程序，可以压缩图片的体积，加快访问速度

## 二、Github图库的创建

1. 在GitHub中新建一个仓库
2. 在新建仓库页面中输入仓库名，将仓库权限选择为公开，并且初始化一个README.md文件
3. 完成仓库的新建
4. 进入到个人GitHub的设置界面
5. 在左侧栏中往下拉，选择`Developer settings`选项
6. 在左侧栏中依次选择`Personal access tokens` > `Tokens(classic)`
7. 点击`Generate new token` > `Generate new token(classic)`
8. 在`Note`栏中输入这个Token的别名（例：cdn）
9. 勾选`Select scopes`中的`repo`
10. 拉到页面底部点击`Generate token`完成token的创建
11. 获得token密钥，**注意：这个token只会选择一次，请注意复制保存，或者不要关闭该网页直到PicGo配置完成**

> token是一个GitHub将权限授予给第三方工具的身份验证密钥

![新建仓库](https://cdn.jsdelivr.net/gh/JaoYoo/cdn/img/%E6%96%B0%E5%BB%BA%E4%BB%93%E5%BA%93.png "新建仓库")

![进入设置界面](https://cdn.jsdelivr.net/gh/JaoYoo/cdn/img/%E8%AE%BE%E7%BD%AE.png "进入设置界面")

![开发者设置](https://cdn.jsdelivr.net/gh/JaoYoo/cdn/img/%E8%BF%9B%E5%85%A5%E5%BC%80%E5%8F%91%E8%80%85%E8%AE%BE%E7%BD%AE.png "开发者设置")

![新建token](https://cdn.jsdelivr.net/gh/JaoYoo/cdn/img/token1.png "新建token")

![token配置](https://cdn.jsdelivr.net/gh/JaoYoo/cdn/img/token2.png "token配置")

## 三、PicGo配置

1. 在[PicGo](https://github.com/Molunerfinn/picgo/releases)发布页下载安装包，安装
2. 打开PicGo，在左侧栏找到`图床设置` > `Github`
3. 按照提示进行Github信息的填写
4. 点击`确认`以及`设为默认图床`按钮

- 设定仓库名：按照用户名/图床仓库名的格式填写
- 设定分支名：maste，有些仓库默认主分支名可能为main，进入图床仓库页面查看
- 设定token：粘贴在GitHub中生成的Token
- 设定存储路径：存储图片的路径，例如`img/`，会在仓库根目录下创建一个img的文件夹并存放图片
- 设定自定义域名：输入`https://cdn.jsdelivr.net/gh/用户名/图床仓库名`，以后上传的图片都会按照这个格式生成访问链接，并通过jsDelive进行加速图片访问

![PicGo配置](https://cdn.jsdelivr.net/gh/JaoYoo/cdn/img/picgo.png "PinGo Github配置")

## 四、imagine图片压缩

1. 在[imagine](https://github.com/meowtec/Imagine/releases)中下载
2. 将图片拉进imagine里
3. 图片下方进度条可以调节压缩率（一般默认即可）
4. 在imagine窗口内点击图片左上角的保存按钮
5. 完成图片的压缩

## 五、上传图片到图床

1. 打开PicGo软解，左侧选择上传区
2. 将图片拉进PicGo上传框界面
3. 上传成功后，默认会自动复制Markdown的插入图片格式
4. 粘贴到Markdown文档中即可
