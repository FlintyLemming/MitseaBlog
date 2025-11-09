+++
author = "FlintyLemming"
title = "Windows 存储空间不等容量磁盘 Parity 逻辑探究"
slug = "a08fab67a7b8430faaeb10edc5dc490c"
date = "2023-11-06"
description = "WinNAS 好"
categories = ["HomeLab", "Windows"]
tags = ["存储空间", "NAS"]
image = "https://assets.mitsea.cn/blog/posts/2023/10/Windows%20%E5%AD%98%E5%82%A8%E7%A9%BA%E9%97%B4%E4%B8%8D%E7%AD%89%E5%AE%B9%E9%87%8F%E7%A3%81%E7%9B%98%20Parity%20%E9%80%BB%E8%BE%91%E6%8E%A2%E7%A9%B6/jigar-panchal-D_ivYIn4jWw-unsplash.avif"
+++

在组建阵列时，一般都推荐使用容量相同的磁盘，不过这种情况下有条件的都使用 RAID 阵列卡获得最大性能了。但是作为垃圾佬，手头的硬盘都是根据市场价格买最便宜的容量，所以会有容量不一的硬盘。此时，如何在不同容量的硬盘上创建单盘冗余阵列并且还能最大化可用空间就成了一个重要的需求。目前主流方案有两个方向：块冗余和文件冗余。

先提一嘴文件冗余，主要代表就是绿联和极空间等国产 NAS 用的比较多，还有就是 Windows 下的 StableBit Drive Pool。

块冗余是更为传统但是性能更好的方案。首先传统 RAID 对于垃圾佬非常不友好，如果是 1+2+3T 的硬盘组合，单盘冗余可用空间就只有 2T。在此基础上，群晖进行了改进，通过划分磁盘为多个部分，通过创建多个 RAID5 达到最大利用效果。

![](https://assets.mitsea.cn/blog/posts/2023/10/Windows%20%E5%AD%98%E5%82%A8%E7%A9%BA%E9%97%B4%E4%B8%8D%E7%AD%89%E5%AE%B9%E9%87%8F%E7%A3%81%E7%9B%98%20Parity%20%E9%80%BB%E8%BE%91%E6%8E%A2%E7%A9%B6/Untitled.avif)

通过下面的对比可以看出 SHR 对于不同容量的硬盘相比传统 RAID5 有着巨大优势

![](https://assets.mitsea.cn/blog/posts/2023/10/Windows%20%E5%AD%98%E5%82%A8%E7%A9%BA%E9%97%B4%E4%B8%8D%E7%AD%89%E5%AE%B9%E9%87%8F%E7%A3%81%E7%9B%98%20Parity%20%E9%80%BB%E8%BE%91%E6%8E%A2%E7%A9%B6/Untitled%201.avif)

那么寻找一个类似 SHR 的超高利用率的阵列方案就至关重要。

首先就是 unraid，unraid 可以选择容量最大那个盘作为校验盘，可用空间就是其他所有盘容量总和。但是 unraid 由于一次只操作一块磁盘，所以性能非常低下，不太行。

然后就是华为 RAID 2.0，这个我没钱，跳过。

然后就是 Windows Server 存储空间了，经过实测，Windows Server 是可以达到类似 SHR 的效果，实现可用空间和性能兼备。

下面，我将以 100G + 200G + 300G + 400G + 500G 的实验环境来展示 Windows Server Parity 校验的特性。请注意，Parity 的详细原理我没找到，以下皆是我根据 Parity 表现出来的特性假想的工作原理，可能与实际不符。

如果在 GUI 上创建单盘冗余的虚拟磁盘，可用大小是可以正确预估的，大约为 1T

![](https://assets.mitsea.cn/blog/posts/2023/10/Windows%20%E5%AD%98%E5%82%A8%E7%A9%BA%E9%97%B4%E4%B8%8D%E7%AD%89%E5%AE%B9%E9%87%8F%E7%A3%81%E7%9B%98%20Parity%20%E9%80%BB%E8%BE%91%E6%8E%A2%E7%A9%B6/Untitled%202.avif)

但是实际创建时会报错

![](https://assets.mitsea.cn/blog/posts/2023/10/Windows%20%E5%AD%98%E5%82%A8%E7%A9%BA%E9%97%B4%E4%B8%8D%E7%AD%89%E5%AE%B9%E9%87%8F%E7%A3%81%E7%9B%98%20Parity%20%E9%80%BB%E8%BE%91%E6%8E%A2%E7%A9%B6/Untitled%203.avif)

问题就出现在红框处，因为 GUI 创建时 **NumberOfColumns** 默认是等于硬盘数量，当前场景下的话就是 5。

**NumberOfColumns 代表操作阵列时的硬盘数量，5 代表着操作这块阵列时，始终要操作五个硬盘。** 可以按照下图所示模拟同时写入5块磁盘，当写入 100G 后，可以同时写入的硬盘就只剩4块了，不符合 NumberOfColumns 数量，剩下的空间都被废弃了。

![](https://assets.mitsea.cn/blog/posts/2023/10/Windows%20%E5%AD%98%E5%82%A8%E7%A9%BA%E9%97%B4%E4%B8%8D%E7%AD%89%E5%AE%B9%E9%87%8F%E7%A3%81%E7%9B%98%20Parity%20%E9%80%BB%E8%BE%91%E6%8E%A2%E7%A9%B6/Untitled%204.avif)

对于可以操作的这5个100G，可以得出如果 NumberOfColumns 为 5，再去掉一盘冗余，可用空间只有 400G。可以通过命令行创建时指定 `-NumberOfColumes 5` 测试，发现确实是。

![](https://assets.mitsea.cn/blog/posts/2023/10/Windows%20%E5%AD%98%E5%82%A8%E7%A9%BA%E9%97%B4%E4%B8%8D%E7%AD%89%E5%AE%B9%E9%87%8F%E7%A3%81%E7%9B%98%20Parity%20%E9%80%BB%E8%BE%91%E6%8E%A2%E7%A9%B6/Untitled%205.avif)

为了加强对于 NumberOfColumns 的理解，我们来看下如果是 4 应该怎么计算可用空间。首先 NumberOfColumns 为 4 意味着操作阵列时始终要调动起4块硬盘。这里模拟多次写入4盘，每次每盘写入100G的场景

![](https://assets.mitsea.cn/blog/posts/2023/10/Windows%20%E5%AD%98%E5%82%A8%E7%A9%BA%E9%97%B4%E4%B8%8D%E7%AD%89%E5%AE%B9%E9%87%8F%E7%A3%81%E7%9B%98%20Parity%20%E9%80%BB%E8%BE%91%E6%8E%A2%E7%A9%B6/Untitled%206.avif)

写入了三次，去掉冗余空间，一共大约是 3×(400-100)=900G

如果 NumberOfColumns 为 3，同样模拟类似操作，可以操作 5 次，一共大约是 5×(300-100)=1000G

![](https://assets.mitsea.cn/blog/posts/2023/10/Windows%20%E5%AD%98%E5%82%A8%E7%A9%BA%E9%97%B4%E4%B8%8D%E7%AD%89%E5%AE%B9%E9%87%8F%E7%A3%81%E7%9B%98%20Parity%20%E9%80%BB%E8%BE%91%E6%8E%A2%E7%A9%B6/Untitled%207.avif)

此时对于这种磁盘阵列，单盘冗余利用率最高

![](https://assets.mitsea.cn/blog/posts/2023/10/Windows%20%E5%AD%98%E5%82%A8%E7%A9%BA%E9%97%B4%E4%B8%8D%E7%AD%89%E5%AE%B9%E9%87%8F%E7%A3%81%E7%9B%98%20Parity%20%E9%80%BB%E8%BE%91%E6%8E%A2%E7%A9%B6/Untitled%208.avif)

那么，是不是 NumberOfColumns 为 3 对于任何情况下都可以取到最大可用容量呢？不一定的。这其实是一个简单的线性规划问题，我们可以直接借助 ChatGPT 帮我们将上面的操作抽象为 Python 代码，假设我们有 1+2+3+4+5+6 6块磁盘，NumberOfColumns 为 4，代码如下

```python
# 初始值
numbers = {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5, 'f': 6}

# 函数执行一次减法操作
def subtract_one(numbers):
    # 找出可以减1的四个最大数
    sorted_numbers = sorted(numbers.items(), key=lambda item: item[1], reverse=True)
    print(sorted_numbers)
    for i in range(4):
        # 如果最大的四个数中有0，则不能执行减法操作
        if sorted_numbers[i][1] == 0:
            return False, numbers
        # 减1操作
        numbers[sorted_numbers[i][0]] -= 1
    return True, numbers

# 执行减法操作直到不能再执行为止
count = 0
while True:
    can_subtract, numbers = subtract_one(numbers)
    if not can_subtract:
        break
    count += 1

print(count)
```

执行这段代码，输出结果如下

```bash
[('f', 6), ('e', 5), ('d', 4), ('c', 3), ('b', 2), ('a', 1)]
[('f', 5), ('e', 4), ('d', 3), ('b', 2), ('c', 2), ('a', 1)]
[('f', 4), ('e', 3), ('c', 2), ('d', 2), ('a', 1), ('b', 1)]
[('f', 3), ('e', 2), ('a', 1), ('b', 1), ('c', 1), ('d', 1)]
[('f', 2), ('c', 1), ('d', 1), ('e', 1), ('a', 0), ('b', 0)]
[('f', 1), ('a', 0), ('b', 0), ('c', 0), ('d', 0), ('e', 0)]
5
```

也就是说我们可以获得 5×(400-100)=1500GB 可用空间。然而，当把 `range(4)` 改成 `range(3)`

结果为

```bash
[('f', 6), ('e', 5), ('d', 4), ('c', 3), ('b', 2), ('a', 1)]
[('f', 5), ('e', 4), ('c', 3), ('d', 3), ('b', 2), ('a', 1)]
[('f', 4), ('d', 3), ('e', 3), ('b', 2), ('c', 2), ('a', 1)]
[('f', 3), ('b', 2), ('c', 2), ('d', 2), ('e', 2), ('a', 1)]
[('d', 2), ('e', 2), ('f', 2), ('a', 1), ('b', 1), ('c', 1)]
[('a', 1), ('b', 1), ('c', 1), ('d', 1), ('e', 1), ('f', 1)]
[('d', 1), ('e', 1), ('f', 1), ('a', 0), ('b', 0), ('c', 0)]
[('a', 0), ('b', 0), ('c', 0), ('d', 0), ('e', 0), ('f', 0)]
7
```

可用空间仅有 7×(300-100)=1400GB 可用空间，实际也确实如此

![](https://assets.mitsea.cn/blog/posts/2023/10/Windows%20%E5%AD%98%E5%82%A8%E7%A9%BA%E9%97%B4%E4%B8%8D%E7%AD%89%E5%AE%B9%E9%87%8F%E7%A3%81%E7%9B%98%20Parity%20%E9%80%BB%E8%BE%91%E6%8E%A2%E7%A9%B6/Untitled%209.avif)

这是因为 NumberOfColums 越小，每次用于冗余的百分比就越大，所以需要根据磁盘实际情况选择合适的 NumberOfColums 才能最大化可用空间。

改写上面代码后，可以找到适合当前情况的最佳 NumberOfColumes。修改 numbers 字段为实际硬盘信息即可

```python
def subtract_one(numbers, columns):
    # 找出可以减1的最大数
    sorted_numbers = sorted(numbers.items(), key=lambda item: item[1], reverse=True)
    for i in range(columns):
        # 如果最大的数中有0，则不能执行减法操作
        if sorted_numbers[i][1] == 0:
            return False, numbers
        # 减1操作
        numbers[sorted_numbers[i][0]] -= 1
    return True, numbers

# 初始值
numbers = {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5, 'f': 6}

# 可变的columns范围为3到numbers的数量-1
max_space = 0
best_columns = 0
for columns in range(3, len(numbers)):
    temp_numbers = numbers.copy()  # 复制原始数据以便重用
    count = 0
    while True:
        can_subtract, temp_numbers = subtract_one(temp_numbers, columns)
        if not can_subtract:
            break
        count += 1

    # 计算当前columns下的space
    space = count * (columns - 1)
    if space > max_space:
        max_space = space
        best_columns = columns

print(best_columns)
print(max_space)
```

> Photo by [Jigar Panchal](https://unsplash.com/@brave4_heart?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-black-background-with-red-and-blue-lines-D_ivYIn4jWw?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
  