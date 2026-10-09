# Nat205 Implementation

```
class Solution:
    def maxProduct(self, nums: list[int]) -> int:
        heapq.heapify_max(nums)

        num1 = heapq.heappop_max(nums)
        num2 = heapq.heappop_max(nums)

        return (num1-1)* (num2-1)
```

# How my implementation works

The problem asks us to return the maximum value of (nums[i]-1) * (nums[j]-1) for two different indices. Since all the numbers are positive, the biggest product will always come from the two biggest numbers in the array. To find them I used a max heap, which we are learning in class. I heapify the array so the largest element is at the root, then pop twice to get the largest and second largest numbers. Popping twice also handles the case where the two largest values are equal. Finally, I subtract 1 from both, multiply them and return the result.


# Runtime and Memory Analysis

## Runtime
Heapify takes O(N) and each pop takes O(log N), so the final runtime is O(N) + O(2 log N) = O(N)


## Space
Heapify works in place and I only store two extra variables, so the space is O(1).
