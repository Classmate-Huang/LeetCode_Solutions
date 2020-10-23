# LeetCode21：合并两个有序链表

## 题目

将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

 

示例：

> 输入：1->2->4, 1->3->4
> 输出：1->1->2->3->4->4

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/merge-two-sorted-lists
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一

这个题目很简单，相信数据结构课都讲过，代码如下：

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        // 构建一个空节点作为虚头结点
        ListNode *head = new ListNode(0), *p;
        p=head;
        // 进行判断与连接
        while(l1 && l2){ 
            if(l1->val<=l2->val){
                p->next=l1;
                l1=l1->next;
            }
            else{
                p->next=l2;
                l2=l2->next;
            }
            p=p->next;
        }
        p->next=l1==nullptr?l2:l1;  // 连接剩下的链表
        return head->next; 
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)