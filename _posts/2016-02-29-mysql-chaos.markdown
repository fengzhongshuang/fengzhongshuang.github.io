---
title:  "MySQL 杂乱知识点"
categories: [MySQL]
tags: [杂]
---

## MySQL 杂乱知识点

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
