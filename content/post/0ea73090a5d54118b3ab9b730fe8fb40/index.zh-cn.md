+++
author = "FlintyLemming"
title = "SQL Server 表迁移"
slug = "0ea73090a5d54118b3ab9b730fe8fb40"
date = "2019-10-14"
description = ""
categories = ["Coding"]
tags = ["SQL"]
image = "https://img.flinty.moe/blog/posts/2019/10/SQL%20Server%20%E8%A1%A8%E8%BF%81%E7%A7%BB/title.avif"
+++

## 手动单表迁移

### 迁移表结构

1. 先删除原表，删除时注意结构，从最次要的表删起
2. 对原表右键 - Script Table as - CREATE To - New Query Editor Window

    ![](https://img.flinty.moe/blog/posts/2019/10/SQL%20Server%20%E8%A1%A8%E8%BF%81%E7%A7%BB/1.avif)

3. 复制全部的脚本
4. 在新数据库下，新建一个查询，然后把刚才复制的脚本粘贴过来，修改 USE 的数据库，执行

### 迁移表内容

1. 由于是单表迁移，不需要使用脚本，直接全查询表的数据

        /****** Script for Select command from SSMS  ******/
        SELECT [xxx],[xxx]
          FROM [xxx].[xxx].[xxx]

2. 查询完毕后，在 Results 中点击左上角空白按钮，全选项目

    ![](https://img.flinty.moe/blog/posts/2019/10/SQL%20Server%20%E8%A1%A8%E8%BF%81%E7%A7%BB/2.avif)

3. 再右键空白按钮，选择 Save Result As...

    ![](https://img.flinty.moe/blog/posts/2019/10/SQL%20Server%20%E8%A1%A8%E8%BF%81%E7%A7%BB/3.avif)

4. 格式可以选择 txt 或者 csv，前者不需要处理，后者要转成 xls

    ![](https://img.flinty.moe/blog/posts/2019/10/SQL%20Server%20%E8%A1%A8%E8%BF%81%E7%A7%BB/4.avif)

    转的话，用表格软件打开，然后另存为就行

    ![](https://img.flinty.moe/blog/posts/2019/10/SQL%20Server%20%E8%A1%A8%E8%BF%81%E7%A7%BB/5.avif)

5. 在新数据库环境下，右键数据库 - 任务 - 导入数据…

    ![](https://img.flinty.moe/blog/posts/2019/10/SQL%20Server%20%E8%A1%A8%E8%BF%81%E7%A7%BB/6.avif)

    **一定不要复制原表全查询结果，然后直接粘贴！会非常慢，尤其是你远程连接的，2w条数据要三个多小时！**

6. 选择数据源里，如果之前导出的是 txt，选择 Flat File Source，如果是转换过的 Excel 文件，选择 Microsoft Excel

    ![](https://img.flinty.moe/blog/posts/2019/10/SQL%20Server%20%E8%A1%A8%E8%BF%81%E7%A7%BB/7.avif)

7. 选择目标这里，选择 Microsoft OLE DB Driver for SQL Server，然后重新输入下账号密码

    ![](https://img.flinty.moe/blog/posts/2019/10/SQL%20Server%20%E8%A1%A8%E8%BF%81%E7%A7%BB/8.avif)

8. 选择 复制一个或多个表或视图的数据

    ![](https://img.flinty.moe/blog/posts/2019/10/SQL%20Server%20%E8%A1%A8%E8%BF%81%E7%A7%BB/9.avif)

9. 目标这里，重新选择下对应的表

    ![](https://img.flinty.moe/blog/posts/2019/10/SQL%20Server%20%E8%A1%A8%E8%BF%81%E7%A7%BB/10.avif)

10. 检查映射，这里如果有IDE无法转换的类型（比如没有处理得当的日期）就会报错，能自动转换的会显示感叹号，可以进行下一步

    ![](https://img.flinty.moe/blog/posts/2019/10/SQL%20Server%20%E8%A1%A8%E8%BF%81%E7%A7%BB/11.avif)

11. 立即运行，然后点击 Finish

    ![](https://img.flinty.moe/blog/posts/2019/10/SQL%20Server%20%E8%A1%A8%E8%BF%81%E7%A7%BB/12.avif)

## 脚本迁移

1. 在数据库上右键，Task - Cenerate Scripts... 创建一个脚本

    ![](https://img.flinty.moe/blog/posts/2019/10/SQL%20Server%20%E8%A1%A8%E8%BF%81%E7%A7%BB/13.avif)

2. 在 Choose Objects 里，选择一个或多个需要的表，然后下一步

    ![](https://img.flinty.moe/blog/posts/2019/10/SQL%20Server%20%E8%A1%A8%E8%BF%81%E7%A7%BB/14.avif)

3. 这里点击 Advanced

    ![](https://img.flinty.moe/blog/posts/2019/10/SQL%20Server%20%E8%A1%A8%E8%BF%81%E7%A7%BB/15.avif)

4. 选择到底是要表结构，还是表数据，还是全都要

    ![](https://img.flinty.moe/blog/posts/2019/10/SQL%20Server%20%E8%A1%A8%E8%BF%81%E7%A7%BB/16.avif)

    但是这里坑，每个大版本的 Studio 这里的界面和表述不一样，自己加以理解并选择

5. 然后他会生成一个 sql 文件，里面是 sql 文，一条一条的 Insert，拿到新表中执行即可

    但是如果你的条目太多（根据机器性能决定），一次性执行可能会崩，要分多次执行

## 整个数据库迁移，再选择需要的表 Insert

1. 首先备份原来的数据库，右键数据库 Tasks - Back Up...

    ![](https://img.flinty.moe/blog/posts/2019/10/SQL%20Server%20%E8%A1%A8%E8%BF%81%E7%A7%BB/17.avif)

    这里要注意你这个数据库名称不能跟目标数据库名称相同，因为后面需要放在一起

2. Destination 里 Add... 选择一个本地路径

    ![](https://img.flinty.moe/blog/posts/2019/10/SQL%20Server%20%E8%A1%A8%E8%BF%81%E7%A7%BB/18.avif)

    **注意，这里的目录是你服务器所在机器的目录，不是你运行这个数据库 IDE 机器的的目录**

3. 目标数据库导入这个数据库后使用 insert 命令导入数据

        INSERT INTO dbname..sheetname SELECT * FROM dbname..sheetname.
