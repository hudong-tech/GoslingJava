# sql

> ⚠️云数据库一定要注意安全问题

### 数据库安全

[mysql 被人勒索比特比并删库](https://ld246.com/article/1576721171359)

1. mysql 端口不要使用默认端口(其实开源的中间件最好都不要用默认端口)
2. 如数据库这样的重要开源软件,不要暴漏在公网上,如果非要通过外网访问,记得加 ip 白名单
3. 数据库用户加权限 root 用户强烈只能 host 访问(感觉目前用 root 当用户的人 挺多的)

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

### 

