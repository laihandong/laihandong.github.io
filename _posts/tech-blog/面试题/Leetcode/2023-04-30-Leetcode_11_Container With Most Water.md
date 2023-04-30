### 思路一（已验证：超出时间限制）

双重循环，遍历所有组合，保存最大值，然后返回



### 官方题解（双指针之短板移动策略）

由题意可得，面积公式：`S(i,j)=min(h[i],h[j])×(j−i)`

其中，i，j为两指针，初始设置为数组的两端，相向遍历直至相遇

相向遍历的过程中，有两点是确定的，一是水槽宽度是递减的；二是长板指针的向内移动只会导致水槽的短板变得更小了或者不变，而且水槽面积一定变小了；三是短板指针的向内移动如果导致短板增大，那么面积可能增大

确定了这三点，那么中间很多状态是可以跳过的，直至将保存的最大面积返回


```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int i = 0, j = height.size() - 1, res = 0;
        while(i < j) {
            res = height[i] < height[j] ? 
                max(res, (j - i) * height[i++]): 
                max(res, (j - i) * height[j--]); 
        }
        return res;
    }
};

作者：Krahets
链接：https://leetcode.cn/problems/container-with-most-water/solutions/11491/container-with-most-water-shuang-zhi-zhen-fa-yi-do/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

