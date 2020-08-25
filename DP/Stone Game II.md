## Stone Game II

Alex and Lee continue their games with piles of stones.  There are a number of piles arranged in a row, and each pile has a positive integer number of stones **piles[i]**.  The objective of the game is to end with the most stones. 

Alex and Lee take turns, with Alex starting first.  Initially, **M = 1**.

On each player's turn, that player can take **all the stones** in the **first X remaining piles**, where **1 <= X <= 2M**.  Then, we set **M = max(M, X)**.

The game continues until all the stones have been taken.

Assuming Alex and Lee play optimally, return the maximum number of stones Alex can get.


### Example 1:
```
Input: piles = [2,7,9,4,4]
Output: 10
Explanation:  If Alex takes one pile at the beginning, Lee takes two piles, then Alex takes 2 piles again. 
Alex can get 2 + 4 + 4 = 10 piles in total. 
If Alex takes two piles at the beginning, then Lee can take all three piles left. 
In this case, Alex get 2 + 7 = 9 piles in total. So we return 10 since it's larger. 
```


> **Constraints:**  
> 1 <= piles.length <= 100  
> 1 <= piles[i] <= 10 ^ 4  
 

### Solutioning
```java
class Solution {
    public int stoneGameII(int[] piles) {
        
        int n = piles.length;
        
        int[] prefixSum = new int[n+1];
        int sum = 0;
        for(int i=piles.length-1;i>=0;i--){
            sum += piles[i];
            prefixSum[i] = sum;
        }
        
        storage = new Integer[n+1][n+1];
        
        return stoneGameII(piles, 0, 1, prefixSum);
    }
    
    Integer[][] storage;
    
    //Recursive function returns the max number of stones a player can get
    private int stoneGameII(int[] piles, int start, int m, int[] prefixSum){
        
        if(storage[start][m] != null){
            return storage[start][m];
        }
        
        if(start == piles.length){
            return 0;
        }
        
        int max = 0;
        for(int x=1;x<=Math.min(2*m, piles.length-start);x++){
            //x Stones picked by current player in his turn
            int maxStones = prefixSum[start] - prefixSum[start+x];
            
            //Max Stones picked by other player in remaining game
            int leeMaxStones = stoneGameII(piles, start+x, Math.max(m,x), prefixSum);
                
            //Max Stones picked by current player in remaining game
            maxStones += prefixSum[start+x] - leeMaxStones;
            
            max = Math.max(max, maxStones);
        }
        
        storage[start][m] = max;
        return  max;
    }
}
```  
**Time Complexity:** O(N<sup>2</sup>)  
**Space Complexity:**  O(N<sup>2</sup>)   



