---
title:  "JDBC系列——5 PreparedStatement"
summary:  本文是JDBC系列文章的第五篇，对 PreparedStatement 接口进行介绍。
categories: [Java]
tags: [JDBC]
---

本文是 JDBC 系列文章的第五篇，本文将对 PreparedStatement 接口进行介绍，由于本人菜鸟一枚，文中若有错误之处，还请各位朋友批评指正，感谢大家花费时间阅读。

---

PreparedStatement 是用于执行 **动态** SQL 语句，并返回结果。它是 Statement 接口的子接口。它为用户提供了以下主要方法：

``` java
// 执行一条 Select 的 SQL 语句，并返回对应的结果集
ResultSet executeQuery() throws SQLException;
// 执行一条无返回值的 DDL 语句，或执行 Data Manipulation Language（DML语句，如 INSERT、UPDATE、DELETE）语句
int executeUpdate() throws SQLException;
// 对指定参数位置进行Xxx类型值的赋值，parameterIndex 从 1 开始
void setXxx(int parameterIndex, Xxx sqlType);
// 执行一条 SQL 语句，可能返回多个结果。需要配合 getResultSet 或 getUpdateCount 和 getMoreResults 方法一起使用。如果第一个结果是一个 ResultSet 对象，则返回true， 若是一个更新数量或无结果，则返回false
boolean execute() throws SQLException;
```

PreparedStatement 的方法与 Statement 接口的基本类似，使用上也非常的相似，区别仅在于 Statement 接口的SQL是在实际执行的位置作为参数传入的，而 PreparedStatement 接口的SQL是在 Connection 创建 PreparedStatement 的时候设置的，并且是预编译的，需要通过 setXxx 方法对每个占位符进行赋值。

```java
public static void testPreparedStatement() {
    Connection connection = null;
    PreparedStatement statement = null;
    try {
        Class.forName("com.mysql.jdbc.Driver");
        connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/jdbc", "root", "123456");

        String sql = "INSERT INTO users(name) VALUES(?);";
        statement = connection.prepareStatement(sql);
        statement.setString(1, "Tim");
        statement.executeUpdate();
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
