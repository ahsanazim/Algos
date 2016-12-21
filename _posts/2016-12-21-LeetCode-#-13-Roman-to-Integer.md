---
layout: post
title: "LeetCode # 13 Roman to Integer"
date:  2016-12-21 13:24:55
categories: Math String
---
**Question:**
Given a roman numeral, convert it to an integer.

Input is guaranteed to be within the range from 1 to 3999.


**Solution:**  
{% highlight python %}
def romanToInt(s):
  """
  :type s: str
  :rtype: int
  """
  num = 0
  converter = { 'I': 1, 'V': 5, 'X': 10, 'L': 50, 'C': 100, 'D': 500, 'M': 1000 }

  for i in range(len(s)):
    curr = converter[s[i]]
    next = converter[s[i+1]] if (i < (len(s) - 1)) else curr
    if (curr < next):
      num -= curr
    else:
      num += curr

  return num
{% endhighlight %}
