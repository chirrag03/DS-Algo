## Valid Square  

Given the coordinates of four points in 2D space, return whether the four points could construct a square.

The coordinate (x,y) of a point is represented by an integer array with two integers.  

### Example 1:
```
Input: p1 = [0,0], p2 = [1,1], p3 = [1,0], p4 = [0,1]
Output: True
```

**Note:**  
> All the input integers are in the range [-10000, 10000].  
> A valid square has four equal sides with positive length and four equal angles (90-degree angles).  
> Input points have no order.  


 ### Solutioning:

```java
class Solution {
    public boolean validSquare(int[] p1, int[] p2, int[] p3, int[] p4) {
        return isSquare(p1, p2, p3, p4) || 
            isSquare(p1, p3, p2, p4) || 
            isSquare(p1, p2, p4, p3) || 
            isSquare(p1, p3, p2, p4);
        
        //We can leave the last case as it is similar to 1 of the 3 cases above
    }
    
    private boolean isSquare(int[] p1, int[] p2, int[] p3, int[] p4){
        int d = dist(p1, p2);
        
        if(d == 0){
            return false;
        }
        
        return  dist(p2, p3) == d && 
                dist(p3, p4) == d && 
                dist(p4, p1) == d && 
                dist(p2, p4) == dist(p1, p3);
    }
    
    private int dist(int[] p1, int[] p2){
        return (p1[0] - p2[0])*(p1[0] - p2[0]) + (p1[1] - p2[1])*(p1[1] - p2[1]);
    }

}
```  
**Time Complexity:** O(1)  
**Space Complexity:**  O(1) 

