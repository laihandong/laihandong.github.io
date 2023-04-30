
### 思路一（未验证）

同时从头遍历两个字符串，浮标右移的条件是，当前对应位置的元素成匹配状态

匹配状态分为三种:
1. 具体的字母，比如`a==a`
2. `.`对应任意，比如`a==.`
3. `*`对应n个相同字母（即上一次的匹配结果）组成的连续字符串`(n>=0)`，比如`a*==aaaaaaa`，或者`a*==a`

（对应上文的例子）对应的匹配结果：
1. 成功：结果为字母`a`；失败：结果为false
2. 成功：结果为字母`a`；失败：结果为false
3. 成功：结果为`null,a,aa,aaa,aaa...`中的一种；<s>失败：结果为false</s>（2/19，不是false，应该继续往下匹配）

过程当中，如果匹配失败，立即返回false。    


### 官方思路（自我总结）

建立状态矩阵，`Arr[i][j]`表示`s`字符串的前`i`个字符和`p`字符串的前`j`个字符的匹配状态

确定状态转移方程，![Pasted image 20230219164535](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202304302035429.webp)

+ 初始状态为`true`，即空空字符串是匹配的
+ 大意很清楚，其中关于`字符+*`的组合的状态转移是可以精简成：匹配则视为放弃s字符串的最后一个或者多次重复p字符串当前的`*`；不匹配时则视为放弃当前p字符串的`*`

```c++
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size();
        int n = p.size();

        auto matches = [&](int i, int j) {
            if (i == 0) {
                return false;
            }
            if (p[j - 1] == '.') {
                return true;
            }
            return s[i - 1] == p[j - 1];
        };

        vector<vector<int>> f(m + 1, vector<int>(n + 1));
        f[0][0] = true;
        for (int i = 0; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (p[j - 1] == '*') {
                    f[i][j] |= f[i][j - 2];
                    if (matches(i, j - 1)) {
                        f[i][j] |= f[i - 1][j];
                    }
                }
                else {
                    if (matches(i, j)) {
                        f[i][j] |= f[i - 1][j - 1];
                    }
                }
            }
        }
        return f[m][n];
    }
};

作者：力扣官方题解
链接：https://leetcode.cn/problems/regular-expression-matching/solutions/295977/zheng-ze-biao-da-shi-pi-pei-by-leetcode-solution/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

其中比如matches的定义涉及匿名函数有关知识，可以看[C++ 闭包和匿名函数 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/303391384)
