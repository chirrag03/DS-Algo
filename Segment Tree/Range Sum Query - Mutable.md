## Range Sum Query - Mutable

Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

The update(i, val) function modifies nums by updating the element at index i to val.  

### Example 1:
```
Given nums = [1, 3, 5]

sumRange(0, 2) -> 9
update(1, 2)
sumRange(0, 2) -> 8
```

**Note:**  
> The array is only modifiable by the update function.  
> You may assume the number of calls to update and sumRange function is distributed evenly.  


```java
class NumArray {

    Integer[] tree;
    int n;
    
    public NumArray(int[] nums) {
        n = nums.length;
        tree = new Integer[4*n + 1];
        buidTree(tree, 1, nums, 0, n-1);
    }
    
    private void buidTree(Integer[] tree, int index, int[] nums, int start, int end){
        
        if(start > end){
            return;
        }
        
        if(start == end){
            tree[index] = nums[start];
            return;
        }
        
        int mid = (start + end) / 2;
        buidTree(tree, 2*index, nums, start, mid);
        buidTree(tree, 2*index+1, nums, mid+1, end);
        tree[index] = tree[2*index] + tree[2*index+1];
    }
    
    public void update(int i, int val) {
        updateHelper(tree, 1, 0, n-1, i, val);
    }
    
    private void updateHelper(Integer[] tree, int index, int start, int end, int i, int val){
        
        if(i > end || i < start){
            return;
        }
        
        if(start == end){
            tree[index] = val;
            return;
        }
        
        int mid = (start + end) / 2;
        updateHelper(tree, 2*index, start, mid, i, val);
        updateHelper(tree, 2*index+1, mid+1, end, i, val);
        tree[index] = tree[2*index] + tree[2*index+1];
    }
    
    public int sumRange(int i, int j) {
        return sumRangeHelper(tree, 1, 0, n-1, i, j);
    }
    
    private int sumRangeHelper(Integer[] tree, int index, int start, int end, int qs, int qe){
        
        if(qs > end || qe < start){
            return 0;
        }
        
        if(qs <= start && qe >= end){
            return tree[index];
        }
        
        int mid = (start + end) / 2;
        int left = sumRangeHelper(tree, 2*index, start, mid, qs, qe);
        int right = sumRangeHelper(tree, 2*index+1, mid+1, end, qs, qe);
        return left + right;
    }
}

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * obj.update(i,val);
 * int param_2 = obj.sumRange(i,j);
 */
```  

**Implementation Without Using An Array**
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
```


