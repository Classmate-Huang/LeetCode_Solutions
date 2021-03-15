# LeetCode81：搜索旋转排序数组 II

## 题目

假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 `[0,0,1,2,2,5,6]` 可能变为 `[2,5,6,0,0,1,2]` )。

编写一个函数来判断给定的目标值是否存在于数组中。若存在返回 `true`，否则返回 `false`。

**示例 1:**

```
输入: nums = [2,5,6,0,0,1,2], target = 0
输出: true
```

**示例 2:**

```
输入: nums = [2,5,6,0,0,1,2], target = 3
输出: false
```

**进阶:**

- 这是 [搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/description/) 的延伸题目，本题中的 `nums` 可能包含重复元素。
- 这会影响到程序的时间复杂度吗？会有怎样的影响，为什么？

## 解法一：二分

相比于`LeetCode33`题的搜索旋转排列数组，本题最大的区别就是可能存在多个相同值。从33题知道，将数组`nums`一分为二，必有一半是非递减序列(另一半可能是，也可能不是)，是不是地把对应的代码copy过来就可以了呢，显然是没那么简单的。

> 简要提示：33题将数组分为两部分**[left, mid]**与**[mid+1, right]**，在判断数组的两部分有序时，根据`nums[left]`与`nums[mid]`之间的关系进行判断，如果`nums[mid] > nums[left]`说明`[left,mid]`之间为有序数组，反之[mid+1, right]有序。根据有序数组进行排除：假设[left, mid]有序，判断target是否在区间内，如果不在区间内，下一轮则考虑[mid+1, right]；反之[mid+1, right]有序时一样。

① 考虑以下数组`[1,0,1,1,1], find 0`，`Left`,`Right`，`Mid`三个指针所指的值均为1，此时如何去判断那边有序呢？

​	这就需要额外引入一个步骤，移动left指针至`nums[left]!=nums[mid] || left==mid`，排除影响。

② 只考虑①就足够了吗？再考虑数组`[1,1,1,2,0], find 2`，经过步骤1，现在变为`[1,2,0]`，现在`mid`和`left`均指向1，`right`指向0，能根据`nums[mid] > nums[left]`来判断那部分有序吗？

​	显然`[mid+1, right]`[2,0]是无序的，因此我们要将判断条件改为`nums[mid] >= nums[left]`。

详细代码如下：

```c++
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int n = nums.size();
        int left = 0, right = n-1, mid;

        while(left<=right){
            mid = (right-left)/2+left;
            // nums[mid] is right?
            if(nums[mid]==target)
                return true;
            // if nums[left]==nums[mid]
            while(nums[left]==nums[mid] && left<mid)
                left++;
            // Left Array is ordered
            if(nums[mid] >= nums[left]){
                if(target>nums[mid] || target<nums[left]){
                    left = mid+1;
                }
                else{
                    right = mid-1;
                }
            }
            // Right Array is ordered
            else{
                if(target>nums[right] || target<nums[mid+1<right?mid+1:right]){	// 避免mid+1越界
                    right = mid-1;
                }
                else{
                    left = mid+1;
                }
            }
        }

        return false;
    }
};
```

时间复杂度：O(logn) 【平均】

空间复杂度：O(1)

> 并且最坏情况可能时间复杂度会退化到O(n)级别，例如`[1,1,1,1,1,1,1,0]，find 0`

