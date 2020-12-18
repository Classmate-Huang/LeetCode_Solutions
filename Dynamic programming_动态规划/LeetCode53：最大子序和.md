# LeetCode53：最大子序和

## 题目

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例:

```
输入: [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

进阶:

如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/maximum-subarray
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一：动态规划

动态规划将问题转换为求解`f[i]`，`f[i]`为以`nums[i]`结尾的最大子序和，最后求解`max(f[i])`即为最后解，`i∈[0,n-1]`。那么考虑到`f[i]`的取值有两种情况，第一种是连接前面的子序列：`f[i-1]+nums[i]`，要么不管前面的序列，重新构造一个序列`nums[i]`。最后代码如下：

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size();
        int f[n];
        for(int i=0; i<n; i++){
            if(i == 0) f[i] = nums[i];
            else{
                f[i] = max(f[i-1]+nums[i], nums[i]);
            }
        }
        return *max_element(f, f+n);
    }
};
```

时间复杂度：O(n)

空间复杂度：O(n)

## 解法二：优化

根据解法一的思想，可以进一步优化，不存储中间结果，每次只保留最大的子序和，这样可以将空间复杂度进一步减低。代码如下：

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size(), ans, pre;
        ans = pre = nums[0];
        for(int i=1; i<n; i++){
            pre = max(pre+nums[i], nums[i]);
            ans = max(pre, ans);
        }
        return ans;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)