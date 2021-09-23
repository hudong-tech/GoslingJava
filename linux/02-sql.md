# sql

### 创建数据库

```sql
mysql -uroot –p #进入数据库控制台
Enter password: #数据库root密码，⚠️输入密码不显示在屏幕上
MySQL [(none)]> create database oneinstack; #特别注意有分号
MySQL [(none)]> show databases; #查看数据库，除oneinstack数据库，其它3个为系统默认库，不能删除
MySQL [(none)]> exit; #退出数据库控制台，特别注意有分号
```

### 创建一个用户

```mysql
mysql -uroot -p
Enter password: #输入数据库的root密码，默认不显示密码
MySQL [(none)]> grant all privileges on db_name.* to dong@'localhost' identified by '366588'; #授权语句，特别注意有分号
MySQL [(none)]> flush privileges; #权限立即生效
MySQL [(none)]> exit; #退出数据库控制台，特别注意有分号
```

