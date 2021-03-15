# LeetCode540：有序数组中的单一元素

## 题目

给定一个只包含整数的有序数组，每个元素都会出现两次，唯有一个数只会出现一次，找出这个数。

**示例 1:**

```
输入: [1,1,2,3,3,4,4,8,8]
输出: 2
```

**示例 2:**

```
输入: [3,3,7,7,10,11,11]
输出: 10
```

**注意:** 您的方案应该在 O(log n)时间复杂度和 O(1)空间复杂度中运行。

## 解法一：二分

看到O(log n)的时间复杂度优先考虑二分，对于本题我们首先应该明确的一点是数组的长度一定是`2n+1`。将数组二分后考虑成三个部分，长度为`N,1,N`，分两种情况讨论：

① `nums[mid]==nums[mid+1]`，再分两种情况讨论：a) N为奇数，此时目标值在`[left, mid-1]`之间；b) N为偶数，此时目标值在`[mid+2, right]`之间。

② `nums[mid]==nums[mid-1]`，同上分两种情况讨论：a）N为奇数，此时目标值在`[mid+1, right]`之间；b) N为偶数，此时目标值在`[left, mid-2]`之间。

> 两种情况，如果不理解可以自行举例。

如果①②均不满足，说明`nums[mid]`就是目标值。

通过以上思路，不难写出代码，如下：

```c++
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {
        int n = nums.size();
        int left = 0, right = n-1, mid;

        while(left<right){
            mid = (right-left)/2+left;
            if(nums[mid]==nums[mid+1]){
                if((right-mid)%2==0)    left=mid+2;
                else    right=mid-1;
            }
            else if(nums[mid]==nums[mid-1]){
                if((right-mid)%2==0)    right=mid-2;
                else    left=mid+1;
            }
            else    return nums[mid];
        }

        return nums[left];
    }
};
```

时间复杂度：O(logn)

空间复杂度：O(1)