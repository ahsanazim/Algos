---
layout: post
title: "LeetCode # 16 - 3Sum Closest"
date:  2016-12-23 15:32:48
categories: Array TwoPointers
---
**Question:**
Given an array `S` of `n` integers, find three integers in `S` such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

```
For example, given array S = {-1 2 1 -4}, and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

**Solution:**  
{% highlight python %}
def threeSumClosest(nums, target):
  n = len(nums)
  nums = sorted(nums)
  sol = None

  # error
  if (n < 3):
    return None

  # idx 0 till 3rd last element (b/c triple)
  for i in range(0, n-2):
    l = i + 1
    r = n - 1
    if (i == 0):  # initialize sol
      sol = nums[i] + nums[l] + nums[r]             

    while (l < r):
      sum = nums[i] + nums[l] + nums[r]
      # better sol found
      if (abs(sum - target) < abs(sol - target)):
        sol = sum
      if (sum == target):   # sum equals target, found solution!
        return sum
      elif (sum < target):  # sum too small, make bigger
        l += 1
      else:                 # vice versa
        r -= 1

    return sol
{% endhighlight %}
