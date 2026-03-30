**Question**: [3 Sum](https://neetcode.io/problems/three-integer-sum/question?list=neetcode150)
## Brute Force:

As the problem suggests, run 3 loops to find 3 elements that combine to get a target of 0.

Note: While doing this problem, I found out something about java. List.of creates immutable lists that cannot be modified. Whereas Arrays.asList creates mutable list, so if we want to perform sorting and stuff it is recommended to use asList else if we are sure to not have any actions on list then we must use List.of.

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        int n = nums.length;
        Set<List<Integer>> ans = new HashSet<>();

        for(int i=0; i<n-2; i++) {
            for(int j = i+1; j<n-1; j++) {
                for(int k = j+1; k < n; k++) {
                    int sum = nums[j] + nums[i] + nums[k];
                    if(sum == 0) {
                        List<Integer> tmp = Arrays.asList(nums[i], nums[j], nums[k]);
                        Collections.sort(tmp);
                        ans.add(tmp);
                    }
                }
            }
        }

        return List.copyOf(ans);
    }
}
```

**Time Complexity**: O(n^3) + Sorting</br>
**SpaceComplexity**: O(3)
## Better

As Discussed in previous problem [[Two Sum II Input Array is Sorted]],  we can sort the full array first. Then select an integer i, and proceed with two pointer approach for j and k. This way we would have the time complexity of n (i traversal) * n (j & k traversal).

```java
class Solution {
        public List<List<Integer>> threeSum(int[] nums) {
            int n = nums.length;
            Set<List<Integer>> ans = new HashSet<>();

            Arrays.sort(nums);

            for(int i=0; i<n-2; i++) {
                int j = i+1, k = n-1;
                while(j < k) {
                    int val = nums[i] + nums[j] + nums[k];
                    if(val == 0) {
                        ans.add(List.of(nums[i], nums[j], nums[k]));
                        k--;
                        j++;
                    } else if (val > 0) {
                        k--;
                    } else {
                        j++;
                    }
                }
            }

            return List.copyOf(ans);
        }
}
```

**Time Complexity**: O(nlogn) + O(n ^ 2) = O(n^2)</br>
**Space Complexity**: O(n^2) - Set

## Optimal

The above solution is almost the answer, but instead of using an extra space can we exclude the duplicate elements. That means how about moving the pointer and excluding elements having the previous elements the same as the current one for all 3 i, j and k.

```java
class Solution {
        public List<List<Integer>> threeSum(int[] nums) {
            int n = nums.length;
            List<List<Integer>> ans = new ArrayList<>();

            Arrays.sort(nums);

            for(int i=0; i<n-2; i++) {
                // skip duplicate i
                if(i > 0 && nums[i] == nums[i-1]) continue;
  
                int j = i+1, k = n-1;
  
                while(j < k) {
                    int val = nums[i] + nums[j] + nums[k];
                    if(val == 0) {
                        ans.add(List.of(nums[i], nums[j], nums[k]));
                        j++;
                        k--;
                        // skip duplicate j
                        while(j < k && nums[j] == nums[j-1]) j++;
                        // skip duplicate k (not necessary, moving i and j is enough)
                        while(j < k && nums[k] == nums[k+1]) k--;
                    } else if (val > 0) {
                        k--;
                    } else {
                        j++;
                    }
                }
            }
            
            return ans;
        }
}
```

**Time Complexity**: O(n log n) + O(n^2) = O(n^2)</br>
**Space Complexity**: O(1)
