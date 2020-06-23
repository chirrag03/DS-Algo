## Pow(x, n)

Implement pow(x, n), which calculates x raised to the power n (x<sup>n</sup>).


### Example 1:
```
Input: 2.00000, 10
Output: 1024.00000
```

### Example 2:
```
Input: 2.10000, 3
Output: 9.26100
```

### Example 3:
```
Input: 2.00000, -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25
```

> **Note:** 
> -100.0 < x < 100.0  
> n is a 32-bit signed integer, within the range [−2<sup>31</sup>, 2<sup>31</sup> − 1]  

 ### Solutioning:

```java
class Solution {
    public double myPow(double x, int n) {
        if(n == 0){
            return 1d;
        }
        
        double subProblemResult = myPow(x, n/2);
        if(n % 2 == 1){
            return subProblemResult*subProblemResult*x;
        }
        if(n % 2 == -1){
            return subProblemResult*subProblemResult/x;
        }
        
        return subProblemResult*subProblemResult;
    }
}
```  
**Time Complexity:** O(log(n))   
**Space Complexity:** O(log(n))  

**Follow Up Solutions Hint**  
Another approach can do this in Time O(log(n)) and Space O(1)
