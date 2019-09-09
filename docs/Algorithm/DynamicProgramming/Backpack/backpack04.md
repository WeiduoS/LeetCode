# 4. Backpack bounded knapsack problem (BKP)

## Description
Assume that you have n yuan. There are many kinds of rice in the supermarket. Each kind of rice is bagged and must be purchased in the whole bag. Given the weight, price and quantity of each type of rice, find the maximum weight of rice that you can purchase.

## Example I

Input:  n = 8, prices = [3,2], weights = [300,160], amounts = [1,6]
Output:  640
Explanation:  Buy the second rice(price = 2) use all 8 money.

## Example II
Input:  n = 8, prices  = [2,4], weight = [100,100], amounts = [4,2 ]
Output:  400	
Explanation:  Buy the first rice(price = 2) use all 8 money.

## Question
https://www.lintcode.com/problem/backpack-vii/description

## Transform Function

```
dp[i][j] = Math.max(dp[i][j], dp[i - 1][j - k * prices[i - 1]] + k * weight[i - 1]);
```

## Template

```Java
class Solution {
    public int backpack(int n, int[] prices, int[] weight, int[] amounts) {
        int m = prices.length;
        int[][] dp = new int[m + 1][n + 1];
        
        for(int i = 1; i <= m; i++) {
            for(int j = 1; j <= n; j++) {
                dp[i][j] = dp[i - 1][j];
                for(int k = 1; k <= amounts[i - 1] && k * prices[i - 1] <= j; k++) {
                    dp[i][j] = Math.max(dp[i][j], dp[i - 1][j - k * prices[i - 1]] + k * weight[i - 1]);
                }
            }
        }
        
        return dp[m][n];
    }
}
```

## Another Template 
```java
class Solution {
    public int backpack(int n, int[] prices, int[] weight, int[] amounts) {
        int m = prices.length;
        int[][] dp = new int[m + 1][n + 1];
        
        for(int i = 1; i <= m; i++) {
            for(int j = 0; j <= amounts[i - 1]; j++) {
                for(int k = 1; k <= n; k++) {
                    if(k >= j * prices[i - 1]) dp[i][k] = Math.max(dp[i][k], dp[i - 1][k - j * prices[i - 1]] + j * weight[i - 1]);
                }
            }
        }
        
        return dp[m][n];
    }
}

```

## Optimize Analysis
```
Wrong Answer: 
for (int i = 1; i <= n; ++i) { 
    for (int j = 1; j <= amounts[i - 1]; ++j) {
        for (int k = n; k >= j * prices[i - 1]; --k) {
            dp[k] = max(dp[k], dp[k - j * prices[i - 1]] + j * weight[i - 1]);
        }
    }
}

1. dp[i - 1][k - j * prices[i - 1]] + j * weight[i - 1])， j = 0…amounts[i-1]
Max(dp[i][j], dp[i - 1][j])

2. dp[k - prices[i - 1]] + weight[i - 1] (j = 1) is the max because k is from max to 1
so dp[k - j * prices[i - 1]] + j * weight[i - 1] is wrong

3. tip: j should begin from 1, because dp[k - (amounts[i - 1] + 1) * prices[i - 1]] is meaningless


```

## Optimize I
```java
class Solution {
    public int backpack(int n, int[] prices, int[] weight, int[] amounts) {
        int m = prices.length;
        int[] dp = new int[n + 1];
        
        for(int i = 1; i <= m; i++) {
            for(int j = 1; j <= amounts[i - 1]; j++){
                for(int k = n; k >= prices[i - 1]; k--) {
                    dp[k] = Math.max(dp[k], dp[k - prices[i - 1]] + weight[i - 1]);
                }
            }
        }
        
        return dp[n];
    }
}
```