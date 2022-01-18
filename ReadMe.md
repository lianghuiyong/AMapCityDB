# 说明
- SQL更新时间：**2022年1月17日**

- 数据导入失败
可能问题是sql文件过大 MySQL 命令行输入以下命令，再执行sql文件
```
mysql> set global max_allowed_packet = 50*1024*1024;
Query OK, 0 rows affected (0.00 sec)
```
- 什么时候用city_code
```
常规市、直辖市、港澳台层级是有city_code的，子级的city_code与父级的city_code相同
```
- 什么时候用town_code
```
乡镇，街道，但是有很多乡镇、街道是没有town_code，而且乡镇街道变化比较大，每年都有变动
```

# 高德数据接口
使用API前先[申请Key](https://lbs.amap.com/dev/key)，若无高德地图API账号需要先申请账号
1. 文档地址：[https://lbs.amap.com/api/webservice/guide/api/district](https://lbs.amap.com/api/webservice/guide/api/district)
2. 乡镇街道级别返回的adcode是所属区县的adcode。

- 行政区划查询
```
https://restapi.amap.com/v3/config/district?subdistrict=4&key=申请Key
```
- 地理编码/逆地理编码
```
"https://restapi.amap.com/v3/geocode/regeo?output=json&location="+ baseAmapDb.getCenter() + "&key=***" + "&radius=100&extensions=base"
```

# level
|level值|说明|对应高德字段|
|-|-|-|
|0|中华人民共和国|country|
|1|省、直辖市|province|
|2|市|city|
|3|区、县|district|
|4|乡镇、街道|street|

# code
|code字段|说明|
|-|-|
|ad_code|区域编码（乡镇、街道没有独有的adcode，均继承父类（区、县）的adcode）|
|parent_code|父类（区、县）的adcode|
|city_code|市、直辖市、港澳台|
|town_code|乡镇、街道编码|

# 示例

|id|level|ad_code|parent_code|name|city_code|town_code|town_name|center|
|-|-|-|-|-|-|-|-|-|
|1|0|100000||中华人民共和国||||116.3683244,39.915085|
|3|1|440000|100000|广东省||||113.266887,23.133306|
|34|1|110000|100000|北京市|010|||116.407387,39.904179|
|73|2|442000|440000|中山市|0760|||113.392517,22.517024|
|751|4|442000|442000|小榄镇|0760|442000100000|小榄镇|113.320629,22.560691|
