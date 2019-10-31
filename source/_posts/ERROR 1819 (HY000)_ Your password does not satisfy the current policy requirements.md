---
title: Your password does not satisfy the current policy requirements
date: 2019-04-22 14:47:40
categories: 
	- linux
	- mysql
tags: 
	- linux
	- mysql
---

在linux中部署项目，搭建数据库环境的时候，需要专门设置一个user来管理响应的数据库。这是我在初步使用mysql，向mysql中user表插入用户时遇到的问题。
<!--more-->
密码策略问题异常信息：
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements

如下图：![在这里插入图片描述](https://img-blog.csdnimg.cn/20191024103619380.png)
解决办法：

#### 1、查看 mysql 初始的密码策略，
输入语句 

```bash
SHOW VARIABLES LIKE 'validate_password%'; 
```
进行查看，
如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191024103903746.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDEzNTkw,size_16,color_FFFFFF,t_70)
#### 2、首先需要设置密码的验证强度等级
设置 validate_password_policy 的全局参数为 LOW 即可，输入设值语句 

```bash
set global validate_password_policy=LOW;
```

进行设值，如下图：![在这里插入图片描述](https://img-blog.csdnimg.cn/20191024104114414.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDEzNTkw,size_16,color_FFFFFF,t_70)
#### 3、设置密码长度
当前密码长度为 8 ，如果不介意的话就不用修改了，此处设置为 4 位的密码，设置 validate_password_length 的全局参数为 6 即可，
输入设值语句
`  set global validate_password_length=4;  `进行设值，如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191024104332805.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDEzNTkw,size_16,color_FFFFFF,t_70)
#### 4.设置密码
现在就可以为mysql的用户设置密码了，只要满足4位长度即可。

```bash
create user 'mmall'@'localhost' identified by 'mmall';
```
执行结果如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191024105338293.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDEzNTkw,size_16,color_FFFFFF,t_70)
至此，用户就添加成功了！
