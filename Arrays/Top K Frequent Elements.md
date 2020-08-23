## Top K Frequent Elements

Given a non-empty array of integers, return the k most frequent elements.



### Example:
```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

### Example:
```
Input: nums = [1], k = 1
Output: [1]
```


> **Note:**   
> You may assume k is always valid, 1 ≤ k ≤ number of unique elements.  
> Your algorithm's time complexity must be better than O(n log n), where n is the array's size.  
> It's guaranteed that the answer is unique, in other words the set of the top k frequent elements is unique.  
> You can return the answer in any order.


```java
import java.util.Map.*;

class Item{
    int data;
    int freq;
    public Item(int d, int f){
        data = d;
        freq = f;
    }
}
class Solution {
    public int[] topKFrequent(int[] nums, int k) {

        Map<Integer, Integer> map = new HashMap<>();
        for(int i=0;i<nums.length;i++){
            map.put(nums[i], map.getOrDefault(nums[i], 0) + 1);       
        }
        
        PriorityQueue<Item> q = new PriorityQueue<>((i1, i2) -> i1.freq - i2.freq);
        for(Entry<Integer, Integer> e: map.entrySet()){
            int data = e.getKey();
            int freq = e.getValue();
            
            if(q.isEmpty() || q.size() < k){
                q.add(new Item(data, freq));
            }else{
                if(q.peek().freq < e.getValue()){
                    q.poll();
                    q.add(new Item(data, freq));
                }
            }
        }
        
        int[] output = new int[k];
        int i=0;
        while(!q.isEmpty()){
            output[i++] = q.poll().data;
        }
        
        return output;
    }
}
```  
