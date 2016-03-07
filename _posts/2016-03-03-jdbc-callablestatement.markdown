---
title:  "JDBC系列——6 CallableStatement"
summary:  本文是JDBC系列文章的第六篇，对 CallableStatement 接口进行介绍。
categories: [Java]
tags: [JDBC]
---

本文是 JDBC 系列文章的第六篇，本文将对 CallableStatement 接口进行介绍，由于本人菜鸟一枚，文中若有错误之处，还请各位朋友批评指正，感谢大家花费时间阅读。

---

CallableStatement 是用于执行 **存储过程或函数** SQL 语句，并返回结果。它是 Statement 接口的子接口。它为用户提供了以下主要方法：

``` java
// 设置输出参数
void registerOutParameter(int parameterIndex, int sqlType)
        throws SQLException;
// 获取输出参数的值
Xxx getXxx (int parameterIndex) throws SQLException;
```

CallableStatement 的方法与 Statement 的方法的差别主要是设置输出参数和获取输出参数。

```msql
create table TMP_SALARY
(
  USER_ID    VARCHAR(20),
  USER_NAME  VARCHAR(10),
  SALARY     DECIMAL(8,2),
  OTHER_INFO VARCHAR(100)
);

insert into TMP_SALARY (USER_ID, USER_NAME, SALARY, OTHER_INFO)
values ('michel', 'Michel', 5000, 'jjjjjj');
insert into TMP_SALARY (USER_ID, USER_NAME, SALARY, OTHER_INFO)
values ('zhangsan', '张三', 10000, null);
insert into TMP_SALARY (USER_ID, USER_NAME, SALARY, OTHER_INFO)
values ('wangwu', '王五', 99999.99, 'twitter account');
insert into TMP_SALARY (USER_ID, USER_NAME, SALARY, OTHER_INFO)
values ('lisi', '李四', 2500, null);

CREATE PROCEDURE TEST_PROCEDURE(
  P_USERID     VARCHAR(20),
  P_SALARY     DECIMAL
)
	update tmp_salary set SALARY = P_SALARY WHERE USER_ID = P_USERID;
```


```java
public static void testCallableStatement() {
    Connection connection = null;
    CallableStatement statement = null;
    try {
        Class.forName("com.mysql.jdbc.Driver");
        connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/jdbc", "root", "123456");
        statement = connection.prepareCall("{call TEST_PROCEDURE(?,?)}");
        statement.setString(1, "michel");
        statement.setBigDecimal(2, new BigDecimal(1000));
        statement.execute();

    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        if (statement != null) {
            try {
                statement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        if (connection != null) {
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```
