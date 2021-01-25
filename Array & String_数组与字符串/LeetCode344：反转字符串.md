# LeetCode344：反转字符串

## 题目

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组`char[]`的形式给出。

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。

你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符。

 

示例 1：

> 输入：["h","e","l","l","o"]
> 输出：["o","l","l","e","h"]

示例 2：

> 输入：["H","a","n","n","a","h"]
> 输出：["h","a","n","n","a","H"]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reverse-string

## 解法一
交换数组

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        int n = s.size();
        for(int t=0; n && t<n/2; t++){
            swap(s[t], s[n-t-1]);
        }
    }
};
```
时间复杂度：O(N)
空间复杂度：O(1)

## 解法二
双指针:用两个指针，分别指向数组首部和尾部。然后交换指向的数据，直到两指针相遇。

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        int i=0, j=s.size()-1;
        while(i<j){
            swap(s[i], s[j]);
            i++; j--;
        }
    }
};
```
时间复杂度：O(N)
空间复杂度：O(1)