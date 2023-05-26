---
title: 8 String to Integer (atoi)
date: 2023-04-30 08:00:00
categories:
- [Leetcode,题解]
---



### 解法一（已验证）

使用分支来处理每一种情况。首先明白所有字符都是有被忽略的可能的（甚者还能中断函数），其中时机和返回值是关键。暂时称有效区间为W，分支如下：
W前的`空格 `全部忽略
W前的字母，立即中断，返回当前结果
`+ - 数字`都标志着，进入W
W中的所有除数字外的，立即中断，返回当前结果。结果的每次赋值前，都应被范围` [-2^31, 2^31 - 1]`截断

```c++
class Solution {

public:

    int myAtoi(string s) {

        bool positive = true;

        bool numberSec = false;

        int rst = 0;

        for (int i = 0; i < s.length(); i++) {

            char ch = s[i];

            switch (ch)

            {

            case '+':

                if (!numberSec) {

                    numberSec = true;

                }else { return positive ? rst : -rst; }

                break;

            case '-':

                if (!numberSec) {

                    numberSec = true;

                    positive = false;

                }

                else { return positive ? rst : -rst; }

                break;

            case '0':

            case '1':

            case '2':

            case '3':

            case '4':

            case '5':

            case '6':

            case '7':

            case '8':

            case '9':

            {

                numberSec = true;

                int tt = '1' - '0';

                int chNum = (ch - '0');

                long long int rst_tmp = static_cast<long long int>(rst) * 10 + chNum;

                if (positive && rst_tmp >= (pow(2, 31) - 1)) { return (pow(2, 31) - 1); }

                else if (!positive && rst_tmp >= (pow(2, 31))) { return -(pow(2, 31)); }

                else { rst = rst * 10 + chNum; }

                break;

            }

            case ' ':

                if (!numberSec) { break; }

                else { return positive ? rst : -rst; }

            default:

                return positive ? rst : -rst;

            }

        }

        return positive ? rst : -rst;

    }

};
```
