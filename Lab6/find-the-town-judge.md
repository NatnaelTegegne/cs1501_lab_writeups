# Nat205 Implementation

```
class Solution:
    def findJudge(self, n: int, trust: list[list[int]]) -> int:
        hashSet = set()
        trustedCount = {}
        for i in range(len(trust)):
            hashSet.add(trust[i][0])
            trustedCount[trust[i][1]] = trustedCount.get(trust[i][1], 0) + 1

        for person in range(1, n + 1):
            if person not in hashSet and trustedCount.get(person, 0) == n - 1:
                return person

        return -1
```

# How my implementation works

The problem gives us n people and a list of trust pairs [a, b] meaning person a trusts person b. The town judge is the person who trusts nobody and is trusted by everyone else, so we have to return that person or -1 if there is no judge. We can think of this as a directed graph where each pair is an edge from a to b, so the judge is the node with no outgoing edges and n - 1 incoming edges. To do this I loop through the trust list once, add every person who trusts someone to a set, and count how many people trust each person in a dictionary. Then I loop through every person from 1 to n and return the first one who is not in the set and is trusted by n - 1 people. If no one matches, I return -1. This also handles the case where n is 1 and the trust list is empty, since that person isn't in the set and is trusted by 0 = n - 1 people.


# Runtime and Memory Analysis

## Runtime
The first loop goes through the trust list once, which is O(T) w/r T is the number of trust pairs, and the set and dictionary operations are O(1) on average. The second loop goes through every person, which is O(N). So the final runtime is O(T + N)


## Space
The set and the dictionary can each hold at most N people, so the space is O(N).
