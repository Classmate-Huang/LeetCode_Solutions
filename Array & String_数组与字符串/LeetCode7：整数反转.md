# LeetCode7：整数反转

## 题目

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

示例 1:

> 输入: 123
> 输出: 321

示例 2:

> 输入: -123
> 输出: -321

示例 3:

> 输入: 120
> 输出: 21

注意:

> 假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−2<sup>31</sup>, 2<sup>31</sup> − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reverse-integer
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一

利用模拟的思想，利用取余`%`将最后一位取出来，再使用`/`对最后一位进行舍弃。在过程中，需要考虑溢出判断，代码如下：

```cpp
class Solution {
public:
    int reverse(int x) {
        int t=0;
        while(x!=0){
            if(t>INT_MAX/10 || (t==INT_MAX/10 && x > INT_MAX%10)) return 0;
            if(t<INT_MIN/10 || (t==INT_MIN/10 && x < INT_MIN%10)) return 0;
            t = t*10 + x%10;
            x = x/10;
        }
        return t;
    }
};
```

时间复杂度：O(log<sub>10</sub>x)

空间复杂度：O(1)