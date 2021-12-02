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

