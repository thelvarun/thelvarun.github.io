---
layout: post
title: Construct Smallest Number From DI String
---
*Note: This is my first writeup - it'll probably be messy. But -- I have to start somewhere :).*

---

### Prompt


```
You are given a 0-indexed string pattern of length n consisting of the characters 'I' meaning increasing and 'D' meaning decreasing.

A 0-indexed string num of length n + 1 is created using the following conditions:
  - num consists of the digits '1' to '9', where each digit is used at most once.
  - If pattern[i] == 'I', then num[i] < num[i + 1].
  - If pattern[i] == 'D', then num[i] > num[i + 1].

Return the lexicographically smallest possible string num that meets the conditions.
```

### Code Solution

```python
def smallestNumber(self, pattern: str) -> str:
    
    # start out with the smallest substring by default and reverse the indices where we see continuous D's all the way to i of D+1
    smallest_num = []
    for i in range(len(pattern) + 1):
        smallest_num.append(str(i+1))
    
    # reverses the list in place from [start, end] inclusive on both start and end
    def reverse_in_place(l: list[str], start: int, end: int):
        while start < end:
            l[start], l[end] = l[end], l[start]
            start += 1
            end -= 1
    
    cur = 0
    while cur < len(pattern):
        if pattern[cur] == 'D':
            start = cur
            # keep looping until we reach the end or we reach an I
            while cur < len(pattern) and pattern[cur] == 'D':
                cur += 1
            end = cur
            reverse_in_place(smallest_num, start, end)
        else:
            cur += 1
    return ''.join(smallest_num)
```

### Why it works

Because we want to make the smallest number, we should prioritize putting the small digits first.

Additionally, whenever you see a string of "D" characters in a row (let's say you see *m* D's in a row), you know you need to "reserve" *m + 1* digits -- that is, the character at the start of the chain of D's should have *m + 1* digits less than it available for use.

For example, if we saw the string "IDDDI", we know that we need at least 4 digits (in this case, *m* = 3 since there's a chain of 3 D's) to fulfill the decrease requirements. We should choose the smallest digit that can fulfill the requirment, because it doesn't make sense to choose a digit larger than what's necessary. If we did that, we obviously wouldn't have the minimum number.

For clarity, what I mean is that is that in this same example ("IDDDI"), it doesn't make sense to have a string of digits for the D's like "187659" because we know we can choose "5" instead of "8" at the start of the string of D's.

So, if we start with the smallest string possible "123456789" (or up til n+1, which may be less than 9 digits) and, for every string of D's, we reverse the indices associated with the indexes of the D's for every continuous string of D's, we know we're choosing the smallest value that we could've chosen for each string of D's. Because if there were another string of D's smaller, then the starting value would have to disobey ascending order we set in the beginning.

### Connection to other solutions

One solution that was pretty interesting was the idea of using a stack to figure out the correct sequence. This briefly went through my mind as I was solving the problem, but I couldn't make it work in my head as easily as the previous solution.

The idea is that you basically keep a stack and an output list. We can iterate through the indices of the pattern, and push each ```index+1``` onto the stack. The question now becomes, when do we pop the numbers off the stack and into our output list?

Whenever we encounter a 'D', we know that the next number should come before what's currently on top of the stack. If we we're to pop the entire stack into the output list on the current iteration, we'd break the requirements since the current index would be greater than the top of the stack. So instead, when we see a 'D', don't pop the stack into the output list.

The idea here is that because the stack is LIFO, this will automatically reverse the numbers for us when we decide to pop them off the stack.

Now, whenever we see an 'I' *OR* reach the end of the last, we can empty the contents of the stack into our output list because we know the next index MUST be larger than everything we've seen up 'til this point (everything on the stack). So, to fulfill the current 'I' requirement, we put everything on the output list. If we saw consecutive I's, then the stack would empty itself of 1 element (the consecutive indices) each time.

The connection between my solution and the stack solution seems like the stack ends up doing the reversing for me. In my solution, I basically go through and emulate what the stack would've done - reverse the elements and then append. Additionally, because I create the smallest numbers in the beginning, I *also* skip the I's because... well... they're already in order!