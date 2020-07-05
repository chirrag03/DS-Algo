## Open the Lock

You have a lock in front of you with 4 circular wheels. Each wheel has 10 slots: '0', '1', '2', '3', '4', '5', '6', '7', '8', '9'. The wheels can rotate freely and wrap around: for example we can turn '9' to be '0', or '0' to be '9'. Each move consists of turning one wheel one slot.

The lock initially starts at '0000', a string representing the state of the 4 wheels.

You are given a list of deadends dead ends, meaning if the lock displays any of these codes, the wheels of the lock will stop turning and you will be unable to open it.

Given a target representing the value of the wheels that will unlock the lock, return the minimum total number of turns required to open the lock, or -1 if it is impossible.  


### Example 1:
```
Input: deadends = ["0201","0101","0102","1212","2002"], target = "0202"
Output: 6
Explanation:
A sequence of valid moves would be "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202".
Note that a sequence like "0000" -> "0001" -> "0002" -> "0102" -> "0202" would be invalid,
because the wheels of the lock become stuck after the display becomes the dead end "0102".
```


### Example 2:
```
Input: deadends = ["8888"], target = "0009"
Output: 1
Explanation:
We can turn the last wheel in reverse to move from "0000" -> "0009".
```

### Example 3:
```
Input: deadends = ["8887","8889","8878","8898","8788","8988","7888","9888"], target = "8888"
Output: -1
Explanation:
We can't reach the target without getting stuck.
```

### Example 4:
```
Input: deadends = ["0000"], target = "8888"
Output: -1
```

> **Follow up:**  
> The length of deadends will be in the range [1, 500].  
> target will not be in the list deadends.  
> Every string in deadends and the string target will be a string of 4 digits from the 10,000 possibilities '0000' to '9999'    


 ### Solutioning:
We can think of this problem as a shortest path problem on a graph: there are 10000 nodes (strings '0000' to '9999'), and there is an edge between two nodes if they differ in one digit, that digit differs by 1 (wrapping around, so '0' and '9' differ by 1), and if both nodes are not in deadends.

```java
class Solution {
    public int openLock(String[] deadends, String target) {
        Set<String> deadendsSet = new HashSet<>(Arrays.stream(deadends).collect(Collectors.toList()));
        
        if(deadendsSet.contains("0000")){
            return -1;
        }
        
        Queue<String> q = new LinkedList<>();
        q.add("0000");
        q.add(null);
        
        Set<String> visited = new HashSet<>();
        visited.add("0000");
        
        String prev = null;
        int level = 0;
        while(!q.isEmpty()){
            String curr = q.poll();
            if(curr != null){
                
                if(target.equals(curr)){
                    return level;
                }
                
                for(int i=1;i<=4;i++){
                    String nextCombination = moveUp(curr, i);
                    if(!deadendsSet.contains(nextCombination) 
                       && !visited.contains(nextCombination)){
                        q.add(nextCombination);
                        visited.add(nextCombination);
                    }
                    
                    nextCombination = moveDown(curr, i);
                    if(!deadendsSet.contains(nextCombination) 
                       && !visited.contains(nextCombination)){
                        q.add(nextCombination);
                        visited.add(nextCombination);
                    }
                    
                }

            }else if(prev != null){
                q.add(null);
                level++;
            }else{
                break;
            }

            prev = curr;
        }
        return -1;
    }
    
    
    private String moveUp(String s, int lockNum){
        char[] arr = s.toCharArray();
        
        int digit = s.charAt(lockNum - 1) - '0';
        digit = (digit + 1) % 10;
        arr[lockNum - 1] = (char) (digit + '0');
        
        return new String(arr);
    }
    
    private String moveDown(String s, int lockNum){
        char[] arr = s.toCharArray();
        
        int digit = s.charAt(lockNum - 1) - '0';
        if(digit == 0){
            digit = 9;
        }else{
            digit = digit - 1;
        }
        arr[lockNum - 1] = (char) (digit + '0');
        
        return new String(arr);
    }
}
```  
**Time Complexity:** O(D + N<sup>2</sup>*A<sup>N</sup>) where A is the number of digits in our alphabet i.e. 0 to 9, N is the number of digits in the lock, and D is the size of deadends  
We need to instantiate our set dead. Plus we might visit every lock combination and we spend O(N^2) time enumerating through and constructing each node.
 
**Space Complexity:**  O(D + A<sup>N</sup>) for the queue and the set dead.

