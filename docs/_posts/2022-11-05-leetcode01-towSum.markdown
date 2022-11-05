---
layout: post
title: "leetcode01-towSum"
date: 2022-11-05 17:17:00 +0000
categories: 算法题
---

## [1]twoSum

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

```c++
/*
    pass
    暴力匹配：
        时间O(n2)
        空间O(1)
        数组长n，遍历所有组合n(n+1)/2
*/
class Solution {
public:
    Solution() {}
    vector<int> twoSum1(vector<int>& nums, int target) {
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                if (nums[i] + nums[j] == target) {
                    return {i, j};
                }
            }
        }
        return {};
    }
};



/*
    pass
    哈希表：
        时间O(n)
        空间O(n)
        在初始化哈希表时，也顺便找与之对应的组合。键：唯一的数，值：数的下标
*/
class Solution {
public:
    Solution() {}
    vector<int> twoSum2(vector<int>& nums, int target) {
        unordered_map<int, int> hashtable;
        for (int i = 0; i < nums.size(); ++i) {
            auto it = hashtable.find(target - nums[i]);
            if (it != hashtable.end()) {
                return {it->second, i};
            }
            hashtable[nums[i]] = i;
        }
        return {};
    }


};
```

```c#
//方法一：暴力法
public int[] TwoSum(int[] nums, int target)
{
    for (int i = 0; i < nums.Length; i++)
    {
        for (int j = i + 1; j < nums.Length; j++)
        {
            if (nums[i] + nums[j] == target)
            {
                return new int[] { i, j };
            }
        }
    }
    return new int[] { 0, 0 };
}




//方法二：两遍哈希表
public int[] TwoSum(int[] nums, int target)
{
    Dictionary<int, int> kvs = new Dictionary<int, int>();
    for (int i = 0; i < nums.Length; i++)
    {
        //需要对重复值进行判断；因为结果的唯一，所以若有重复值，且答案中包含了重复值的话，说明必有 重复值*2==target; 否则直接忽略重复值即可
		//第一次迭代中，我们将每个元素的值和它的索引添加到表中
        if (kvs.ContainsKey(nums[i]))
        {
            if (nums[i] * 2 == target)
            {
                return new int[] { i, kvs[nums[i]] };
            }
        }
        else
        {
            kvs.Add(nums[i], i);
        }
    }
    //在第二次迭代中，我们将检查每个元素所对应的目标元素（target - nums[i]）是否存在于表中
    for (int i = 0; i < nums.Length; i++)
    {
    	int complement = target - nums[i];
        if (kvs.ContainsKey(complement) && kvs[complement] != i)
        {
            return new int[] { i, kvs[complement] };
        }
    }
    return new int[] { 0, 0 };
}


作者：DaWuFanCn
链接：https://leetcode.cn/problems/two-sum/solution/leetcode-1-two-sum-liang-shu-zhi-he-c-ha-xi-biao-d/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
