---
layout: post
title: The k-th Lexicographical String of All Happy Strings of Length n
tag: algorithms
---
### Prompt

```
A happy string is a string that:
  - consists only of letters of the set ['a', 'b', 'c'].
  - s[i] != s[i + 1] for all values of i from 1 to s.length - 1 (string is 1-indexed).

For example, strings "abc", "ac", "b" and "abcbabcbcb" are all happy strings and strings "aa", "baa" and "ababbc" are not happy strings.

Given two integers n and k, consider a list of all happy strings of length n sorted in lexicographical order.

Return the kth string of this list or return an empty string if there are less than k happy strings of length n.
```

### Code Solution

```python
def getHappyString(self, n: int, k: int) -> str:
  # there are 3 * 2 * 2 * 2 * 2 ... up 'til n-1 happy strings
  # therefore are 3 * (2^(n-1)) possible happy strings
  num_happy_strings = 3 * (2 ** (n-1))
  if k > num_happy_strings:
      return ""
  output = []
  lo = 1
  hi = num_happy_strings // 3
  if 1 <= k <= num_happy_strings // 3:
      output.append('a')
      lo = 1
      hi = num_happy_strings // 3
  elif (num_happy_strings // 3) + 1 <= k <= (num_happy_strings // 3) * 2:
      output.append('b')
      lo = (num_happy_strings // 3) + 1
      hi = (num_happy_strings // 3) * 2
  else:
      output.append('c')
      lo = ((num_happy_strings // 3) * 2) + 1
      hi = num_happy_strings
  
  def smaller_string(c: str):
      if c == 'a':
          return 'b'
      return 'a'
  
  def larger_string(c: str):
      if c == 'c':
          return 'b'
      return 'c'
      
  
  while lo < hi:
      if lo <= k <= (((lo + hi) // 2)):
          output.append(smaller_string(output[-1]))
          hi = ((lo + hi) // 2)
      else:
          output.append(larger_string(output[-1]))
          lo = (((lo + hi) // 2) + 1)
  return ''.join(output)
```

### Why it works

We can show that there are 3 * 2^(n-1) happy strings by trying to build the number of happy strings.
For the first char, we have 3 choices. For each subsequent char, we have 2 choices since we cannot use the same character as the one we used previously.

For example, if we construct the 1st char to be 'a' and the 2nd letter to be 'b', we know the 3rd character cannot be a 'b', so it must be an 'a' or a 'c'. Let's say the third character is a 'c'. Now we know the 4th character cannot be a 'c', so it must be an 'a' or 'b' - 2 choices again. We can see that this will keep repeating for every character past the first character.

Now, let me list all the happy strings of length 4, in order. By our formula, we should have `3 * (2^(4-1)) = 3 * 2^3 = 3 * 8 = 24` happy strings.

```
abab 01
abac 02
abca 03
abcb 04
acab 05
acac 06
acba 07
acbc 08
---
baba 09
babc 10
baca 11
bacb 12
bcab 13
bcac 14
bcba 15
bcbc 16
---
caba 17
cabc 18
caca 19
cacb 20
cbac 21
cbab 22
cbca 23
cbcb 24
```

Notice anyting? If k was in [1, 8] (inclusive), we know it would start with 'a'. If it was in [9, 16], it would start with 'b'. If it was in [17, 24] it would start with 'c'. Why? It's because after the first character, there are `2^3 = 8` (from our formula!) strings that can be made with the remaining characters. 

We can use that same idea to figure out which would be the next character. Let's say `k = 22`. Since `k` is in the 3rd 8, we know the first character must be a 'c'. Now, can we deduce what the next character will be? We notice that the first `4` happy strings start with 'a', and the second `4` start with 'b'. Since 22 is in the 2nd set of four, we can assume the next character starts with 'b'.

At each choice after the first character, we can figure out which character comes next by checking which "half" of the number we're in. Then, we can narrow down the bucket by half depending on which bucket we chose. This is essentially a binary search, where if we're in the smaller bucket, we choose the smaller character. If we're in the larger bucket, we choose the larger character. The "smaller" and "larger" character are relative to the last character we just appended - for example, if the last character is a 'c', the "larger" character is a 'b'. 

### Complexity
***Time Complexity***

The loop will run `n` times (one iteration for each character), which means the time complexity is `O(n)`.

***Space Complexity***

Because we use a string of `n` characters, the output will use `O(n)` space.