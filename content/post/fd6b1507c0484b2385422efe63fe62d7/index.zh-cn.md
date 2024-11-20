+++
author = "FlintyLemming"
title = "将有道词典的单词本导入到欧路词典中"
slug = "fd6b1507c0484b2385422efe63fe62d7"
date = "2019-12-19"
description = ""
categories = ["LifeTec"]
tags = ["有道词典", "欧路词典"]
image = "https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2019/12/%E5%B0%86%E6%9C%89%E9%81%93%E8%AF%8D%E5%85%B8%E7%9A%84%E5%8D%95%E8%AF%8D%E6%9C%AC%E5%AF%BC%E5%85%A5%E5%88%B0%E6%AC%A7%E8%B7%AF%E8%AF%8D%E5%85%B8%E4%B8%AD/title.avif"
+++

有道词典停止了iPad HD版本的维护，只能使用放大版的手机版，本人对此非常不满，于是又在群友的推荐下，选择了欧路词典

但是最大的问题是如何转移单词本，毕竟是竞争产品，直接转移是不可能的，只能通过“导出”“导入”的方式转移，但是操作起来又不是很简单，这里就记一下具体的过程

1. 首先要用电脑版的有道词典导出单词本，本人主要使用 macOS ，但是 macOS 版的有道词典没有导出功能，所以我选择使用 Windows 版本（作为备用系统追求稳定使用的是老旧的 Windows 7，请勿介意）的有道词典。
2. 在这个位置，就可以找到导出选项

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2019/12/%E5%B0%86%E6%9C%89%E9%81%93%E8%AF%8D%E5%85%B8%E7%9A%84%E5%8D%95%E8%AF%8D%E6%9C%AC%E5%AF%BC%E5%85%A5%E5%88%B0%E6%AC%A7%E8%B7%AF%E8%AF%8D%E5%85%B8%E4%B8%AD/1.avif)

3. 由于欧路词典仅支持导入xml格式的文件，并且名字必须是StudyList，所以我们这里选择xml格式，名字改为StudyList

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2019/12/%E5%B0%86%E6%9C%89%E9%81%93%E8%AF%8D%E5%85%B8%E7%9A%84%E5%8D%95%E8%AF%8D%E6%9C%AC%E5%AF%BC%E5%85%A5%E5%88%B0%E6%AC%A7%E8%B7%AF%E8%AF%8D%E5%85%B8%E4%B8%AD/2.avif)

4. 为了对比文件内容，还需要导出欧路词典的单词本（如果是第一次用请先查一个单词并加入单词本中）。导入方法就是在app中点 工具-软件设置-头像-导入导出生词本-导出生词本，iOS的话就可以在 iTunes 中获取这个文件
5. 之后我使用 Visual Studio Code 打开该文件，这里使用什么编辑器无所谓，只要后面的替换你能查到你所有的编辑器的正则表达式就可以。如果是新手那就下一个 Visual Studio Code 好了
6. 打开两个xml文件，接下来进行对比。对比后发现，欧路词典的文件中，单词保存在CustomizeListItem 这个标签中，并且不像有道，保存了注释、音标等内容。不过欧路词典中还包括保存时间等变量，经过测试，即使后面所有变量都一样也不影响导入，比如所有单词后面都是

    ```
    itemType="-9999" addTimeP="20180730T013426" rating="1" categoryTag="@0" fakeRecordId="-1" fakeLibId="0" searchCount="0" deleted="0" serverTimestamp="20180730T015428" localTimestamp="19700101T000000" str1="" meta=""
    ```

    是没问题的。

7. 那么思路有了，对于有道词典导出的文件，除了第一项，只要把

    ```
    </item><item>    <word>
    ```

    替换成

    ```
    <CustomizeListItem word="
    ```

    再将

    ```
    </word>
    <trans><![CDATA[adj. 产前的；胎儿期的； 出生以前的]]></trans>
    <phonetic><![CDATA[[priː'neɪt(ə)l]]]></phonetic>
    <tags></tags>
    <progress>-1</progress>
    ```

    替换为

    ```
    " itemType="-9999" addTimeP="20180730T013426" rating="1" categoryTag="@0" fakeRecordId="-1" fakeLibId="0" searchCount="0" deleted="0" serverTimestamp="20180730T015428" localTimestamp="19700101T000000" str1="" meta="" />
    ```

    即可，其中的注释虽然不尽相同，但是可以用正则表达式替换。

8. 我们可以轻松的使用替换功能（Ctrl+F）将

    ```
    </item><item>    <word>
    ```

    替换成

    ```
    <CustomizeListItem word="
    ```

9. 对于后面，在 [https://msdn.microsoft.com/zh-cn/library/2k3te2cs.aspx](https://msdn.microsoft.com/zh-cn/library/2k3te2cs.aspx) 查阅正则表达式的用法后，准备用

    ```
    /word>.*</progress>
    ```

    替换成

    ```
    itemType="-9999" addTimeP="20180730T013426" rating="1" categoryTag="@0" fakeRecordId="-1" fakeLibId="0" searchCount="0" deleted="0" serverTimestamp="20180730T015428" localTimestamp="19700101T000000" str1="" meta="" />
    ```

    发现该方法对于原文有多行则无法查找到。之后我试了很多方法，包括替换换行等，都无效。

10. 于是我使用了一种传统方法，使用Word替换。
11. 将所有内容复制到Word中，打开“查找和替换”，将

    ```
    \\</word\\>*\\</progress\\>（注意这里的尖括号一定要加转义字符）
    ```

    替换为

    ```
    abcd
    ```

    这里你不能替换为上述的那一长串

12. 之后再把这些复制回 Visual Studio Code 覆盖，把

    ```
    abcd
    ```

    替换为

    ```
    " itemType="-9999" addTimeP="20180730T013426" rating="1" categoryTag="@0" fakeRecordId="-1" fakeLibId="0" searchCount="0" deleted="0" serverTimestamp="20180730T015428" localTimestamp="19700101T000000" str1="" meta="" />13.
    ```

    修改一下第一项的格式，删除结尾的

    ```
    </item></wordbook>
    ```

    然后复制到欧路词典导出文件的<StudyLists>里面即可