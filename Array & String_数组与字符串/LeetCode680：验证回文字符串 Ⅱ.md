# LeetCode680：验证回文字符串 Ⅱ

## 题目

给定一个非空字符串 `s`，**最多**删除一个字符。判断是否能成为回文字符串。

**示例 1:**

```
输入: "aba"
输出: True
```

**示例 2:**

```
输入: "abca"
输出: True
解释: 你可以删除c字符。
```

**注意:**

1. 字符串只包含从 a-z 的小写字母。字符串的最大长度是50000。

## 解法一：两次遍历

使用双指针判断是否为回文串，最多只能删除一个字符，那么遇到`s[left]!=s[right]`时有两种做法：① 忽略`s[left]` : right不动，移动left；② 忽略`s[right]` : left不动，移动right。如果①②处理后，有一种情况满足回文串，则返回true，否则返回false。代码如下：

```c++
class Solution {
public:
    bool validPalindrome(string s) {
        int n = s.size();
        // 处理方式① Left ++ 
        int left = 0, right = n-1, dis = 0;	// dis用于判断s[left]与s[right]不相等的次数
        while(left<right){
            if(s[left]==s[right]){
                left++; right--;
            }
            else{
                left++; dis++;
            }
            if(dis>=2)  break;   
        }
        if(dis<2)   return true;
        // 处理方式② Right -- 
        left = 0, right = n-1, dis = 0;
        while(left<right){
            if(s[left]==s[right]){
                left++; right--;
            }
            else{
                right--; dis++;
            }
            if(dis>=2)  break;   
        }
        if(dis<2)   return true;

        return false;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)

>  参考官方题解优化代码：

```c++
class Solution {
public:
    bool checkpalindrome(string s, int left, int right){	// 判断字符串s的子串[left,right]是否回文
        while(left<right){
            if(s[left]!=s[right])   return false;
            left++; right--;
        }
        return true;
    }

    bool validPalindrome(string s) {
        int n = s.size();
        int left = 0, right = n-1;
        while(left<right){
            if(s[left]!=s[right])	// 出现一次不相等情况
                return checkpalindrome(s, left+1, right) || checkpalindrome(s, left, right-1);
            left++; right--;
        }
        return true;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)