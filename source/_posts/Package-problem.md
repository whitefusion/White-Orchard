---
title: 01 Package problem
date: 2018-06-02 19:56:01
tags: 
    - algorithm 
    - c++
    - dp
---
背包九讲：[01背包问题][1]


实例原帖：[Solving 0/1 knapsack problem][2]


《背包九讲》没有实例无法真正理解，所以找了实例与大家分享。


---
## 问题 ##

> 你的背包容量为 Capacity, 现在有n个物品，物品重量 w[n], 物品价值 v[n]。如何选择物品价值最大？


特点：每种物品仅有一件，可以选择放或不放, 即01.

实例输入：背包承重量10，有5件物品，重量[2,3,3,4,6], 价值[1,2,5,9,4].

## 动态规划表格 ##
![01_package][3]

### 表格说明 ###

 - 第一行： 表示最大允许重量 0 ～ 10。
    - 比如7那一列 表示我们将背包的最大承重量当作7来求解。
 - 第一列： 表示物品的 重量（价值）。
    - 比如第四行 3（5）： 我们只考虑物品集合{ 2(1)，3(2)，3(5)}，剩下的 4（9），6（4） 当做不存在。
 - 表格中具体某一个数，表示当前行列条件下能获取的最大价值。
    - 比如第五行=4（9）第八列=6， 意味着当背包最大承重量为6，从 2（1），3（2），3（5），4（9）中挑选物品可以获取的最大价值，即10.

### 如何生成表格 ###
比如最大承重量为5，第三行只考虑2（1），3（2）的情况：
   - 如果不拿3（2）， 那么获取的价值可以从上一行得到，即只有2（1）的情况，结果为1.
   - 如果拿3（2），那么获取的价值为 2 + 最大承重量为(5-3)=2的时候拿除了3（2）的物品能获得的最大价值，这个数值能从上一行最大承重2的那列获取，结果为3.

综上，应该拿3（2），最大价值为3.

### 转换关系 ###
第i行，j列的数值应该等于： max (dp\[i-1][j], v[i]+ dp\[i-1][j-w[i]])

##代码##

```cpp

#include <iostream>
#include <vector>

using namespace std;

// Input
// 5(n) 10(capacity)
// 1 2 5 9 4 (v[n])
// 2 3 3 4 6 (w[n])

int maxValue (int n , int c , vector<int> v, vector<int> w){
    int **dp = new int*[n];

    for(int i = 0; i < n; i++){
        dp[i] = new int[c+1];
        for(int j = 0 ; j <= c; j++)
            dp[i][j] = 0;
    }

    // Populate the first row
    for(int j = 0 ; j <= c ; j++)
        dp[0][j] += j >= w[0] ? v[0] : 0;

    // Populate the rest
    for(int i = 1 ; i < n; i++ ){
        for(int j = 0; j <= c; j++){
            if(j<w[i]) dp[i][j] = dp[i-1][j];
            else dp[i][j] = max(v[i]+dp[i-1][j-w[i]], dp[i-1][j]);
        }
    }

    return dp[n-1][c];
}

int main() {
    int n = 0, capa = 0;
    cin >> n >> capa;

    vector<int> values(n,0);
    vector<int> weights(n,0);

    for(int i = 0; i < n ; i++)
        cin >> values[i];

    for(int j = 0; j < n; j++)
        cin >> weights[j];

    cout << maxValue (n,capa,values,weights) << endl;

    return 0;
}
```

### 优化解法
以上方法的空间效率为O(n*c)，还有一个优化空间效率到O(c)的方法。<br>
原理是我们发现每次更新一个表格cell的时候实际上只用到前一行和当前行，所以只要用一个O(c)大小的数列去维护就可以了。<br>
需要注意的是，如果从前往后遍历更新的话那么前一行的数据就会被本行的数据覆盖了，所以应该从后往前遍历。<br>

### 优化解法代码
```cpp
#include <iostream>
#include <vector>

using namespace std;

int maxValue(int n, int c, vector<int> v, vector<int> w) {
    int* dp = new int[c+1];
    for(int i = 0; i <= c; ++i)
        dp[i] = i >= w[0] ? v[0] : 0;

    for(int i = 1; i < n; ++i){
        for(int j = c; j >= 0; --j){
            if(j >= w[i])
                dp[j] = max(dp[j],dp[j-w[i]]+v[i]);
        }
    }

    return dp[c];
}

int main() {
    int n = 0, capa = 0;
    cin >> n >> capa;

    vector<int> values(n,0);
    vector<int> weights(n,0);

    for(int i = 0; i < n; i++)
        cin >> values[i];

    for(int j = 0; j < n; j++)
        cin >> weights[j];

    cout << maxValue(n,capa, values, weights) << endl;
    return 0;
}
```

### 延伸：物品能重复拿
上面的题之所以要从后往前遍历其实就是为了避免重复，所以能重复拿的情况就只要从前往后遍历就可以了。


  [1]: http://love-oriented.com/pack/P01.html
  [2]: http://techieme.in/solving-01-knapsack-problem-using-dynamic-programming/
  [3]: ../images/01_package.png