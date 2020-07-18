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

