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