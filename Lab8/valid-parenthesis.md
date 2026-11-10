# Nat205 Implementation

```
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []

        for par in s:
            if(par in ("{", "(", "[")):
                stack.append(par)
            elif(len(stack)==0 and par in ("}", "]", ")")):
                return False
            else:
                last = stack.pop()
                if((par == ")" and last == "(") or (par == "}" and last =="{") or (par == "]" and last == "[")):
                    continue
                else:
                    return False

               
        
        if(len(stack) != 0):
            return False

        return True
```

# How my implementation works

The problem gives us a string made of only the characters (, ), {, }, [ and ] and asks us to check if every open bracket is closed by the same type of bracket and in the correct order. Since the most recent open bracket always has to be the first one closed, a stack is a good fit for this problem. I loop through each character, and if it's an open bracket I push it onto the stack. If it's a closing bracket and the stack is empty, there is nothing for it to close so I return False. Otherwise, I pop the last open bracket and check if it matches the closing bracket, and if it doesn't I return False. After the loop, if the stack still has brackets left, some of them were never closed so I return False, otherwise I return True.


# Runtime and Memory Analysis

## Runtime
I loop through the string once and every push, pop and comparison is O(1), so the final runtime is O(N) w/r N is the length of the string.


## Space
In the worst case the string is all open brackets, so all of them end up on the stack, which makes the space O(N).
