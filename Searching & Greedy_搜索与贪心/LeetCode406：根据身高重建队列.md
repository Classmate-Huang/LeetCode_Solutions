# LeetCode406：根据身高重建队列

 [toc]

## 题目

假设有打乱顺序的一群人站成一个队列，数组 people 表示队列中一些人的属性（不一定按顺序）。每个 people[i] = [hi, ki] 表示第 i 个人的身高为 hi ，前面 正好 有 ki 个身高大于或等于 hi 的人。

请你重新构造并返回输入数组 people 所表示的队列。返回的队列应该格式化为数组 queue ，其中 queue[j] = [hj, kj] 是队列中第 j 个人的属性（queue[0] 是排在队列前面的人）。

 

示例 1：

```
输入：people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
输出：[[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]
解释：
编号为 0 的人身高为 5 ，没有身高更高或者相同的人排在他前面。
编号为 1 的人身高为 7 ，没有身高更高或者相同的人排在他前面。
编号为 2 的人身高为 5 ，有 2 个身高更高或者相同的人排在他前面，即编号为 0 和 1 的人。
编号为 3 的人身高为 6 ，有 1 个身高更高或者相同的人排在他前面，即编号为 1 的人。
编号为 4 的人身高为 4 ，有 4 个身高更高或者相同的人排在他前面，即编号为 0、1、2、3 的人。
编号为 5 的人身高为 7 ，有 1 个身高更高或者相同的人排在他前面，即编号为 1 的人。
因此 [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]] 是重新构造后的队列。


```

示例 2：

```
输入：people = [[6,0],[5,0],[4,0],[3,2],[2,2],[1,4]]
输出：[[4,0],[5,0],[2,2],[3,2],[1,4],[6,0]]
```


提示：

```
1 <= people.length <= 2000
0 <= hi <= 106
0 <= ki < people.length
题目数据确保队列可以被重建
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/queue-reconstruction-by-height
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一：排序+遍历（从低到高）

先对队列中所有人按身高`hj`从低到高进行排序，其中，同一身高的按照`kj`从高到低排序。优先安排前面的人，比如`[[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]`排序后为`[4,4],[5,2],[5,0],[6,1],[7,1],[7,0]`。

然后构造`n`个位置的队列`queue`，对于`[4,4]`表明他前面有4个比他高/同等身高的人在它前面，所以前面要留4个空位置，也就是安排到`queue[4]`。第二个人`[5,2]`表示前面有2个满足要求的人在他前面，安排到`queue[3]`，同理即可安排所有人。

代码如下：

```c++
class Solution {
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        int n = people.size();
        if(n<2) return people;	// 空队列/1人队列 直接返回

        vector<vector<int>> ans(n, vector<int>(2,-1));	// ans 队列

        sort(people.begin(), people.end(), [](vector<int> a, vector<int> b){	// 排序
            if(a[0]==b[0]){
                return a[1]>b[1];
            }
            else    return a[0] < b[0];
        });

        for(auto p:people){	// 遍历
            int idx = 0;
            int t = p[1];
            while(t){	// 留空位
                if(ans[idx][0]==-1){
                    idx++;
                    t--;
                }
                else idx++;
            }
            while(ans[idx][0]!=-1) idx++;	// 找空位坐下
			// 赋值
            ans[idx][0] = p[0];
            ans[idx][1] = p[1];
        }

        return ans;
    }
};
```

时间复杂度：O(n<sup>2</sup>) ，找位置时的遍历

空间复杂度：O(logn)，排序所需栈空间

代码优化后，如下：

```c++
class Solution {
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        int n = people.size();
        if(n<2) return people;
        sort(people.begin(), people.end(), [](vector<int> &a, vector<int> &b){
            return a[0]<b[0] || (a[0]==b[0] && a[1]>b[1]);
        });

        vector<vector<int>> ans(n);
        for(auto p:people){
            int space = p[1]+1;
            for(int i=0; i<n; i++){
                if(ans[i].empty()){
                    space--;
                    if(!space){
                        ans[i]=p;
                        break;
                    }
                }
            }
        }
        
        return ans;
    }
};
```

## 解法二：排序+遍历（从高到低）

与解法一相反，也可以从高到矮进行排序，按身高`hj`从高到低进行排序，其中，同一身高的按照`kj`从低到高排序。优先安排前面的人，比如`[[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]`排序后为`[7,0],[7,1],[6,1],[5,0],[5,2],[4,4]`。

然后构造`n`个位置的队列`queue`，对于`[7,0]`表明直接安排到`queue[0]`。第二个人`[7,1]`表示前面有1个满足要求的人在他前面，插到当前队列第1个人的后面`queue[1]`；`[6,1]`表示前面有1个满足要求的人在他前面，则插入当前队列第1个人的后面`queue[1]`，同理安排所有人。

利用插入操作，代码如下：

```c++
class Solution {
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        int n = people.size();
        if(n<2) return people;
        sort(people.begin(), people.end(), [](vector<int> &a, vector<int> &b){
            return a[0]>b[0] || (a[0]==b[0] && a[1]<b[1]);
        });

        vector<vector<int>> ans;

        for(auto p:people){
            ans.insert(ans.begin()+p[1], p);
        }
        
        return ans;
    }
};
```

时间复杂度：O(n<sup>2</sup>) ，找位置时的遍历

空间复杂度：O(logn)，排序所需栈空间

