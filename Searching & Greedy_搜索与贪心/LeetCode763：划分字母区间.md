# LeetCode763：划分字母区间

## 题目

字符串 S 由小写字母组成。我们要把这个字符串划分为尽可能多的片段，同一字母最多出现在一个片段中。返回一个表示每个字符串片段的长度的列表。

示例：

```
输入：S = "ababcbacadefegdehijhklij"
输出：[9,7,8]
解释：
划分结果为 "ababcbaca", "defegde", "hijhklij"。
每个字母最多出现在一个片段中。
像 "ababcbacadefegde", "hijhklij" 的划分是错误的，因为划分的片段数较少。
```


提示：

```
S的长度在[1, 500]之间。
S只包含小写字母 'a' 到 'z' 。
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/partition-labels
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一：贪心

与区间安排问题类似，首先记录每个字母最后出现的位置。从头开始遍历，利用`end`指针代表当前所遍历过字母的最长区间。假设`abca`，那么遍历了`S[0]`，`end`就会更新为`3`。当遍历到end位置时，说明之前所遍历的那一段字母区间满足题意，放入解中即可。为了简洁，可额外使用一个变量来存储区间的起始位置。代码如下：

```c++
class Solution {
public:
    vector<int> partitionLabels(string S) {
        vector<int> flag(26, -1);
        vector<int> ans;

        for(int i=0; i<S.size(); i++){
            flag[S[i]-'a'] = i;
        }

        int start = 0, idx = 0, end = 0;
        for(int i=0; i<S.size(); i++){
            end = max(flag[S[i]-'a'], end);
            if(i==end){
                ans.push_back(end-start+1);
                start = end+1;
            }
        }

        return ans;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)