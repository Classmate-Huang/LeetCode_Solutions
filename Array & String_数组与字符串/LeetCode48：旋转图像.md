# LeetCode48：旋转图像

## 题目

给定一个 n × n 的二维矩阵表示一个图像。

将图像顺时针旋转 90 度。



说明：

你必须在原地旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要使用另一个矩阵来旋转图像。



示例 1:

> 给定 matrix = 
> [
>   [1,2,3],
>   [4,5,6],
>   [7,8,9]
> ],
>
> 原地旋转输入矩阵，使其变为:
> [
>   [7,4,1],
>   [8,5,2],
>   [9,6,3]
> ]

示例 2:

> 给定 matrix =
> [
>   [ 5, 1, 9,11],
>   [ 2, 4, 8,10],
>   [13, 3, 6, 7],
>   [15,14,12,16]
> ], 
>
> 原地旋转输入矩阵，使其变为:
> [
>   [15,13, 2, 5],
>   [14, 3, 4, 1],
>   [12, 6, 8, 9],
>   [16, 7,10,11]
> ]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/rotate-image
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一

直接从矩阵的层面来分析，顺时针旋转90度相当于将矩阵进行转置变换，然后对每一行的元素取逆。

```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n=matrix.size();
        for(int i=0; i<n; i++){
            for(int j=i+1; j<n; j++)
                swap(matrix[i][j], matrix[j][i]);
        }
        for(int i=0; i<n; i++) 
            reverse(matrix[i].begin(), matrix[i].end());
    }
};
```

时间复杂度：O(n<sup>2</sup>)

空间复杂度：O(1)

## 解法二

第二种解法就是利用`模拟`的思想，将整个过程实现一遍，思路可以参照官方代码的连接：https://leetcode-cn.com/problems/rotate-image/solution/xuan-zhuan-tu-xiang-by-leetcode/，里面包含图示，理解起来很方便，这里就不搬运了。与官方实现不同的是，我这里是从最外一圈往里一层一层进行模拟，可能更符合直觉逻辑，代码如下：

```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n=matrix.size();
        for(int i=0; i<(n+1)/2; i++){
            for(int j=i; j<n-1-i; j++){
                int t = matrix[i][j];
                matrix[i][j] = matrix[n-1-j][i];
                matrix[n-1-j][i] = matrix[n-1-i][n-1-j];
                matrix[n-1-i][n-1-j] = matrix[j][n-1-i];
                matrix[j][n-1-i] = t;
            }
        }
    }
};
```

时间复杂度：O(n<sup>2</sup>)

空间复杂度：O(1)