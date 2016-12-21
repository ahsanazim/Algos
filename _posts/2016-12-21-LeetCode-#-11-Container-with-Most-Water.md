---
layout: post
title: "LeetCode # 11 Container with Most Water"
date:  2016-12-21 13:20:09
categories: Array TwoPointers
---
**Question:**
Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container.


**Solution:**  
{% highlight python %}
def maxArea(height):
  """
  :type height: List[int]
  :rtype: int
  """
  if (len(height) < 2):       # error check
    return 0     

  maxArea = 0
  leftIdx = 0     # move this to right
  rightIdx = len(height) - 1      # move this to left

  while (leftIdx < rightIdx):
    firstH = height[leftIdx]    # height at this spot
    lastH = height[rightIdx]
    currArea = min(lastH,firstH) * (rightIdx - leftIdx)     # h x l
    # update area if necessary
    maxArea = currArea if (currArea > maxArea) else maxArea
    # move shorter height further in
    # since shorter determines area
    if (firstH < lastH):    
      leftIdx += 1
    else:
      rightIdx -= 1

    return maxArea
{% endhighlight %}
