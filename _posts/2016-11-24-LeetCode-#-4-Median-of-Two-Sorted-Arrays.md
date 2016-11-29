---
layout: post
title:  "LeetCode # 4	-  Median of Two Sorted Arrays"
date:   2016-11-24 11:52:00
categories: LeetCode BinarySearch Array DivideAndConquer
---
**Question:**
There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

**Example:**  

Given
```
nums1 = [1, 3]
nums2 = [2]
```
The median is `2.0`

Given
```
nums1 = [1, 2]
nums2 = [3, 4]
```
The median is (2 + 3)/2 = `2.5`

**Solution:**

{% highlight python %}
def findMedianSortedArrays(self, nums1, nums2):
  """
  :type nums1: List[int]
  :type nums2: List[int]
  :rtype: float
  """
  len1 = len(nums1)
  len2 = len(nums2)
  sumLen = len1 + len2
  oddMedIdx = False if ((sumLen % 2) == 0) else True
  targetIdx = (sumLen / 2) if oddMedIdx else ((sumLen / 2) - 1)
  currIdx = 0
  currIdx1 = 0
  currIdx2 = 0

  # go till target idx, or till you fall off one arr
  while ((currIdx < targetIdx) and (currIdx1 < len1) and (currIdx2 < len2)):
    if (nums1[currIdx1] < nums2[currIdx2]):
        currIdx1 += 1
    else:
        currIdx2 += 1
    currIdx += 1

  # if you still have space in both arrs
  if (currIdx == targetIdx) and (currIdx1 < len1) and (currIdx2 < len2):
    if (oddMedIdx):
        if (nums1[currIdx1] < nums2[currIdx2]):
          return nums1[currIdx1]
        else:
          return nums2[currIdx2]
    else:
      first = -1
      second = -1
      # finding first half of median
      if (nums1[currIdx1] < nums2[currIdx2]):
        first = nums1[currIdx1]
        currIdx1 += 1
      else:
        first = nums2[currIdx2]
        currIdx2 += 1

      # second half of median

      # reached end of one arr
      if not ((currIdx1 < len1) and (currIdx2 < len2)):  
        second = nums1[currIdx1] if (currIdx1 < len1) else nums2[currIdx2]
      else:   # still have space
        if (nums1[currIdx1] < nums2[currIdx2]):
          second = nums1[currIdx1]
          currIdx1 += 1
        else:
          second = nums2[currIdx2]
          currIdx2 += 1                    

      return (first + second) / 2.0
  # you fell off one side
  else:  
    arr = nums1 if (currIdx1 < len1) else nums2     # which arr
    arrIdx = currIdx1 if (currIdx1 < len1) else currIdx2    # which
    while (currIdx < targetIdx):
      arrIdx += 1
      currIdx += 1
    if (oddMedIdx):
      return arr[arrIdx]
    else:
      return (arr[arrIdx] + arr[arrIdx + 1]) / 2.0

  return -1       # error   
{% endhighlight %}

***runtime beats 94.85% of python submissions ;)***

Although the solution above is by no means clean, it is efficient! I may
try and make it more readable in the future.
