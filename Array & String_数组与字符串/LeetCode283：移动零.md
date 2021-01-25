# LeetCode283：移动零

## 题目
给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

示例:

> 输入: [0,1,0,3,12]
> 输出: [1,3,12,0,0]

说明:

> 必须在原数组上操作，不能拷贝额外的数组。
> 尽量减少操作次数。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/move-zeroes

## 解法一
使用双指针将非零元素全部依次移到数组首部，然后使慢指针后元素全置为0，代码如下

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int i=0,j=0,n=nums.size();
        while(j<n){
            if(nums[j]!=0){
                nums[i++]=nums[j++];
            }
            else j++;
        }
        for(int t=i;t<n;t++){
            nums[t]=0;
        }
    }
};
```
**复杂度分析**
时间复杂度：O(n)
空间复杂度：O(1)

## 解法二
同样用到了快慢指针，操作细节上进行了优化。对于解法一，如果出现的情况为`[0,0,0,0,0,1]`，在第二个循环中要执行n-1次，所以在零元素较多的数组存在问题。而解法只需要一个循环即可解决，直接进行元素交换，代码如下：

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int i=0,j=0,n=nums.size();
        while(j<n){
            if(nums[j]!=0){
                swap(nums[i],nums[j]);
                i++;j++;
            }
            else{
                j++;
            }
        }
    }
};

```
**复杂度分析**
时间复杂度：O(n)
空间复杂度：O(1)