## Shuffle an Array (Random Permutation)

Shuffle a set of numbers without duplicates.


### Example 1:
```
// Init an array with set 1, 2, and 3.
int[] nums = {1,2,3};
Solution solution = new Solution(nums);

// Shuffle the array [1,2,3] and return its result. Any permutation of [1,2,3] must equally likely to be returned.
solution.shuffle();

// Resets the array back to its original configuration [1,2,3].
solution.reset();

// Returns the random shuffling of array [1,2,3].
solution.shuffle();
```


 ### Solutioning:

```java
class Solution {

    int[] original;
    public Solution(int[] nums) {
        this.original = nums;
    }
    
    /** Resets the array to its original configuration and return it. */
    public int[] reset() {
        return original;
    }
    
    /** Returns a random shuffling of the array. */
    public int[] shuffle() {
        int[] array = original.clone();
        Random rand = new Random();
        for(int i=0;i<array.length;i++){
            
            //nextInt() gives between 0 (inclusive) and the specified value (exclusive)
            //but we need to generate a number between i (inclusive) and the end of array
            int randNum = rand.nextInt(array.length - i) + i;
            
            int temp = array[i];
            array[i] = array[randNum];
            array[randNum] = temp;
        }
        
        return array;
    }
    
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(nums);
 * int[] param_1 = obj.reset();
 * int[] param_2 = obj.shuffle();
 */
```  
**Time Complexity:** O(N)   
**Space Complexity:** O(N) 
