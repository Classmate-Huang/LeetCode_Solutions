# LeetCode34：在排序数组中查找元素的第一个和最后一个位置

## 题目

给定一个按照升序排列的整数数组 `nums`，和一个目标值 `target`。找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 `target`，返回 `[-1, -1]`。

**进阶：**

- 你可以设计并实现时间复杂度为 `O(log n)` 的算法解决此问题吗？

 

**示例 1：**

```
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
```

**示例 2：**

```
输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]
```

**示例 3：**

```
输入：nums = [], target = 0
输出：[-1,-1]
```

 

**提示：**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`
- `nums` 是一个非递减数组
- `-109 <= target <= 109`

## 解法一：双指针遍历

利用双指针遍历数组即可，代码如下：

```c++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int n = nums.size();
        int left = 0, right = n-1;
        vector<int> ans(2, -1);		// 初始化
        
        while(left<n && right>=0 && left<=right){
            
            if(nums[left]==target && nums[right]==target){
                ans[0] = left; ans[1] = right;
                break;
            }

            if(nums[left]!=target)
                left++;

            if(nums[right]!=target)
                right--;
        }
        return ans;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)

## 解法二：二分法

分别使用二分法找到左右边界，代码如下：

```c++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int n = nums.size();
        int left = 0, right = n-1, mid;
        vector<int> ans(2, -1);
        if(n==0)    return ans;	// 特殊情况处理
        // 先找到左边界
        while(left<right){
            mid = (right-left)/2+left;
            if(nums[mid]<target)    left = mid+1;
            else    right = mid;
        }
        if(nums[left]!=target)  return ans;	// 数组中没有存在target
        ans[0] = left;
        // 再找到右边界
        left = 0, right = n-1;
        while(left<right){
            mid = (right-left+1)/2+left;	// 重点！向上取整
            if(nums[mid]>target)    right = mid-1;
            else    left = mid;
        }
        ans[1] = right;
        return ans;
    }
};
```

时间复杂度：O(logn)

空间复杂度：O(1)

>值得注意的是，在寻找右边界时，我们需要在`nums[left]<=target`的情况下移动`Left`指针，在下一轮搜索时`mid`的取值如果还是`(right-left)/2+left`，程序将会存在死循环。假设当前`[left,right]`已经遍历到`[8,9]`，target为8，下一轮的`nums[mid]`将一直为8，陷入死循环。因此需要将`mid`改为向上取整，即`(right-left+1)/2+left`.

