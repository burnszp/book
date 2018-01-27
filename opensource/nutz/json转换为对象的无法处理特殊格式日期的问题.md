# json转为对象的时候无法处理特殊格式的日期的问题

## 问题：
json里面有个日期字符串：Jan 13, 2018 12:00:00 AM

用 Json.fromJsonAsList(CampaignType.class,json);将json直接转换为对象的时候异常了
异常信息为 无法解析日期，

## 解决方法
