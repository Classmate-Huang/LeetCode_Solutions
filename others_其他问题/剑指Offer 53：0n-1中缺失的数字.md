# 剑指Offer 53：0~n-1中缺失的数字

## 题目

一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。

 

示例 1:

```
输入: [0,1,3]
输出: 2
```

示例 2:

```
输入: [0,1,2,3,4,5,6,7,9]
输出: 8
```


限制：

1 <= 数组长度 <= 10000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一：一次遍历

因为传入的数组为有序数组，因此直接一次遍历即可，检测到不匹配的数字直接输出。

```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(), nums.end());

        for(int i=0; i<n; i++){
            if(nums[i] != i)    return i;
        }
        return n;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)

## 解法二：二分法

假设输入为`[0,1,2,3,5,6]`将数组视为两部分，Left为`[0,1,2,3]`，Right为`[5,6]`，需要找到Right的起始位置(本例中为`4`)。可以利用二分法，代码如下：

```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size();
        int low = 0, high = n-1;

        while(low<=high){
            int mid = (low+high)/2;
            if(nums[mid]==mid)  low = mid+1;	// 如果匹配，说明mid停留在Left部分
            else high = mid-1;	// 不匹配，mid停留在Right部分
        }

        return low;
    }
};
```

时间复杂度：O(logn)

空间复杂度：O(1)