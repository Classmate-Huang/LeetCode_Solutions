# LeetCode19：删除链表的倒数第N个节点

## 题目

给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。

示例：

> 给定一个链表: 1->2->3->4->5, 和 n = 2.
>
> 当删除了倒数第二个节点后，链表变为 1->2->3->5.

说明：

> 给定的 n 保证是有效的。

进阶：

> 你能尝试使用一趟扫描实现吗？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一

要删除链表中倒数第n个节点，先求得链表长度L，那么相当于删除第(L-n+1)个节点。只需要找到该节点的前一个节点，然后使其指向下下个节点即可(**要注意考虑删除节点为第一个节点的特殊情况**)，代码如下：

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode * p = head;
        int L=0;
        while(p){
            L++;
            p=p->next;
        }
        p=head;
        if(n==L){   // 如果待删除的节点为第一个节点
            head = head->next;
            delete p;
            return head;
        }
        // 如果不为第一个节点，找到待删除节点的前一个节点
        for(int i=1; i<(L-n); i++){
            p = p->next;
        }
        ListNode * q = p->next;
        p->next = q->next;
        delete q;
        return head;
    }
};
```

时间复杂度：O(L)，L为链表长度

空间复杂度：O(1)

## 解法二

如果仅能执行一次遍历，就需要用到双指针，构建两个指针`p1, p2`，`p1`首先往前走n步，然后`p2`再出发，`p1`与`p2`之间始终保持着n个节点的距离，当`p1`指向最后一个节点时，`p2`指向的就是**待删除节点的上一节点**，与解法一一样，需要处理删除首节点的特殊情况。代码如下：

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode * p1, * p2;
        p1=head;
        while(--n){
            p1=p1->next;
        }
        if(p1->next==NULL){ // 删除节点为第一个节点
            p2 = head;
            head = head->next;
            delete p2;
            return head;
        }
        // 待删除节点不为第一个节点，找到待删除节点的前一个节点
        p1=p1->next; p2=head;
        while(p1->next!=NULL){
            p1=p1->next;
            p2=p2->next;
        }
        p1=p2->next;
        p2->next = p1->next;
        delete p1;
        return head;
    }
};
```

时间复杂度：O(L)，仅执行一次遍历

空间复杂度：O(1)