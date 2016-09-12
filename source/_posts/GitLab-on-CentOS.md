---
title: GitLab on CentOS
permalink: gitlab-on-centos
id: 19
updated: '2015-01-27 22:10:06'
date: 2015-01-27 22:09:34
tags:
---

### NECTAR

+ 创建instance时记得选择keypair 以及security group.
+ 使用ssh连接nectar.
  ssh -i .ssh/xxx.pem root@xxx.xxx.xxx.xxx(ip address of the vps)
  
### GitLab 搭建

- https://github.com/gitlabhq/gitlab-recipes/tree/master/install/centos#configure-the-default-editor

- postfix 邮件发送系统，可以再说。

- curl 用来下载实在是不行，最好用wget...

- CentOS 里面是 yum作为包管理器。

- sudo gem命令会出现：sudo: gem: command not found， 需要先执行：
  sudo yum install rubygems
  
- sudo -u git -H xxx 都不能成功执行，只能强行切换到root 然后直接运行后面的xxx

- /home/git/ 文件夹的权限太低，数据库配置不成功，只能设置成755。

- Mysql 密码修改请看http://www.cnblogs.com/allenblogs/archive/2010/08/12/1798247.html

- Initialize Database and Activate Advanced Features 一直提示 ident 失败，只能修改pg\_hba.conf 文件，全部把ident 改成trust...然后重启。。。
这个问题涉及到权限，但是又不太对，总觉得是因为系统用户太复杂，数据库用户也多，然后又分组，所以实在是太烦。。