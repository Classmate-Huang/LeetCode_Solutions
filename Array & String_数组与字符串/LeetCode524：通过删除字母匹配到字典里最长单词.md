# LeetCode524：通过删除字母匹配到字典里最长单词

## 题目

给定一个字符串和一个字符串字典，找到字典里面最长的字符串，该字符串可以通过删除给定字符串的某些字符来得到。如果答案不止一个，返回长度最长且字典顺序最小的字符串。如果答案不存在，则返回空字符串。

**示例 1:**

```
输入:
s = "abpcplea", d = ["ale","apple","monkey","plea"]

输出: 
"apple"
```

**示例 2:**

```
输入:
s = "abpcplea", d = ["a","b","c"]

输出: 
"a"
```

**说明:**

1. 所有输入的字符串只包含小写字母。
2. 字典的大小不会超过 1000。
3. 所有输入的字符串长度不会超过 1000。

## 解法一：双指针+遍历

直接使用双指针，将字符串`s`与字典中所有字符串比较一遍即可。代码如下：

```c++
class Solution {
public:
    string findLongestWord(string s, vector<string>& d) {
        int sLen = s.size();
        string ans = "";

        for(string t:d){
            int i = 0, j = 0, tLen = t.size();
            while(i<sLen && j<tLen){
                if(s[i]!=t[j])  i++;
                else{
                    i++; j++;
                }
            }
            if(j==tLen && (tLen>ans.size() || (tLen==ans.size() && t<ans)))
                ans = t;
        }

        return ans;
    }
};
```

时间复杂度：O(n<sup>2</sup>)

空间复杂度：O(1)