# LeetCode206：反转链表

## 题目

反转一个单链表。

示例:

> 输入: 1->2->3->4->5->NULL
> 输出: 5->4->3->2->1->NULL

进阶:

> 你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reverse-linked-list
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一

最简单的解法就是利用`头插法`重新构造一条新的链表，代码如下：

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* NewHead = NULL;
        ListNode* p1=head, *p2;
        while(p1){
            ListNode * p = new ListNode(p1->val);
            p->next = NewHead;
            NewHead = p;
            p2=p1;
            p1 = p1->next;
            delete p2;
        }
        return NewHead;
    }
};
```

时间复杂度：O(n)，n为链表长度

空间复杂度：O(n)

## 解法二

解法二就是使用双指针进行替代，`Pre`指针指向前一个节点，`Cur`指针指向当前节点，进行**迭代**。代码如下：

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode *Cur=head, *Pre=NULL;
        while(Cur){
            ListNode *temp = Cur->next;
            Cur->next = Pre;
            Pre=Cur;
            Cur=temp;
        }
        head=Pre;
        return head;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)

## 解法三

进行递归操作，代码如下：

① 直接将解法二用递归函数实现

```cpp
class Solution {
public:
    ListNode* reverse(ListNode* head, ListNode* other){
        if(other==NULL) return head;
        ListNode *t = other->next;
        other->next=head;
        head=other;
        return reverse(head, t);
    }
    ListNode* reverseList(ListNode* head) {
        return reverse(NULL, head);
    }
};
```

② 递归自身

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head==NULL || head->next==NULL){
            return head;
        }
        ListNode * p = reverseList(head->next);
        head->next->next = head;
        head->next=NULL;
        return p;
    }
};
```

代码②的理解来自官方题解，如理解优酷男女，可以参照评论以及官方的解答(https://leetcode-cn.com/problems/reverse-linked-list/solution/fan-zhuan-lian-biao-by-leetcode/)