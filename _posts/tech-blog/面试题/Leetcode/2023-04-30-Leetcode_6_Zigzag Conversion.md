将一个给定字符串 s 根据给定的行数 numRows ，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "PAYPALISHIRING" 行数为 3 时，排列如下：

```
P   A   H   N
A P L S I I G
Y   I   R
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："PAHNAPLSIIGYIR"。

请你实现这个将字符串进行指定行数变换的函数：

`string convert(string s, int numRows);`
 

示例 1：

输入：s = "PAYPALISHIRING", numRows = 3
输出："PAHNAPLSIIGYIR"
示例 2：
输入：s = "PAYPALISHIRING", numRows = 4
输出："PINALSIGYAHRPI"
解释：
```
P     I    N
A   L S  I G
Y A   H R
P     I
```

示例 3：

输入：s = "A", numRows = 1
输出："A"
 

提示：

+ 1 <= s.length <= 1000
+ s 由英文字母（小写和大写）、',' 和 '.' 组成
+ 1 <= numRows <= 1000

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/zigzag-conversion
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


### 解法一（未验证）

遍历字符串，将每个字符当成一个结点，以行数为周期，分别挂载到相应的节点后，最后按顺序依次读取结点链表。


### 解法二（未验证）

第一轮遍历字符串，以行数为离散函数的周期，分别给每个字符标注其所在的行数。然后第二轮遍历字符串，读取第一行的字符，重复此操作，直到读取完所有行的字符。


### 解法三

遍历字符串，以行数为离散函数的周期，计算出每个字符串应该在的位置，以此组成“位置：字符”存入map，重复此过程，直到遍历完字符串。将map按key增序排序，输出字符串。

若行数为n(n>1)，则对应的离散函数周期为`2n-1`，一周期内的元素个数为`2n-2`（周期为左闭右开/或者左开右闭）。此时，若元素为第`m`个(m>0)，那么它在第`m mod n`行的第`m/(2*n-2)+1`个


# 参考

法一：[0006. Zig Zag Conversion | LeetCode Cookbook (halfrost.com)](https://books.halfrost.com/leetcode/ChapterFour/0001~0099/0006.ZigZag-Conversion/)
大意：无关算法，重点在对程序控制的能力