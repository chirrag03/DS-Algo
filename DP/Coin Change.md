## Coin Change

You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.


### Example 1:
```
Input: coins = [1, 2, 5], amount = 11
Output: 3 
Explanation: 11 = 5 + 5 + 1
```

### Example 2:
```
Input: coins = [2], amount = 3
Output: -1
```

> **Note:** 
> You may assume that you have an infinite number of each kind of coin.



 ### Solutioning:
 
 Why can't we use a greedy approach? Let's assume you need to make a total of 450 using denominations of (100, 90, 1). Greedy will fail in this case.  
 
 As a result we need to check all possible combinations. Now if we go with the recursive approach then the complexity will be:  

**Time Complexity:** O(n<sup>c</sup>)  where where n is the amount, c is denomination count. In the worst case, complexity is exponential. The reason is that every coin denomination i.e. c<sub>i</sub> could have atmost n/c<sub>i</sub> values. Therefore the possible no. of comibations is :
(n/c<sub>1</sub>)*(n/c<sub>2</sub>)*(n/c<sub>3</sub>)...(n/c<sub>n</sub>) = (n<sup>c</sup>)/(c<sub>1</sub>*c<sub>2</sub>*c<sub>3...</sub>*c<sub>n</sub>)  

**Space Complexity:** O(n)   

Thus, we try a bottom up approach using dp.

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
       
        int[] dp = new int[amount + 1];
        dp[0] = 0;
        
        for(int i=1;i<=amount;i++){
            int min = Integer.MAX_VALUE - 1;
            for(int j = 0;j<coins.length;j++){
                int remAmt = i - coins[j];
                if(remAmt >= 0){
                    min = Math.min(min, dp[remAmt] + 1);
                }
            }
            dp[i] = min;
        }
        
        if(dp[amount] == Integer.MAX_VALUE - 1){
            return -1;
        }
        
        return dp[amount];
        
    }
}
```  
**Time Complexity:** O(n*c)  where where n is the amount, c is denomination count  
**Space Complexity:** O(n) 

