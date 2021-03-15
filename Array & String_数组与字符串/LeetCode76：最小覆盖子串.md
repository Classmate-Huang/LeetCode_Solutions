# LeetCode76：最小覆盖子串

[toc]

## 题目

给你一个字符串 `s` 、一个字符串 `t` 。返回 `s` 中涵盖 `t` 所有字符的最小子串。如果 `s` 中不存在涵盖 `t` 所有字符的子串，则返回空字符串 `""` 。

**注意：**如果 `s` 中存在这样的子串，我们保证它是唯一的答案。

 

**示例 1：**

```
输入：s = "ADOBECODEBANC", t = "ABC"
输出："BANC"
```

**示例 2：**

```
输入：s = "a", t = "a"
输出："a"
```

**提示：**

- `1 <= s.length, t.length <= 105`
- `s` 和 `t` 由英文字母组成

**进阶：**你能设计一个在 时间内解决此问题的算法吗？



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/minimum-window-substring
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一：滑动窗口

根据题意，首先需要确定字符串`t`中各出现的频次`tFreq`，并用数组保存下来(也可以用哈希表)。

接着构造滑动窗口`[left, right)`，判断窗口内`s`的子串是否满足要求(字母出现的频次`sFreq`≥`tFreq`)。为了高效地判断是否满足要求，这里利用`distance`来判断窗口内子串与`t`之间的差距，初始化为`t`的长度 (参考自**汉明距离**)。

值得注意的地方：① `sFreq`和`tFreq`初始化为长度64的int数组，因为字符串为纯字母，最小`A`的ASCII码为65，最大的`z`为122，因此数组长度为64即可。 ② 滑动窗口先移动right，当distance==0时再固定right，移动left。

具体参考代码，如下：

```c++
class Solution {
public:
    string minWindow(string s, string t) {
        int sLen = s.size(), tLen = t.size();
        int distance = tLen, left = 0, right = 0, minLen = sLen+1, begin = 0;
        int sFreq[64] = {0}, tFreq[64] = {0};

        // 得到tFreq
        for(auto i:t)   tFreq[i-'A']++;

        while(right<sLen){
            // 先移动右边界
            if(distance>0){
                if(sFreq[s[right]-'A'] < tFreq[s[right]-'A']){
                    distance--;
                }
                sFreq[s[right]-'A']++;
                right++;
            }
            // 满足要求移动右边界
            while(distance==0){
                if(right-left<minLen){
                    begin = left;
                    minLen = right-left;
                }
                sFreq[s[left]-'A']--;
                if(sFreq[s[left]-'A'] < tFreq[s[left]-'A']){
                    distance++;
                }
                left++;
            }
        }

        if(minLen==sLen+1)  return "";	
        return s.substr(begin, minLen);
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)