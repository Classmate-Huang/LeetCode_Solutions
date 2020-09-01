# LeetCode350：两个数组的交集II

## 题目

给定两个数组，编写一个函数来计算它们的交集。

示例 1：

> 输入：nums1 = [1,2,2,1], nums2 = [2,2]
> 输出：[2,2]

示例 2:

> 输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
> 输出：[4,9]

说明：

> 输出结果中每个元素出现的次数，应与元素在两个数组中出现次数的最小值一致。
> 我们可以不考虑输出结果的顺序。

进阶：

> 如果给定的数组已经排好序呢？你将如何优化你的算法？
> 如果 nums1 的大小比 nums2 小很多，哪种方法更优？
> 如果 nums2 的元素存储在磁盘上，磁盘内存是有限的，并且你不能一次加载所有的元素到内存中，你该怎么办？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/intersection-of-two-arrays-ii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一
**使用unordered_map来存储某个数组(长度最小)中每个数字出现的次数**，对另一个数组进行遍历，每次查询到unordered_map中的相同数字，判断次数是否为0，**若>0，就输出到结果中，并把对应的次数减一，否则跳过**。这样只需要分别对两个数组进行一次遍历即可，一次存储到unordered_map中，一次进行遍历输出。

```cpp
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        if( nums1.size() > nums2.size()){	 //确保unordered_map是对最短数组操作
            return intersect(nums2, nums1);
        }
        vector<int> res;  // 保存结果数组
        unordered_map<int, int> m;        // unordered_map
        for(int i=0; i<nums1.size(); i++){		// 对nums1使用map
            if(m.find(nums1[i])==m.end()){
                m[nums1[i]]=1;;
            }
            else{
                m[nums1[i]]++;
            }
        }
        for(int i=0; i<nums2.size(); i++){		// 对数组nums2进行遍历
            if(m.find(nums2[i])!=m.end() && m[nums2[i]]!=0){	// 查询交集情况
                m[nums2[i]]--;
                res.push_back(nums2[i]);
            }
        }
        return res;
    }
};
```
时间复杂度：O(M+N)，M和N分别为两个数组的长度。
空间复杂度：O(min(M,N))，map和res数组的长度都不会超过最短数组。

## 解法二
对于进阶问题，如果给定的两个数组已经为有序数组。不用unorder_map来存储次数，而是变成了一个双指针问题：**使用两个指针分别对两个数组进行遍历**。这里使用进阶的思想作为我们的解法二，先用`sort()`分别对两个数组进行排序，然后再使用双指针进行遍历。

```cpp
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        sort(nums1.begin(), nums1.end());	// 排序
        sort(nums2.begin(), nums2.end());
        int m=nums1.size(), n=nums2.size(), i=0, j=0;
        vector<int> res;
        while(i<m && j<n){	// 双指针遍历
            if(nums1[i]==nums2[j]){		// 相等即送入输出结果中
                res.push_back(nums1[i]);
                i++;j++;
            }
            // 哪个数组的值小，哪个往前走
            else if(nums1[i]>nums2[j])   j++;
            else i++;
        }
        return res;
    }
};
```
**复杂度分析：**
时间复杂度：O(MlogM + NlogN)，排序的时长为MlogM。
空间复杂度：O(min(M,N))


## 进阶分析：
解法二即为对应排序数组的解法，如果数组本身即为有序数组，那么使用解法二会比解法一高效；
如果nums1比nums2小很多，那么解法一会更高效；
如果 nums2 的元素存储在磁盘上，磁盘内存是有限的，并且你不能一次加载所有的元素到内存中，那么就使用解法一。