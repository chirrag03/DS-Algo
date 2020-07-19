## Range Sum Query 2D - Mutable

Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).

### Example 1:
```
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
update(3, 2, 2)
sumRegion(2, 1, 4, 3) -> 10
```


### Solutioning:

Create an array of pointers to Segment Trees, one for each row of the matrix.  
For update(i, j) queries, we simply call the update function for the ith row's segment tree. Time Complexity: O(logN).  
For sum queries, we iterate through all the segment trees for all rows and get the cumulative sum for the given column range. Time Complexity: O(MlogN).

```java
class Node{
    
    int val;
    int start;
    int end;
    
    Node left;
    Node right;
    
    public Node(int s, int e, int v){
        val = v;
        start = s;
        end = e;
    }
}

class SegmentTree{
    
    public static Node buildTree(int[] arr, int start, int end){

        //Base case 1
        if(start > end){
            return null;
        }

        //Base case 2 - Leaf node
        //value of a leaf node is the value of element in arr itself
        if(start == end){
            return new Node(start, end, arr[start]);
        }

        //Recursive calls on left and right to build left and right subtrees
        int mid = (start + end) / 2;
        Node leftSubtree = buildTree(arr, start, mid);
        Node rightSubtree = buildTree(arr, mid+1, end);

        //No need to check if left and right are null because they will always be there
        Node root = new Node(start, end, leftSubtree.val + rightSubtree.val); 
        root.left = leftSubtree;
        root.right = rightSubtree;

        return root;
    }
    
    public static int query(Node root, int qs, int qe){
        if(root == null){
          return 0;
        }

        //No overlap
        if(qe < root.start || qs > root.end){
          return 0;
        }

        //Complete Overlap
        if(qs <= root.start && qe >= root.end){
          return root.val;
        }

        //Partial Overlap
        return query(root.left, qs, qe) + query(root.right, qs, qe);
    }
    
    public static void updateNode(Node root, int i, int val){
        if(root == null){
          return;
        }

        //No overlap
        if(i < root.start || i > root.end){
          return;
        }

        //Complete overlap i.e reached leaf...Only for node that need to be changed
        if(root.start == root.end){
          root.val = val;
          return;
        }

        //Partial Overlap... lying in range - i is lying between start and end
        updateNode(root.left, i, val);
        updateNode(root.right, i, val);

        //It will never be the case that left or right is null
        root.val = root.left.val + root.right.val;
    }
}

class NumMatrix {

    int m;
    int n;
    int[][] matrix;
    Node[] trees;
    
    public NumMatrix(int[][] matrix) {
        m = matrix.length;
        n = m > 0 ? matrix[0].length : 0;
        trees = new Node[m];
                
        for(int i=0;i<m;i++){
            trees[i] = SegmentTree.buildTree(matrix[i], 0, n-1);
        }
    }
    
    public void update(int row, int col, int val) {
        SegmentTree.updateNode(trees[row], col, val);
    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {
        int sum = 0;
        for(int i=row1;i<=row2;i++){
            sum += SegmentTree.query(trees[i], col1, col2);
        }
        return sum;
    }
}

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix obj = new NumMatrix(matrix);
 * obj.update(row,col,val);
 * int param_2 = obj.sumRegion(row1,col1,row2,col2);
 */
```


**Time Complexity:**  
update: O(logN)  
sum : O(MlogN) where M-rows, N-columns  

**Space Complexity:** O(MN) For each row a segment tree will requires 2N space, thus M x 2N = O(MN) 
