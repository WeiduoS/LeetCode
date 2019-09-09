# 1. Backpack 0-1 knapsack problem

## Description
Given n items with size Ai, an integer m denotes the size of a backpack. How full you can fill this backpack?

## Question Link
https://www.lintcode.com/problem/backpack/description

## Example

Example 1:
	Input:  [3,4,8,5], backpack size=10
	Output:  9

Example 2:
	Input:  [2,3,5,7], backpack size=12
	Output:  12

## Analysis

| Weight Limit(i)  | 0  |  1  |  2  |  3  |  4  |  5  |  6  |  7  |  8  |  9  | 10  | 11  |
| -----------------|---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| w1 = 2           | 0  |  0  |  2  |  2  |  2  |  2  |  2  |  2  |  2  |  2  |  2  |  2  |
| w2 = 3           | 0  |  0  |  2  |  3  |  3  |  5  |  5  |  5  |  5  |  5  |  5  |  5  |
| w3 = 5           | 0  |  0  |  2  |  3  |  3  |  5  |  5  |  7  |  8  |  8  |  10 |  10 |
| w4 = 7           | 0  |  0  |  2  |  3  |  3  |  5  |  5  |  7  |  8  |  9  |  10 |  10 |



## Transform Function

```
dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - wt[i - 1]] + wt[i - 1])
```

## Template

```Java
class Solution {
    public int backpack(int w, int[] wt) {
        int n  = wt.length;
        int[][] dp = new int[n + 1][w + 1];
        
        for(int i = 1; i <= n; i++) {
            for(int j = 1; j <= w; j++) {
                if(j >= wt[i - 1]) {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - wt[i - 1]] + wt[i - 1]);
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
                    dp[j] = Math.max(dp[j], dp[j - wt[i - 1]] + wt[i - 1]);
                }
            }
        }
        return dp[w];
    }
}
```