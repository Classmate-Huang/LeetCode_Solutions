# LeetCode98：验证二叉搜索树

## 题目

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

> 节点的左子树只包含小于当前节点的数。
> 节点的右子树只包含大于当前节点的数。
> 所有左子树和右子树自身必须也是二叉搜索树。

示例 1:

> 输入:
>
> ```
>  2
> / \
> 1  3
> ```
>
> 输出: true
>
> 输出: true

示例 2:

> 输入:
>
> ```
>   5
>  / \
> 1   4
>    / \
>   3   6
> ```
>
> 输出: false
> 解释: 输入为: [5,1,4,null,null,3,6]，根节点的值为 5 ，但是其右子节点值为 4 。
>
> 输出: false
> 解释: 输入为: [5,1,4,null,null,3,6]，根节点的值为 5 ，但是其右子节点值为 4 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/validate-binary-search-tree
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一

直接利用二叉搜索树的定义，左子树节点值小于当前节点，右子树节点值大于当前节点。同样使用递归的思想，代码如下：

```cpp
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        return isRight(root, LONG_MIN, LONG_MAX); // 节点可能存在int边界值，所以使用long类型
    }
    bool isRight(TreeNode* root, long long low, long long high){
        if(root==NULL) return true;  // 节点为空
        if(root->val<=low || root->val>=high) return false;  // 不满足要求
        return isRight(root->left, low, root->val) & isRight(root->right, root->val, high); // 递归
    }
};
```

时间复杂度：O(n)

空间复杂度：O(n)

## 解法二

**如果一棵树的中序遍历是递增的，那么它就为二叉搜索树**，利用这个思想进行编码。使用代码如下：

```cpp
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        stack<TreeNode*> stk;
        long long now = (long long)INT_MIN - 1;
        TreeNode * t = root;

        while(!stk.empty() || t != nullptr){
            while(t!=nullptr){	// 左子树不为空
                stk.push(t);
                t = t->left;
            }

            t = stk.top();
            stk.pop();
            if(t->val <= now) return false;	// 判断是否满足要求
            now = t->val;
            t = t->right;
        }

        return true;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(n)