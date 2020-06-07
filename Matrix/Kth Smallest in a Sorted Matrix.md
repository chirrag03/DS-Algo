## Kth Smallest Element in a Sorted Matrix

Given a n x n matrix where each of the rows and columns are sorted in ascending order, find the kth smallest element in the matrix.

Note that it is the kth smallest element in the sorted order, not the kth distinct element.


### Example 1:
```
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

return 13.
```

> **Note:** You may assume k is always valid, 1 ≤ k ≤ n2.

 ### Solutioning:
If you think about it, we can reframe the problem as finding the **K**th smallest elements from amongst N sorted lists right?  
We know that the rows are sorted and so are the columns. So, we can treat each row (or column) as a sorted list in itself.  

Now do we need to initially create a heap of all elements in the first column everytime?  
Not really....We just need a heap of size = min(m,k)

```java
class Item{
    int data;
    int i;
    int j;
    public Item(int d, int i, int j){
        this.data = d;
        this.i = i;
        this.j = j;
    }
}
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        
        int m = matrix.length;
        int n = matrix[0].length;
        
        PriorityQueue<Item> q = new PriorityQueue<>((i1, i2) -> (i1.data - i2.data));
        
        for(int i=0;i<Math.min(m, k);i++){
            q.add(new Item(matrix[i][0], i, 0));
        }
        
        
        for(int i=1;i<k;i++){
            Item item = q.poll();
            
            if(item.j + 1 < n){
                q.add(new Item(matrix[item.i][item.j+1], item.i, item.j+1));
            }
        }
        
        return q.poll().data;   
    }
}
```  
**Time Complexity:** Let, X = min(K, N)..Now O(X) time to create heap. Then we perform k iterations performing add/extractMin operations on the heap both taking log(X) time.....Thus, O(X + k*log(X)) where X = min(K, N)  
**Space Complexity:** O(X) occupied by the heap 


 ### Alternate Efficient Solutioning:
Using Binary Search

```java
class Solution {

  public int kthSmallest(int[][] matrix, int k) {

    int n = matrix.length;
    int start = matrix[0][0], end = matrix[n - 1][n - 1];
    while (start < end) {

      int mid = start + (end - start) / 2;
      // first number is the smallest and the second number is the largest
      int[] smallLargePair = {matrix[0][0], matrix[n - 1][n - 1]};

      int count = this.countLessEqual(matrix, mid, smallLargePair);

      if (count == k) return smallLargePair[0];

      if (count < k) start = smallLargePair[1]; // search higher
      else end = smallLargePair[0]; // search lower
    }
    return start;
  }

  private int countLessEqual(int[][] matrix, int mid, int[] smallLargePair) {

    int count = 0;
    int n = matrix.length, row = n - 1, col = 0;

    while (row >= 0 && col < n) {

      if (matrix[row][col] > mid) {

        // as matrix[row][col] is bigger than the mid, let's keep track of the
        // smallest number greater than the mid
        smallLargePair[1] = Math.min(smallLargePair[1], matrix[row][col]);
        row--;

      } else {

        // as matrix[row][col] is less than or equal to the mid, let's keep track of the
        // biggest number less than or equal to the mid
        smallLargePair[0] = Math.max(smallLargePair[0], matrix[row][col]);
        count += row + 1;
        col++;
      }
    }

    return count;
  }
}
```  
**Time Complexity:** O(N * log(max - min)) where Max is the maximum element in the array and likewise, Min is the minimum element.  
**Space Complexity:** O(1)
