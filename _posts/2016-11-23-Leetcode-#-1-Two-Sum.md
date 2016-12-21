---
layout: post
title:  "LeetCode # 1	- Two Sum"
date:   2016-11-23 10:26:00
categories: LeetCode Array Hashtable
---
**Question:**
Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.
You may assume that each input would have ***exactly*** one solution.

**Example:**  

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

**Solution:**

{% highlight python %}
def twoSum(self, nums, target):
    """
    :type nums: List[int]
    :type target: int
    :rtype: List[int]
    """
    dict = {}   # functions as a hashmap

    # add to dictionary, with numbers = keys & indices = values
    for indx,val in enumerate(nums):
        dict[val] = indx

    # make sure that you're not just finding the same number
    # at same index (i.e. second `and`)
    for indx,val in enumerate(nums):
        compl = target - val
        if (compl in dict) and (dict[compl] != indx):
            return [indx,dict[compl]]

    # error
    return [-1,-1]
{% endhighlight %}

A crucial fact in the above is that when the map encounters a repeated key, it overwrites its corresponding value. This is important for when we're dealing with something like `nums = [1,5,2,9,0,21,4,4,18,20,12]` and `target=8`. You clearly want `[6,7` returned, and this is only possible because, in the second `and`, you're doing `dict[4] !=  6`, which becomes `6 != 7`, which is `True`, and hence you proceed. Crucially, the index for the repeated number stored is 7, not 6. If this were not the case, then your algorithm would fail.


Here is an ***Alternate Solution*** employing the two pointer technique. I stumbled
upon it while attempting the 3sum problem.

{% highlight python %}
def twoSum(self, nums, target):
  """
  :type nums: List[int]
  :type target: int
  :rtype: List[int]
  """
  nums_s = sorted(nums)
  l_idx = 0
  r_idx = len(nums_s) - 1
  l = None
  r = None
  sol = []

  # find the two numbers
  while (l_idx < r_idx):
    l = nums_s[l_idx]
    r = nums_s[r_idx]
    if (l + r == target):
      break
    elif (l + r < target):
      l_idx += 1
    else:
      r_idx -= 1

  # find their indices in original arr
  for idx, i in enumerate(nums):
    if ((i == l) or (i==r)):
      sol.append(idx)


  return sol
{% endhighlight %}
