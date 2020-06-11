## Evaluate Reverse Polish Notation

Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are +, -, *, /. Each operand may be an integer or another expression.


### Example 1:
```
Input: ["2", "1", "+", "3", "*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```

### Example 2:
```
Input: ["4", "13", "5", "/", "+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
```

### Example 3:
```
Input: ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
Output: 22
Explanation: 
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

> **Note:**  
> Division between two integers should truncate toward zero.  
> The given RPN expression is always valid. That means the expression would always evaluate to a result and there won't be any divide by zero operation.

 ### Solutioning:

```java
class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> s = new Stack<>();
        
        for(int i=0;i<tokens.length;i++){
            if(tokens[i].equals("+")){
                int operand2 = s.pop();
                int operand1 = s.pop();
                
                s.push(operand1 + operand2);
            }else if(tokens[i].equals("-")){
                int operand2 = s.pop();
                int operand1 = s.pop();
                
                s.push(operand1 - operand2);
            }else if(tokens[i].equals("/")){
                int operand2 = s.pop();
                int operand1 = s.pop();
                
                s.push(operand1 / operand2);
            }else if(tokens[i].equals("*")){
                int operand2 = s.pop();
                int operand1 = s.pop();
                
                s.push(operand1 * operand2);
            }else{
                s.push(Integer.parseInt(tokens[i]));
            }
        }
        
        return s.pop();
    }
}
```  
**Time Complexity:** O(N)   
**Space Complexity:** O(N) In the worst case, the stack will have all the numbers on it at the same time. This is never more than half the length of the input array.



