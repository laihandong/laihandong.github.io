---
title: 5 Longest Palindromic Substring
date: 2023-04-30 08:00:00
categories:
- [Leetcode,题解]
---



#### 题目

给你一个字符串 `s`，找到 `s` 中最长的回文子串。

 

**示例 1：**

```
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
```

**示例 2：**

```
输入：s = "cbbd"
输出："bb"
```

 

**提示：**

- `1 <= s.length <= 1000`
- `s` 仅由数字和英文字母组成



#### 第一次尝试

##### 初步理解

题目要求从字符串中找子集，子集的特性是对称，可以从子集的特点出发，求解

子集的特点：

+ 对称（不论奇偶）
+ 连续（使子集长度和可能的数量能够*对应*）：比如`s`长度为10，子集长度如果为9，那么可能的数量为 2，在这两种情况中求解
+ 最大长度就是`s`本身，最小长度的情况就是单个字符



##### 代码

```c++\
class Solution {
public:
    string longestPalindrome(string s) {
        //穷举法：从最长的子集长度开始，判断每一种可能性
        uint len_s = s.length();
        for (uint len_assumed = len_s; len_assumed >= 1; len_assumed--) {
            for ( uint number_of_results = len_s + 1 - len_assumed; number_of_results >= 1; number_of_results--) {
            	
                for ( uint section_index_start = number_of_results - 1; section_index_start >= 0; section_index_start--) {
                    uint iter_index_start = section_index_start, iter_index_end = section_index_start + len_assumed - 1;
                    while (iter_index_start <= iter_index_end) {
                        if (s[iter_index_start] == s[iter_index_end]) { 
                            iter_index_start++; iter_index_end--; 
                        } else {
                            break;
                        }
                    }
                    if (iter_index_start > iter_index_end) { 
                        return s.substr(section_index_start, len_assumed); 
                    }
                }
                
            }
            
        }
        return s.substr(0,1);
    }
};
```

##### 提交情况

【bug】给索引使用`unsigned int`时，当为0且进行`--`操作后，会等于一个很大的正数：4294967295，而不是-1，于是会报错；

【未通过】提示超出时间限制；

【效率】时间复杂度O(n^3^)，空间复杂度O(1)

##### 优化方法

```c++
class Solution {
public:
    string longestPalindrome(string s) {
        //穷举法
        unsigned int len_s = s.length();
        for ( int len_assumed = len_s; len_assumed >= 1; len_assumed--) {
            for ( int number_of_results = len_s + 1 - len_assumed, section_index_start = 0; number_of_results >= 1; number_of_results--, section_index_start++) {
                int iter_index_start = section_index_start, iter_index_end = section_index_start + len_assumed - 1;
                while (iter_index_start <= iter_index_end) {
                    if (s[iter_index_start] == s[iter_index_end]) {
                        iter_index_start++; iter_index_end--;
                    }
                    else {
                        break;
                    }
                }
                if (iter_index_start > iter_index_end) {
                    return s.substr(section_index_start, len_assumed);
                }
            }
        }
        return s.substr(0, 1);
    }
};
```

【优化】将第三层循环给融合进第二层循环中了

#### 他人解法
