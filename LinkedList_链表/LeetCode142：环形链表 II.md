# LeetCode142：环形链表 II

## 题目

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 `null`。

为了表示给定链表中的环，我们使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 `pos` 是 `-1`，则在该链表中没有环。**注意，`pos` 仅仅是用于标识环的情况，并不会作为参数传递到函数中。**

**说明：**不允许修改给定的链表。

**进阶：**

- 你是否可以使用 `O(1)` 空间解决此题？

 

**示例 1：**

<img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png" />

```
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2：**

<img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png" />

```
输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例 3：**

<img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png" />

```
输入：head = [1], pos = -1
输出：返回 null
解释：链表中没有环。
```

 

**提示：**

- 链表中节点的数目范围在范围 `[0, 104]` 内
- `-105 <= Node.val <= 105`
- `pos` 的值为 `-1` 或者链表中的一个有效索引

## 解法一：存储已遍历节点

使用`unordered_set`存储已遍历的节点，当出现第二次被遍历到的节点时，即为环的第一个节点。代码如下：

```c++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        unordered_set<ListNode*> st;
        
        ListNode *p = head;
        while(p){
            if(st.find(p)!=st.end())  return p;
            st.insert(p);
            p = p->next;
        }

        return NULL;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(n)

## 解法二

如`LeetCode141`题，我们也可以使用快慢指针来解决。与141题不同的是，本题需要找到入环的第一个节点。

如果链表中出现环，根据141的分析，慢指针一定会在某个

```c++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *fast=head, *low=head, *ptr=head;
        while(fast){
            low = low->next;
            if(fast->next==nullptr || fast->next->next==nullptr) return nullptr;
            fast = fast->next->next;
            if(low==fast)   break;
        }
        while(low!=ptr){
            low = low->next;
            ptr = ptr->next;
        }
        return ptr;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)

