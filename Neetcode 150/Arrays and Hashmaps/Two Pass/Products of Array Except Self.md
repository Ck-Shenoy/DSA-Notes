## Brute Force

Go through each element and loop through the entire array and get the product for it. 
**Time Complexity**: O(n ^ 2)
**Space Complexity**: O(n)

## Better

The array elements can have either a zero, positive and negative values.
1. If there is more than 1 zero, every element including 0th element would have the product as 0.
2. If there's just one zero, all the elements in the output would be 0 except the 0th element since we are excluding it.

So to tackle all these scenarios, we can calculate the product excluding zero's and have it in a variable. If the scenarios are something excluding above one's, then we can divide the product by current element and save it in the output array.

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] output = nums;
        int prod = 1, n = nums.length, zero = 0;
        
        // Collect products and the count of zero's present
        for(int num: nums) {
            if(num == 0) {
                zero++;
                continue;
            }
            prod *= num;
        }

		// Validate and populate the output array
        for(int i=0; i < n; i++) {
            if(
                (output[i] == 0 && zero > 1) // Case 1
                || (output[i] != 0 && zero > 0) // Case 2
            ) {
                output[i] = 0;
                continue;
            }

			// Case 2 and Generic Case
            output[i] = output[i] == 0 ? prod : (prod / output[i]);
        }

        return output;
    }
}
```

**Time Complexity**: O(n)
**Space Complexity**: O(n)
## Optimal 1

**Follow-up**: Could you solve it in O(n) time without using the division operation?

As the question mentions, we must not use the division operator. If that's the case how can we calculate the product?

**For Example**: \[1, 2, 3, 4, 5\]
If I want to know the product of the index 2, without the element 3 then can we say If I know the prefix product till index 1 and postfix product till index 3. I can multiply both and find the product without 3?

The same has been implemented here
```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int prod = 1, n = nums.length, zero = 0;

        int[] prefix = new int[n];
        int[] postfix = new int[n];
        int[] ans = new int[n];
        
        prefix[0] = nums[0];
        postfix[n-1] = nums[n-1];

        for(int i=1; i<n; i++) {
            prefix[i] = prefix[i-1] * nums[i];
            postfix[n-i-1] = postfix[n-i] * nums[n-i-1];
        }

        for(int i=0; i<n; i++) {
            if(i==0) {
                ans[i] = postfix[i+1];
            } else if(i == n-1) {
                ans[i] = prefix[i-1];
            } else {
                ans[i] = prefix[i-1] * postfix[i+1];
            }
        }

        return ans;
    }

}
```

**Time Complexity**: O(n)
**Space Complexity**: O(2n)
## Optimal - 2

We can reduce the space complexity by ommitting the prefix and postfix array using 2 pass method.
1. **Capture Prefix**: Start from 1 and calculate all the prefix last but 1 because we are just calculating the prefixes. Last element isn't a prefix of any other element
2. **Calculate Suffix and Update the output array**: Now that we already have the prefix values, we just need to calculate the suffixes and multiply it with the already existing prefix. For n-1 the suffix would be 1, n-2 it would be (1 * nums[n-1]). The output value would be current prefix value multiplied by suffix value;

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int prod = 1, n = nums.length, zero = 0;  
        int[] ans = new int[n];
        ans[0] = 1;

        // Prefix computation
        for(int i=1; i<n; i++) {
            ans[i] = nums[i-1] * ans[i-1];
        }

        int suffix = 1;

        // Postfix computation
        for(int i=n-2; i>=0; i--) {
            suffix *= nums[i+1];
            ans[i] = ans[i] * suffix;
        }

        return ans;
    }
}
```

**Example Illustration**:
nums = \[1, 2, 3, 4, 5]

-- Prefix Calculation --
for index 0 there isn't any prefix so -> 1
for index 1 the prefix is 1 in nums * existing prefix ans\[i-1]  is 1 -> 1 * 1 = 1
for index 2 the prefix is 2 in nums * existing prefix ans\[i-1] is 1 -> 2 * 1 = 2
for index 3 the prefix is 3 in nums * existing prefix ans\[i-1] is 2 -> 3 * 2 = 6
for index 4 the prefix is 4 in nums * existing prefix ans\[i-1] is 6 -> 4 * 6 = 24

\[1, 1, 2, 6, 24]

-- Suffix Calculation --
for index n-1 there isn't any suffix so suffix is 1 * prefix value of n-1 is 24 --> 1 * 24 = 24
for index n-2 suffix is 1 * nums\[i-1] is 5 * prefix value of n-2 is 6 --> suffix = 5 * 1 = 5, 5 * 6 = 30
for index n-3 suffix is 5 * nums\[i-2] is 4 * prefix value of n-2 is 2 -->suffix = 5 * 4 = 20, 20 * 2 = 40
for index n-4 suffix is 20 * nums\[i-3] is 3 * prefix value of n-2 is 1 --> suffix = 20 * 3 = 60, 60 * 1 = 60
for index n-5 suffix is 60 * nums\[i-4] is 2 * prefix value of n-2 is 1 --> suffix = 60 * 2 = 120, 120 * 1 = 120

**Ans**: \[24, 30, 40, 60, 120]

**Time Complexity**: O(n)
**Space Complexity**: O(1)