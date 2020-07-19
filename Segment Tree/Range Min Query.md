## Range Min Query

Given an integer array arr, find the min of the elements between indices i and j (i ≤ j), inclusive.


### Solutioning:

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

public class SegmentTreeOptimised {

  //Time: O(n)
  private Node buildTree(int[] arr, int start, int end){

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

    Node root = new Node(start, end, Math.min(leftSubtree.val, rightSubtree.val)); //No need to check if left and right are null because they will always be there
    root.left = leftSubtree;
    root.right = rightSubtree;

    return root;
  }

  //Time: O(log(n))
  private int query(Node root, int qs, int qe){
    if(root == null){
      return Integer.MAX_VALUE;
    }

    //No overlap
    if(qe < root.start || qs > root.end){
      return Integer.MAX_VALUE;
    }

    //Complete Overlap
    if(qs <= root.start && qe >= root.end){
      return root.val;
    }

    //Partial Overlap
    return Math.min(query(root.left, qs, qe), query(root.right, qs, qe));
  }

  //Time: O(log(n))
  private void updateNode(Node root, int i, int val){
    if(root == null){
      return;
    }

    //No overlap
    if(i < root.start || i > root.end){
      return;
    }

    //Complete overlap i.e reached leaf...Only for node that need to be changed
    if(root.start == root.end){
      root.val += val;
      return;
    }

    //Partial Overlap... lying in range - i is lying between start and end
    updateNode(root.left, i, val);
    updateNode(root.right, i, val);

    //It will never be the case that left or right is null
    root.val = Math.min(root.left.val, root.right.val);
  }

  public static void main(String[] args) {
    SegmentTreeOptimised sg = new SegmentTreeOptimised();
    sg.buildTree(new int[]{1,3,2,5}, 0, 3);
  }

}
```


**NOTE:**  

Last layer: N nodes 
Pre-last layer: N/2 nodes 
....

So, we have (N) + (N/2) + (N/4)+.... = N*(1 + 1/2 + 1/4 + ...) = 2N nodes. (using gp sum formula of a/(1-r))  
So, there is a total of 2N nodes in a segment tree.  

But for an array we need to take a space of (4N + 1)
