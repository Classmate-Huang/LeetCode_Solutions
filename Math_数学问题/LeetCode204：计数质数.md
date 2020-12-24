# LeetCode204：计数质数

## 题目

统计所有小于非负整数 n 的质数的数量。

 

示例 1：

```
输入：n = 10
输出：4
解释：小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7 。
```

示例 2：

```
输入：n = 0
输出：0
```

示例 3：

```
输入：n = 1
输出：0
```


提示：

0 <= n <= 5 * 106

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/count-primes
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一：埃氏筛

这个题很容易想到利用C语言课上讲的暴搜解决，但暴搜的时间复杂度为O(n<sup>3/2</sup>)，会超时。因此通常使用筛选法，这里介绍比较容易掌握的埃氏筛法。假设`x`为质数，那么`1x`，`2x`，...都为合数，因此要判断[2,n)之间质数个数，我们利用数组`primep[n]`来记录对应位置上是否为质数将很容易实现，代码如下：

```c++
class Solution {
public:
    int countPrimes(int n) {
        if(n==0 || n==1)    return 0;

        vector<int> prime(n, 1);
        int ans = 0;

        for(int i=2; i<n; i++){
            if(prime[i]==1){
                ans++;
                if((long long)i*i < n){		// 从i*i开始判断
                    for(int j=i*i; j<n; j=j+i){
                        prime[j]=0;
                    }
                }
            }
        }

        return ans;
    }
};
```

> 每次在记录合数时，直接从`i*i`开始判断，因为小于i的合数肯定在之前的循环里都已判断过了。

时间复杂度：O(nlogn)

空间复杂度：O(n)

