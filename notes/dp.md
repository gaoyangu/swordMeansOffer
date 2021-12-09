## 动态规划

## 42. 连续子数组的最大和

- [牛客网: JZ42 连续子数组的最大和](https://www.nowcoder.com/practice/459bd355da1549fa8a49e350bf3df484)
- [leetcode: 剑指 Offer 42. 连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

### 贪心算法

[1, -2, 3, 10, -4, 7, 2, -5]

- 初始化和为0
- 第一步加上数字1，和为1
- 第二步加上数字-2，和为-1
- 第三步加上数字3，和为2，比3本身还小

从第一个数字开始的子数组的和会小于从第三个数字开始的子数组的和

✅
```cpp
int FindGreatestSumOfSubArray(vector<int> array) {
    if(array.size() <= 0){
        return 0;
    }
    int nCurSum = 0;
    int nGreatestSum = INT_MIN;
    for(int i = 0; i < array.size(); i++){
        if(nCurSum <= 0){
            nCurSum = array[i];
        }else{
            nCurSum += array[i];
        }
        if(nCurSum > nGreatestSum){
            nGreatestSum = nCurSum;
        }
    }
    return nGreatestSum;
}
```

### 动态规划
✅
```cpp
int FindGreatestSumOfSubArray(vector<int> array) {
    int res = array[0];
    for(int i = 1; i < array.size(); i++){
        array[i] += max(array[i-1], 0);
        res = max(res, array[i]);
    }
    return res;
}
```

## 10. 斐波那契数列

题目一：求斐波那契数列的第n项
- [牛客网: JZ10 斐波那契数列](https://www.nowcoder.com/practice/c6c7742f5ba7442aada113136ddea0c3)

题目二：青蛙跳台阶问题
- [牛客网: JZ69 跳台阶](https://www.nowcoder.com/practice/8c82a5b80378478f9484d87d1c5f12a4)

扩展：
- [牛客网: JZ71 跳台阶扩展问题](https://www.nowcoder.com/practice/22243d016f6b47f2a6928b4313c85387)

相关题目：
- [牛客网: JZ70 矩形覆盖](https://www.nowcoder.com/practice/72a5a919508a4251859fb2cfb987a0e6)

### 效率很低的解法
- 存在大量的重复计算
```cpp
long long Fibonacci(unsigned int n) {
    if(n <= 0){
        return 0;
    }
    if(n == 1){
        return 1;
    }
    return  Fibonacci(n - 1) + Fibonacci(n - 2);
}
```

### 时间复杂度O(n)的解法
✅
```cpp
long long Fibonacci(unsigned int n) {
    int result[2] = {0, 1};
    if(n < 2){
        return result[n];
    }
    long long fibNMinusOne = 1;
    long long fibNMinusTwo = 0;
    long long fibN = 0;
    for(int i = 2; i <= n; i++){
        fibN = fibNMinusOne + fibNMinusTwo;
        fibNMinusTwo = fibNMinusOne;
        fibNMinusOne = fibN;
    }
    return fibN;
}
```

### 扩展

- [牛客网: JZ71 跳台阶扩展问题](https://www.nowcoder.com/practice/22243d016f6b47f2a6928b4313c85387)

暴力方法：
```cpp
int jumpFloorII(int n) {
    if(n == 0 || n == 1){
        return 1;
    }
    vector<int> dp(n+1, 0);
    dp[0] = 1;
    dp[1] = 1;
    for(int i = 2; i <= n; i++){
        for(int j = 0; j < i; j++){
            dp[i] += dp[j];
        }
    }
    return dp[n];
}
```

优化方法：
- 斜率优化

✅
```cpp
int jumpFloorII(int n) {
    if(n == 0 || n == 1){
        return 1;
    }
    int fibNMinusOne = 1;
    int fibN = 0;
    for(int i = 2; i <= n; i++){
        fibN = fibNMinusOne << 1;
        fibNMinusOne = fibN;
    }
    return fibN;
}
```

数学归纳法：f(n) = 2^(n-1)
```cpp
int jumpFloorII(int n) {
    if (n == 0 || n == 1) {
        return 1;
    }
    return 1<<(n-1);
}
```

## 60. n个骰子的点数

- [leetcode: 剑指 Offer 60. n个骰子的点数](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/)

分析：
- n个骰子的点数和的最小值为n，最大值为6n
- n个骰子的所有点数的排列数为6^n

### 解法一：基于递归求骰子点数，时间效率不够高

求n个骰子的点数和，可以先把n个骰子分成两堆：第一堆只有一个，另一堆有n-1个

- 单独的那一个有可能出现`1~6`的点数，需要计算`1~6`的每一种点数和剩下n-1个骰子来计算点数和

定义长度为 6n-n+1 的数组，将和为 s 的点数出现的次数保存在数组的第 s-n 个元素中

```cpp
int g_maxValue = 6;
vector<double> dicesProbability(int n) {
    vector<double> res;
    if(n < 1){
        return res;
    }
    int maxSum = n * g_maxValue;
    int* pProbabilities = new int[maxSum - n + 1];
    for(int i = n; i <= maxSum; i++){
        pProbabilities[i-n] = 0;
    }

    Probability(n, pProbabilities);

    int total = pow((double)g_maxValue, n);
    for(int i = n; i <= maxSum; i++){
        double ratio = (double)pProbabilities[i - n] / total;
        res.push_back(ratio);
    }

    delete [] pProbabilities;
    
    return res;
}

void Probability(int n, int* pProbabilities){
    for(int i = 1; i <= g_maxValue; i++){
        Probability(n, n, i, pProbabilities);
    }
}
// 可变参数：
//      current: 还剩多少个骰子
//      sum: 当前的点数和
void Probability(int original, int current, int sum, int* pProbabilities){
    if(current == 1){
        pProbabilities[sum - original]++;
    }else{
        for(int i = 1; i <= g_maxValue; i++){
            Probability(original, current-1, i+sum, pProbabilities);
        }
    }
}
```

### 解法二：基于循环求骰子点数，时间性能好

- 考虑用两个数组来存储骰子点数的每个总数出现的次数
- 在第一轮循环中，第一个数组中的第 n 个数字表示骰子和为 n 出现的次数
- 在下一轮循环中，加上一个新的骰子，此时和为 n 的骰子出现的次数应该等于上一轮循环中骰子点数和为 n-1、n-2、n-3、n-4、n-5、n-6 的次数的总和


```cpp
int g_maxValue = 6;
vector<double> dicesProbability(int n) {
    vector<double> res;
    if(n < 1){
        return res;
    }

    int* pProbabilities[2];
    pProbabilities[0] = new int[g_maxValue*n + 1];
    pProbabilities[1] = new int[g_maxValue*n + 1];
    for(int i = 0; i < g_maxValue*n + 1; i++){
        pProbabilities[0][i] = 0;
        pProbabilities[1][i] = 0;
    }

    int flag = 0;
    for(int i = 1; i <= g_maxValue; i++){
        pProbabilities[flag][i] = 1;
    }
    for(int k = 2; k <= n; k++){
        for(int i = 0; i < k; i++){
            pProbabilities[1-flag][i] = 0;
        }
        for(int i = k; i <= g_maxValue*k; i++){
            pProbabilities[1-flag][i] = 0;
            for(int j = 1; j <= i && j <= g_maxValue; j++){
                pProbabilities[1-flag][i] += pProbabilities[flag][i-j];
            }
        }
        flag = 1 - flag;
    }

    int total = pow((double)g_maxValue, n);
    for(int i = n; i <= g_maxValue*n; i++){
        double ratio = (double)pProbabilities[flag][i] / total;
        res.push_back(ratio);
    }

    delete [] pProbabilities[0];
    delete [] pProbabilities[1];
    return res;
}
```

## 14. 剪绳子

- [牛客网: 剑指 Offer 14. 剪绳子](https://www.nowcoder.com/practice/57d85990ba5b440ab888fc72b0751bf8)
- [leetcode: 剑指 Offer 14- I. 剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)

### 暴力递归

```cpp
int cutRope(int number) {
    if(number == 2){
        return 1;
    }else if(number == 3){
        return 2;
    }
    return process(number);
}
int process(int n){
    if(n <= 4){
        return n;
    }
    int res = INT_MIN;
    for(int i = 1; i < n; i++){
        res = max(res, i*process(n-i));
    }
    return res;
}
```

### 记忆化递归

```cpp
int cutRope(int number) {
    if(number == 2){
        return 1;
    }else if(number == 3){
        return 2;
    }
    vector<int> dp(number+1, -1);
    return process(number, dp);
}
int process(int n, vector<int>& dp){
    if(n <= 4){
        return n;
    }
    if(dp[n] != -1){
        return dp[n];
    }
    int res = INT_MIN;
    for(int i = 1; i < n; i++){
        res = max(res, i*process(n-i, dp));
    }
    dp[n] = res;
    return dp[n];
}
```

### 动态规划

```cpp
int cutRope(int number) {
    if(number == 2){
        return 1;
    }else if(number == 3){
        return 2;
    }
    vector<int> dp(number+1, -1);
    for(int i = 1; i<= 4; i++){
        dp[i] = i;
    }
    for(int i = 5; i <= number; i++){
        for(int j = 1; j < i; j++){
            dp[i] = max(dp[i], j*dp[i-j]);
        }
    }
    return dp[number];
}
```

- 剑指offer
```cpp
int cutRope(int number) {
    if(number < 2){
        return 0;
    }else if(number == 2){
        return 1;
    }else if(number == 3){
        return 2;
    }
    int* dp = new int[number+1];
    dp[0] = 0;
    dp[1] = 1;
    dp[2] = 2;
    dp[3] = 3;
    int max = 0;
    for(int i = 4; i <= number; i++){
        max = 0;
        for(int j = 1; j <= i/2; j++){
            int product = dp[j]*dp[i-j];
            max = max < product ? product : max;
            dp[i] = max;
        }
    }
    max = dp[number];
    delete [] dp;
    return max;
}
```

### 贪心算法

当 n >= 5时，尽可能多地剪长度为3的绳子；当剩下的绳子长度为4时，把绳子剪成两段长度为2的绳子。
```cpp
int cutRope(int number) {
    if(number < 2){
        return 0;
    }else if(number == 2){
        return 1;
    }else if(number == 3){
        return 2;
    }
    // 尽可能多的剪去长度为3的绳子段
    int timesOf3 = number / 3;
    // 当最后剩下的长度为4时，把绳子剪成长度为2的两段
    if(number - timesOf3*3 == 1){
        timesOf3 -= 1;
    }
    int timesOf2 = (number - timesOf3*3)/2;
    return (int)(pow(3, timesOf3))*(int)(pow(2, timesOf2));
}
```

## 47. 礼物的最大价值

- [牛客网：JZ47 礼物的最大价值](https://www.nowcoder.com/practice/2237b401eb9347d282310fc1c3adb134)
- [leetcode: 剑指 Offer 47. 礼物的最大价值](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/)

### 暴力递归
```cpp
int maxValue(vector<vector<int>>& grid) {
    // write code here
    int m = grid.size()-1;
    int n = grid[0].size()-1;
    return process(m, n, 0, 0, 0, grid);
}
int process(int m, int n, int x, int y, int sum, const vector<vector<int>>& grid){
    if(x > m || y > n){
        return -1;
    }
    if(x == m && y == n){
        return grid[x][y] + sum;
    }
    return max(grid[x][y]+process(m, n, x+1, y, sum, grid), 
                grid[x][y]+process(m, n, x, y+1, sum, grid));
}
```

### 记忆化搜索
```cpp
int maxValue(vector<vector<int>>& grid) {
    // write code here
    int m = grid.size()-1;
    int n = grid[0].size()-1;
    vector<vector<int>> dp;
    for(int i = 0; i <= m; i++){
        vector<int> tmp;
        for(int j = 0; j <= n; j++){
            tmp.push_back(-1);
        }
        dp.push_back(tmp);
    }
    return process(m, n, 0, 0, 0, grid, dp);
}
int process(int m, int n, int x, int y, int sum, const vector<vector<int>>& grid,
            vector<vector<int>>& dp){
    if(x > m || y > n){
        return -1;
    }
    if(dp[x][y] != -1){
        return dp[x][y];
    }
    if(x == m && y == n){
        dp[x][y] = grid[x][y] + sum;
        return dp[x][y];
    }
    dp[x][y] = max(grid[x][y]+process(m, n, x+1, y, sum, grid, dp), 
                grid[x][y]+process(m, n, x, y+1, sum, grid, dp));
    return dp[x][y];
}
```

### 动态规划
```cpp
int maxValue(vector<vector<int> >& grid) {
    // write code here
    int m = grid.size();
    int n = grid[0].size();
    return process(grid, m, n);
}
int process(const vector<vector<int>>& grid, int m, int n){
    if(grid.size() < 0 || m <= 0 || n <= 0){
        return 0;
    }
    vector<int> maxValues(n, 0);
    for(int i = 0; i < m; i++){
        for(int j = 0; j < n; j++){
            int left = 0;
            int up = 0;
            if(i > 0){
                up = maxValues[j];
            }
            if(j > 0){
                left = maxValues[j-1];
            }
            maxValues[j] = max(left, up) + grid[i][j];
        }
    }
    return maxValues[n-1];
}
```
