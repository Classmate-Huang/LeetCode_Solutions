# LeetCode28：实现strStr

## 题目

实现 strStr() 函数。

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。

示例 1:

> 输入: haystack = "hello", needle = "ll"
> 输出: 2

示例 2:

> 输入: haystack = "aaaaa", needle = "bba"
> 输出: -1

说明:

> 当 needle 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 needle 是空字符串时我们应当返回 0 。这与C语言的 strstr() 以及 Java的 indexOf() 定义相符。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/implement-strstr

## 解法一

利用C++的`substr()`进行一次遍历

```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        int len_h = haystack.size(), len_n = needle.size();
        if(len_n==0) return 0;
        for(int i=0; i<len_h-len_n+1; i++){
            string s=haystack.substr(i,len_n);
            if(s==needle) return i;
        }
        return -1;
    }
};
```

时间复杂度：O(n)，未考虑字符匹配和子串提取的时间复杂度

空间复杂度：O(1)

## 解法二

经典字符串匹配算法KMP，其重点在于构造KMP算法，关于KMP可以看我这篇博客[暴搜 / KMP 详解](https://blog.csdn.net/qq_36560894/article/details/105210736)。代码如下：

```cpp
class Solution {
public:
    void get_next(vector<int> & next, string t){	// 构建next数组
        next.push_back(-1);
        for(int j=1; j<t.size(); j++){
            int k=next[j-1];
            while(k!=-1 && t[j-1]!=t[k]){
                k=next[k];
            }
            next.push_back(k+1);
        }
    }
    int find_str(string s, string t){ // KMP匹配
        vector<int> next;
        get_next(next, t);
        int s_n=s.size(), t_n=t.size(), i=0, j=0;
        while(i<s_n && j<t_n){
            while(j!=-1 && s[i]!=t[j]){
                j=next[j];
            }
            i++; j++;
        }
        if(j!=t_n) return -1;
        return i-j;
    }
    int strStr(string haystack, string needle) {
        int len_h = haystack.size(), len_n = needle.size();
        if(len_n==0) return 0;
        return find_str(haystack,needle);
    }
};
```

时间复杂度：O(m+n)，m,n为两个字符串的长度

空间复杂度：O(n)，next数组