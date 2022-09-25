+++
author = "FlintyLemming"
title = "JDBC 从连接到报错"
slug = "90e4bc18801b40a3a15a7f373d39050e"
date = "2019-01-01"
description = ""
categories = ["Coding"]
tags = ["SQL", "Java"]
+++

最近做一个水果店管理系统的作业，需求就是常规的商品增删改查。用的是Java + MySQL，没做界面，就是纯命令行。

增加商品和修改商品信息的方法用的jdbc update数据库中的值一点问题都没有，问题出在入库上，因为入库的过程要先从数据库中读一个数值。我按照增加和修改的方法写了一个这样的方法。后面的return 0好像处理有点问题，不要在意。

```
public int in (Connection con, FruitManage fruitManage) throws Exception {
        String sql_balance="select balance from remain where id=?";
        PreparedStatement balance=con.prepareStatement(sql_balance);
        balance.setInt(1,fruitManage.getId());
        ResultSet rs = balance.executeQuery(sql_balance);

        if (rs.next()) {
            int number = rs.getInt("balance");
            String sql = "update remain set balance=? where id=?";
            PreparedStatement pstmt = con.prepareStatement(sql);
            pstmt.setInt(1, fruitManage.getBalance() + number);
            pstmt.setInt(2, fruitManage.getId());
            return pstmt.executeUpdate();
        }
        return 0;
    }
```

基本的逻辑就是先查出来原来的库存，然后再加上读到的值。这里读数值的类我就不放出来了，反正是按照设定好的一个 FruitManage 数据结构传过来的。问题就出在前面的读取，也就是这一段：

```
String sql_balance="select balance from remain where id=?";
PreparedStatement balance=con.prepareStatement(sql_balance);
balance.setInt(1,fruitManage.getId());
```

按照jdbc的使用方法，这个获取到的商品id，也就是`fruitManage.getId()`应该是要传给`select balance from remain where id=?`这个sql文的问号。但是不行，一直报错，内容是`java.sql.SQLSyntaxErrorException: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '?' at line 1`，也就是说值压根没传给问号。公司里几个人帮忙看，各种改，一直就不行，具体也试了很多方法，不一一说了。

但是我写的修改方法也用的类似的语句，就没有问题，这里贴出来，可以对比一下，没区别。

```
public int setPrice(Connection con, FruitManage fruitManage) throws Exception {
        String sql = "update remain set price=? where id=?";
        PreparedStatement pstmt = con.prepareStatement(sql);
        pstmt.setDouble(1, fruitManage.getPrice());
        pstmt.setInt(2, fruitManage.getId());
        return pstmt.executeUpdate();
    }
```

最后我的前辈介绍了一个无奈的方法，我直接把sql文字符串这么写：

```
String sql_balance = "select balance from remain where id = "+ fruitManage.getId();
```

虚假的sql文，不过反正jdbc也认，能执行。问题就这么尴尬的解决了。
