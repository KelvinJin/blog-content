---
title: 将本地ruby on rails 网站部署到openshift，包含GIT库迁移
permalink: untitled-4
id: 8
updated: '2015-01-27 19:01:56'
date: 2015-01-27 19:01:13
tags:
---

###具体过程
1. 首先使用rhc命令在OpenShift 上创建新的应用。  
`rhc app create MyApp ruby-1.9`
2. 然后创建数据库Postgresql。  
`rhc cartridge add postgresql-9.2 -a MyApp`
3. 然后使用刚才创建网站时返回来的SSH地址,例如：  
`ssh://52b68797500446875512345f@MyApp-yourname.rhcloud.com/~/git/MyApp.git/`
4. 在本地的rails工程目录下执行：  
`git pull ssh://52b68797500446875512345f@MyApp-yourname.rhcloud.com/~/git/MyApp.git/`
5. 然后会提示你有冲突，无所谓，主要是Readme和config等文件，自己动手把冲突解决。然后执行commit提交：  
`git commit -a -m "fixing OpenShift merge"`
6. 然后推到OpenShift上去  
`git push ssh://52b68797500446875512345f@MyApp-yourname.rhcloud.com/~/git/MyApp.git/ master`
7. 注意后面加了一个master。正常情况下以后就可以这样推上去了。  

<!--more-->

####附A：

*在我的转移过程中遇到了bundle版本号问题。因为OpenShift 此时还是Ruby 1.9 而且bundle 是1.1.4版本，导致本来Gemfile 里面第一行有一个Ruby 2.0.0无法识别。据说要bundle 1.2以上才支持。理论上直接SSH到云端然后Gem Bundle update什么的应该能解决，不过我采用了直接删除这一行。。。然后：*
`git commit -am "remove ruby in Gemfile"`
*然后再push一次：*
`git pull ssh://52b68797500446875512345f@MyApp-yourname.rhcloud.com/~/git/MyApp.git/`
*这次可以看到云端开始安装gem了。*

####附B：
*之后服务器自动重启，到了数据库那一步的时候又出问题了，原因是：*
`Could not load database configuration. No such file`
*也就是缺少database.yml文件，这个文件和你本地的可能有区别，所以我没了推上去（在.gitignore里面我设置了忽略）。下一步自然就是把这个文件搞定：*
```
...
production:
adapter: postgresql
encoding: utf8
reconnect: false
pool: 5
database: <%=ENV['OPENSHIFT_APP_NAME']%>;
username: <%=ENV['OPENSHIFT_POSTGRESQL_DB_USERNAME']%>;
password: <%=ENV['OPENSHIFT_POSTGRESQL_DB_PASSWORD']%>;
host: <%=ENV['OPENSHIFT_POSTGRESQL_DB_HOST']%>;
port: <%=ENV['OPENSHIFT_POSTGRESQL_DB_PORT']%>;
```
*再推一次：*
`git pull ssh://52b68797500446875512345f@MyApp-yourname.rhcloud.com/~/git/MyApp.git/`
*好了，理论上讲应该不会出错了。下面就是数据库的部署了。
请看[Postgresql 数据库从本地通过SSH 迁移到Openshift 云端](http://kelvinj.logdown.com/posts/176938)这篇文章*