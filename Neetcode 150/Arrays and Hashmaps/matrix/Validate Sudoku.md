**Question**: [Validate Sudoku](https://neetcode.io/problems/valid-sudoku/question?list=neetcode150)
## Brute Force:

There isn't any intuition as such for this problem. As the question mentions we need not even check for the upcoming elements. We just have to loop through each row and each column and 3 * 3 boxes and check whatever elements that are present right now are unique or not.
While finding unique elements across rows and columns are easy, finding it on a 3 * 3 matrix is difficult. 
## Intuition

If we had any matrix of some length we can create an unique index so as to store in map using the below formula
int index = current row (i) * row length (n) + current col (j);
So if its a 2 * 3 matrix --> m = 2 & n = 3

i = 0, j = 0 -> 0
i = 0, j = 1 -> 1
i = 0, j = 2 -> 2
i = 1, j = 0 -> 3
i = 1, j = 1 -> 4
i = 1, j = 2 -> 5
all the computed values are unique and elements corresponding to these indexes can be stored in the map with these unique elements.

For our usecase, we can combine 3 columns and 3 rows to consider it to be one block. Like 
row 0->2 & col 0->2 -> row =0, col = 0
row 3->5 & col 3->5 -> row = 1, col = 1
row 6->8 & col 6->8 -> row = 2, col = 2
now from 9 * 9 we came to 3 * 3.
However how can we achieve this?
Simply by division. 
We just need to divide the column indexes by 3 and that way we will get indexes in this way and using above mentioned formula we can map each values on these indexes inside a common map.
Refer the code for better understanding..
## Solution

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        int m = board.length, n = board[0].length;

        HashMap<Integer, Set<Integer>> cube = new HashMap<>(), row = new HashMap<>(), col = new HashMap<>();


        for(int i=0; i<n; i++) {
            for(int j=0; j<n; j++) {
                char ch = board[i][j];

                // Exclude empty spaces
                if(ch == '.') continue;
                int num = Integer.parseInt(ch + "");

                // Row Logic
                if(!row.containsKey(i)) {
                    row.put(i, new HashSet<>());
                }

                // Column Logic
                if(!col.containsKey(j)) {
                    col.put(j, new HashSet<>());
                }

                // 3 * 3 Logic
                int idx = (i/3) * 4 + (j/3);
                if(!cube.containsKey(idx)) {
                    cube.put(idx, new HashSet<>());
                }

                // Validation
                if(cube.get(idx).contains(num)
                || row.get(i).contains(num)
                || col.get(j).contains(num)) {
                    return false;
                }

                // Update Maps
                cube.get(idx).add(num);
                row.get(i).add(num);
                col.get(j).add(num);
            }
        }

        return true;
    }
}
```

**Time Complexity**: O(M * N)</br>
**Space Complexity**: O(3 * M * N)