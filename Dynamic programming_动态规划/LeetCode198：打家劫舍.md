# LeetCode198：打家劫舍

## 题目

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

 

示例 1：

```
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

示例 2：

```
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/house-robber
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一：动态规划

构造子问题`f[i]`，其中`f[i]`表示在`[0...i]`区间内的最优解。进行分析，存在两种情况：① 偷窃第i家，那么就不能偷窃第`i-1`家； ②  不偷窃第i家。那么`f[i] = max{f[i-1], f[i-2]+nums[i]}`。代码如下：

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if(n == 0)  return 0;
        if(n == 1)  return nums[0];

        int f[n];
        f[0] = nums[0]; f[1] = max(nums[0], nums[1]);

        for(int i=2; i<n; i++){
            f[i] = max(f[i-1], f[i-2]+nums[i]);
        }

        return f[n-1];
    }
};
```

时间复杂度：O(n)

空间复杂度：O(n)

## 解法二：优化

可以进一步优化，利用滚动数组降低空间复杂度。代码如下：

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if(n == 0)  return 0;
        if(n == 1)  return nums[0];

        int pre2 = nums[0], pre1 = max(nums[0], nums[1]);
        int ans = pre1;

        for(int i=2; i<n; i++){
            ans = max(pre2+nums[i], pre1);
            pre2 = pre1;
            pre1 = ans;
        }

        return ans;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)