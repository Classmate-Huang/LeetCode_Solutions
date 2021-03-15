# LeetCode633：平方数之和

## 题目

给定一个非负整数 `c` ，你要判断是否存在两个整数 `a` 和 `b`，使得 `a2 + b2 = c` 。

 

**示例 1：**

```
输入：c = 5
输出：true
解释：1 * 1 + 2 * 2 = 5
```

**示例 2：**

```
输入：c = 3
输出：false
```

**示例 3：**

```
输入：c = 4
输出：true
```

**示例 4：**

```
输入：c = 2
输出：true
```

**示例 5：**

```
输入：c = 1
输出：true
```

 

**提示：**

- `0 <= c <= 231 - 1`

## 解法一：双指针

借鉴`两数之和`的双指针做法，指针`low`从0开始，指针`high`从sqrt(c)开始，当`low * low + high * high`小于c时`low`指针右移，大于c时`high`指针左移。

值得注意的是，直接计算`low * low + high * high`会导致溢出，所以这里将`c - low*low`与`high*high`比较。

代码如下：

```c++
class Solution {
public:
    bool judgeSquareSum(int c) {
        int low = 0, high = sqrt(c);

        while(low<=high){
            int res = c - low*low;
            if(res==high*high) return true;
            else if(res>high*high)  low++;
            else high--;
        }

        return false;
    }
};
```

时间复杂度：O(sqrt(n))

空间复杂度：O(1)

