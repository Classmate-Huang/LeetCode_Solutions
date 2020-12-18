# LeetCode121：买卖股票的最佳时机

## 题目

给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。

注意：你不能在买入股票前卖出股票。

 

示例 1:

```
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```

示例 2:

```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一：动态规划

这题直接用循环来暴搜是会超时的，因此可以利用动态规划。设`f[i]`为第i天卖出股票所能收获的最大利润，最后找到`[1,n]`之间的最大值即为最终的解。求解`f[i]`其实比较简单，找到`prices[0:i]`之间的最小值`min`，`f[i] = prices[i] - min`。代码如下：

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int _min = INT_MAX, n = prices.size();
        if(n==0)    return 0;

        int f[n];
        for(int i=0; i<n; i++){
            _min = min(_min, prices[i]);
            f[i] = prices[i] - _min;
        }

        return *max_element(f, f+n);
    }
};
```

时间复杂度：O(n)

空间复杂度：O(n)

### 解法二：一次遍历

利用解法一动态规划的思想，优化后，可以只用一次遍历得到最终结果，并且不需要数组来存储中间结果。代码如下：

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int _min = INT_MAX, ans = 0;

        for(auto t:prices){
            _min = min(_min, t);	// t之前的最小值
            ans = max(t-_min, ans);	// 选取最大值
        }

        return ans;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)