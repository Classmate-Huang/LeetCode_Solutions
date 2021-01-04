# LeetCode20：有效的括号

## 题目

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

示例 1:

输入: "()"
输出: true
示例 2:

输入: "()[]{}"
输出: true
示例 3:

输入: "(]"
输出: false
示例 4:

输入: "([)]"
输出: false
示例 5:

输入: "{[]}"
输出: true

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/valid-parentheses
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一

利用`Stack`和`Map`解决该题，代码如下：

```c++
class Solution {
public:
    bool isValid(string s) {
        unordered_map<char, char> m = {{'(',')'}, {'[',']'}, {'{','}'}};	// 存储括号对应关系
        stack<char> stk;	// 栈

        for(auto i:s){
            if(i==')' || i=='}' || i==']'){
                if(stk.empty() || m[stk.top()]!=i) return false;	// 右括号前无左括号 or 括号不匹配
                else stk.pop();
            }
            else if(i=='(' || i=='[' || i=='{') stk.push(i);
        }

        return stk.empty();	// 是否栈中还存在左括号未被匹配
    }
};
```

时间复杂度：O(n)

空间复杂度：O(n)

