# Spring Data JPA知识点集锦


## 查询

### in查询

使用sql语句使用in查询拼接查询条件的时候容易写成：

```
select  * from table_name where column_1 in (:column_1);
query.setParameter("state","1,2,3");
```
上述写法不会报错，但是也不会正常查询，原因在条件必须是一个集合而不能是个逗号分隔的字符串，正确写法为：

```
select  * from table_name where column_1 in (:column_1);
query.setParameter("state",Lists.newArrayList(1,2,3));
```
