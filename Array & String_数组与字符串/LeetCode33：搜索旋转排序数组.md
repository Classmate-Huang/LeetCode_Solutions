# LeetCode33：搜索旋转排序数组

## 题目

整数数组 `nums` 按升序排列，数组中的值 **互不相同** 。

在传递给函数之前，`nums` 在预先未知的某个下标 `k`（`0 <= k < nums.length`）上进行了 **旋转**，使数组变为 `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`（下标 **从 0 开始** 计数）。例如， `[0,1,2,4,5,6,7]` 在下标 `3` 处经旋转后可能变为 `[4,5,6,7,0,1,2]` 。

给你 **旋转后** 的数组 `nums` 和一个整数 `target` ，如果 `nums` 中存在这个目标值 `target` ，则返回它的索引，否则返回 `-1` 。

 

**示例 1：**

```
输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4
```

**示例 2：**

```
输入：nums = [4,5,6,7,0,1,2], target = 3
输出：-1
```

**示例 3：**

```
输入：nums = [1], target = 0
输出：-1
```

 

**提示：**

- `1 <= nums.length <= 5000`
- `-10^4 <= nums[i] <= 10^4`
- `nums` 中的每个值都 **独一无二**
- `nums` 肯定会在某个点上旋转
- `-10^4 <= target <= 10^4`

 

**进阶：**你可以设计一个时间复杂度为 `O(log n)` 的解决方案吗？

## 解法一：二分

当然这个题目可以直接遍历求解，但在面试时显然不行。看到排序数组的搜索问题，应该首先想到二分法，对于本题，存在**旋转**，怎么应用二分法呢？

可以明确一点，二分将**数组分成两份:[Left, Mid] 与 [Mid+1, Right]，其中必有一个数组是有序的**，如果`num[mid]>nums[left]`说明左边数组有序，反之则右边数组有序，利用有序数组来进行排除：**假设左边有序，我们直接判断是否满足`nums[left]<=target<nums[mid]`，如果不成立则target必不在左半部数组，下一次搜索右边的数组；右边同理**。

> 在有序数组中的判定与普通二分一致

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = nums.size();
        int left = 0, right = n-1, mid;

        while(left<=right){
            mid = (right-left)/2+left;
            // nums[mid] is right?
            if(nums[mid]==target)   
                return mid;
            // Left array is ordered
            if(nums[mid] > nums[left]){
                if(target > nums[mid] || target < nums[left]){
                    left = mid+1;
                }
                else
                    right = mid-1;
            }
            // Right array is ordered
            else{
                if(target > nums[right] || target < nums[mid+1<right?(mid+1):right]){	// 控制mid+1不越界
                    right = mid-1;
                }
                else
                    left = mid+1;
            }
        }

        return -1;
    }
};
```

时间复杂度：O(logn)

空间复杂度：O(1)
