# LeetCode118：杨辉三角

## 题目

给定一个非负整数 numRows，生成杨辉三角的前 numRows 行。



在杨辉三角中，每个数是它左上方和右上方的数的和。

示例:

输入: 5
输出:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/pascals-triangle
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一

入门题，代码如下：

```c++
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> ans;

        for(int i=0; i<numRows; i++){
            vector<int> t;
            for(int j=0; j<=i; j++){
                if(j==0 || j==i)    t.push_back(1);
                else t.push_back(ans[i-1][j-1]+ans[i-1][j]);
            }
            ans.push_back(t);
        }

        return ans;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)