**Question**: [Trapping Rain Water](https://neetcode.io/problems/trapping-rain-water/solution)
## Solution

Similar to [[Products of Array Except Self]], We need to find the highest left height and the highest right height and so to do that. We can have 2 arrays, max left and max right height. After finding those if we can subtract it with the actual height of the input array we can get the water trapped.

![[Pasted image 20260330081700.png]]

As per the image, the total water trapped is `1 + 1 + 2 + 1 + 1 = 6`(the top blue line).

However these extra spaces can be mitigated using 2 pointer approach.

**Half of this was not understandable so skipping for now**

