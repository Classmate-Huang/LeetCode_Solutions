# LeetCode108：将有序数组转换为二叉搜索树

## 题目

将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。



示例:

给定有序数组: [-10,-3,0,5,9],

一个可能的答案是：[0,-3,9,-10,null,5]，它可以表示下面这个高度平衡二叉搜索树：

          0
         / \
       -3   9
       /   /
     -10  5


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一：分治

要构建高度平衡二叉搜索树，首先搜索树满足：左子树的值小于中间节点，右子树的值大于中间节点。而提供给我们的本身就是递增序列，因此我们每次在构造中间节点时，利用二分法，选择区间内的中间元素构造中间节点，左半部分继续用于构造左子树，右半部分继续用于构造右子树。代码如下：

```c++
class Solution {
public:
    TreeNode* bulidtree(int high, int low, vector<int>& nums){
        if(low>high)    return nullptr;
        int mid = (high-low)/2 + low;
        TreeNode* node = new TreeNode(nums[mid]);
        node->left = bulidtree(mid-1, low, nums);
        node->right = bulidtree(high, mid+1, nums);
        return node;
    }
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        int n = nums.size();
        if(n == 0)    return nullptr;
        return bulidtree(n-1, 0, nums);
    }
};
```

时间复杂度：O(n)，所有节点都要遍历一遍

空间复杂度：O(logn)，递归栈的深度

