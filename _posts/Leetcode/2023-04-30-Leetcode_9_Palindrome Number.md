---
title: 9 Palindrome Number
date: 2023-04-30 08:00:00
categories:
- [Leetcode,题解]
---



### 解法一（未验证）

将数字转为字符串，获取长度，相向遍历，直到`j < i`或者中途有不等的情况，否则返回true，其余则返回false


### 解法二(验证偏差)

先判断是否为`-2^31`，是的话返回false；逐位整除遍历数字，获取位数。首位以整除、求余的操作，比较最高位和最低位的数字，相同则取中间的数，当成参数递归本函数，直到中间的数不存在或者为1位则返回递归，中途存在不等的情况立即返回false

李奶奶~我没注意审题，写成了(不考虑正负时)判断是否对称了
```c++
class Solution {

public:

    bool isPalindrome(int x) {

        if (x == -pow(2, 31)) { return false; }

        return compareHL(abs(x));

    }

    bool compareHL(int absX) {

        if (absX < 10) { return true; }

        int x_len = 1;

        while ((absX / (int)(pow(10, x_len))) > 0) { x_len++; }

  

        int highest = absX / (int)(pow(10.0, x_len - 1));

        int lowest = absX % 10;

        if (highest == lowest) {

            int nextAbsX = (absX - highest * (int)pow(10.0, x_len - 1) - lowest) / 10;

            return compareHL(nextAbsX);

        }

        else { return false; }

    }

};
```


### 解法三

