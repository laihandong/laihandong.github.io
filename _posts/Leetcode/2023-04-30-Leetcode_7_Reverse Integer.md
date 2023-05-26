---
title: 7 Reverse Integer
date: 2023-04-30 08:00:00
categories:
- [Leetcode,题解]
---



给你一个 32 位的有符号整数 `x` ，返回将 `x` 中的数字部分反转后的结果。

如果反转后整数超过 32 位的有符号整数的范围 `[−231,  231 − 1]` ，就返回 0。

**假设环境不允许存储 64 位整数（有符号或无符号）。**

**示例 1：**

**输入：**x = 123
**输出：**321

**示例 2：**

**输入：**x = -123
**输出：**-321

**示例 3：**

**输入：**x = 120
**输出：**21

**示例 4：**

**输入：**x = 0
**输出：**0

**提示：**

-   `-231 <= x <= 231 - 1`


解法一（已验证）

常量：将2^31-1按位顺序存入数组Arr
首先计算传入数字的位数（因为有范围，所以是个常数），按位从高到低遍历数字，依次和Arr中的数字相比（没有的视为0），首先设置哨兵判断当前是否为最高位，然后比较。若不大于，则放大相应的倍数并加到（返回值）rst中，且小于时，将哨兵置为false，表示不是最高位。若大于，则立即返回0
```c++
class Solution {
public:
    int reverse(int x) {
        int symbol = x >= 0 ? 1 : -1;
        int arr[10] = { 2,1,4,7,4,8,3,6,4,7 };
        int x_arr[10] = { 0,0,0,0,0,0,0,0,0,0 };
        int x_len = 1;
        int x_tmp = abs(x);
        while (true) {
            int divisor = 10;
            int quo = x_tmp / divisor;
            int rem = x_tmp - quo * divisor;
            x_arr[x_len - 1] = rem;//确定比较范围
            if (quo == 0) { break; }
            else {
                x_tmp = quo;
                x_len += 1;
            }
        }
        int rst = 0;
        bool highest = true;
        for (int i = 0; i < 10; i++) {
            int ind = i - 10 + x_len;//确定对比范围
            int x_arr_tmp = ind >= 0 ? x_arr[ind] : 0;
            if (highest && x_arr_tmp > arr[i]) { return 0; }//作为最高位，若大于，则视为溢出，返回0
            else if (highest && x_arr_tmp == arr[i]) {
                rst += x_arr_tmp * pow(10, (9 - i));
            }
            else { 
                rst += x_arr_tmp * pow(10, (9 - i)); 
                highest = false;
            }
            
        }
        return symbol * rst;
    }
};

```




解法二（leetcode验证通过，但不符合题目假设）
遍历数字，将其按位放到数组中，然后依次取出叠加到结果rst中，与最大值绝对值比较，若大于，立即返回0，若小于，带上符号再返回rst
```c++
class Solution {
public:
    int reverse(int x) {
        int symbol = x >= 0 ? 1 : -1;
        int arr[10] = { 2,1,4,7,4,8,3,6,4,7 };
        int x_arr[10] = { 0,0,0,0,0,0,0,0,0,0 };
        int x_len = 1;
        int x_tmp = abs(x);
        while (true) {
            int divisor = 10;
            int quo = x_tmp / divisor;
            int rem = x_tmp - quo * divisor;
            x_arr[x_len - 1] = rem;//确定比较范围
            if (quo == 0) { break; }
            else {
                x_tmp = quo;
                x_len += 1;
            }
        }
        long long int rst = 0;
        
        for (int i = 0; i < 10; i++) {
            int ind = i - 10 + x_len;//确定对比范围
            int x_arr_tmp = ind >= 0 ? x_arr[ind] : 0;
            rst += x_arr_tmp * pow(10, (9 - i));
            if (rst > 2147483647) { return 0; }
        }
        return symbol * rst;
    }
};
```