# LeetCode141：环形链表

## 题目

给定一个链表，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

如果链表中存在环，则返回 true 。 否则，返回 false 。

 

进阶：

> 你能用 O(1)（即，常量）内存解决此问题吗？

 

示例 1：

![image-20201023101904757](F:\LeetCode_Solutions\LinkedList_链表\LeetCode141：环形链表.assets\image-20201023101904757.png)

> 输入：head = [3,2,0,-4], pos = 1
> 输出：true
> 解释：链表中有一个环，其尾部连接到第二个节点。

示例 2：

![image-20201023101922503](F:\LeetCode_Solutions\LinkedList_链表\LeetCode141：环形链表.assets\image-20201023101922503.png)

> 输入：head = [1,2], pos = 0
> 输出：true
> 解释：链表中有一个环，其尾部连接到第一个节点。

示例 3：

![image-20201023101941994](F:\LeetCode_Solutions\LinkedList_链表\LeetCode141：环形链表.assets\image-20201023101941994.png)

> 输入：head = [1], pos = -1
> 输出：false
> 解释：链表中没有环。


提示：

> 链表中节点的数目范围是 [0, 10<sup>4</sup>]
> -10<sup>5</sup> <= Node.val <= 10<sup>5</sup>
> pos 为 -1 或者链表中的一个 有效索引 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/linked-list-cycle
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一

这题其实是有bug的，如果单从AC的角度来说，节点数目限制在10<sup>4</sup>之内，直接遍历链表，如果超过10<sup>4</sup>还未遍历到`NULL`，那么肯定存在环形链表。但这种做法在面试时不可取，代码如下：

```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        int cnt=10001;
        ListNode* p=head;
        while(p!=NULL && cnt){
            p=p->next;
            cnt--;
        }
        if(cnt==0) return true;
        return false;
    }
};
```

时间复杂度：O(1)

空间复杂度：O(1)

## 解法二

如果没有空间限制，可以用一个哈希表/集合来保存已遍历过的结点，如果某个结点被重复遍历，那证明存在环形链表，这个做法比较简单，这里就不展示了。

在空间复杂度的限制下，如何判断呢？这里有一个著名的算法：龟兔算法，其实就是快慢指针。快指针每次走两步，慢指针走一步，在初始时它们都指向头结点，如果存在环形链表，它们必相遇；如果不存在环形链表，快指针会率先遍历到`NULL`。代码如下：

```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode *slow=head, *fast=head;
        while(fast!=NULL && fast->next!=NULL){
            fast=fast->next->next;
            slow=slow->next;
            if(fast==slow) return true;
        }
        return false;
    }
};
```

时间复杂度：O(n)，n为链表长度，如果不存在环形链表，O(n)易证；如果存在环形链表，假设目前快慢指针均进入到环形链表中，快指针与慢指针之间相距k个节点，每前进一步之后，它们之间的距离会减少1，因此最多走n-k步，复杂度仍为O(n)。

空间复杂度：O(1)