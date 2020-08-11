## Stone Game

Alex and Lee play a game with piles of stones.  There are an **even number** of piles arranged in a row, and each pile has a positive integer number of stones piles[i].

The objective of the game is to end with the most stones.  **The total number of stones is odd, so there are no ties.**

Alex and Lee take turns, with **Alex starting first**.  Each turn, a player takes the entire pile of stones from either the **beginning or the end** of the row.  This continues until there are no more piles left, at which point the person with the most stones wins.

Assuming Alex and Lee play optimally, return True if and only if Alex wins the game.


### Example 1:
```
Input: [5,3,4,5]
Output: true
Explanation: 
Alex starts first, and can only take the first 5 or the last 5.
Say he takes the first 5, so that the row becomes [3, 4, 5].
If Lee takes 3, then the board is [4, 5], and Alex takes 5 to win with 10 points.
If Lee takes the last 5, then the board is [3, 4], and Alex takes 4 to win with 9 points.
This demonstrated that taking the first 5 was a winning move for Alex, so we return true.
```


> **Note:**  
> 2 <= piles.length <= 500  
> piles.length is even.  
> 1 <= piles[i] <= 500  
> sum(piles) is odd.  
 

### Solutioning
```java
class Solution {
    public boolean stoneGame(int[] piles) {
        int n = piles.length;
        
        int sum = 0;
        for(int i=0;i<n;i++){
            sum += piles[i];
        }
        
        storage = new Integer[n][n];
        
        int alexStones = stoneGame(piles, 0, piles.length-1, sum);
        int leeStones = sum - alexStones;
        return alexStones > leeStones;
    }
    
    Integer[][] storage;
    
    private int stoneGame(int[] piles, int start, int end, int sum) {
        if(start > end){
            return 0;
        }
        
        if(storage[start][end] != null){
            return storage[start][end];
        }
        
        int max = 0;
        
        int alexStones = piles[start];
        int leeStones = stoneGame(piles, start+1, end, sum - piles[start]);
        alexStones += sum - piles[start] - leeStones;
        max = Math.max(max, alexStones);
        
        alexStones = piles[end];
        leeStones = stoneGame(piles, start, end-1, sum - piles[end]);
        alexStones += sum - piles[end] - leeStones;
        max = Math.max(max, alexStones);
        
        storage[start][end] = max;
        
        return max;
    }
}
```  

**Time Complexity:** O(N<sup>2</sup>)  
**Space Complexity:**  O(N<sup>2</sup>)   

**NOTE:** Without memomization, the complexity is 2<sup>N</sup> i.e. exponential...Why?   `
1) From intuition, at each step we have 2 choices, either to take from start or the end, so 2<sup>N</sup>  
2) From master's theorem, solving the recursion T(n) = 2*T(n-1) + k  
T(n) = C + 2∗C + 4∗C + ⋯ + 2<sup>N-1</sup>∗C = C∗(2<sup>N</sup> − 1)

