---
layout: post
title:  "Longest Common Substring"
date:   2016-11-26 14:46:00
categories: String
---
Finding the longest common substring between two strings ends up being a simple,
yet useful, stepping stone to more involved problems such as finding the longest
common subsequence, or finding the longest palindromic substring. Here I'll go through a decent way to attempt this problem.

The **basic algorithm** is as follows:

Iterate through both strings, gradually filling a 2d matrix whose rows and columns represent the two strings respectively.

Each entry of the matrix represents the length of the longest common substring up to that character (essentially the *longest common suffix* - for this reason you'll need the matrix's dimensions to be 1 longer than the length of the respective strings).

You simultaneously update a `longest` string `int`, as well as a `set` of strings - each time you encounter a new 'longest' substring, you add it to the set. The set can end up containing more than one string if
there are multiple "longest" strings.

This runs in `O(m*n)` time, with `m` and `n` being the lengths of the strings respectively.

Some more specific **pseudocode** might help:

```
function longestCommonSubstr(S[1..m], T[1..n])
    L := array(1..m, 1..n)          
    z := 0                          
    ret := {}
    for i := 1..m
        for j := 1..n
            if S[i] == T[j]                   
                if i == 1 or j == 1           
                    L[i,j] := 1
                else
                    L[i,j] := L[i-1,j-1] + 1        
                if L[i,j] > z                       
                    z := L[i,j]
                    ret := {S[i-z+1..i]}            
                else
                if L[i,j] == z                      
                    ret := ret ∪ {S[i-z+1..i]}          
            else                                
                L[i,j] := 0                    
    return ret
```

Since the above from wikipedia almost seems harder to read than the actual
algorithm, this might be easier to digest:

```
def longestCommonSubstr(s1,s2):

  initialize:
  - matrix      
  - set
  - longest

  for each of the m*n character combinations:
    if same character:
      in matrix, increment length of longest substring till this character
      if we have a new  longest substring:
        specify curr substring as longest
        empty the set and add this as sole new longest string
      if curr equals length of pre-existing longest:
        add curr substring to set

  return set
```

The exact **code** is below:

{% highlight python %}
def longestCommonSubstr(s1,s2):

  len1 = len(s1)
  len2 = len(s2)

  # 2d matrix with num columns = len(s2) + 1, num rows = len(s1) + 1
  lcsMat = [[0 for _ in range(len2 + 1)] for _ in range(len1 + 1) ]
  lcsSet = set()
  longest = 0

  for i in range(len1):
    for j in range(len2):
      if (s1[i] == s2[j]):
        currLen = lcsMat[i][j] + 1
        lcsMat[i+1][j+1] = currLen
        currSubstr = s1[i-currLen+1:i+1]
        if (currLen > longest):
            longest = currLen
            lcsSet = set()      
            lcsSet.add(currSubstr)
        elif (currLen == longest):
            lcsSet.add(currSubstr)

  return lcsSet
{% endhighlight %}


Note that this implementation, which leverages python list comprehension and such,
is also different from the first pseudocode in another crucial way:

For each index `mat[i][j]`, the value at said character combination represents the length of the
longest common suffix for the character combination at `mat[i+1][j+1]`. This is why
we need an *m+1 * n+1* matrix: the `m+1,n+1`th entry refers to the substring ending at the `m,n`the entry (take care of indices here, in the matrix it'll be shifted by one).


**Sources:**  
[This](http://www.bogotobogo.com/python/python_longest_common_substring_lcs_algorithm_generalized_suffix_tree.php) blog post and [wikipedia's](https://en.wikipedia.org/wiki/Longest_common_substring_problem) explanation proved super helpeful.  
In fact, my code is pretty much exactly what is explained in the first link.
