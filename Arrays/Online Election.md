## Online Election

In an election, the **i-th vote** was cast for **persons[i]** at time **times[i]**.

Now, we would like to implement the following query function: **TopVotedCandidate.q(int t)** will return the number of the person that was leading the election at time **t**.  

Votes cast at time **t** will count towards our query.  In the case of a tie, the most recent vote (among tied candidates) wins.  


### Example 1:
```
Input: ["TopVotedCandidate","q","q","q","q","q","q"], [[[0,1,1,0,0,1,0],[0,5,10,15,20,25,30]],[3],[12],[25],[15],[24],[8]]
Output: [null,0,1,1,0,0,1]
Explanation: 
At time 3, the votes are [0], and 0 is leading.
At time 12, the votes are [0,1,1], and 1 is leading.
At time 25, the votes are [0,1,1,0,0,1], and 1 is leading (as ties go to the most recent vote.)
This continues for 3 more queries at time 15, 24, and 8.
```

> **Note:**  
> 1 <= persons.length = times.length <= 5000  
> 0 <= persons[i] <= persons.length  
> times is a strictly increasing array with all elements in [0, 10^9].  
> TopVotedCandidate.q is called at most 10000 times per test case.  
> TopVotedCandidate.q(int t) is always called with t >= times[0].  


 ### Solutioning:

```java
class TopVotedCandidate {

    int[] times;
    int[] leaders;
    public TopVotedCandidate(int[] persons, int[] times) {
        this.leaders = new int[persons.length];
        this.times = times;
        
        Map<Integer, Integer> countVoteMap = new HashMap<>();
        int leaderSoFar = -1;
        int countSoFar = 0;
        
        for(int i=0;i<persons.length;i++){
            int person = persons[i];
            int count = countVoteMap.getOrDefault(person, 0) + 1;
            countVoteMap.put(person, count);
            
            if(count >= countSoFar){
                leaders[i] = person;
                leaderSoFar = person;
                countSoFar = count;
            }else{
                leaders[i] = leaders[i-1];
            }
        }
    }
    
    public int q(int t) {
        int index = findFloor(times, 0, times.length - 1, t);
        return leaders[index];
    }
    
    private int findFloor(int[] arr, int start, int end, int target){
        if(target >= arr[end]){
            return end;
        }
        
        if(target < arr[start]){
            return -1;
        }
        
        int mid = (start + end) / 2;
        
        // this check is placed to handle infinite loop
        if(mid == start){
            return mid;
        }
        
        if(target == arr[mid]){
            return mid;
        }else if(target > arr[mid]){
            return findFloor(arr, mid, end, target);
        }else{
            return findFloor(arr, start, mid-1, target);
        }
    }
}

/**
 * Your TopVotedCandidate object will be instantiated and called as such:
 * TopVotedCandidate obj = new TopVotedCandidate(persons, times);
 * int param_1 = obj.q(t);
 */
```