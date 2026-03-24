
## Intuition:

**Brute force**:

Here we need to know the Top K frequent elements and so since we need to count the frequency we can use hashmap. 
Each hashmap would store the number of times each element has occurred.
**Example** {
	 1 --> 2
	 5 --> 1
	 3 --> 6
	 4 --> 4
}
K = 2
Then to find the frequent k, we need to fetch each element put it into a list with List<Int, Int> and sort it by the frequency. Then get the values.

**Example**: [{3, 6}, {4, 4} , {1, 2}, {5, 1}]
**Ans**: [3, 4]

**Optimal**:

In the previous approach, we had 2 phases. 
1. Counting the occurences
2. Sorting as per the occurence
Counting the occurence is inevitable and cannot be optimized since its already O(n). But during sorting we are actually taking another List of size O(n), then sorting it with time complexity of nlogn. Instead, we can use priority queue of size k.
Using min heap, we can keep on inserting **occurences - element** pair. So the if the pq size is more than k then the value with less occurence would be removed and Hence the resulting priority queue would just have k elements.

**Example**: [
	 6 --> 3
	 4 --> 4
	 1 --> 2
]

**Solution**:

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        PriorityQueue<Pair<Integer, Integer>> pq
            = new PriorityQueue<>((u, v) -> u.getKey() - v.getKey());
        HashMap<Integer, Integer> mp = new HashMap<>();

        for(int num: nums) {
            if(!mp.containsKey(num)) {
                mp.put(num, 0);
            }

            mp.put(num, mp.get(num) + 1);
        }

        for(var entry: mp.entrySet()) {
            pq.add(new Pair(entry.getValue(), entry.getKey()));
            if(pq.size() > k) {
                pq.poll();
            }
        }

        int[] ans = new int[k];

        for(int i=0; i<k; i++) {
            ans[i] = pq.poll().getValue();
        }

        return ans;
    }
}
```