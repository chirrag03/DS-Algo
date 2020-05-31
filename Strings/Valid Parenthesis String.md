## Valid Parenthesis String

Given a string containing only three types of characters: '(', ')' and '*', write a function to check whether this string is valid. We define the validity of a string by these rules:

* Any left parenthesis '(' must have a corresponding right parenthesis ')'.
* Any right parenthesis ')' must have a corresponding left parenthesis '('.
* Left parenthesis '(' must go before the corresponding right parenthesis ')'.
* '*' could be treated as a single right parenthesis ')' or a single left parenthesis '(' or an empty string.
* An empty string is also valid.

### Example 1:
```
Input: "()"
Output: True
```

### Example 2:
```
Input: "(*)"
Output: True
```

### Example 3:
```
Input: "(*))"
Output: True
```


> **Note:** The string size will be in the range [1, 100].

 ### Solutioning:
Start iterating the string.  
Push the index of "(" in s1. Push the index of "\*" in s2.  
Now whenever we encounter ")", we need to check whether "(" is available for the same. If not, then whether "*" is available for the same.  

At last we need to check that for the remaining "(", do we have a "*" after each of the "(".

```java
class Solution {
    public boolean checkValidString(String s) {
        Stack<Integer> s1 = new Stack<>();
        Stack<Integer> s2 = new Stack<>();
                
        for(int i=0;i<s.length();i++){
            if(s.charAt(i) == '('){
                s1.push(i);
            }else if(s.charAt(i) == '*'){
                s2.push(i);
            }else{
                if(s1.isEmpty() && s2.isEmpty()){
                    return false;
                }
                
                if(!s1.isEmpty()){
                    s1.pop();
                }else if(!s2.isEmpty()){
                    s2.pop();
                }
            }
        }
        
        if(s1.size() > s2.size()){
            return false;
        }
        
        while(!s1.isEmpty()){
            if(s1.pop() > s2.pop()){
                return false;
            }
        }
        
        return true;
    }
}
```  
