---
layout: post
title:  "LeetCode # 3	- Longest Substring without Repeating Characters"
date:   2016-11-23 11:35:00
categories: LeetCode TwoPointers Hashtable String
---
**Question:**
Given a string, find the length of the longest substring without repeating characters.

**Example:**  

Given `abcabcbb`, the answer is `abc`, which the length is 3.

Given `bbbbb`, the answer is `b`, with the length of 1.

Given `pwwkew`, the answer is `wke`, with the length of 3. Note that the answer must be a substring, `pwke` is a subsequence and not a substring.


**Solution:**

{% highlight python %}
def lengthOfLongestSubstring(self, s):
    """
    :type s: str
    :rtype: int
    """
    longest = 0
    currLen = 0
    right = 0
    left = 0
    hashmap = {}

    for idx,c in enumerate(s):
        # is it in the current window?
        if (c in hashmap) and (hashmap[c] >= left):      
            left = hashmap[c] + 1       # window starts @ prev+1
            hashmap[c] = idx     # new idx for word (c repeated)
            currLen = right - left + 1             # view note
        else:
            hashmap[c] = idx
            currLen += 1
            if currLen > longest:
                longest = currLen
        right += 1

    return longest   
{% endhighlight %}

***runtime beats 94.66% of python submissions ;)***

Note on `currLen = right - left + 1`:
when we're at that point, then, firstly, `right` and `left` **are**  correct indices --> especially `right` ---> `right` is incremented at the end of each loop to ready it for the new loop. Hence if `l = 2`, `r = 4`, then they are the correct positions!

In this case, if we do `length = r - l`, then `length = 2`. Buttt, it should actually be `length = 3` (basic indexing stuff --> `abcdec` means `[0]a,[1]b,[2]c,[3]d,[4]e,[5]c`, where `length` is actually `3`).
