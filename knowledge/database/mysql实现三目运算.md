# mysql实现三目运算

 - 表达式：
 ```sql
 if(expression,result1,result2)
 /**如果expression返回true，返回result1，否则，返回result2，**/
 ```
 - 例如：
 ```sql
 select id ,if(age>60,'老人','普通人') from user;
 ```
