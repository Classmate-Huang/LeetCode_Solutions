# LeetCode69：x 的平方根

## 题目

实现 `int sqrt(int x)` 函数。

计算并返回 *x* 的平方根，其中 *x* 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

**示例 1:**

```
输入: 4
输出: 2
```

**示例 2:**

```
输入: 8
输出: 2
说明: 8 的平方根是 2.82842..., 
     由于返回类型是整数，小数部分将被舍去。
```

## 解法一：遍历

设置一个指针`ans`，让其从0开始遍历，判断其是否满足`ans * ans > x`，如果满足，则返回`ans-1`即可。代码如下：

```c++
class Solution {
public:
    int mySqrt(int x) {
        int ans = 0;
        while((long long)ans*ans<=x){
            ans++;
        }
        return ans-1;
    }
};
```

时间复杂度：O(n<sup>1/2</sup>)

空间复杂度：O(1)

## 解法二：二分

使用二分法，代码如下：

```c++
class Solution {
public:
    int mySqrt(int x) {
        int left = 0, right = x, mid;
        
        while(left<=right){
            mid = (right-left)/2+left;
            if((long long)mid*mid>x){
                right = mid-1;
            }
            else if((long long)mid*mid<x){
                left = mid+1;
            }
            else return mid;
        }
        
        return left-1;
    }
};
```

时间复杂度：O(logn)

空间复杂度：O(1)

代码二：

```c++
class Solution {
public:
    int mySqrt(int x) {
        int left = 0, right = x, mid, ans;	// ans用于保存结果
        while(left<=right){
            mid = (right-left)/2+left;
            if((long long)mid*mid>x){
                right = mid-1;
            }
            else{
                ans = mid;	// 先保存结果，确保ans里是最大的mid*mid <= x的取值
                left = mid+1;
            }
        }
        return ans;
    }
};
```

代码三：

```c++
class Solution {
public:
    int mySqrt(int x) {
        int left = 1, right = x;
        if(!x)  return 0;	// 特殊情况
        while(left<right){
            int mid = (right-left+1)/2+left;	// 避免死循环
            if((long long) mid*mid > x){
                right = mid-1;
            }
            else left = mid;
        }
        return left;
    }
};
```

