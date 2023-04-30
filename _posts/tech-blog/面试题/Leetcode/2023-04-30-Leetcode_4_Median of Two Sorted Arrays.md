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