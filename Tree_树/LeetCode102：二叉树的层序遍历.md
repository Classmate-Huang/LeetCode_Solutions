# LeetCode102：二叉树的层序遍历

## 题目

给你一个二叉树，请你返回其按 层序遍历 得到的节点值。 （即逐层地，从左到右访问所有节点）。

 

示例：
二叉树：[3,9,20,null,null,15,7],

        3
       / \
      9  20
        /  \
       15   7

返回其层次遍历结果：

```
[
  [3],
  [9,20],
  [15,7]
]
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/binary-tree-level-order-traversal
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一：利用队列

题目要求将二叉树中每一层元素都保留到一个单独的vector中，因此我们利用两个队列构造层次遍历：将每层的结点保留在一个队列中，遍历队列时，将值保存到一个vecotor中，并且在利用左右孩子指针遍历下一层节点时，下一层的元素保留在另一个队列里。依次遍历，指导遍历所有子树元素，代码如下：

```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        vector<queue<TreeNode *>> q(2);
        if(root==nullptr)   return ans;
        q[0].push(root);
        
        while(!q[0].empty() || !q[1].empty()){
			// 遍历某层节点时，下一层节点保留在另一队列中
            int id = q[0].empty()?1:0;
            int sid = id==0?1:0;
            
            vector<int> layer;    
            while(!q[id].empty()){
                TreeNode * n = q[id].front(); q[id].pop();
                layer.push_back(n->val);
                if(n->left != nullptr) q[sid].push(n->left);
                if(n->right != nullptr) q[sid].push(n->right);
            }
            
            ans.push_back(layer);
        }
        return ans;
    }
};
```

时间复杂度：O(n)，遍历所有元素

空间复杂度：O(n)



## 解法二：基于解法一的优化

可以不用两个队列，使用一个**变量**来保存每一层的结点个数即可，代码如下：

```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        queue<TreeNode *> q;
        if(root==nullptr)   return ans;
        q.push(root);
        
        while(!q.empty()){
            int len = q.size();
            vector<int> layer;
  
            while(len--){
                TreeNode * n = q.front(); q.pop();
                layer.push_back(n->val);
                if(n->left != nullptr) q.push(n->left);
                if(n->right != nullptr) q.push(n->right);
            }
            
            ans.push_back(layer);
        }
        return ans;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(n)