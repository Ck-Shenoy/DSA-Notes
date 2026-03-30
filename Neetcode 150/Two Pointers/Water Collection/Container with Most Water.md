**Question**: [Container With Most Water](https://neetcode.io/problems/max-water-container/question?list=neetcode150)
## Brute Force:

As the question says, we need to find 2 heights that provide us the maximum area. The area can be defined as Minimum height between selected 2 heights and the width of 2 points. Basically 
`height * width = area`. So to find these 2 points we can have 2 loops running, and everytime we would find the maximum area. 

```java
class Solution {
    public int maxArea(int[] heights) {
        int n = heights.length;
        int ans = -1;
  
        for(int i=0; i<n-1; i++) {
            for(int j=i+1; j<n; j++) {
                int width = j - i;
                int height = Math.min(heights[i], heights[j]);
                ans = Math.max(ans, height * width);
            }
        }
  
        return ans;
    }
}
```

**Time Complexity**: O(n^2) **</br>**
**Space Complexity**: O(1)

## Optimal

As we know, we need 2 points with the maximum width so as to get a greater area. How about using 2 pointers. One would point to the front and the other would point to the end. Here the array isn't sorted yet we can make use of 2 pointers. If the left pointer has lesser value than the right pointer, we will try to move the left pointer so that we get a better height than current else we would move the right pointer. Because larger the height and width, better the value

```java
class Solution {
    public int maxArea(int[] heights) {
        int n = heights.length;
        int left = 0, right = n-1, ans = -1;
  
        while(left < right) {
            int height = Math.min(heights[left], heights[right]);
            int width = right - left;
            int area = height * width;
  
            ans = Math.max(ans, area);

            if(heights[left] <= heights[right]) {
                left++;
            } else {
                right--;
            }
        }
  
        return ans;
    }
}
```

**Time Complexity**: O(n)</br>
**Space Complexity**: O(1)