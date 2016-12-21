---
layout: post
title: "LeetCode # 14 Longest Common Prefix"
date:  2016-12-21 13:26:27
categories: String
---
**Question:**
Write a function to find the longest common prefix string amongst an array of strings.

**Solution:**  
{% highlight python %}
def isCommonLetter(self,idx,char,strs):
  """ True if all strings have same `char` at specified `idx` """
  for s in strs:
    if ((idx >= len(s)) or (s[idx] != char)):
      return False
  return True

def longestCommonPrefix(self, strs):
  """
  :type strs: List[str]
  :rtype: str
  """
  prefix = ""
  if ((len(strs) == 0) or (strs[0] == "")):
    return prefix

  for idx, i in enumerate(strs[0]):
    if (self.isCommonLetter(idx,i,strs)):
      prefix += i
    else:
      break

  return prefix
{% endhighlight %}
