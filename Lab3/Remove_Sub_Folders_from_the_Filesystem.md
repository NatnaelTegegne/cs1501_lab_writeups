# Nat205 Implementation

```class Solution:
    def removeSubfolders(self, folder: list[str]) -> list[str]:
        if len(folder) == 0:
            return
        if len(folder) == 1:
            return folder
        folder.sort()

        result = []
        result.append(folder[0])
        last_in_res = 0

        for f in folder:
            prev_in_arr =result[last_in_res] + "/"
            for c1, c2 in zip(f, prev_in_arr):
                if c1 != c2:
                    result.append(f)
                    last_in_res += 1
                    break
            
        return result
```

# How my implementation works

What the problem asks us to solve is that, we have different folders where some are the parent folder and others are subfolders and we are given an array of these folders and subfolders. We are asked to remove the subfolders which are inside these parent folders and return only the parent folders. To do this I sorted the array to put as much parent folders in the lower index of the array, then I have a result array where I append the first parent folder, then loop through the list of the folders to compare their prefix with the one I appended to my  result array in my last iteration. In my for loop I check the previous array with "/" included because the folder names might not only be single letters, so we have to make sure to compare to the end of whichever folder name is smallest in length and makes sure we are matching every char in the name up to the "/". Finally, I compare each char and if they don't match (it's a different path from the current parent or nested differently within the current parent) I add it to my result array and increment its current pointer then exit the inner loop. I do this for every folder in the given array and return the result at the end.


# Runtime and Memory Analysis

## Runtime
I am sorting the initial array and looping through every folder, with a nested loop of the shorter folder name. The sorting is in place and it takes O(N), the outer for loop takes O(N) and the inner takes O(M) where M is the length of the current folder name.
The final runtime ends up being: O(N) + O(N * M) = O(N * M)


## Space
As I mentioned above the sort is in place which makes it best case O(1) but it still uses tim sort under the hood so in the worst case it's O(N). I also use result array which takes O(M) where M is the max different paths from each root folder. So the overall space is O(N) + O(M)



I know this implementation could be more optimal if I used Tries that we are learning in class.