---
layout: post
title:  CtCi 1.2 - Check Permutation
date:   2016-11-27 18:14:00
categories: Array String
---
**Question:** Given two strings, write a method to determine if one is a permutation
of the other.

**Solution:**

Firstly confirm from interviewer things like whether whitespace matters, whether permutation
is case sensitive, etc.

A **first solution** is presented below. It uses a hashtable to keep track of the characters
seen and their respective counts. Each count should be `0` at the end. This will become cleared when dicussing
the other possible solutions.

{% highlight python %}
def isPerm(s1,s2):
  if (len(s1) != len(s2)):      # not perm
    return False
  hashmap = {}      # char-count mapping

  # initialize counts from first string
  for c in s1:
    if (c in hashmap):
      hashmap[c] += 1    # seeing it another time; increment
    else:
      hashmap[c] = 1      # seeing it first time

  # decrement count for each char seen     
  for c in s2:
    if (c in hashmap):  
      hashmap[c] -= 1
    else:               # new char, hence s2 not perm
      return False

  # check if all counts ultimately 0
  for key in hashmap:
    if (hashmap[key] != 0):
      return False

  return True
{% endhighlight %}

A **slight variation of the above** can use a similar principle to `CtCi 1.1`, in terms
using an array of length equaling that of the set of all possible characters in our
character set of choice (e.g. 128 or 256 in ASCII). This time, though, the value at each
index will be a count of the number of times the character with code value equal to the index
has appeared in the string.

You increment the counts as you iterate through one string, and decrement as you go through the other.

Then you take one final iteration through the array (this check can be done simultaneously as well, fyi), making sure that each character has count of `0`.

{% highlight python %}
def isPerm(s1,s2):
  if (len(s1) != len(s2)):      # not perm
    return False

  chars = [0 for _ in range(256)]      # char-count mapping

  # initialize counts from first string
  for c in s1:
    intC = ord(c)
    chars[intC] += 1

  # decrement count for each char seen     
  for c in s2:
    intC = ord(c)
    chars[intC] -= 1
    if (chars[intC] < 0):    # checking as you go along
      return False

  return True
{% endhighlight %}

*Note* that when we're checking as we go along, we only check whether the value is
less than `0`. We shouldn't check if it is equal to `0` (it may be set to `0` later).

This works because of a few facts:

- Two strings being permutations of each other necessitates they be of equal length.
- Thus if they are of the same length, and aren't permutations, then it means certain
characters are present in greater or fewer quantity in one string relative to the other.
- If one character is present less often in one string than the other, then another
character *must* be present in greater quantity in that string relative to the other.
- It will be the character present in greater quantity that triggers us to `return False`.

This takes **O(n) time** and **O(1) space**.

**Another solution** works by first sorting the string, and then checking if the sorted strings
are identical, as should be the case for permutations.

{% highlight python %}
def isPerm(s1,s2):
  if (len(s1) != len(s2)):      # not perm
    return False

  # convert strings to sorted arr of chars
  sortedArrS1 = sorted(s1)    
  sortedArrS2 = sorted(s2)

  # join the chars by placing empty strings in between
  sortedS1 = ''.join(sortedArrS1)
  sortedS2 = ''.join(sortedArrS2)

  # compare
  return (sortedS1 == sortedS2)
{% endhighlight %}

This takes **O(nlogn) time** and **O(n) space**.
