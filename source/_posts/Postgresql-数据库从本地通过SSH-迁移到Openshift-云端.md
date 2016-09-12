---
title: Postgresql 数据库从本地通过SSH 迁移到Openshift 云端
permalink: untitled-3
id: 7
updated: '2015-01-27 19:00:49'
date: 2015-01-27 19:00:11
tags:
---

数据库的dump和restore
------
1. 确保本地安装postgresql, 可以使用以下命令查看：  
`psql -version`
2. dump当前数据库文件到指定文件.  
`pg_dump -h hostname(eg: localhost) -p port(5432 default) -U username -f "location/to/the/file" database`   
(**注意首先需要创建备份文件。**)    
    <!--more-->
    
3. 使用cat命令结合SSH  
`cat filename | ssh hostname@hostname psql database`  
这里注意hostname@hostname 就是用来ssh 访问openshift 的那个地址。database这里是指导入到哪个database。
我在导入的时候遇到了role不一致的情况，但是似乎不影响导入。
4. 为了使得OpenShift能自动执行rake db:migrate命令，添加下面代码到工作目录下的*.openshift/action_hooks/deploy*：
```shell
pushd ${OPENSHIFT_REPO_DIR} > /dev/null
echo "exec rake db:migrate RAILS_ENV=${RAILS_ENV:-production}"
bundle exec rake db:migrate RAILS_ENV=${RAILS_ENV:-production}
popd > /dev/null
```
5. 推上去吧。

使用Navicat 管理远端数据库。
------
直接使用命令行来操作数据库着实让人觉得很麻烦，特别是对于像我这样的新手。搜索了以下，发现openshift的CLI提供了一个命令：  
`rhc port-forward -a applicationname`
这个命令可以实现将本地的端口转发到云端。这个命令会显示如下信息：
```shell
To connect to a service running on OpenShift, use the Local address
Service Local OpenShift
---------- --------------- ---- -----------------
httpd 127.0.0.1:8080 => 127.10.65.100:8080
postgresql 127.0.0.1:5433 => 127.10.65.100:5432
ruby 127.0.0.1:30325 => 127.10.65.100:30325
```
也就是说如果你访问`127.0.0.1:5433`这个本地端口就相当于在访问`127.10.65.100:5432`这个远端。这样我们就可以用Navicat或者其他数据库管理软件来链接本地这个端口，这样就避免使用SSH而产生的一系列安全配置问题。
当然这个命令不是一劳永逸的，它实际上是运行了一个进程来实现转发。所以每次访问远端数据库前都需要调用这个命令。