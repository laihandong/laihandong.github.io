---
title: lambda 基本概念
date: 2022-12-02 08:00:00
categories: 
- [Python]
---



# 简介

+ lambda 是一种小的匿名函数
+ lambda 这个函数可以接受**任意数量**的参数，但**只能有一个**表达式，形势如下：
    ```
    lambda arguments : expression  # 理解了这个基本就能对付一半以上的lambda使用场景
    ```

# 结合其他python特性

## for...in...if
```
# 解析字符串-获取html的名字即可
test = "18011403-lhd-report_1.html,18011403-lhd-report_2.html"

get_html_name = lambda test_str : [x.split('-')[2] for x in test_str.split(',')]

print(get_html_name(test))  # output:
                            # ['report_1.html', 'report_2.html']
```
