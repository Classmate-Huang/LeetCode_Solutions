# LeetCode125：验证回文串

## 题目

给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

说明：本题中，我们将空字符串定义为有效的回文串。

示例 1:

> 输入: "A man, a plan, a canal: Panama"
> 输出: true

示例 2:

> 输入: "race a car"
> 输出: false

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/valid-palindrome
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一

很简单，利用双指针和一些内置API即可完成，代码如下：

```cpp
class Solution {
public:
    bool isPalindrome(string s) {
        int n=s.size();
        int left=0, right=n-1;
        while(left<=right){
            while(!isalpha(s[right]) && !isdigit(s[right]) && right) right--;
            while(!isalpha(s[left]) && !isdigit(s[left]) && left<n) left++;
            if(left<=right && toupper(s[left])!=toupper(s[right])) return false;
            left++;right--;
        }
        return true;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)