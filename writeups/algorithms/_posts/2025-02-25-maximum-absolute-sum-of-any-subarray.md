---
layout: post
title: Maximum Absolute Sum of Any Subarray
---
### Prompt

```
You are given an integer array nums. The absolute sum of a subarray [numsl, numsl+1, ..., numsr-1, numsr] is abs(numsl + numsl+1 + ... + numsr-1 + numsr).

Return the maximum absolute sum of any (possibly empty) subarray of nums.

Note that abs(x) is defined as follows:
  - If x is a negative integer, then abs(x) = -x.
  - If x is a non-negative integer, then abs(x) = x.
```

### Code Solution

```python
def maxAbsoluteSum(self, nums: List[int]) -> int:
  # keep track of the max positive and the max negative subarray sums
  best = abs(nums[0])

  # we don't even need an array
  best_positive = nums[0]
  best_negative = nums[0]
  for num in nums[1:]:
      best_positive = max(best_positive + num, num)
      best_negative = min(best_negative + num, num)
      best = max(abs(best_positive), abs(best_negative), best)
  return best
```

### Why it works

Imagine if we were to ask for the maximum subarray sum if all the integers were positive. `EX: [1, 5, 4, 53 , 19]`. We would just take the entire subarray! So now, to make this question a little more interesting, let's throw some negative numbers into the mix and ask the same question.

`EX: [1, 5, -4, -53 , 19]`. Now, let's go through each number, asking if we can make the max subarray value by adding this number or not. Coming from our previous example, an observation we made was that we should basically add every positive number because... why wouldn't we if we want the max subarray sum? 

But what happens if we run into negative numbers? Well consider the subarray `[-1, 5]`. As we iterate through the array, we get to `-1` and the max subarray sum so far is `-1`. But when we run into `5`, should we add the `5` to our subarray of `-1`? Of course not! It's better to just start a new subarray starting with 5 instead of including the `-1`.

Now, let's think about consecutive negative numbers: `[-3, -2, -1]`. Let's start the iteration at `-3`. Max subarray so far is `-3`. Now, when we get to `-2`, should we combine the `-3` with `-2`? Of course not! We would just start at `-2`. It seems that at each element in the array, we should make a decision - either we add this current element to our subarray sum so far OR we just start a new subarray with this element. And we would just start a new subarray when the sum between the previous max subarray sum and the current element is less than JUST the current element by itself.

This question basically adds onto this problem by introducing the absolute value arrangement. So we can work around that by keeping track of both the max subarray sum and the min subarray sum. Then, during our iteration, it's important to choose the best of the both, using the absolute value.

### Complexity
***Time Complexity***

The loop will run `n` times (one iteration for each element in the array), which means the time complexity is `O(n)`.

***Space Complexity***

Because we don't use any space proportional to the length of the array, the space complexity is `O(1)`.