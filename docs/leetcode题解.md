layout: page
title: "Leetcode, one problem one day?"
permalink: /algorithms/leetcode

# Leetcode Problems

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



## [2]addTwoNumbers

给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。



```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */



/*
    Failed：long long int都表示不了
    [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1]
	[5,6,4]
	
    先把数读出来，相加后再存进链表
    时间O(n)
    空间O(n)
*/
class Solution {
public: 

    ListNode* addTwoNumbers1(ListNode* l1, ListNode* l2) {
        long long int x1 = 0, x2 = 0;
        for(int i = 0; l1 != nullptr || l2 != nullptr; i++){
            if(l1 != nullptr){
                x1 = x1 + l1->val * pow(10, i);
                l1 = l1->next;
                
            }
            if(l2 != nullptr){
                x2 = x2 + l2->val * pow(10, i);
                l2 = l2->next;
            }           
        }

        ListNode* tmpNode = new(ListNode);
        ListNode* rst = tmpNode;
        long long int y = x1 + x2;
        for(int i = 0; y != 0; i++){
            tmpNode->val = y%10;
            y = long double(y/10);
            if(y != 0){
                tmpNode->next = new(ListNode);
                tmpNode = tmpNode->next;
            }
        }

        return rst;
    }
};




/*
    对应的位相加，更新值和进位，更新遍历状态（重点）
    
    时间O(n)
    空间O(1)
    
*/
class Solution {
public: 
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int carryNumber = 0, tempL1Val = 0, tempL2Val = 0;
        ListNode *out1 = l1, *out2 = l2;
        bool rst[2] = {false, false};//代表对应的链表是否遍历完
        while(true){
            //取值
            if(!rst[0]) tempL1Val = l1->val; else tempL1Val = 0;
            if(!rst[1]) tempL2Val = l2->val; else tempL2Val = 0;
            //计算
            int tempSum = tempL1Val + tempL2Val + carryNumber;
            //更新节点的值
            carryNumber = tempSum / 10;
            if(!rst[0]) l1->val = tempSum % 10; 
            if(!rst[1]) l2->val = tempSum % 10; 
            //更新遍历的状态（重点）
            if(l1->next != nullptr) l1 = l1->next; else rst[0] = true;
            if(l2->next != nullptr) l2 = l2->next; else rst[1] = true;
            //是否跳出循环
            if(rst[0]&&rst[1]) break;
        }
        ListNode* tempNode = new(ListNode);
        if(carryNumber != 0) tempNode->val = carryNumber; else tempNode = nullptr;
        if(tempL1Val != 0){
            l1->next = tempNode;
            return out1;
        }
        else{
            l2->next = tempNode;
            return out2;
        }
    }
};
```

## [3]lengthOfLongestSubstring

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

**提示：**

- `0 <= s.length <= 5 * 104`
- `s` 由英文字母、数字、符号和空格组成

```c++
/*
在遍历字符串时，用哈希表存储字符和其下标，并判断是否重复，根据此更新最长子串状态和当前记录的子串状态
时间O(n)
空间O(1)：最多就是128个字符ASCII 
*/
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char, int> uniqueStr;
        int maxLength = 0, startOfString = 0, endOfString = 0, currentLength = endOfString - startOfString;
        for(int i = 0; i < s.length(); ++i) {
            endOfString = i;//当前子串结尾下标 
            if (uniqueStr.find(s[i]) != uniqueStr.end()) {//字符重复（已遍历过且加进了哈希表）
                if (uniqueStr[s[i]] >= startOfString) {//有效重复（重点1）
                    startOfString = uniqueStr[s[i]] + 1;//更新当前子串的开头
                    currentLength = endOfString - startOfString + 1;//更新当前长度
                }
                else {//无效重复
                    currentLength += 1;
                }
                uniqueStr[s[i]] = i;//更新重复字符的最新（近）下标（重点2）
            }
            else {
                uniqueStr[s[i]] = i;//第一次遍历到，则插入哈希表
                currentLength += 1;//更新当前长度
            }
            if (currentLength > maxLength) maxLength = currentLength;//更新最大长度
        }
        return maxLength;
    }
};
/*
简化上一次的提交：将字典存储变为固定128长度的数组，下标对应字符的ASCII码
重点3：字符没有遇到重复之前，读取到的都是初始值ascii[s[i]]=0，就不会影响更新子串的开头
重点4：第二次读取到同一字符，应该读取的是这个字符的下一位的下标，才能赋值给并更新子串开头
*/
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int ascii[128] = {0};
        int start = 0;//子串开头
        int maxLen = 0;
        for (int i = 0; i < s.length(); ++i) {
            start = max(start, ascii[s[i]]);//更新子串开头（重点3）
            maxLen = max(maxLen, i - start + 1);
            ascii[s[i]] = i + 1;//更新（重复）字符最新下标（重点4）
        }
        return maxLen;
    }
};
```

## [4]findMedianSortedArrays

给定两个大小分别为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。请你找出并返回这两个正序数组的 中位数 。

算法的时间复杂度应该为 O(log (m+n)) 



提示：

- nums1.length == m
- nums2.length == n
- 0 <= m <= 1000
- 0 <= n <= 1000
- 1 <= m + n <= 2000
- -106 <= nums1[i], nums2[i] <= 106



```c++
/*
递归法：用两个数组最中间的数进行比较，则必有一个数至少大于等于一半的数，另一个数至少小于等于一半的数，那么中位数一定是在这两个数表示的范围之间，如此递归可得。
*/
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int lenVec1 = nums1.size(), lenVec2 = nums2.size();
        if (lenVec1 % 2 == 0 && lenVec2 % 2 == 0) {
            return 14333.0;
        }
        double midNum = 0;//中位数
        int mid1 = (lenVec1 - 1) / 2, mid2 = (lenVec2 - 1) / 2;
        if (lenVec1 && lenVec2) {
            //1:两个数组均为奇数个
            if (lenVec1 % 2 == 1 && lenVec2 % 2 == 1) {
                if (nums1[mid1] >= nums2[mid2]) {
                    if (lenVec2 == 1) {
                        midNum = (nums2[mid2] + nums1[mid1]) / 2.;
                        return midNum; 
                    }
                    else {
                        midNum = (nums2[mid2] + std::min(nums1[mid1], nums2[mid2 + 1])) / 2.;
                        return midNum;
                    }
                }
                else {
                    if (lenVec1 == 1) {
                        midNum = (nums1[mid1] + nums2[mid2]) / 2.;
                        return midNum;
                    }
                    else {
                        midNum = (nums1[mid1] + std::min(nums2[mid2], nums1[mid1 + 1])) / 2.;
                        return midNum;
                    }
                }
            }
            //2:两个数组均为偶数
            else if (lenVec1 % 2 == 0 && lenVec2 % 2 == 0) {
                if (nums1[mid1] >= nums2[mid2]) {
                    if (lenVec1 == 2 && lenVec2 == 2) {
                        midNum = (nums1[mid1] + std::min(nums1[mid1 + 1], nums2[mid2 + 1])) / 2.;
                        return midNum;
                    }
                    else if (lenVec1 == 2 && lenVec2 > 2) {
                        double min1 = min(nums1[mid1], nums2[mid2 + 1], nums2[mid2 + 2]);
                        double min2 = sec_min(nums1[mid1], nums2[mid2 + 1], nums2[mid2 + 2]);
                        midNum = (min1 + min2) / 2.;
                        return midNum;
                    }
                    else if (lenVec1 > 2 && lenVec2 == 2){
                        double min1 = min(nums1[mid1], nums1[mid1 + 1], nums2[mid2 + 1]);
                        double min2 = sec_min(nums1[mid1], nums1[mid1 + 1], nums2[mid2 + 1]);
                        midNum = (min1 + min2) / 2.;
                        return midNum;
                    }
                    else {
                        double min1 = min_of_four(nums1[mid1], nums1[mid1 + 1], nums2[mid2 + 1], nums2[mid2 + 2]);
                        double min2 = sec_min_of_four(nums1[mid1], nums1[mid1 + 1], nums2[mid2 + 1], nums2[mid2 + 2]);
                        midNum = (min1 + min2) / 2.;
                        return midNum;
                    }
                }
                else {
                    if (lenVec1 == 2 && lenVec2 == 2) {
                        midNum = (nums2[mid2] + std::min(nums2[mid2 + 1], nums1[mid1 + 1])) / 2.0;
                        return midNum;
                    }
                    else if (lenVec1 == 2 && lenVec2 > 2) {
                        double min1 = min(nums2[mid2], nums2[mid2 + 1], nums1[mid1 + 1]);
                        double min2 = sec_min(nums2[mid2], nums2[mid2 + 1], nums1[mid1 + 1]);
                        midNum = (min1 + min2) / 2.;
                        return midNum;
                    }
                    else if (lenVec1 > 2 && lenVec2 == 2){
                        double min1 = min(nums2[mid2], nums1[mid1 + 1], nums1[mid1 + 2]);
                        double min2 = sec_min(nums2[mid2], nums1[mid1 + 1], nums1[mid1 + 2]);
                        midNum = (min1 + min2) / 2.;
                        return midNum;
                    }
                    else {
                        double min1 = min_of_four(nums2[mid2], nums2[mid2 + 1], nums1[mid1 + 1], nums1[mid1 + 2]);
                        double min2 = sec_min_of_four(nums2[mid2], nums2[mid2 + 1], nums1[mid1 + 1], nums1[mid1 + 2]);
                        midNum = (min1 + min2) / 2.;
                        return midNum;
                    }
                }
            }
            //3:两个数组为一偶一奇
            else if (lenVec1 % 2 == 0 && lenVec2 % 2 == 1) {
                if (nums1[mid1] >= nums2[mid2]) {
                    if (lenVec2 == 1) {
                        midNum = nums1[mid1];
                        return midNum;
                    }
                    else {
                        midNum = std::min(nums1[mid1], nums2[mid2 + 1]);
                        return midNum;
                    }
                }
                else {
                    if (lenVec1 == 1) {
                        midNum = nums2[mid2];
                        return midNum;
                    }
                    else {
                        midNum = std::min(nums2[mid2], nums1[mid1 + 1]);
                        return midNum;
                    }
                }
            }
            //4:两个数组为一奇一偶
            else {
                if (nums1[mid1] >= nums2[mid2]) {
                    midNum = std::min(nums1[mid1], nums2[mid2 + 1]);
                    return midNum;
                }
                else {
                    if (lenVec1 == 1) {
                        midNum = std::min(nums2[mid2], nums1[mid1 + 1]);
                        return midNum;
                    }
                    else {
                        midNum = nums2[mid2];
                        return midNum;
                    }
                }
            }
        }
        else if (lenVec1) {
            if (lenVec1 % 2 == 0) {
                midNum = (nums1[mid1] + nums1[mid1 + 1]) / 2.;
                return midNum;
            }
            else {
                midNum = nums1[mid1];
                return midNum;
            }
        }
        else {
            if (lenVec2 % 2 == 0) {
                midNum = (nums2[mid2] + nums2[mid2 + 1]) / 2.;
                return midNum;
            }
            else {
                midNum = nums2[mid2];
                return midNum;
            }
        }
    }
    double min(int a, int b, int c) {
        if (a <= b && a <= c) return a;
        else if (b <= a && b <= c) return b;
        else return c;
    }
    double sec_min(int a, int b, int c) {
        if (a <= b && a >= c) return a;
        else if (b <= a && b >= c) return b;
        else return c;
    }
    double min_of_four(int a, int b, int c, int d) {
        if (a <= b && a <= c && a <= d) return a;
        else if (b <= a && b <= c && b <= d) return b;
        else if (c <= a && c <= b && c <= d) return c;
        else return c;
    }
    double sec_min_of_four(int a, int b, int c, int d) {
        if (b >= a && b <= c && b <= d) return b;
        else if (c >= a && c <= b && c <= d) return c;
        else if (d >= a && d <= b && d <= d) return d;

        else if (a >= b && a <= c && a <= d) return a;
        else if (c >= b && c <= a && c <= d) return c;
        else if (d >= b && d <= a && d <= c) return d;

        else if (a >= c && a <= b && a <= d) return a;
        else if (b >= c && b <= a && b <= d) return b;
        else if (d >= c && d <= a && d <= b) return d;

        else if (a >= d && a <= b && a <= c) return a;
        else if (b >= d && b <= a && b <= c) return b;
        else return c;
    }
};
```

