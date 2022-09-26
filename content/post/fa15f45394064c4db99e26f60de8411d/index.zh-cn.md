+++
author = "FlintyLemming"
title = "数据库 insert into from 写到 values 里"
slug = "fa15f45394064c4db99e26f60de8411d"
date = "2020-05-31"
description = ""
categories = ["Coding"]
tags = ["SQL"]
+++

先来看个例子

Table1
No Name
1 Andy
2 Bob
3 Cathey

Table2
No Score(int)
1 85
2 98
3 95

在往 Table2 里写数据的话，最简单的可以这样：

```sql
insert into Table(No, Score) values ("1", 85);
insert into Table(No, Score) values ("2", 98);
insert into Table(No, Score) values ("3", 95);
```

那如果 Table2 中的 No 值想引用 Table 1 的值呢，除了在弄个临时表中转一下，也可以直接把 insert into from 写到 insert into values 里。比如第一条可以这样写：

```sql
insert into Table(No, Score) values ((select No from Table1 where NAME="Andy"), 85);
```

括号一定要加，这是一个小坑点。