# 安装
yum install -y mysql

# 开启不了可以安装 mariadb (mysql 的社区版本)
service mariadb restart

## centos 安装 mysql 源
sudo rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
yum install mysql mysql-server mysql-libs mysql-server

#开机自启
system enable mysqld 
system start mysqld

## 本地服务
#更改密码
mysql> UPDATE mysql.user SET Password = PASSWORD('root') WHERE User = 'root';
#远程授权连接
mysql> update user set host = '%' where user = 'root';

#查看配置文件
   netstat -lunapt|grep 3306
   mysql配置文件检查
   检查my.cnf的配置，bind-address=addr可以配置绑定ip地址。
   不配置或者IP配置为0.0.0.0，表示监听所有客户端连接。
