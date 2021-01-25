# LeetCode36：有效的数独

## 题目

判断一个 9x9 的数独是否有效。只需要根据以下规则，验证已经填入的数字是否有效即可。

1. 数字 1-9 在每一行只能出现一次。
2. 数字 1-9 在每一列只能出现一次。
3. 数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。

(相关图例请参见官方链接)

示例 1:

> 输入:
> [
>   ["5","3",".",".","7",".",".",".","."],
>   ["6",".",".","1","9","5",".",".","."],
>   [".","9","8",".",".",".",".","6","."],
>   ["8",".",".",".","6",".",".",".","3"],
>   ["4",".",".","8",".","3",".",".","1"],
>   ["7",".",".",".","2",".",".",".","6"],
>   [".","6",".",".",".",".","2","8","."],
>   [".",".",".","4","1","9",".",".","5"],
>   [".",".",".",".","8",".",".","7","9"]
> ]
> 输出: true

示例 2:

> 输入:
> [
>   ["8","3",".",".","7",".",".",".","."],
>   ["6",".",".","1","9","5",".",".","."],
>   [".","9","8",".",".",".",".","6","."],
>   ["8",".",".",".","6",".",".",".","3"],
>   ["4",".",".","8",".","3",".",".","1"],
>   ["7",".",".",".","2",".",".",".","6"],
>   [".","6",".",".",".",".","2","8","."],
>   [".",".",".","4","1","9",".",".","5"],
>   [".",".",".",".","8",".",".","7","9"]
> ]
> 输出: false
> 解释: 除了第一行的第一个数字从 5 改为 8 以外，空格内其他数字均与 示例1 相同。
>      但由于位于左上角的 3x3 宫内有两个 8 存在, 因此这个数独是无效的。

说明:

> 一个有效的数独（部分已被填充）不一定是可解的。
> 只需要根据以上规则，验证已经填入的数字是否有效即可。
> 给定数独序列只包含数字 1-9 和字符 '.' 。
> 给定数独永远是 9x9 形式的。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/valid-sudoku
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一

最简单的一种思路就是模拟，因为数独大小固定为`9x9`，根据题目的三个要求利用`unordered_set`依次检查一遍数独即可。需要注意的是关于9个区域的判断，设置了一个`3x3`大小的`set`，对于每个`nums[i][j]`均属于区域`[i/3][j/3]`，代码如下：

```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        // 上 → 下
        for(int i=0; i<9; i++){
            unordered_set<char> s;
            for(int j=0; j<9; j++){
                char c = board[i][j];
                if(c!='.'){
                    if(s.find(c)!=s.end())  return false;
                    s.insert(c);
                }
            }
        }
        // 左 → 右
        for(int j=0; j<9; j++){
            unordered_set<char> s;
            for(int i=0; i<9; i++){
                char c = board[i][j];
                if(c!='.'){
                    if(s.find(c)!=s.end())  return false;
                    s.insert(c);
                }
            }
        }
        // 9个区域
        vector<vector<unordered_set<char>>> s(3, vector<unordered_set<char>>(3));
        for(int i=0; i<9; i++){
            for(int j=0; j<9; j++){
                char c=board[i][j];
                if(c!='.'){
                    if(s[i/3][j/3].find(c)!=s[i/3][j/3].end()) return false;
                    s[i/3][j/3].insert(c);
                }
            }
        }
        return true;
    }
};
```

时间复杂度：O(1)，大小固定

空间复杂度：O(1)，大小固定

## 解法二

也可以只遍历一次数组，利用三组`unordered_set`来存储三种情况中`1-9`的出现情况，代码如下：

```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        vector<unordered_set<char>> raw(9);
        vector<unordered_set<char>> col(9);
        vector<vector<unordered_set<char>>> block(3,vector<unordered_set<char>>(3));
        for(int i=0; i<9; i++){
            for(int j=0; j<9; j++){
                char c=board[i][j];
                if(c=='.') continue;
                if(raw[i].find(c)!=raw[i].end() || col[j].find(c)!=col[j].end() || block[i/3][j/3].find(c)!=block[i/3][j/3].end())  return false;
                raw[i].insert(c);
                col[j].insert(c);
                block[i/3][j/3].insert(c);
            }
        }
        return true;
    }
};
```

时间复杂度：O(1)

空间复杂度：O(1)