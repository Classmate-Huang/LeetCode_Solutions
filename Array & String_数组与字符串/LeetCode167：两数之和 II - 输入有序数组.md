# LeetCode167：两数之和 II - 输入有序数组

[toc]

## 题目

给定一个已按照***升序排列\*** 的有序数组，找到两个数使得它们相加之和等于目标数。

函数应该返回这两个下标值 index1 和 index2，其中 index1 必须小于 index2*。*

**说明:**

- 返回的下标值（index1 和 index2）不是从零开始的。
- 你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素。

**示例:**

```
输入: numbers = [2, 7, 11, 15], target = 9
输出: [1,2]
解释: 2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。
```

## 解法一：双指针

由于输入数列有序，那么构造双指针一个指针指向数组尾部，一个指向数组头部。当两个指针加起来的值小于目标值时，左指针向右走，大于目标值时，右指针向左走，直到找到满足要求的数据。代码如下：

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int n=numbers.size();
        int left = 0, right = n-1;
        vector<int> ans;

        while(left<right){
            int t = numbers[left]+numbers[right];
            if(t==target){
                ans.push_back(left+1);
                ans.push_back(right+1);
                break;
            }
            else if(t<target)   left++;
            else right--;
        }

        return ans;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)