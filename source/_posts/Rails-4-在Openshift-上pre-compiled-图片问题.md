---
title: Rails 4 在Openshift 上pre-compiled 图片问题
permalink: untitled-5
id: 15
updated: '2015-01-27 19:03:48'
date: 2015-01-27 19:02:55
tags:
---

###这个问题真是。。。
将
`background: url("header_bg.png") repeat;`
改为：
`background: asset_data_url("header_bg.png") repeat;`

###相当于嵌入CSS,感觉不好，不过真是不知道怎么办了。。

###查了以下维基百科，嵌入式的这种方式叫做Data URI    

<!--more-->

####优势：
- HTTP的overhead太大的时候，Data URI的效率更高（没能理解。。)
- 尽管通常情况下浏览器不会单独缓存嵌入到CSS的图片，但是却会缓存CSS文件本身（这。。），所以如果一个包含10个嵌入式图片的CSS文件需要的请求数只有1次，而且因为有缓存，所以只要10个图片都不改变，那么以后访问就可以直接调用缓存。而单独缓存图片的情况就会导致请求数变为11次。（虽然这么说，但是如果图片更新较快的话，前者一旦有一个图片发生改变，其他图片就要重新加载（实际上是加载新的CSS），而后者一个图片改了就只加载这一个图片。不知道这么理解对不对。）所以可以把更新不那么频繁的图片采用嵌入式的方式放进CSS，而经常需要换的图片就采用传统的单独缓存机制。

####劣势：
- 上面讲了，只要局部改变，就会导致全部重新加载。
- 一些加密的问题。
- 另外下载的时候由于没有单独存放，所以名字会特别奇怪。

*Update:*

仔细看完Rails的guide后，对assets pipeline 有了新的认识。

> precompiling 会将指定的（production模式下默认是app/assets, lib/assets以及vendor/assets目录下的所有除application.js以及application.css以外的非js,css文件）在public/assets目录下生成一堆带MD5码的缓存文件。浏览器可以直接读取这些文件，从而判断是否需要重新加载新的。

在application.rb中设置：
`config.assets.initialize_on_precompile = false`
在production.rb中设置：
`config.assets.compress = true`
以上都是为了加速预编译过程以及缩小js.css文件大小方便浏览器快速读取。然后将原本的图片访问路径改为：
将
`background: url("header_bg.png") repeat;`
改为：
`background: asset_url("header_bg.png") repeat;`