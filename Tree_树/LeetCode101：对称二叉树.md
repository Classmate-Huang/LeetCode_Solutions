# LeetCode101：对称二叉树

## 题目

给定一个二叉树，检查它是否是镜像对称的。

 

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

        1
       / \
      2   2
     / \ / \
    3  4 4  3
 


但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

        1
       / \
      2   2
       \   \
       3    3
 


进阶：

你可以运用递归和迭代两种方法解决这个问题吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/symmetric-tree
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一：递归

递归其实是最好实现的，构造一个函数，判断两颗子树是否对称即可。代码如下：

```c++
class Solution {
public:
    bool isRight(TreeNode* pLeft, TreeNode* pRight){
        if(pLeft==nullptr && pRight==nullptr)   return true;
        if(pLeft==nullptr || pRight==nullptr)   return false;
        if(pLeft->val != pRight->val)   return false;
        
        return isRight(pLeft->left, pRight->right) & isRight(pLeft->right, pRight->left);
    }
    bool isSymmetric(TreeNode* root) {
        if(root==nullptr)   return true;
        return isRight(root->left, root->right);
    }
};
```

时间复杂度：O(n)，遍历树上所有节点

空间复杂度：O(n)

## 解法二：迭代

利用**队列**进行判断，其实就是将递归的思路用队列实现，代码如下：

```c++
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
       if(root == nullptr)  return true;
       queue<TreeNode*> q1, q2;
       q1.push(root->left); q2.push(root->right);
       while(!q1.empty() && !q2.empty()){
           TreeNode * n1 = q1.front(); q1.pop();
           TreeNode * n2 = q2.front(); q2.pop();

           if(n1==nullptr && n2==nullptr)   continue;
           if(n1==nullptr || n2==nullptr)   return false;
           if(n1->val != n2->val) return false;

           if(n1->left==nullptr || n2->right==nullptr){
               if(n1->left!=nullptr || n2->right!=nullptr)  return false;
           }
           if(n1->right==nullptr || n2->left==nullptr){
               if(n1->right!=nullptr || n2->left!=nullptr)  return false;
           }

           q1.push(n1->left); q2.push(n2->right);
           q1.push(n1->right); q2.push(n2->left);
       }
        return true;
    }
};
```

时间复杂度：O(n)，遍历树上所有节点

空间复杂度：O(n)