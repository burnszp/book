# mysql分组查询符合条件的记录

假设有人口普查表：id,用户名，年龄，省份，地址。
要求查询：每个省份年龄最大的人的详细信息：

```
SELECT
    t1.*
FROM
    普查表 t1
        LEFT JOIN
    (SELECT
        a.id, MAX(a.年龄) AS 最大年龄
    FROM
        普查表 a
    GROUP BY 省份) AS x ON t1.id = x.id
        AND t1.年龄 = x.最大年龄

```
