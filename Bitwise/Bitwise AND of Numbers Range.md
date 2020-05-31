## Bitwise AND of Numbers Range

Given a range [m, n] where 0 <= m <= n <= 2147483647, return the bitwise AND of all numbers in this range, inclusive.


### Example:
```
Input: [5,7]
Output: 4
```

### Example:
```
Input: [0,1]
Output: 0
```

> **Note:** You may not require to iterate the entire range.


 ### Solutioning:

Start iterating from the end to start. Why??? because the greater the number more will the number of 1's in it.
As we iterate towards lower numbers, the number of 0's in the binary form will increase and if the bitwise AND results in 0 then no need to iterate till start.

```java
class Solution {
    public int rangeBitwiseAnd(int m, int n) {
        if(m==n){
            return m;
        }
        
        int bitAnd = m;
        
        for(int i=n;i>=m+1;i--){
            bitAnd &= i;
            if(bitAnd == 0){
                return 0;
            }
        }
        
        return bitAnd;
    }
}
```  
