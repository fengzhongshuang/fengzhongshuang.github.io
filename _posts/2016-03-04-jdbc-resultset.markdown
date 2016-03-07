---
title:  "JDBC系列——7 ResultSet"
summary:  本文是JDBC系列文章的第七篇，对 ResultSet 接口进行介绍。
categories: [Java]
tags: [JDBC]
---

本文是 JDBC 系列文章的第七篇，本文将对 ResultSet 接口进行介绍，由于本人菜鸟一枚，文中若有错误之处，还请各位朋友批评指正，感谢大家花费时间阅读。

---

ResultSet 是用来表示查询结果的集合，是通过执行查询数据库的 SQL 语句产生的。默认情况下， ResultSet 是不可更改的，并且只能从第一行遍历到最后一行，但是可以通过在创建 Statement 时设置一些参数来是 ResultSet 变成可更改和可随意滚动的。
```java
Statement stmt = con.createStatement(
          ResultSet.TYPE_SCROLL_INSENSITIVE,
          ResultSet.CONCUR_UPDATABLE);
ResultSet rs = stmt.executeQuery("SELECT a, b FROM TABLE2");
```

ResultSet 主要提供了以下方法：
```java
// 使游标移动到下一个位置。
boolean next() throws SQLException;
// 获取行中指定列的值，列从 1 开始。
Xxx getXxx(int columnIndex) throws SQLException;
// 获取行中指定列名的值
Xxx getXxx(String columnLabel) throws SQLException;
// 获取结果集的元数据
ResultSetMetaData getMetaData() throws SQLException;
// 更新指定列的值
void updateXxx(int columnIndex, Xxx x) throws SQLException;
void updateXxx(String columnLabel, Xxx x) throws SQLException;
// 向ResultSet中插入一条记录
void insertRow() throws SQLException;
// 向ResultSet更新一条记录
void updateRow() throws SQLException;
// 删除ResultSet中的一条记录
void deleteRow() throws SQLException;
// 在数据库中刷新当前行的记录
void refreshRow() throws SQLException;
```

在介绍 statement 时已经使用过 ResultSet 了，下面记录引用这个例子。
```java
public static void testExecuteQuery() {
    Connection connection = null;
    Statement statement = null;
    ResultSet resultSet = null;

    try {
        String sql = "SELECT id, name FROM users";
        Class.forName("com.mysql.jdbc.Driver");
        connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/jdbc", "root", "123456");
        statement = connection.createStatement();
        resultSet = statement.executeQuery(sql);

        while (resultSet.next()) {
            int id = resultSet.getInt("id");
            String name = resultSet.getString("name");

            System.out.println("id = " + id + " name = " + name);
        }
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        if (resultSet != null) {
            try {
                resultSet.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

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
