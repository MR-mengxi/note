## centos7完全卸载MySQL

```yaml
## 使用yum安装mysql后，如果想要完全卸载mysql，可以使用下面的方式
1. 查看mysql安装了哪些东西
rpm -qa |grep -i mysql
2. 开始卸载
yum remove mysql-community-common-5.7.20-1.el7.x86_64
yum remove mysql-community-client-5.7.20-1.el7.x86_64
yum remove mysql57-community-release-el7-11.noarch
yum remove mysql-community-libs-5.7.20-1.el7.x86_64
yum remove mysql-community-server-5.7.20-1.el7.x86_64
3. 查看是否卸载完成
rpm -qa |grep -i mysql
4. 查找mysql相关目录
find / -name mysql
5. 删除相关目录
rm -rf /var/lib/mysql
rm -rf /var/lib/mysql/mysql
rm -rf /usr/share/mysql
6. 删除/etc/my.cnf
rm -rf /etc/my.cnf
7. 删除/var/log/mysqld.log(如果不删除这个文件，新安装的mysql无法生成新密码，会导致无法重新登录)
rm -rf /var/log/mysqld.log
```

