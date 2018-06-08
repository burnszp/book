# mysql查询结果横转纵

表结构：

```
id int
id_borrow_application int
data_code varchar(32)
data_value varchar(32)
```

查询记录：

```
SELECT
	t1.id_borrow_application AS '申请单id',
	t1.data_code AS '数据类别',
	t1.data_value AS '评分值'
FROM
	`anti_fraud_output_data` t1
```

查询结果:

```
申请单id 数据类别       评分值
113623	CREDITSCORE	    610.420980478186
113623	SCORECARD_0814	423.340501710447
338089	CREDITSCORE     623.82745236595
338089	SCORECARD_0814	413.328819095534
20061	 CREDITSCORE	    586.667855363785
20061	 SCORECARD_0814 	404.160379289446

```

针对上述结果，每个申请单有两条记录，分别为 CREDITSCORE 和SCORECARD_0814的值，现在要求每个申请单展示一条记录，将CREDITSCORE 和SCORECARD_0814展示为一行,
实现方式如下：

```

SELECT
	t1.id_borrow_application AS '申请单id',
	max( case t1.data_code when 'SCORECARD_0814' THEN t1.data_value else null end )  'SCORECARD_0814',
	max( case t1.data_code when 'CREDITSCORE' THEN t1.data_value else null end)  'CREDITSCORE'
FROM
	`t_snow_anti_fraud_output_data` t1
group by t1.id_borrow_application;
```

查询结果如下：

```
申请单id      SCORECARD_0814      CREDITSCORE
113623		   423.340501710447     610.420980478186
338089	     413.328819095534     623.82745236595
20061	 	     404.160379289446     586.667855363785
```
