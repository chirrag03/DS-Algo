## Count of Smaller Numbers After Self

You are given an integer array nums and you have to return a new counts array. The counts array has the property where counts[i] is the number of smaller elements to the right of nums[i].


### Example 1:
```
Input: [5,2,6,1]
Output: [2,1,1,0] 
Explanation:
To the right of 5 there are 2 smaller elements (2 and 1).
To the right of 2 there is only 1 smaller element (1).
To the right of 6 there is 1 smaller element (1).
To the right of 1 there is 0 smaller element.
```


 ### Solutioning:
**Naive Approach:** We will use a treemap that will store element as the key and it freq as the value.  
Start iterating the array from right and keep updating the frequency map.
For an element at index i, we need the sum of frequencies of all elements less than nums[i].  

This approach will have the complexity as O(n<sup>2</sup>) for the worst case [5,4,3,2,1]. Why?  
Let say we are at nums[3] = 2....for this we iterate the submap with keys ranging from 1 to 1...i.e. 1 entry  
Let say we are at nums[2] = 3....for this we iterate the submap with keys ranging from 1 to 2...i.e. 2 entries
Let say we are at nums[1] = 4....for this we iterate the submap with keys ranging from 1 to 3...i.e. 3 entries
and so on..  

**Optimized Approach:** Let say there was an array with indexes ranging from min to max and each index stores the frequency of that element.  
And we create a segment tree with sum query over this. Then we can get the sum of freq in O(log(n)) time.  

So initially we build a segment tree with all values in leaf nodes as 0....so evenntually all nodes are 0 in the tree initially.  
As we start iterating from right, we update the freq for that element by 1 and query on the tree for sum between an interval.  


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
class Solution {
    public List<Integer> countSmaller(int[] nums) {
        int n = nums.length;
        
        int max = Integer.MIN_VALUE;
        int min = Integer.MAX_VALUE;
        for(int i=0;i<nums.length;i++){
            max = Math.max(max, nums[i]);
            min = Math.min(min, nums[i]);
        }
        
        List<Integer> output = new ArrayList<>();
        
        Node root = buildTree(0, max-min);
        for(int i=nums.length-1;i>=0;i--){
            output.add(query(root, 0, nums[i]-1-min));
            updateNode(root, nums[i]-min, 1);
        }
        
        Collections.reverse(output);
        return output;
    }
    
    //We construct a sum segment tree with all values as 0 initially
    //The leaf nodes will be storing the freq of element i.e. 0 initially    
    private Node buildTree(int start, int end){

        //Base case 1
        if(start > end){
          return null;
        }

        //Base case 2 - Leaf node
        //value of a leaf node is the value of element in arr itself
        if(start == end){
          return new Node(start, end, 0);
        }

        //Recursive calls on left and right to build left and right subtrees
        int mid = (start + end) / 2;
        Node leftSubtree = buildTree(start, mid);
        Node rightSubtree = buildTree(mid+1, end);

        Node root = new Node(start, end, 0); 
        root.left = leftSubtree;
        root.right = rightSubtree;

        return root;
    }
    
    private int query(Node root, int qs, int qe){
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
        root.val = root.left.val + root.right.val;
    }
    
}
```  
**Time Complexity:** O(n*log(n))   

**Space Complexity:** O(2*R) where R reperesents the range i.e. (max - min) because an optimised segment tree takes 2*N space  

