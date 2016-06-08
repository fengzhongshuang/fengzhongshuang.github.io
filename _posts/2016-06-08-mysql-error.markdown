---
title:  "MySQL错误解决"
categories: [Java]
tags: [Java]
---

环境：
系统版本: CentOS release 7
数据库版本: 5.5.44 MariaDB

问题描述：
使用PHP的mysql_connect连接基于CentOS 7服务器上的Mysql，报错：Can't connect to MySQL server on 'XXX' (13)

解决方法：
通常情况下，可以进行如下设置
1. 进入MySQL的控制台
# mysql -u root -p

2. 在MySQL的控制台中输入如下命令
mysql> grant all privileges on *.* to 'root'@'your-host-ip' identified by 'your-mysql-password' with grant option;
mysql> flush privileges;
mysql> exit

3. 重启Mysql
# /etc/init.d/mysqld restart
至此，不出意外已经完成修改，并验证通过。

但是如果此时还是出现Can't connect to MySQL server on 'XXX' (13)的错误提示，可以尝试如下方法：

1. 查看httpd_can_network_connect的值是否为off（例如：httpd_can_network_connect --> off）
# getsebool -a | grep httpd

2. 修改httpd_can_network_connect的值为on
# setsebool httpd_can_network_connect 1

3. 重新验证httpd_can_network_connect
# getsebool -a | grep httpd

4. 重启http
# /etc/init.d/httpd restart

5. 进入客户端重启登录验证
此时错误信息变为：Access denied for user 'root'@'your-host-ip'(usring password:YES)

6. 进入Mysql控制台重新GRANT（如上）
