# LeetCode461：汉明距离

## 题目

两个整数之间的汉明距离指的是这两个数字对应二进制位不同的位置的数目。

给出两个整数 x 和 y，计算它们之间的汉明距离。

注意：
0 ≤ x, y < 231.

示例:

```
输入: x = 1, y = 4

输出: 2

解释:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑
```

上面的箭头指出了对应二进制位不同的位置。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/hamming-distance
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一：逐位与运算

先将`x`和`y`进行异或运算，然后统计异或运算结果中1的个数即可。代码如下：

```c++
class Solution {
public:
    int hammingDistance(int x, int y) {
        int z = (x^y), ans=0;
        unsigned int mask = 1;

        for(int i=0; i<32; i++){
            if((z&mask) != 0)   ans++;
            mask = mask<<1;
        }

        return ans;
    }
};
```

时间复杂度：O(1)

空间复杂度：O(1)

## 解法二：`n`与`n-1`与运算

首先说明一个小`trick`，要将一个二进制串最后一位由1变为0，只需要将`n`和`n-1`做与运算即可。比如`1100 & 1011 = 1000`，`011 & 010 = 010`。因此，只需要重复执行`n`和`n-1`的与运算即可，相比于解法一，时间复杂度更优，因为解法二最多为32次。代码如下：

```c++
class Solution {
public:
    int hammingDistance(int x, int y) {
        int z = (x^y), ans = 0;

        while(z){
            z = (z & (z-1));
            ans++;
        }

        return ans;
    }
};
```

时间复杂度：O(1)

空间复杂度：O(1)