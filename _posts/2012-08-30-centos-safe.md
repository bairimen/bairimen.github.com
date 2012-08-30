---
layout: post
title: "centos 笔记"
description: ""
category: 
tags: centos linux 
comment: true
published: true
---

CentOS 笔记 安全相关


###sysctl 保安
yum install kernel-doc 这个组件，并参阅 Documentation/networking/ip-sysctl.txt

{% highlight bash linenos %}
$ cat /etc/sysctl.conf 

net.ipv4.ip_forward = 0
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0
net.ipv4.tcp_max_syn_backlog = 1280
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.secure_redirects = 0
net.ipv4.conf.all.log_martians = 1
net.ipv4.conf.default.accept_source_route = 0
net.ipv4.conf.default.accept_redirects = 0
net.ipv4.conf.default.secure_redirects = 0
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.icmp_ignore_bogus_error_responses = 1
net.ipv4.tcp_syncookies = 1
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.default.rp_filter = 1
net.ipv4.tcp_timestamps = 0

{% endhighlight %}


###x86_64 用户 简单就是美

采用自定组件清单及只选择基本组件这个简单策略。当你这样做之后，列出现时安装了的组件，然后继续清除你不需要的东西。




{% highlight bash %}
yum list installed >> ~/installed.txt
{% endhighlight %}

如果不需要 i386/i686 组件来提供兼容性，那就 yum remove *.i?86 来删除它们，
然后在 /etc/yum.conf 内加入 exclude = *.i?86 来防止它们再次出现。
ps:32 位元的应用程序，包括一些只提供 32 位元的浏览器插件，将会不能再运作。
原因是 /usr/share/ 内的项目（同时由两组组件所共享）有时会在删除 32 位元 RPM 组件时被一并删除。

### yum 仓库顺序

yum repolist all 列出所有软件库。如意要显示优先次序，就要用下面的

{% highlight bash %}
sed -n -e "/^\[/h; /priority *=/{ G; s/\n/ /; s/ity=/ity = /; p }" /etc/yum.repos.d/*.repo | sort -k3n 
{% endhighlight %}

### 挂载选项 noatime、nodiratime

这个挂载选项告诉系统不要更新 inode 的访问时间。这个选项适用于网络服务器、新闻服务器及其它高访问量的文件系统。
{% highlight bash %}
$ vi /etc/fstab
/dev/VolGroup00/LogVol00 / ext3 defaults,noatime 1 1
{% endhighlight %}


### 挂载windows 网络硬盘
{% highlight bash %} 
$ yum -y install samba-client samba-common autofs 
$ echo "/mnt/win /etc/auto.mymount" >> /etc/auto.master
$ echo "winbox  -fstype=cifs,rw,noperm,user=aaa,pass=bbb ://winbox/getme" > /etc/auto.mymount 
$ /sbin/service autofs restart


{% endhighlight %}