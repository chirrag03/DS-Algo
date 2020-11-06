## Rectangle Overlap

An axis-aligned rectangle is represented as a list **[x1, y1, x2, y2]**, where **(x1, y1)** is the coordinate of its bottom-left corner, and **(x2, y2)** is the coordinate of its top-right corner. Its top and bottom edges are parallel to the X-axis, and its left and right edges are parallel to the Y-axis.

Two rectangles overlap if the area of their intersection is positive. To be clear, two rectangles that only touch at the corner or edges do not overlap.

Given two axis-aligned rectangles **rec1** and **rec2**, return **true if they overlap, otherwise return false**.

  

### Example 1:
```
Input: rec1 = [0,0,2,2], rec2 = [1,1,3,3]
Output: true
```

### Example 2:
```
Input: rec1 = [0,0,1,1], rec2 = [1,0,2,1]
Output: false
```

### Example 3:
```
Input: rec1 = [0,0,1,1], rec2 = [2,2,3,3]
Output: false
```

**Constraints:**  
> rect1.length == 4  
> rect2.length == 4  
> -109 <= rec1[i], rec2[i] <= 109  
> rec1[0] <= rec1[2] and rec1[1] <= rec1[3]  
> rec2[0] <= rec2[2] and rec2[1] <= rec2[3]  


 ### Solutioning:
**Approach 1:** Check Position  
If the rectangles do not overlap, then rec1 must either be higher, lower, to the left, or to the right of rec2.  

The condition "rec1 is to the left of rec2" is rec1[2] <= rec2[0], that is the right-most x-coordinate of rec1 is left of the left-most x-coordinate of rec2. The other cases are similar.


```java
class Solution {
    public boolean isRectangleOverlap(int[] rec1, int[] rec2) {
        if(isLineOrDot(rec1) || isLineOrDot(rec2)){
            return false;
        }
        
        return !(rec1[2] <= rec2[0] ||   // left
                 rec1[3] <= rec2[1] ||   // bottom
                 rec1[0] >= rec2[2] ||   // right
                 rec1[1] >= rec2[3]);    // top
    }
    
    private boolean isLineOrDot(int[] rec){
        if(rec[0] - rec[2] == 0 || rec[1] - rec[3] == 0){
            return true;
        }
        return false;
    }
}
```  
**Time Complexity:** O(1)  
**Space Complexity:**  O(1) 

**Approach 2:** Check Area  
If the rectangles overlap, they have positive area. This area must be a rectangle where both dimensions are positive, since the boundaries of the intersection are axis aligned.  

Say the area of the intersection is width * height, where width is the intersection of the rectangles projected onto the x-axis, and height is the same for the y-axis. We want both quantities to be positive.  

The width is positive when min(rec1[2], rec2[2]) > max(rec1[0], rec2[0]), that is when the smaller of (the largest x-coordinates) is larger than the larger of (the smallest x-coordinates). The height is similar.  

```java
class Solution {
    public boolean isRectangleOverlap(int[] rec1, int[] rec2) {
        return (Math.min(rec1[2], rec2[2]) > Math.max(rec1[0], rec2[0]) && // width > 0
                Math.min(rec1[3], rec2[3]) > Math.max(rec1[1], rec2[1]));  // height > 0
    }
}
```  
**Time Complexity:** O(1)  
**Space Complexity:**  O(1) 
