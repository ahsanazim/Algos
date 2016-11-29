---
layout: post
title:  "LeetCode # 5	-  Longest Palindromic Substring"
date:   2016-11-26 22:36:00
categories: LeetCode String DynamicProgramming
---
**Question:**
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

*Example:*  
- Input: `"babad"`  
- Output: `"bab"`  
Note: `"aba"` is also a valid answer.

*Example II:*  
- Input: `"cbbd"`  
- Output: `"bb"`

**Solution:**

An intuitive **O(n<sup>2</sup>) solution** operates on the principle that palindromes will be
common to a string and its reversed form. E.g. `"bdb"` in `"abdbcef"` and `"fecbdba"`.

This requires implementing the *longest common substring* algorithm, but with a slight
modification - you have to check for the case where [there exists a reversed copy of a non-palindromic substring in some other part of](https://leetcode.com/articles/longest-palindromic-substring/#approach-3-dynamic-programming-accepted) the string. This is expanded upon in the actual code below. Note that the code presented
is more verbose than would be entered in an actual timed contest, but is more valuable for
pedagogic purposes.

{% highlight python %}
def reverseStr(self,s):
  """ helper function to reverse string """
  revS = ""
  i = len(s) - 1
  while (i >= 0):
      revS += s[i]
      i -= 1
  return revS

def validIdxs(self,idx1,idx2,lenArr,currLen):
  """
  tests if reversed form of substring created
  via idx2 is of same indices as that formed
  via idx1
  """
  idxsFirst = range(idx1-currLen+1,idx1+1)
  idxsSecond = range(idx2-currLen+1,idx2+1)

  for j in idxsSecond:
      revIdx = lenArr - 1 - idx2
      if not (revIdx in idxsFirst):
          return False

  return True


def longestCommonSubstr(self,s1,s2):
  """
  longest common substring b/w s, revS
  while taking care to exclude "bcg" in
  abcgefgcb / bcgfegcba --> an edge case
  and keep "xyx" in xyxdefv / vfedxyx
  """
  len1 = len(s1)
  len2 = len(s2)
  mat = [[0 for _ in range(len2 + 1)] for _ in range(len1 + 1)]
  longest = 0
  lcs = []

  for i in range(len1):
      for j in range(len2):
          if (s1[i] == s2[j]):
              currLen = mat[i][j] + 1
              mat[i+1][j+1] = currLen
              currSubstr = s1[i-currLen+1:i+1]
              if ((currLen > longest) and (self.validIdxs(i,j,len2,currLen))):
                  longest = currLen
                  lcs = []
                  lcs.append(currSubstr)
              elif (currLen == longest):
                  lcs.append(currSubstr)

  if (len(lcs) >= 1):
      return lcs[0]       # return any
  else:
      return ""     # no valid commmon substr

def longestPalindrome(self,s):
  """ longest palindromic substring in s """

  # reverse string
  revS = self.reverseStr(s)

  # find longest common section of both
  # original and reversed, while taking
  # care of edge case
  rev = self.longestCommonSubstr(s,revS)

  return rev
{% endhighlight %}

There are a few **faster solutions** available to us as well:

**Brute Force with O(n<sup>3</sup>) time, O(1) space**

Simply iterate through all the substrings of the input string, and
continuously update the longest palindrome you find among them.

{% highlight python %}
def isPalind(self,s):
    """ true if s is a palindrome """
    for idx, c in enumerate(s):
      if (idx == (len(s) / 2)):       # reached middle
        break
      compl = s[len(s) - idx - 1]   
      if (c != compl):
        return False
    return True

def genSubstrings(self,s):
  """ returns array of all unique substrings of s """
  subs = []
  n = len(s)
  for i in range(0,n):
    for j in range(1,n + 1 - i):
      currSubstring = s[i:i+j]
      if not (currSubstring in subs):
        subs.append(currSubstring)
  return subs

def longestPalindrome(self,s):
  longest = ""
  substrings = self.genSubstrings(s)
  for i in substrings:
    if ((self.isPalind(i)) and (len(i) > len(longest))):
      longest = i
  return longest
{% endhighlight %}

**Dynamic Programming with O(n<sup>2</sup>) time, O(n<sup>2</sup>) space**

Here we operate on the principle that if a string `aba` is a palindrome, and it is
surrounded one the left and right by the same character, then said new string will also
be a palindrome.

We therefore create a matrix of booleans with entries representing whether the
substring at indices `s[i:j]` is a palindrome or not.

We begin by initializing the 1 and 2 letter palindromes as `True` - these base cases
cannot be inductively derived.

Then we take a bottom-up approach, building up palindromic substrings of progressively
increasing lengths.

Crucially, note that a substring `s[i:j]` is only a palindrome if the character at `s[i]` is
equal to that at `s[j]`, *and* if `s[i+1][j-1]` is a palindrome as well - with the latter being
gleaned from our table. Note that in a string such as `cbabc`, with us testing the `c` characters,
`s[i]` and `s[j]` are both c, and `s[i+1][j-1]` is `bab`.

The full solution below is heavily commented (I translated [this](http://www.geeksforgeeks.org/dynamic-programming-set-12-longest-palindromic-subsequence/) solution to python but kept the comments), so should end up relatively self-explanatory.

{% highlight python %}
def longestPalindrome(self,s):

  n = len(s)  # get length of input string
  start = 0
  end = 1
  maxLength = 1

  # table[i][j] will be false if substring str[i..j]
  # is not palindrome.
  # Else table[i][j] will be true
  table = [[False for _ in range(n)] for _ in range(n)]

  # All substrings of length 1 are palindromes
  for i in range(n):
      table[i][i] = True

  # check for sub-string of length 2.
  for i in range(n-1):
      if (s[i] == s[i+1]):
          table[i][i+1] = True
          start = i
          end = i + 2
          maxLength = 2

  # Check for lengths greater than 2. k is length
  # of substring
  for k in range(3,n+1):
      # Fix the starting index
      for i in range(0,n-k+1):
          # Get the ending index of substring from
          # starting index i and length size
          j = i + k - 1

          # checking for sub-string from ith index to
          # jth index iff str[i+1] to str[j-1] is a
          # palindrome
          if ((table[i+1][j-1]) and (s[i] == s[j])):
              table[i][j] = True

              if (k > maxLength):
                  start = i
                  end = j + 1
                  maxLength = k

  return s[start:end]
{% endhighlight %}

The O(n<sup>2</sup>) space results from our table.

**Expanding around the center with O(n<sup>2</sup>) time, O(1) space**

For now I'll just blatantly quote [leetcode]() here:

> We observe that a palindrome mirrors around its center. Therefore, a palindrome can be expanded from its center, and there are only `2n−1` such centers.

>You might be asking why there are `2n −1` but not `n` centers? The reason is the center of a palindrome can be in between two letters. Such palindromes have even number of letters (such as `abba`) and its center are between the two `b`s.


To be updated later, hopefully :)

{% highlight python %}
def expandAroundCenter(self,s,left,right):
  L = left
  R = right
  while (((L >= 0) and (R < len(s))) and (s[L] == s[R])):
      L -= 1
      R += 1
  return R - L - 1

def longestPalindrome(self, s):
  start = 0
  end = 0

  for i in range(len(s)):
      len1 = self.expandAroundCenter(s, i, i)
      len2 = self.expandAroundCenter(s, i, i + 1)
      length = max(len1, len2)
      if (length > (end - start)):
          start = i - ((length - 1) / 2)
          end = i + (length / 2)

  return s[start:end + 1]
{% endhighlight %}
