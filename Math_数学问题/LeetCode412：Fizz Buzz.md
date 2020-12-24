# LeetCode412：Fizz Buzz

## 题目

写一个程序，输出从 1 到 n 数字的字符串表示。

1. 如果 n 是3的倍数，输出“Fizz”；
2. 如果 n 是5的倍数，输出“Buzz”；
3. 如果 n 同时是3和5的倍数，输出 “FizzBuzz”。



示例：

```
n = 15,

返回:
[
    "1",
    "2",
    "Fizz",
    "4",
    "Buzz",
    "Fizz",
    "7",
    "8",
    "Fizz",
    "Buzz",
    "11",
    "Fizz",
    "13",
    "14",
    "FizzBuzz"
]
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/fizz-buzz
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法一：直接条件判断

最简单的条件判断，代码如下：

```c++
class Solution {
public:
    vector<string> fizzBuzz(int n) {
        vector<string> ans;
        for(int i=1; i<=n; i++){
            if(i%15==0) ans.push_back("FizzBuzz");
            else if(i%3==0) ans.push_back("Fizz");
            else if(i%5==0) ans.push_back("Buzz");
            else    ans.push_back(to_string(i));
        }
        return ans;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)

## 解法二：通用解法

试想一下当题设条件变多，解法一就要写大量的判断条件代码。而解法二虽然增加了空间复杂度，但是使得编码过程变得优雅，代码如下：

```c++
class Solution {
public:
    vector<string> fizzBuzz(int n) {
        map<int, string> m = {{3, "Fizz"}, {5, "Buzz"}};
        vector<string> ans;

        for(int i=1; i<=n; i++){
            string t="";
            for(auto e:m){
                if(i%e.first==0)    t += e.second;
            }
            if(t.size()==0) t += to_string(i);
            ans.push_back(t);
        }

        return ans;
    }
};
```

