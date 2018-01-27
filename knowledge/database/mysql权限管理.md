# mysql权限管理

下面语句授权用户统一从本地（localhost）登录，如果需要用户可以从远程登录将Host更改为“%”即可。

- 创建用户
```
insert into mysql.user(Host,User,Password) values("localhost","enilu",password("1234"));
flush privileges;
```
- 为用户授权
```
create database testdb;
grant all privileges on testdb.* to enilu@localhost identified by '1234';
flush privileges;
```
- 为用户授权（查询和修改）
```
grant select,update on testdb.* to enilu@localhost identified by '1234';
flush privileges;
```

- 删除用户
```
DELETE FROM user WHERE User="enilu" and Host="localhost";
flush privileges;
```
- 修改用户的密码
```
update mysql.user set password=password('新密码') where User="enilu" and Host="localhost";
```
