## Next Closest Time  

Given a time represented in the format "HH:MM", form the next closest time by reusing the current digits. There is no limit on how many times a digit can be reused.

You may assume the given input string is always valid. For example, "01:34", "12:09" are all valid. "1:34", "12:9" are all invalid.


### Example 1:
```
Input: "19:34"
Output: "19:39"
Explanation: The next closest time choosing from digits 1, 9, 3, 4, is 19:39, which occurs 5 minutes later.  It is not 19:33, because this occurs 23 hours and 59 minutes later.
```

### Example 2:
```
Input: "23:59"
Output: "22:22"
Explanation: The next closest time choosing from digits 2, 3, 5, 9, is 22:22. It may be assumed that the returned time is next day's time since it is smaller than the input time numerically.
```

 ### Solutioning:

```java
class Solution {
    public String nextClosestTime(String time) {
        Queue<String> q = new LinkedList<>();
        
        String srcTime = time.substring(0, 2) + time.substring(3);
        
        String[] digits = new String[4];
        for(int i=0;i<srcTime.length();i++){
            digits[i] = String.valueOf(srcTime.charAt(i));
            q.add(digits[i]);
        }
        
        
        int diff = Integer.MAX_VALUE;
        String outputHrs = "00";
        String outputMins = "00";
        
        while(!q.isEmpty()){
            
            String curr = q.poll();
            
            if(curr.length() != 4){
                //Here it comes total 84 times i.e 4 + 16 + 64
                for(int i=0;i<digits.length;i++){
                    q.add(curr + digits[i]);
                }
            }else{
                //Here it comes total 256 times
                String destTime = curr;
                if(isValid(destTime) && !srcTime.equals(destTime)){
                    int currDiff = timeDiff(srcTime, destTime);
                    if(currDiff < diff){
                        diff = currDiff;
                        outputHrs = destTime.substring(0, 2);
                        outputMins = destTime.substring(2);
                    }
                }
            }
        }
        
        if(diff == Integer.MAX_VALUE){
            return time;
        }
        return outputHrs + ":" + outputMins;
    }
    
    private boolean isValid(String time){
        int hrs = Integer.valueOf(time.substring(0, 2));
        int mins = Integer.valueOf(time.substring(2));
        
        if(mins >= 0 && mins < 60 && hrs >= 0 && hrs < 24){
            return true;
        }
        return false;
    }
    
    private int timeDiff(String srcTime, String time){
        
        int srcHrs = Integer.valueOf(srcTime.substring(0, 2));
        int srcMins = Integer.valueOf(srcTime.substring(2));
        
        int destHrs = Integer.valueOf(time.substring(0, 2));
        int destMins = Integer.valueOf(time.substring(2));
        
        //Convert time to a number between 0 to 1439 i.e. from 00:00 to 23:59
        int src = (srcHrs*60) + srcMins;
        int dest = (destHrs*60) + destMins;
        
        if(src <= dest){
            return Math.abs(src - dest);
        }
        
        return Math.abs(1439 - src) + dest;
    }
}
```  
**Time Complexity:** O(1)   

**Space Complexity:** O(1) 

