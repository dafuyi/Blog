# Java 连接 MySQL

## 前言

一直用PHP 连接 MySQL，由于 PHP 对 MySQL 连接做了很好的封装基本做到了“开箱即用”。而 Java 相比而言基本上属于“手动挡”。Java 语法“又臭又长”为人所诟病。这里我也尝试了用 Java 来连接 MySQL，好久没写 Java 键盘有些颤抖。

<!--more-->
## 环境

+   Window10
+   IDEA
+   Tomcat9.0
+   JDK 1.8
+   MySQL 8.0.3 rc

## 代码

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>

<%@ page import="com.mysql.jdbc.Driver"%>
<%@ page import="java.sql.*" %>

<html>
<head>
    <title>MySQL connect test</title>
</head>
<body>
<%
    String driverName = "com.mysql.jdbc.Driver";
    String userName = "root";
    String passWorld = "1203my";
    String dbName = "daliy-test";
    String tableName = "test";

    String url = "jdbc:mysql://localhost/"+dbName+"?user="+userName+"&password="+passWorld;
    try {
        Class.forName("com.mysql.jdbc.Driver").newInstance();
        Connection connection = DriverManager.getConnection(url);
        Statement statement = connection.createStatement();
        String sql = "SELECT * FROM "+tableName;
        ResultSet result = statement.executeQuery(sql);
        ResultSetMetaData rmate = result.getMetaData();
        int numCount = rmate.getColumnCount();

        while ( result.next() ) {
            out.print(result.getInt(1));
            out.print(result.getString(2));
            out.print(result.getInt(3));
            out.print("<br>");
        }

        result.close();
        statement.close();
        connection.close();
    } catch (Exception e) {
        e.getMessage();
    }
%>
</body>
</html>
```
