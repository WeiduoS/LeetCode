# 3. Backpack unbounded knapsack problem (UKP)

## Description
Given n items with size wt_i and value v_i (i : 0 ~ n - 1), an integer W denotes the size of a backpack. Each item you can pick multiple times, How full you can fill this backpack to maximize the total value?

## Example I

Input: A = [2, 3, 5, 7], V = [1, 5, 2, 4], m = 10
Output: 15
Explanation: Put three item 1 (A[1] = 3, V[1] = 5) into backpack.

## Example II
Input: A = [1, 2, 3], V = [1, 2, 3], m = 5
Output: 5
Explanation: Strategy is not unique. For example, put five item 0 (A[0] = 1, V[0] = 1) into backpack.

## Question
https://www.lintcode.com/problem/backpack-iii/description

## Analysis

| Weight Limit(i)  | 0  |  1  |  2  |  3  |  4  |  5  |  6  |  7  |  8  |  9  | 10  |
| -----------------|---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| w1 = 2  v1 = 1   | 0  |  0  |  1  |  1  |  2  |  2  |  3  |  3  |  4  |  4  |  5  |
| w2 = 3  v2 = 5   | 0  |  0  |  1  |  5  |  5  |  6  |  10 |  10 |  11 |  15 |  15 |
| w3 = 5  v3 = 2   | 0  |  0  |  1  |  5  |  5  |  6  |  10 |  10 |  11 |  15 |  15 |
| w4 = 7  v4 = 4   | 0  |  0  |  1  |  5  |  5  |  6  |  10 |  10 |  11 |  15 |  15 |


## Transform Function

```
dp[i][j] = Math.max(dp[i - 1][j - k * wt[i - 1]] + k * val[i - 1] | k >= 0)
```

## Template

```Java
class Solution {
    public int backpack(int w, int[] wt, int[] val) {
        int n  = wt.length;
        int[][] dp = new int[n + 1][w + 1];
        
        for(int i = 1; i <= n; i++) {
            for(int j = 1; j <= w; j++) {
                for(int k = 0; k * wt[i - 1] <= j; k++) {
                    dp[i][j] = Math.max(dp[i][j], dp[i - 1][j - k * wt[i - 1]] + k * val[i - 1]);
                }
            }
        }
        
        return dp[n][w];
    }
}
```

## Optimize Transform Function

```
dp[i][j] = Math.max(dp[i - 1][j - k * wt[i - 1]] + k * val[i - 1] | k >= 0)
dp[i][j] = Math.max(dp[i - 1][j], max{dp[i - 1][j - k * wt[i - 1] + k * val[i - 1] | k >= 1})
dp[i][j] = Math.max(dp[i - 1][j], max{dp[i - 1][(j - w[i - 1]) - k * wt[i - 1] + k * val[i - 1] | k >= 0} + val[i - 1])
dp[i][j] = Math.max(dp[i - 1][j], max{dp[i][j - w[i - 1] + val[i - 1])
```

## Optimize I
```java
class Solution {
    public int backpack(int w, int[] wt, int[] val) {
        int n = wt.length;
        int[][] dp = new int[n + 1][w + 1];
        
        for(int i = 1; i <= n; i++) {
            for(int j = 1; j <= w; j++) {
                if(j < wt[i - 1]) {
                    dp[i][j] = dp[i - 1][j];
                }else{
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - wt[i - 1]] + val[i - 1]);
                }
            }
        }
        return dp[n][w];
    }
}
```

## Optimize II
```java
class Solution {
    public int backpack(int w, int[] wt, int[] val) {
        int n = wt.length;
        int[] dp = new int[w + 1];
        
        for(int i = 1; i <= n; i++) {
            for(int j = wt[i - 1]; j <= w; j++) {
                dp[j] = Math.max(dp[j], dp[j - wt[i - 1]] + val[i - 1]);
            }
        }
        return dp[w];
    }
}
```