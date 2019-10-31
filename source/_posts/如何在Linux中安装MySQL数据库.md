---
title: 如何在Linux中安装MySQL数据库
date: 2019-04-22 14:47:40
categories: [Linux]
tags: 
	- Linux
	- mysql
	- 服务器
---

最近在阿里云买了一个云服务器，centos 7.3。打算把自己写的项目部署进去来玩一玩，熟悉线上部署的流程。部署项目之前肯定要搭建好环境，比如jdk、tomcat、maven、mysql等等的安装。因为之前没有用过linux系统，对其中的原理和各种命令不尽熟悉，所以遇到了许多困难。
<!--more-->
说出来大家可能不信，光是装mysql就装了两天 ，装上之后各种报错。不是不能启动mysql，就是启动了mysql service但是由于本地mysql.sock找不到不能连接，反正是心力交瘁。来来回回装了好几遍，找各种原因，各种解决方案，但是不管怎么样，总算是装好了，并且在装的过程中也学到了很多东西。
那我就来说下装成功的过程，希望接下来我的博客能帮助到大家，互相学习。
# 一.安装wget.
wget命令用来从指定的URL下载文件,wget非常稳定，它在带宽很窄的情况下和不稳定网络中有很强的适应性，如果是由于网络的原因下载失败，wget会不断的尝试，直到整个文件下载完毕。如果是服务器打断下载过程，它会再次联到服务器上从停止的地方继续下载。这对从那些限定了链接时间的服务器上下载大文件非常有用。
如果虚拟机中未安装wget，则：
输入命令`yum -y install wget`
*其中：命令后面加上 -y 后，当执行该命令后，出现 需要选择确认或取消的时候，（即选择y/n的时候），自动选择y*


# 二.在MySQL官网下载MySQL仓库
我使用的是阿里云服务器中的centos7.3，这里提供一个仓库链接：
1.[Red Hat Enterprise Linux 7 / Oracle Linux 7 (Architecture Independent), RPM Package.7版本的](https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm)

2.[Red Hat Enterprise Linux 6 / Oracle Linux 6 (Architecture Independent), RPM Package.6版本的](https://dev.mysql.com/get/mysql80-community-release-el6-1.noarch.rpm)

如果想要其他版本，比如Windows、Ubuntu、Macos，也可以去mysql官网进行不同系统不同版本的下载或者复制响应的链接。
可以来到如下页面：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191023202601505.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDEzNTkw,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191023202736366.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDEzNTkw,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191023202844815.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDEzNTkw,size_16,color_FFFFFF,t_70)
# 三.开始下载MySQL仓库
进入下载的目标目录（我是下载到/download/目录）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191023203145610.png)
##### 1.使用wget进行下载
使用命令# `wget  https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm `
下载成功后就可以在/download/目录下看到该文件。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191023203332908.png)
##### 2.安装
使用命令#  `rpm -Uvh mysql80-community-release-el7-1.noarch.rpm` 
##### 3.接下来就可以正式安装mysql了
默认安装的是8.0版本的，我在这里演示一下5.7版本，如果需要的就是8.0版本可以直接跳过。
修改文件 `vim  /etc/yum.repos.d/mysql-community.repo `
找到这里有一个8.0，有一个人5.7，用哪一个就把它的enabled修改为1, 
这里我用的是5.7的，所以把5.7的enabled=1,把8.0的enable。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191023203819117.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDEzNTkw,size_16,color_FFFFFF,t_70)
修改好保存退出 
然后执行`yum -y install mysql-community-server `进行正式安装。

# 四.修改mysql的密码
执行完毕上述步骤，MySQL就算安装完成，但是需要尽快在7天之内修改默认密码，mysql初始登录需要修改密码。

##### 1.重置密码的第一步就是跳过MySQL的密码认证过程
方法如下：
```bash
#vim /etc/my.cnf(注：windows下修改的是my.ini)
```
在文档内搜索mysqld定位到[mysqld]文本段：
/mysqld(在vim编辑状态下直接输入该命令可搜索文本内容)

在[mysqld]后面任意一行`skip-grant-tables`用来跳过密码验证的过程，如下图所示：
在这里插入图片描述
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191023210015640.png)
保存文档并退出：`:wq`
##### 2.接下来我们需要重启MySQL：
使用如下命令操作mysql：
systemctl start mysqld.service 启动
systemctl stop mysqld.service 停止
systemctl restart mysqld.service 重启
##### 3.重启之后输入`mysql`即可进入mysql
##### 4.进行密码重置
虽然已经走到了安装mysql的最后一步，但是因为第一次使用linux系统和在linux系统中安装mysql，所以遇到了很多问题。
1.使用mysql数据库
```bash
use mysql;
```
2.更新user表中的密码
```bash
update user set password=password('root') where user='root' and host='localhost';
```
在使用上述sql语句之后会报错
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019102322360746.png)
这是因为在/etc/my.cnf文件中加入了`skip-grant-tables`，所以user表删除了password列，若想修改password列的值，要使用authentication_string代替password
即：
```bash
update user set authentication_string=password("root") where user='root' and host='localhost';
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191023223824345.png)
至此，安装mysql所有步骤全部完成！
