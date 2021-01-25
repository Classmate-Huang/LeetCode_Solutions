# LeetCode452：用最少数量的箭引爆气球

## 题目

在二维空间中有许多球形的气球。对于每个气球，提供的输入是水平方向上，气球直径的开始和结束坐标。由于它是水平的，所以纵坐标并不重要，因此只要知道开始和结束的横坐标就足够了。开始坐标总是小于结束坐标。

一支弓箭可以沿着 x 轴从不同点完全垂直地射出。在坐标 x 处射出一支箭，若有一个气球的直径的开始和结束坐标为 xstart，xend， 且满足  xstart ≤ x ≤ xend，则该气球会被引爆。可以射出的弓箭的数量没有限制。 弓箭一旦被射出之后，可以无限地前进。我们想找到使得所有气球全部被引爆，所需的弓箭的最小数量。

给你一个数组 points ，其中 points [i] = [xstart,xend] ，返回引爆所有气球所必须射出的最小弓箭数。


示例 1：

```
输入：points = [[10,16],[2,8],[1,6],[7,12]]
输出：2
解释：对于该样例，x = 6 可以射爆 [2,8],[1,6] 两个气球，以及 x = 11 射爆另外两个气球
```

示例 2：

```
输入：points = [[1,2],[3,4],[5,6],[7,8]]
输出：4
```

示例 3：

```
输入：points = [[1,2],[2,3],[3,4],[4,5]]
输出：2
```

示例 4：

```
输入：points = [[1,2]]
输出：1
```

示例 5：

```
输入：points = [[2,3],[2,3]]
输出：1
```


提示：

```
0 <= points.length <= 104
points[i].length == 2
-231 <= xstart < xend <= 231 - 1
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一：排序+贪心

类似于区间安排问题，按照气球的x<sub>end</sub>进行升序排序，判断有多少个区间存在冲突(区间重叠)。从前往后遍历，冲突区间内的气球可用一根箭引爆。代码如下：

```c++
class Solution {
public:
    
    int findMinArrowShots(vector<vector<int>>& points) {
        int n = points.size(), ans = n;
        if(n<2) return ans;

        sort(points.begin(), points.end(), [](const vector<int>& u, const vector<int>& v) {	// 排序
            return u[1] < v[1];
        });

        int end_time = points[0][1];  // 冲突区间x_end
        for(int i=1; i<n; i++){
            if(end_time >= points[i][0]) ans--;	 // 在同一个冲突区间内每多一个气球，便可少用一支箭
            else end_time = points[i][1];	// 更新冲突区间
        }

        return ans;	// 返回结果
    }
};
```

时间复杂度：O(nlogn)，排序算法

空间复杂度：O(logn)，排序所需栈空间

