# LeetCode136：只出现一次的数字

[toc]

## 题目

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

==说明：==

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

示例 1:

> 输入: [2,2,1]
> 输出: 1

示例 2:

> 输入: [4,1,2,1,2]
> 输出: 4

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/single-number
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一

不使用额外的存储空间，我们可以采取`排序 + 遍历`的思路进行判断。比较简单，就不过多赘述了，直接上代码：

```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int n=nums.size();
        sort(nums.begin(), nums.end());
        for(int i=0; i<n; i++){
            if(i+1==n || nums[i+1]!=nums[i]){
                if(i-1<0 || nums[i-1]!=nums[i])
                    return nums[i];
            }
        }
        return -1;
    }
};
```

时间复杂度为：O(n log(n))，排序需要的时间

空间复杂度为：O(1)

## 解法二

解法二比较巧妙，使用`位运算`从而只需要遍历一边数组，本题用的是`异或`运算，它有如下性质：

1. a⊕0=a;
2. a⊕a=0;
3. a⊕b⊕b=b⊕a⊕b=a⊕(b⊕b)=a 满足交换律和结合律

根据上述的性质就不难想到以下解法，直接上代码：

```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int res=0;
        for(int i:nums){
            res ^= i;
        }
        return res;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)

