# LeetCode234：回文链表

## 题目

请判断一个链表是否为回文链表。

示例 1:

> 输入: 1->2
> 输出: false

示例 2:

> 输入: 1->2->2->1
> 输出: true

进阶：

> 你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/palindrome-linked-list
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一

不考虑空间复杂度的要求，直接用数组就可以解决，但是这种方法在面试的时候通不过(leetcode评论区有位老哥面试快手的时候就问了同样的题目，总之链表的题最好不要用数组来解)。代码如下：

```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        vector<int> a;
        ListNode * p=head;
        // 将链表元素依次存入数组
        while(p){
            a.push_back(p->val);
            p=p->next;
        }
        int n=a.size();
        // 在数组中进行比较
        for(int i=0; i<=n/2 && n; i++){
            if(a[n-1-i]!=a[i]) return false;
        }
        return true;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(n)

## 解法二

一样思想，使用栈实现，听说栈是解链表题最后的底线...，代码如下：

```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        stack<int> stk;
        ListNode *s=head, *f=head; // 使用快慢指针
        // s每次走一步，f走两步，当f走到头的时候，s正好指在中间节点
        while(f && f->next){
            stk.push(s->val);
            s=s->next;
            f=f->next->next;
        }
        // 进行调整，如果链表长度为奇数，s往后移一位
        if(f) s=s->next;
        // 利用栈先进后出的特点，进行比较
        while(!stk.empty() && s){
            if(s->val!=stk.top()) return false;
            stk.pop();
            s=s->next;
        }
        return true;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(n)

## 解法三

为了解决空间复杂度的问题(不开辟额外空间)，那么就需要将链表拆成两半，进行反转然后比较。代码如下：

```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        ListNode *s=head, *f=head, *p, *pre=NULL;
        // 进行遍历的同时，直接将前半段链表进行反转
        while(f && f->next){
            p=s->next;
            f=f->next->next;
            // 反转
            s->next=pre;
            pre=s;
            s=p;
        }
        // 保存第一条链的结尾 以及第二条链表的开头 为了方便进行恢复
        ListNode *first_last=pre, *second_start=s;
        // 判断奇偶，调整s值
        if(f) s=s->next;
        // 进行判断
        while(s && pre){
            if(s->val!=pre->val) return false;
            s=s->next;
            pre=pre->next;
        }
        // 反转，恢复链表
        s=first_last;
        pre=second_start;
        while(s){
            p=s->next;
            s->next=pre;
            pre=s;
            s=p;
        }
        return true;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)