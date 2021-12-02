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
