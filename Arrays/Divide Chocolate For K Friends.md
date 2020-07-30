## Divide Chocolate For K Friends

You have one chocolate bar that consists of some chunks. Each chunk has its own sweetness given by the array sweetness.

You want to share the chocolate with your **K friends** so you start cutting the chocolate bar into **K+1 pieces using K cuts**, each piece consists of some consecutive chunks.

Being generous, you will eat the piece with the **minimum total sweetness** and give the other pieces to your friends.

Find the **maximum total sweetness** of the piece you can get by cutting the chocolate bar optimally.


### Example 1:
```
Input: sweetness = [1,2,3,4,5,6,7,8,9], K = 5
Output: 6
Explanation: You can divide the chocolate to [1,2,3], [4,5], [6], [7], [8], [9]
```

### Example 2:
```
Input: sweetness = [5,6,7,8,9,1,2,3,4], K = 8
Output: 1
Explanation: There is only one way to cut the bar into 9 pieces.
```


### Example 3:
```
Input: sweetness = [1,2,2,1,2,2,1,2,2], K = 2
Output: 5
Explanation: You can divide the chocolate to [1,2,2], [1,2,2], [1,2,2]
```

> **Constraints:**  
> 0 <= K < sweetness.length <= 10^4  
> 1 <= sweetness[i] <= 10^5   


 ### Solutioning:
Required sweetness of the piece can vary between min sweetness of a chunk (this case occurs when only this chunk is there in a piece) and the total sum of all chunks (this case occurs when all chunks need to be present in 1 piece)  
We will do a binary search over the possbile sweetness values of a piece.  

```java
class Solution {
    public int maximizeSweetness(int[] sweetness, int K) {
        
        int min = Integer.MAX_VALUE;
        int sum = 0;
        for(int i : sweetness){
            sum += i;
            min = Math.min(min, i);
        }
                
        int start = min;
        int end = sum;
        
        int result = -1;
        
        while(start <= end){
            int mid = end + (start - end) / 2;
            
            if(areKPiecesPossible(sweetness, K+1, mid)){
                result = mid;
                start = mid + 1;
            }else{
                end = mid - 1;
            }
        }
        
        return result;
    }
    
    private boolean areKPiecesPossible(int[] nums, int K, int minSweetness) {
        // minSweetness is minimum sweetness which will allow us to cut chocolate in a way that no friend
       // can get more at least minSweetness. Since we want maximize minSweetness, split as soon as
       // any friend get minSweetness. In this way we can split chocolate with more number of friends by
       // honoring the constraint(all friends will get at least minSweetness).
        int friends = 0;
        int currSum = 0;
        for(int i=0;i<nums.length;i++){
            currSum += nums[i];
            if(currSum >= minSweetness){
                currSum = 0;
                friends++;
            }
        }
                
        return friends >= K;
    }
}
```  
**Time Complexity:** O(n*log(R)) where R is the range of numbers  
**Space Complexity:** O(log(R)) stack space  

