---
title:  "MySQL 杂乱知识点"
categories: [MySQL]
tags: [杂]
---

1. 时间戳处理
  * 时间戳转时间
  ```SQL
  SELECT FROM_UNIXTIME(timestamp, format);
  ```
  * 时间转时间戳
  ```SQL
  SELECT UNIX_TIMESTAMP();
  ```
2. INSERT INTO .. ON DUPLICATE KEY UPDATE  

  当插入行后会导致在一个 **UNIQUE索引** 或 **PRIMARY KEY** 中出现重复值，则执行旧行UPDATE；如果不会导致唯一值列重复的问题，则插入新行。

  如果INSERT多行记录，ON DUPLICATE KEY UPDATE后面字段的值使用VALUES()函数处理来保证每条记录都得到修改。
  ```SQL
  INSERT INTO TABLE (a,b,c) VALUES
    (1,2,3),
    (2,5,7),
    (3,3,6),
    (4,8,2)
  ON DUPLICATE KEY UPDATE b=VALUES(b);
  ```
3. DATE_FORMAT(date, format)
  ```
    format 的取值
    %M 月名字(January……December)
  　%W 星期名字(Sunday……Saturday)
  　%D 有英语前缀的月份的日期(1st, 2nd, 3rd, 等等。）
  　%Y 年, 数字, 4 位
  　%y 年, 数字, 2 位
  　%a 缩写的星期名字(Sun……Sat)
  　%d 月份中的天数, 数字(00……31)
  　%e 月份中的天数, 数字(0……31)
  　%m 月, 数字(01……12)
  　%c 月, 数字(1……12)
  　%b 缩写的月份名字(Jan……Dec)
  　%j 一年中的天数(001……366)
  　%H 小时(00……23)
  　%k 小时(0……23)
  　%h 小时(01……12)
  　%I 小时(01……12)
  　%l 小时(1……12)
  　%i 分钟, 数字(00……59)
  　%r 时间,12 小时(hh:mm:ss [AP]M)
  　%T 时间,24 小时(hh:mm:ss)
  　%S 秒(00……59)
  　%s 秒(00……59)
  　%p AM或PM
  　%w 一个星期中的天数(0=Sunday……6=Saturday ）
  　%U 星期(0……52), 这里星期天是星期的第一天
  　%u 星期(0……52), 这里星期一是星期的第一天
  　%% 字符% )
  ```
4. 时间与秒数的转换
  * SEC_TO_TIME(seconds) 以'HH:MM:SS'或HHMMSS格式返回秒数转成的TIME值
  ```
  select SEC_TO_TIME(2378);
  output: '00:39:38'
  ```
  * TIME_TO_SEC(time) 返回time值有多少秒
  ```
  select TIME_TO_SEC('00:39:38');
  output:  2378
  ```
5. 当前时间和当前日期
  * CURDATE() 、CURRENT_DATE()   以'YYYY-MM-DD'或YYYYMMDD格式返回当前日期值
  * CURTIME() CURRENT_TIME() 以'HH:MM:SS'或HHMMSS格式返回当前时间值
6. 日期加减操作
  * DATE_ADD(date,INTERVAL expr type)
  * DATE_SUB(date,INTERVAL expr type)
  * ADDDATE(date,INTERVAL expr type)
  * SUBDATE(date,INTERVAL expr type)
  ```
  type 的取值
  SECOND 秒 SECONDS
  MINUTE 分钟 MINUTES
  HOUR 时间 HOURS
  DAY 天 DAYS
  MONTH 月 MONTHS
  YEAR 年 YEARS
  MINUTE_SECOND 分钟和秒   "MINUTES:SECONDS"
  HOUR_MINUTE 小时和分钟 "HOURS:MINUTES"
  DAY_HOUR 天和小时 "DAYS HOURS"
  YEAR_MONTH 年和月 "YEARS-MONTHS"
  HOUR_SECOND 小时, 分钟， "HOURS:MINUTES:SECONDS"
  DAY_MINUTE 天, 小时, 分钟 "DAYS HOURS:MINUTES"
  DAY_SECOND 天, 小时, 分钟, 秒 "DAYS HOURS:MINUTES:SECONDS"
  ```
  举例：
  ```
  SELECT DATE_SUB("1998-01-01 00:00:00",INTERVAL "1 1:1:1" DAY_SECOND);
　output: 1997-12-30 22:58:59
  SELECT DATE_ADD("1998-01-01 00:00:00", INTERVAL "-1 10" DAY_HOUR);
　output：1997-12-30 14:00:00
  ```
7. 其他日期函数
  * DAYOFWEEK(date)
　返回日期date是星期几(1=星期天,2=星期一,……7=星期六,ODBC标准)
  * WEEKDAY(date)
　返回日期date是星期几(0=星期一,1=星期二,……6= 星期天)。
  * DAYOFMONTH(date)
　返回date是一月中的第几日(在1到31范围内)      
  * DAYOFYEAR(date)
　返回date是一年中的第几日(在1到366范围内)
  * MONTH(date)
　返回date中的月份数值
  * DAYNAME(date)
　返回date是星期几(按英文名返回)
  * MONTHNAME(date)
　返回date是几月(按英文名返回)
  * QUARTER(date)
　返回date是一年的第几个季度
  * WEEK(date,first)
　返回date是一年的第几周(first默认值0,first取值1表示周一是周的开始,0从周日开始)
  * YEAR(date)
　返回date的年份(范围在1000到9999)
  * HOUR(time)
　返回time的小时数(范围是0到23)
  * MINUTE(time)
　返回time的分钟数(范围是0到59)
  * SECOND(time)
　返回time的秒数(范围是0到59)
  * PERIOD_ADD(P,N)
　增加N个月到时期P并返回(P的格式YYMM或YYYYMM)
  * PERIOD_DIFF(P1,P2)
　返回在时期P1和P2之间月数(P1和P2的格式YYMM或YYYYMM)
