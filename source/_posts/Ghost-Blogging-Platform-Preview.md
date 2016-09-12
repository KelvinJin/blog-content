---
title: Ghost Blogging Platform Preview
permalink: untitled-8
id: 13
updated: '2015-01-27 22:07:08'
date: 2015-01-27 22:06:10
tags:
---

##首先，什么是Ghost博客平台？
简单的讲，就是和WordPress一样，从博客前台到博客后台的一个整体解决方案。如果说和WordPress最大的区别是什么，我认为应该是Ghost采用了近年来比较热门，效率超过[PHP](http://www.appdynamics.com/blog/2013/10/17/an-example-of-how-node-js-is-faster-than-php/)和Ruby好几倍的node.js编写。而基于PHP的WordPress经过10年的发展，已经堆积了大量冗余的代码，加上众多商业化的应用，使其已经从最初的Blog Driven变成了有些带商业意味的服务性平台。这也是Ghost的创作者决定[从头开始](https://ghost.org)的重要原因。

---

##那么Ghost会取代WordPress么？
尽管从目前的发展趋势来看，Ghost成为下一个WordPress不是不可能。但是由于其刚上线不久，甚至连正式版都还未发布（目前仅为测试版-[版本号0.4](https://github.com/TryGhost/Ghost)），所以最重要的后台功能依然未完善。例如在Ghost主页醒目的Dashboard功能似乎也还遥不可及。虽然作者着重强调了博客平台最重要的编辑功能Editor（双屏实时同步显示，如下图），但其功能依然有限。（例如当前的Markdown解析器功能不够强大，不能像logdown一样支持Latex编写）在Ghost的[开发展望](https://github.com/TryGhost/Ghost/wiki/Planned-Features)中可以明显看到将要对Editor进行改进，不过什么时候能推出依然还是个问号。

另外，已经有不少网友开始[吐槽Ghost](http://www.technologyreview.com/view/514451/ghosts-blogging-dashboard-doesnt-need-to-exist/)的设计理念了。比方说，作者在介绍视频里面提到了 *专注写作* 这么一个特性，但是又强调了Dashboard这么一个华丽却和 *writting* 毫不沾边的东西。又比方说现在的Ghost是基于存在磁盘上的spile3数据库，这个方法显然不太理想，特别是在node.js高效的性能光环下， *存在磁盘上的数据库* 就显得有点落后了。

不过，从github上的反响来看(8000+的star)，Ghost还是很有希望得到快速发展的，况且是拿了钱的。。。（当年Ghost在kickstarter上48小时筹了10W刀）。所以我个人对其还是抱有很高的期望的。

<img class="center" src="http://user-image.logdown.io/user/6437/blog/6426/post/177149/heixq4LxTnGqYNqFYqOw_Screen%20Shot%202014-01-24%20at%202.16.51%20am.png" alt="Ghost Editor">

---

##Ghost特性总结
+ node.js编写，响应速度比WordPress快了近10倍。
+ 得益于node.js，Ghost使用动态渲染页面（当然你可以手动设置静态页面来提高访问速度），也就是说你无需重新启动服务器，也不需要重新生成静态页面，任何修改都将在下一次请求的时候生效。这个特性也使得Ghost的整体架构能更加清晰。
+ 体积小。Ghost 0.4版仅有1.4Mb，全部安装配置完成也不到30Mb。
+ 安装部署迅捷。Ghost如果自己配置，仅仅需要3步（当然前提是你已经有一台装有node.js的服务器）。
+ 博客实时预览。也就是左边是编辑窗口，右边就是预览窗口。现在很多网站都流行这个模式。
+ 前台开发简单。得益于[handlebar.js](http://handlebarsjs.com)，前台开发者现在可以使用简单的命令生成所需要的信息。这使得各种主题的开发缩减为千行代码量级。尽管现在的主题数量还远不如WordPress多。

---

##Ghost服务器支持
就目前来看，主流主机服务商或者PaaS平台已经开始着手适配Ghost，有的甚至已经推出了[一键安装Ghost服务](https://www.runkite.com)，比如OpenShift和Digital Ocean。虽然自己搭建Ghost平台已经不是什么难事，不过想要更加便捷的服务和自动化的管理，也许找一个好的博客托管服务也不错。而且现在Ghost官网也已经开启了博客托管服务，尽管对于需要更多个性化配置的人来说不太合适（似乎无法进行上传下载）。

---

##总结
总得来说，Ghost的崛起代表了WordPress的时代已经过去，无论以后会不会有更多类似的博客平台的产生，这片领域依然充满了机遇。特别是在Responsive开发模式的兴起。这里不得不说以下，Ghost在开发Responsive前台方面有着很好的支持。对于我自己来说，以后也会常常关注这方面的消息。也许等Ghost真正成熟的那天，我也会考虑把博客迁过去。