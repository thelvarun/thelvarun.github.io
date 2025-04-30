---
layout: post
title: Count Subarrays With Score Less Than k
---
### Prompt

```
The score of an array is defined as the product of its sum and its length.

For example, the score of [1, 2, 3, 4, 5] is (1 + 2 + 3 + 4 + 5) * 5 = 75.

Given a positive integer array nums and an integer k, return the number of non-empty subarrays of nums whose score is strictly less than k.

A subarray is a contiguous sequence of elements within an array.
```

### Code Solution

```cpp
long long countSubarrays(vector<int>& nums, long long k) {
    // compute the prefix sums
    vector<long long> prefix(nums.size() + 1);
    prefix[0] = nums[0];
    for (long long i = 0; i < nums.size(); ++i) {
        prefix[i + 1] = nums[i] + prefix[i];
    }
    long long a = 1;
    long long b = 1;
    long long count = 0;
    while (a < prefix.size()) {
        
        // edge case: b is at the edge of the array
        if (b == prefix.size()) {
            long long score = (prefix[prefix.size() - 1] - prefix[a-1]) * ( (prefix.size() - 1) - a + 1);
            if (score < k) {
                count += b - a;
            }
            a++;
        } else {
            long long score = (prefix[b] - prefix[a - 1]) * (b - a + 1);
            if (score < k) {
                b++;
            } else {
                count += b - a; // b is too much, so if we go back, we have indices a, b-1. ( (b-1) - a + 1 = b - a);
                a++;
            }

            // if the subarray of 1 is too big, we just move forward
            if (a > b) {
                b = a;
            }
        }
    }
    return count;
}
```

### Why it works

Let's make some observations. Here's a nice example that we can use and refer to (I've labeled the indices above nums for convenience):

```
k = 30

           0    1    2    3    4    5    6    7    8
nums = [   5,   1,   2,   3, 100,   2,   2,   2,   1]
```

- Let's say you have a subarray `s_a = [i, j]`, representing the subarray in nums from indices i to j, inclusive on both ends. Let `score` be the score for `s_a`. Once `score` exceeds k, you now know that no other subarrays starting at `i` and ending at or past `j` will have a score < k.
    - For example, let's say `i = 0` and `j = 3`.
        - `s_a = [5, 1, 2, 3]` -> `score` is `(5 + 1 + 2 + 3) * len(s_a) = 11 * 4 = 44`, which is greater than `k` (30).
        - We know that every subarray beginning with `i` and ending at an index >= `j` must have a score greater than `k`. Because all the integers in the subarray are positive and we're multiplying by positive number.
    - From this fact, once we find the first `j` where `[i, j] and score >= k`, we now ALL the subarrays where `i` is the first element and the score is less than `k`. It's just all possible subarrays starting at `i` and ending before `j`. The number of subarrays is `(j - 1) - i + 1 = j - i`.
        - EX: Let's take the previous example, `s_a = [5, 1, 2, 3]` -> `score` is `(5 + 1 + 2 + 3) * len(s_a) = 11 * 4 = 44`. `i = 0` and `j = 3` in this case. Our calculation states that there are `j - i` subarrays with `score < k` --> `3 - 0 = 3 subarrays starting with i = 0 with a score < k`.
        - Since `i = 0` and `j = 3` has a score > k, and `j = 3` is the first index where this is true, we know all the possible subarrays starting at `i = 0` and ending at `j = 2` will have a score less than `k`.
            - `[5] (i = 0, j = 0, representing a single element subarray)` 
            - `[5, 1] (i = 0, j = 1)` 
            - `[5, 1, 2] (i = 0, j = 2)` 
        - That's it. There are 3 subarrays starting at `i` that have `score < k`. Our calculation state
- If we knew the number of subarrays with score < k starting at `i` for each index in nums, we could sum the counts to get our final answer.
- Now, here's the real interesting part.
    - Let's say we start building a subarray until the score of the subarray exceeds k at indices `[i, j]`, starting with `i = 0`.
    - Now, how do we continue? We know the number of subarrays that start at `i = 0` is j - i. But now we want to find the number of subarrays that start at `i + 1`. How can we transform this `[i, j]` into `[i + 1, j_]`.
    - Just increment `i`! And check if the score is < k.
- If the score for `[i+1, j]` is < k, that means we can regrow our subarray by increasing the ending index of the subarray (j).
- Here is the tricky part: if  score for `[i+1, j]` is >= k, we know that j is *potentially* the first index for subarrays starting at `[i+1]` where `score >= k`. There is no point to check for indices < j because we know that the scores will always be < k.
    - Why? How are we _so_ certain that there doesn't exist another index i+1 <= m  < j s.t. `[i+1, m]` has a score > k?
        - Well because we know in the first place, in order for `j` to even BE where it is right now, there had to have been another case `i, j-1` where `score < k`. Otherwise, we wouldn't have incremented `j` to its current position. AND since `[i+1, j-1]` is a subarray of `[i, j-1]`, we know FOR SURE that the score must be `< k` because again, we wouldn't have gotten to this point in the execution.
- So the algorithm basically works like a sliding window
    - Keep increasing the window while the score < k
    - When score >= k found, add j - 1 - i + 1 = j - i to the output count. Then, move the leftmost end "forward".
    - And handle some cases at the edge.

### Complexity
***Time Complexity***

Each element in the array is processed at max twice. This means it takes at worst 2 * N, which is `O(N)`.

***Space Complexity***

We use `O(N)` to compute the prefix sums of the nums.