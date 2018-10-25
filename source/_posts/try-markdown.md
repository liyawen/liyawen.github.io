---
title: 关于 Yelee 主题背景图的压缩与优化
date: 2016-05-24 14:40:01
categories:
- 术业有专攻
tags:
- Yelee
- PhotoShop
toc:
permalink: image-compression
---

目前主题里可以使用多张随机大图作背景，如果不做好图片压缩和优化，可能会严重影响网站的流畅性。下面简单介绍两种比较简单便捷的优化方案。


<!-- more -->
## 优化说明
- 请使用`jpg`后缀名的背景图片；
- 如果可以，使用`渐进式JPEG`，让图片加载时逐渐清晰；
- 将图片品质尽量调低，一般可以压缩到几十kb（背景图半透明显示，因此细节并不重要）；
> 图1 为目前主题自带背景图的分辨率和文件大小展示
> 图2 为渐进式图片加载示例

![Yelee Background info.](/resources/image-compression-1.png)  ![Progressive JPEGs](/resources/image-compression-2.gif)

## 优化方案
### PhotoShop
- 电脑上如果安装了PhotoShop，那用其来优化图片那是再好不过了
- PS 打开图片 -> 文件 -> 存储为 Web 所用格式 -> JPEG 格式，品质 0，勾选 `连续`（渐进式），最后存储，这样一张高压缩渐进式的 JPG 背景图就做好了

![image Compression by PhotoShop](/resources/image-compression-ps.png)

### 智图
- [智图](http://zhitu.isux.us/) 是腾讯 ISUX 出品的在线图片优化工具，可以方便的对比优化前后的图片，同时可以自行调节图片品质，除了不能设置 `渐进式JPEG`，其他基本满足背景图片优化需求
- 进入网站后按提示上传图片，在调低图片品质，之后下载图片即可
> 因为主题背景图为 `jpg`格式，其他格式请转为 `jpg` 格式后再上传到网上优化。转换的方式有很多，比如，打开图片预览后另存为jpg

![智图](/resources/zhitu.png)

### 优化方案
- 原图为 420kb，智图选择最低品质(10)后约为 129kb；
- PhotoShop 选择最低品质 0，可以压缩到 62k，同时可以选择为 `渐进式JPEG`；
- 因此，有条件的话，更推荐使用 PS 压缩图片。

## 相关链接
1. **智图**： <http://zhitu.isux.us/>
1. **TinyPNG**： <http://tinypng.com/>
1. ***渐进式***： by **张鑫旭** on <code>2013/01/07</code>: <http://www.zhangxinxu.com/wordpress/2013/01/progressive-jpeg-image-and-so-on/>
1. **呆毛王示例壁纸下载** <http://moxfive.xyz/resources/saber.jpg>
1. **最人性化的壁纸网站** <http://www.wallpaperpcmobile.com/>



