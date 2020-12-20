# LeetCode384：打乱数组

## 题目

给你一个整数数组 nums ，设计算法来打乱一个没有重复元素的数组。

实现 Solution class:

```
Solution(int[] nums) 使用整数数组 nums 初始化对象
int[] reset() 重设数组到它的初始状态并返回
int[] shuffle() 返回数组随机打乱后的结果
```


示例：

```
输入
["Solution", "shuffle", "reset", "shuffle"]
[[[1, 2, 3]], [], [], []]
输出
[null, [3, 1, 2], [1, 2, 3], [1, 3, 2]]

解释
Solution solution = new Solution([1, 2, 3]);
solution.shuffle();    // 打乱数组 [1,2,3] 并返回结果。任何 [1,2,3]的排列返回的概率应该相同。例如，返回 [3, 1, 2]
solution.reset();      // 重设数组到它的初始状态 [1, 2, 3] 。返回 [1, 2, 3]
solution.shuffle();    // 随机返回数组 [1, 2, 3] 打乱后的结果。例如，返回 [1, 3, 2]
```


提示：

1 <= nums.length <= 200
-106 <= nums[i] <= 106
nums 中的所有元素都是 唯一的
最多可以调用 5 * 104 次 reset 和 shuffle

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shuffle-an-array
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一：洗牌算法

本题的关键问题在于如果构造真正随机打乱的结果？这句话什么意思呢。假设目前传入的数组为`[1,2,3]`，根据排列组合的公式，随机打乱的结果只有六种`A(3,3)=6`，分别为`[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]`，并且这六种组合出现的概率是相等的，都为`1/6`。我猜测LeetCode后台的测试也是根据概率统计来进行判断正误的（大数定理-蒙特卡洛统计）。

这里介绍`随机乱置算法`，或者叫`洗牌算法`，来解决这类问题。洗牌法很简单，核心代码如下：

```c++
vector<int> data;  // 需要打乱的数组
int n = data.size();

for(int i=0; i<n; i++){
    t = randint(i,n-1);		// randint(int, int) 获取[i,n-1]之间的随机值
    swap(data[i], data[t]);		// 交换
}
```

为啥这样做就是对的呢？其实不难理解，每次打乱，我们都从第一位开始做决策`选择交换元素`，假设`[1,2,3]`，那么经过一次洗牌算法，它能够出现的交换种类为`3x2x1`，为`n!`种，这样就满足每种排列随机出现的概率相等。完整代码如下：

```c++
class Solution {
public:
    vector<int> raw_data;	// 保存原数组
    vector<int> data;	// 当前数组
    int n;

    Solution(vector<int>& nums) {
        data = nums;
        raw_data.assign(nums.begin(), nums.end());
        n = nums.size();
    }
    
    /** Resets the array to its original configuration and return it. */
    vector<int> reset() {	// 将data更新，并返回原数组
        data.assign(raw_data.begin(), raw_data.end());
        return raw_data;
    }
    
    int randint(int low, int high){		// 获取随机数，rand()为伪随机数
        return rand()%(high-low+1)+low;
    }

    /** Returns a random shuffling of the array. */
    vector<int> shuffle() {		// 洗牌算法
        for(int i=0; i<n; i++){
            int t = randint(i, n-1);
            swap(data[i], data[t]);
        }
        return data;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(n)