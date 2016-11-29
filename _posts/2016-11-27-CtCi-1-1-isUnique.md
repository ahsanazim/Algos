---
layout: post
title:  CtCi 1.1 - isUnique
date:   2016-11-27 17:46:00
categories: Array String
---
**Question:** Implement an algorithm to determine if a string has all unique characters. What
if you cannot use additional data structures?

**Solution:**

The obvious and straightforward solution is presented below. Simply use a boolean array with length equal to that of the set of all possible characters (256, 128, etc). Thus there's a 1-to-1 mapping
between character codes and the actual characters themselves. You begin with each entry initialized to `False`.

As you iterate through the string, check whether the boolean at index represented by the
current character's unicode code (accessible via `ord()` function in python)
is already `True`. If it is, then we're seeing the character for a second time. If not, then mark it as `True` and continue iterating.

{% highlight python %}
def isUnique(s):
  if (len(s) > 256):        # clarify w/ interviewer
    return False
  chars = [False for _ in range(0,256)]

  for c in s:
    intC = ord(c)
    if (chars[intC]):      # seeing char for 2nd time
      return False
    else:                 # seeing it 1st time, so mark seen
      chars[intC] = True

  return True             # saw nothing for 2nd time
{% endhighlight %}

This solution runs in **O(n) time**, with **O(1) space**.

There are other solutions available to us as well:

- Instead of using an array of booleans, we can use an integer as a bit vector. This solution
is slightly hacky, but is explained at length [here](http://stackoverflow.com/questions/19484406/detecting-if-a-string-has-unique-characters-comparing-my-solution-to-cracking)
- Compare every character to every other character - this doesn't require extra space (`O(1)` space),
but runs in O(n<sup>2</sup>) time.
- An solution taking `O(nlogn)` time would involve sorting the string, and then comparing each character
to its neighbouring ones. The space complexity here varies according to which sorting algorithm we use.
