title: time和datetime模块
author: hero576
toc: true
tags:
  - python
categories:
  - python基础
date: 2018-04-08 17:05:00
---
## 时间模块
python 中时间表示方法有：时间戳，即从1975年1月1日00:00:00到现在的秒数；格式化后的时间字符串；时间struct_time 元组。

struct_time元组中元素主要包括：
- tm_year（年）、tm_mon（月）、tm_mday（日）、tm_hour（时）、tm_min（分）、tm_sec（秒）、tm_wday（weekday0 - 6（0表示周日））、tm_yday（一年中的第几天1 - 366）、tm_isdst（是否是夏令时）

## time
### 常用函数

- time.time()返回当前时间戳

```python
>>> time.time()
1465370844.096474
```

- time.ctime() 返回这种格式的时间字符'Wed Jun 8 15:27:48 2016'，显示当前时间.同时也可以转换时间戳

```python
>>> time.ctime()
'Wed Jun  8 15:27:48 2016'

>>> time.ctime(time.time()-86400)
'Tue Jun  7 15:29:36 2016'
```

- time.gmtime 将时间戳转换成struct_time格式，此显示的是格林威治0时区的时间

```python 
>>> time.gmtime()
time.struct_time(tm_year=2016, tm_mon=6, tm_mday=8, tm_hour=7, tm_min=34, tm_sec=28, tm_wday=2, tm_yday=160, tm_isdst=0)

>>> time.gmtime(time.time() - 86400)
time.struct_time(tm_year=2016, tm_mon=6, tm_mday=7, tm_hour=7, tm_min=34, tm_sec=41, tm_wday=1, tm_yday=159, tm_isdst=0)
```

- time.localtime 将当前系统时间戳转化为struct_time格式 

```python 
>>> time.localtime()
time.struct_time(tm_year=2016, tm_mon=6, tm_mday=8, tm_hour=15, tm_min=35, tm_sec=33, tm_wday=2, tm_yday=160, tm_isdst=0)

>>> time.localtime(time.time() - 86400)
time.struct_time(tm_year=2016, tm_mon=6, tm_mday=7, tm_hour=15, tm_min=37, tm_sec=10, tm_wday=1, tm_yday=159, tm_isdst=0)
```

- time.mktime 将struct_time格式转回成时间戳

```python 
>>> now = time.localtime()
>>> now
time.struct_time(tm_year=2016, tm_mon=6, tm_mday=8, tm_hour=15, tm_min=38, tm_sec=28, tm_wday=2, tm_yday=160, tm_isdst=0)
>>> time.mktime(now)
1465371508.0
```
 
- time.strftime 将struct_time格式转成指定的字符串格式

```python 
>>> now = time.localtime()
>>> now
time.struct_time(tm_year=2016, tm_mon=6, tm_mday=8, tm_hour=15, tm_min=38, tm_sec=28, tm_wday=2, tm_yday=160, tm_isdst=0)
>>> last = time.localtime(time.time() - 86400)
>>> last
time.struct_time(tm_year=2016, tm_mon=6, tm_mday=7, tm_hour=15, tm_min=40, tm_sec=23, tm_wday=1, tm_yday=159, tm_isdst=0)
>>> time.strftime("%Y-%m-%d %H:%M:%S",last)
'2016-06-07 15:40:23'
>>> time.strftime("%Y-%m-%d %H:%M:%S",now)
'2016-06-08 15:38:28'
```
 
- time.strptime 将自定义时间格式的字符串转换为struct_time格式

```python 
>>> time.strptime("2016-06-08","%Y-%m-%d")
time.struct_time(tm_year=2016, tm_mon=6, tm_mday=8, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=2, tm_yday=160, tm_isdst=-1)
>>> time.strptime("2016-06-08 15:50:44","%Y-%m-%d %H:%M:%S")
time.struct_time(tm_year=2016, tm_mon=6, tm_mday=8, tm_hour=15, tm_min=50, tm_sec=44, tm_wday=2, tm_yday=160, tm_isdst=-1)
```

- time.sleep 暂停时间，类似于shell的sleep()

- 其他
 - 时间格式：

|格式	|说明|
| -------- | :----: |
|%a	|显示简化星期名称|
|%A	|显示完整星期名称|
|%b|	显示简化月份名称|
|%B	|显示完整月份名称|
|%c	|本地相应的日期和时间表示|
|%d	|显示当月第几天|
|%H	|按24小时制显示小时|
|%I	|按12小时制显示小时|
|%j	|显示当年第几天|
|%m	|显示月份|
|%M	|显示分钟数）|
|%p	|本地am或者pm的相应符|
|%S	|显示秒数）|
|%U	|一年中的星期数|
|%w	|显示在星期中的第几天，默认从0开始表示周一|
|%W	|和%U基本相同|
|%x	|本地相应日期|
|%X	|本地相应时间|
|%y	|去掉世纪的年份（00 - 99）|
|%Y	|完整的年份|
|%Z	|时区的名字（如果不存在为空字符）|
|%%	|‘%’字符|
|o	|时间函数之间转换关系|
 ￼
## datetime

datime是time的升级版，可以对date(日期)、time(时间)、datetime（日期时间）等三种单独管理。主要是由下面四个类组成

- datetime.datetime常用函数（datetime.date datetime.time通用）
 
- datetime.datetime.today() 默认返回当前日期和时间的对象，也可以自定义日期和时间

```python
>>> today = datetime.datetime.today()
>>> print(today)
2016-06-08 16:34:08.163371
>>> last = datetime.datetime(2016,5,8,11,23,55)  ＃注意此处表示日期只能是实际月份，不能带0
>>> print(last)
2016-05-08 11:23:55

>>>last = datetime.datetime(2016,05,08,11,23,55)
File "<stdin>", line 1
last = datetime.datetime(2016,05,08,11,23,55)
                    ^
SyntaxError: invalid token
```

- datetime.datetime.now() 返回当前时间

```python
>>> datetime.datetime.now()
datetime.datetime(2016, 6, 8, 16, 44, 29, 694398)
```
    
- datetime.strftime(format)  ＃自定义格式化时间

```python
>>> today.strftime("%I:%M:%S %p %d/%m/%Y")
 '04:34:08 PM 08/06/2016'
```

- datetime.datetime.timple() 将时间转换为struct_time 格式

```python
>>>today.timetuple()
time.struct_time(tm_year=2016, tm_mon=6, tm_mday=8, tm_hour=16, tm_min=34, tm_sec=8, tm_wday=2, tm_yday=160, tm_isdst=-1)
```

- datetime.replace（）返回一个替换后的date对象

```python
>>>last = today.replace(1949,10,1)
>>> print(last)
1949-10-01 16:34:08.163371
>>> last = today.replace(year=1919,month=3,day=2)
>>> print(last)
1919-03-02 16:34:08.163371
```

- datetime.datetime.strptime  将字符串转换为日志格式对象

```python
>>> a = "2016-06-08 17:18:19"
>>> b = datetime.datetime.strptime(a,"%Y-%m-%d %H:%M:%S")
>>> print(a)
2016-06-08 17:18:19
>>> print(b)
2016-06-08 17:18:19
>>> print(type(a))
<class 'str'>
>>> print(type(b))
<class 'datetime.datetime'>
```
 
- datetime.timedelta 时间运算
 - 可用参数：days seconds microseconds milliseconds minutes hours weeks
 
```python 
today = datetime.datetime.now()
>>> print(today)
2016-06-08 16:51:31.698122
>>> yesterday = today - datetime.timedelta(days=1)
>>> print(yesterday)
2016-06-07 16:51:31.698122
>>> last_hour = today - datetime.timedelta(hours=1)
>>> print(last_hour)
2016-06-08 15:51:31.698122
```