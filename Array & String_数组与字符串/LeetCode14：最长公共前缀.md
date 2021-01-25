# LeetCode14：最长公共前缀

## 题目

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

示例 1:

> 输入: ["flower","flow","flight"]
> 输出: "fl"

示例 2:

> 输入: ["dog","racecar","car"]
> 输出: ""
> 解释: 输入不存在公共前缀。

说明:

> 所有输入只包含小写字母 a-z 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-common-prefix
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一

纵向比较，第一轮比较所有`string`的第1个字符是否相等，然后第i轮比较第i个字符是否相当。一直比较下去，直到出现不相等的情况或者越界的情况就停止遍历，返回结果。

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        int n=strs.size();
        if(n==0) return "";
        for(int i=0; i<strs[0].size(); i++){
            char c=strs[0][i];
            for(auto s:strs){
                if(i==s.size() || c!=s[i]){
                    return strs[0].substr(0,i);
                }
            }
        }
        return strs[0];
    }
};
```



## 解法二

最先想到的思路就是一个个接着比较，比如有三个字符串a,b,c，先找到a和b的最长公共前缀s1，再找到s1和c的最长公共前缀。

```cpp
string longestCommonPrefix(vector<string>& strs) {
        if(strs.empty())    return "";	// 如果为空
        if(strs.size()==1)  return strs[0];	//如果只有一个字符串
        int len=strs[0].size();
        for(int i=1; i<strs.size(); i++){
            while(strs[i].substr(0,len)!=strs[0].substr(0,len)) len--;
        }
        return strs[0].substr(0,len);
    }
```
**时间复杂度：**
时间复杂度：O(N + m)，其中N是strs数组的长度，m是strs[0]第一个字符串的长度。(未考虑API时间复杂度)
空间复杂度：O(1)。

