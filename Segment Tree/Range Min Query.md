## Range Min Query
  
```java
import java.util.Arrays;

public class SegmentTree {


  public Integer[] constructTree(int[] arr){
    Integer[] tree = new Integer[4*arr.length + 1];
    buildTree(tree, 1, arr, 0, arr.length-1);
    System.out.println(Arrays.toString(tree));
    return tree;
  }

  //Time: O(n)
  public void buildTree(Integer[] tree, int index, int[] arr, int start, int end){

    //Base case 1
    if(start > end){
      return;
    }

    //Base case 2 - Leaf node
    //value of a leaf node is the value of element in arr itself
    if(start == end){
      tree[index] = arr[start];
      return;
    }

    //Recursive calls on left and right to build left and right subtrees
    int mid = (start + end) / 2;
    buildTree(tree, 2*index, arr, start, mid);
    buildTree(tree, 2*index+1, arr, mid+1, end);

    //Once the left and right subtrees are filled, we can fill the current node
    tree[index] = Math.min(tree[2*index], tree[2*index+1]);
  }

  //Time: O(log(n))
  public int query(Integer[] tree, int index, int start, int end, int queryStart, int queryEnd){

    //No overlap
    if(queryEnd < start || queryStart > end){
      return Integer.MAX_VALUE;
    }

    //Complete Overlap
    if(queryStart <= start && queryEnd >= end){
      return tree[index];
    }

    //Partial Overlap
    int mid = (start + end) / 2;
    int left = query(tree, 2*index, start, mid, queryStart, queryEnd);
    int right = query(tree, 2*index+1, mid+1, end, queryStart, queryEnd);

    return Math.min(left, right);
  }

  //Time: O(log(n))
  public void updateNode(Integer[] tree, int index, int start, int end, int i, int value){

    //No overlap
    if(i < start || i > end){
      return;
    }

    //Complete overlap i.e reached leaf...Only for nodes that need to be changed
    if(start == end){
      tree[index] = value;
      return;
    }

    //Partial Overlap... lying in range - i is lying between start and end
    int mid = (start + end) / 2;
    updateNode(tree, 2*index, start, mid, i, value);
    updateNode(tree, 2*index+1, mid+1, end, i, value);

    tree[index] = Math.min(tree[2*index], tree[2*index+1]);
  }

  //Time: O(n)
  //Increment all values in the range by inc
  public void updateRange(Integer[] tree, int index, int start, int end, int rangeStart, int rangeEnd, int inc){

    //No overlap
    if(rangeStart < start || rangeEnd > end){
      return;
    }

    //Reached leaf.. Only for nodes that need to be changed
    if(start == end){
      tree[index] += inc;
      return;
    }

    //Partial Overlap... lying in range
    int mid = (start + end) / 2;
    updateRange(tree, 2*index, start, mid, rangeStart, rangeEnd, inc);
    updateRange(tree, 2*index+1, mid+1, end, rangeStart, rangeEnd, inc);

    tree[index] = Math.min(tree[2*index], tree[2*index+1]);
  }


  public static void main(String[] args) {

    int[] arr = new int[]{1, 7, 3, 2, 5, 11};
    SegmentTree segmentTree = new SegmentTree();
    segmentTree.constructTree(arr);
  }

}
```  

