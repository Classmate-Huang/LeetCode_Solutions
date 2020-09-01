# LeetCode66：加一

## 题目
给定一个由整数组成的非空数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

示例 1:

> 输入: [1,2,3]
> 输出: [1,2,4]
> 解释: 输入数组表示数字 123。

示例 2:

> 输入: [4,3,2,1]
> 输出: [4,3,2,2]
> 解释: 输入数组表示数字 4321。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/plus-one

## 解法一
先从数组的最后一个元素开始加1，判断是否大于9，如果大于9就置零，再给前一位+1。但这里要考虑一种特殊情况，例如：[9,9]就需要在首部添加一个元素变成[1,0,0]，代码如下

```cpp
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        for(int i=digits.size()-1; i>=0; i--){
            digits[i] += 1;
            if(digits[i] < 10)  return digits;
            digits[i] = 0;
        }
        digits.insert(digits.begin(), 1);
        return digits;
    }
};

```

时间复杂度：O(n)

空间复杂度：O(1)
