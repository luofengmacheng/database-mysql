## mysql中的日期时间操作

### 1 获取当前日期和时间

* now() 获取当前日期和时间
* curdate() 获取当前日期
* curtime() 获取当前时间

### 2 获取日期和时间的每个部分

* date(now()) 获取当前时间的日期
* time(now()) 获取当前时间
* year(now()) 获取当前年份
* month(now()) 获取当前月份
* day(now()) 获取当前日
* hour(now()) 获取当前的小时
* minute(now()) 获取当前的分
* second(now()) 获取当前的秒

### 3 ...of...系列函数

* dayof...系列函数：计算时间是当周(dayofweek())、当月(dayofmonth())、当年(dayofyear())的多少天

### 4 日期的计算

* date_add(now(), interval '1:00:00' hour_second) 将当前时间加上1个小时，interval表示后面是一个时间区间，然后是时间，最后表示时间的范围：从小时到秒(说明前面的时间包含了小时、分钟、秒)
* date_sub(now(), interval '1:00:00' hour_second) 跟日期的加一样

### 5 日期的差值

* datediff(date1, date2) date1 - date2的天数
* timediff(time1, time2) time1 - time2的差值
* timestampdiff(interval, datetime1, datetime2) interval指示结果的单位，可以是YEAR、QUARTER、MONTH、WEEK、DAY、HOUR、MINUTE、SECOND、FRAC_SECOND。

### 6 日期的转换

* time_to_sec(time) 时间转换成秒
* sec_to_time(seconds) 秒转换成时间
* date_format(date, format) 将日期转换成特定格式
* time_format(time, format) 将时间转换成特定格式