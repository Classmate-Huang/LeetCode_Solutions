# LeetCode387：字符串中的第一个唯一字符

## 题目

给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。

 

示例：

> s = "leetcode"
> 返回 0
>
> s = "loveleetcode"
> 返回 2


提示：你可以假定该字符串只包含小写字母。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/first-unique-character-in-a-string
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一

利用`unordered_map`存储每个字符的出现个数，进行两次遍历，代码如下：

```cpp
class Solution {
public:
    int firstUniqChar(string s) {
        unordered_map<char, int> map;
        for(auto i:s){
            if(map.find(i)==map.end()) map[i]=1;
            else map[i]++;
        }
        for(int i=0; i<s.size(); i++){
            if(map[s[i]]==1)
                return i;
        }
        return -1;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(n)