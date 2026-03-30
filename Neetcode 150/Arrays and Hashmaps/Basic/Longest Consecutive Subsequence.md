**Question**: [Longest Consecutive Subsequence](https://neetcode.io/problems/longest-consecutive-sequence/question?list=neetcode150)
## Brute

As the question suggests, loop through each element in nums and check if the next element exists in the array. If yes, capture the length of the consecutive sequence and update the answer variable with the max value.

**Time Complexity**: O(n^2)</br>
**Space Complexity**: O(1)
## Better

To optimize the approach further, we can sort the whole array and loop through each element. If the current element is 1 greater than previous we will increment the count else we will reset the count back to 1.

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        int count = 1, ans = 0, n = nums.length;
        Arrays.sort(nums);

        for(int i=0; i<n; i++) {
            int num = nums[i];
            if(i > 0 && num == nums[i-1]) {
                continue; // skip same elements
            } else if(i > 0 && num == nums[i-1] + 1) {
                count++; // increment
            } else {
                count = 1; // reset
            }
            
            ans = Math.max(count, ans);
        }

        return ans;
    }
}
```

**Time Complexity**: O(nlogn)</br>
**Space Complexity**: O(1)
## Optimal

To optimise the time complexity even further we need to come up with an algorithm that runs in linear time.
So we know one thing that the sequence at which the element occurs at doesn't matter at all. That means we just need to make sure the elements are present in the array. If that's the case, how about using a set and store all the unique elements. 
Then to find the sequence count, let's loop through each element in the array. For every element let's check if there is any previous element that's also present in the array. Since if this element is in middle of some sequence, we need to reconsider this for other sequences. Better we will find the root element. 
Then from root element we will count the full sequence that's present in the array, and to make sure we don't reiterate the same elements again let's remove these visited elements.
Ideally, after this each element must have been visited only once, since as per the below code we count the path from current -> first node and current -> last node. So we won't process duplicate paths.

**Time Complexity**: O(n)</br>
**Space Complexity**: O(n)

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        int n = nums.length, ans = 0;
        HashSet<Integer> mp = new HashSet<>();
  
        for(int i=0; i<n; i++) {
            if(!mp.contains(nums[i])) {
                mp.add(nums[i]);
            }
        }

        for(int curr: nums) {
            int count = 1;
            int num = curr;
  
			// Traverse the path from current to start node
            while(mp.contains(num-1)) {
                mp.remove(num-1); // remove visited nodes, so that we don't visit it again.
                count++;
                num -= 1;
            }

            num = curr;

			// Traverse the path from current to last node.
            while(mp.contains(num+1)) {
                mp.remove(num); // remove visited nodes.
                count++;
                num += 1;
            }

            ans = Math.max(ans, count); // count the maximum path
        }

        return ans;
    }
}
```