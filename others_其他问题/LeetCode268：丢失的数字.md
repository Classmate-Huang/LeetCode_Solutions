# LeetCode268：丢失的数字

## 题目

给定一个包含 [0, n] 中 n 个数的数组 nums ，找出 [0, n] 这个范围内没有出现在数组中的那个数。

 

进阶：

你能否实现线性时间复杂度、仅使用额外常数空间的算法解决此问题?


示例 1：

```
输入：nums = [3,0,1]
输出：2
解释：n = 3，因为有 3 个数字，所以所有的数字都在范围 [0,3] 内。2 是丢失的数字，因为它没有出现在 nums 中。
```

示例 2：

```
输入：nums = [0,1]
输出：2
解释：n = 2，因为有 2 个数字，所以所有的数字都在范围 [0,2] 内。2 是丢失的数字，因为它没有出现在 nums 中。
```

示例 3：

```
输入：nums = [9,6,4,2,3,5,7,0,1]
输出：8
解释：n = 9，因为有 9 个数字，所以所有的数字都在范围 [0,9] 内。8 是丢失的数字，因为它没有出现在 nums 中。
```

示例 4：

```
输入：nums = [0]
输出：1
解释：n = 1，因为有 1 个数字，所以所有的数字都在范围 [0,1] 内。1 是丢失的数字，因为它没有出现在 nums 中。
```


提示：

n == nums.length
1 <= n <= 104
0 <= nums[i] <= n
nums 中的所有数字都 独一无二

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/missing-number
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一：空间换时间

额外构造一个标记数组存储出现的数字，然后遍历该数组找到丢失的数字。代码如下：

```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size();
        vector<int> flag(n+1, 0);

        for(auto i:nums){
            flag[i] = 1;
        }

        for(int i=0; i<=n; i++){
            if(!flag[i])    return i;
        }

        return -1;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(n)

## 解法二：排序后遍历

先将数组排序变为有序数组，然后遍历该数组找到不匹配数字。代码如下：

```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(), nums.end());

        for(int i=0; i<n; i++){	// 0~n-1是否匹配
            if(nums[i]!=i)  return i;
        }

        return n;	// n为匹配
    }
};
```

时间复杂度：O(nlogn)

空间复杂度：O(1)

## 解法三：XOR运算

① 一个数与它本身进行异或运算`XOR`会得到0 (eg. `A ^ A = 0`)；

② 0与任何数进行异或运算会得到这个数本身 (eg. `0 ^ A = A`)。

根据上述的两个结论，可以将[0, n]与数组中的所有值进行一次XOR运算，最后剩下的值即为答案。代码如下：

```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size(), ans = 0;
        
        for(int i=0; i<n; i++){
            ans = ans ^ (i^nums[i]);
        }

        return ans^n;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)

## 解法四：求和作减法

先求和`[0,n]`，然后减去数组中的元素和，剩下的即为结果。代码如下：

```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size();
        int sum = n%2==0?(n/2*(n+1)):((n+1)/2*n);

        for(int i=0; i<n; i++){
            sum -= nums[i];
        }

        return sum;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)