# LeetCode154：寻找旋转排序数组中的最小值 II

## 题目

假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 `[0,1,2,4,5,6,7]` 可能变为 `[4,5,6,7,0,1,2]` )。

请找出其中最小的元素。

注意数组中可能存在重复的元素。

**示例 1：**

```
输入: [1,3,5]
输出: 1
```

**示例 2：**

```
输入: [2,2,2,0,1]
输出: 0
```

**说明：**

- 这道题是 [寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/description/) 的延伸题目。
- 允许重复会影响算法的时间复杂度吗？会如何影响，为什么？

## 解法一：二分

此题与33、81是同一个系列，主要考察二分法的应用，如果熟悉前两个题做法的话，此题并不复杂，并没有达到`Hard`难度。

首先二分将数组一分为二，`[Left, Mid]`与`[Mid, Right]`，与之前不同，本题不是匹配目标值而是找寻最小值，因此区间划分也不一样，这是为了防止`nums[Mid]`的比较被遗漏。

判断数组是否有序，这部分与81题一致：① 当`nums[left]==nums[mid] && left<mid`时，left++；② 接着判断是否满足`nums[left]≥nums[mid]`，如果满足说明[Left, Mid]为有序数组，反之[Mid, Right]有序。

判断哪部分数组有序后，利用有序数组进行排除，假设左半部数组有序，那么只需要判断`min(ans, nums[left])`即可，然后下一阶段搜索[mid+1, right]；反之右半部数组有序同理。

代码如下：

```c++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int n = nums.size();
        int left = 0, right = n-1, mid;
        int ans = INT_MAX;

        while(left<=right){
            mid = (right-left)/2+left;
            // if nums[left]==nums[mid]
            while(nums[left]==nums[mid] && left<mid){
                left++;
            }
            // if Left Array is ordered
            if(nums[mid]>=nums[left]){
                ans = min(ans, nums[left]);
                left = mid+1;
            }
            // if Right Array is ordered
            else{
                ans = min(ans, nums[mid]);
                right = mid-1;
            }
        }

        return ans;
    }
};
```

时间复杂度：O(logn) 【平均】

空间复杂度：O(1)

> 最坏情况下，时间复杂度退化到O(n)级别