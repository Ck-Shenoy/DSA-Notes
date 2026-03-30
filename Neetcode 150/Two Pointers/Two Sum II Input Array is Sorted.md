**Question**: [Two Sum II](https://neetcode.io/problems/two-integer-sum-ii/question?list=neetcode150)
## Brute 

We know that we need 2 elements that add up to the sum. We can have 2 for loops that run and checks if 2 elements add up to the target

**Time Complexity**: O(n^2)</br>
**Space Complexity**: O(1)

## Better:

So to traverse each element we need one loop. However, there's one more loop to check for the second element. Is it really necessary?
The array is sorted, if that's the case we can just apply binary search and find the second element here right.

**Time Complexity**: O(n log n)</br>
**Space Complexity**: O(1)

## Optimal

As we know the array is sorted, and we need 2 indexes which aggregates to the target. Can we use two pointers here. We will have a start pointer and an end pointer. The start pointer moves towards right and the end pointer moves opposite. We will add the first and last elements, if the sum is greater than target that means there's no point incrementing start pointer since that's the lowest. In that case we can decrement the end pointer so as to get smaller sum than current and the vice versa to be done when the sum is lesser than target (i.e., increment the i'th pointer)

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int i =0, j = numbers.length - 1;
        
        while(i <= j) {
            int ans = numbers[i] + numbers[j];
            
            if(ans == target) {
                return new int[]{i+1, j+1};
            } else if (ans > target) {
                j--;
            } else {
                i++;
            }
        }

        return new int[]{-1, -1};
    }
}
```

**Time Complexity**: O(n)</br>
**Space Complexity**: O(1)