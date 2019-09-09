# 2. Backpack 0-1 knapsack problem

## Description
There are n items and a backpack with size m. Given array A representing the size of each item and array V representing the value of each item.

## Example I

Input: m = 10, A = [2, 3, 5, 7], V = [1, 5, 2, 4]
Output: 9
Explanation: Put A[1] and A[3] into backpack, getting the maximum value V[1] + V[3] = 9 

## Example II
Input: m = 10, A = [2, 3, 8], V = [2, 5, 8]
Output: 10
Explanation: Put A[0] and A[2] into backpack, getting the maximum value V[0] + V[2] = 10 


## Questions
https://www.lintcode.com/problem/backpack-ii/description

## Analysis

| Weight / Value (i)  | 0  |  1  |  2  |  3  |  4  |  5  |  6  |  7  |  8  |  9  | 10  | 11  |
| --------------------|---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| w1 = 1 v1 = 1       | 0  |  1  |  1  |  1  |  1  |  1  |  1  |  1  |  1  |  1  |  1  |  1  |
| w2 = 2 v2 = 6       | 0  |  1  |  6  |  7  |  7  |  7  |  7  |  7  |  7  |  7  |  7  |  7  |
| w3 = 5 v3 = 18      | 0  |  1  |  6  |  7  |  7  |  18 |  19 |  24 |  25 |  25 |  25 |  25 |
| w4 = 6 v3 = 22      | 0  |  1  |  6  |  7  |  7  |  18 |  22 |  24 |  28 |  29 |  29 |  40 |
| w5 = 7 v5 = 28      | 0  |  1  |  6  |  7  |  7  |  18 |  22 |  28 |  29 |  34 |  35 |  40 |


## Transform Function

```
dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - wt[i - 1]] + val[i - 1])
```

## Template

```Java
class Solution {
    public int backpack(int w, int[] wt, int[] val) {
        int n  = wt.length;
        int[][] dp = new int[n + 1][w + 1];
        
        for(int i = 1; i <= n; i++) {
            for(int j = 1; j <= w; j++) {
                if(j >= wt[i - 1]) {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - wt[i - 1]] + val[i - 1]);
                }else{
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
        
        return dp[n][w];
    }
}
```

## Optimize 
```java
class Solution {
    public int backpack(int w, int[] wt, int[] val) {
        int n = wt.length;
        int[] dp = new int[w + 1];
        
        for(int i = 1; i <= n; i++) {
            for(int j = w; j >= 0; j--) {
                if(j >= wt[i - 1]) {
                    dp[j] = Math.max(dp[j], dp[j - wt[i - 1]] + val[i - 1]);
                }
            }
        }
        return dp[w];
    }   
}

```