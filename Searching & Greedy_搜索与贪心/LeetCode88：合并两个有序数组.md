# LeetCode88：合并两个有序数组

## 题目

给你两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 nums1 成为一个有序数组。

 

说明：

初始化 nums1 和 nums2 的元素数量分别为 m 和 n 。
你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。




示例：

输入：

```
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

输出：[1,2,2,3,5,6]
```


提示：

```
-10^9 <= nums1[i], nums2[i] <= 10^9
nums1.length == m + n
nums2.length == n
```



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/merge-sorted-array
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一：从后往前

最简单的办法就是先合并两个数组，然后快排一遍，但面试这种code肯定是过不了的。显然这个题目的最优解不是这个，我们应该使用从后往前的思想：首先构造三个指针，p1,p2两个指针指向`nums1`和`nums2`的最后一个有效元素，一个指针指向`m+n-1`·。接下来利用双指针，将nums1[p1]与nums2[p2]所指向的最大值送入nums1[p]，当把nums2中所有元素插入nums1就结束。代码如下：

```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int p = m+n-1, p1 = m-1, p2 = n-1;
        while(p2>=0){
            while(p1>=0 && nums1[p1]>nums2[p2]){
                nums1[p] = nums1[p1];
                p1--; p--;
            }
            nums1[p] = nums2[p2];
            p2--; p--;
        }
    }
};
```

时间复杂度：O(m+n)

空间复杂度：O(1)