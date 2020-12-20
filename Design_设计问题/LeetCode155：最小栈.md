# LeetCode155：最小栈

## 题目

设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

push(x) —— 将元素 x 推入栈中。
pop() —— 删除栈顶的元素。
top() —— 获取栈顶元素。
getMin() —— 检索栈中的最小元素。


示例:

```
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```


提示：

pop、top 和 getMin 操作总是在 非空栈 上调用。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/min-stack
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一：vector

使用`vector`模拟栈以及栈的用法，比较简单。代码如下：

```c++
class MinStack {
public:
    /** initialize your data structure here. */
    vector<pair<int, int> > stk;
    int tail;

    MinStack() {	// 栈顶索引初始化置为-1
        tail = -1;
    }
    
    void push(int x) {
        if(tail == -1){
            stk.push_back(make_pair(x, x));
        }
        else{
            int _min = min(x, stk[tail].second);
            stk.push_back(make_pair(x, _min));
        }
        tail++;
    }
    
    void pop() {
        stk.erase(stk.begin()+tail);	// 删除元素
        tail--;
    }
    
    int top() {
        return stk[tail].first;
    }
    
    int getMin() {
        return stk[tail].second;
    }
};
```

