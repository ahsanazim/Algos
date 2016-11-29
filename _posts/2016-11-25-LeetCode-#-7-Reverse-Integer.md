---
layout: post
title:  "LeetCode # 7	-  Reverse Integer"
date:   2016-11-25 13:36:00
categories: LeetCode Math
---
**Question:**
Reverse digits of an integer.

Example1: `x = 123`, return `321`
Example2: `x = -123`, return `-321`

*Hints*:

Here are some good questions to ask before coding. Bonus points for you if you have already thought through this!

If the integer's last digit is `0`, what should the output be? ie, cases such as `10`, `100`.

Did you notice that the reversed integer might overflow? Assume the input is a 32-bit integer, then the reverse of `1000000003` overflows. How should you handle such cases?

For the purpose of this problem, assume that your function returns `0` when the reversed integer overflows.

**Solution:**

{% highlight python %}
def reverse(self, x):
  # do reversal w/ strings
  strX = str(x)
  strXRev = ""
  intToReturn = 0
  zerosOk = False if (len(strX) == 0) else True     
  maxInt = 2147483648             # overflow

  currDigit = len(strX) - 1
  endReverse = 0

  # take care of negative sign
  if (strX[0] == "-"):
      strXRev += "-"
      endReverse = 1

  # reverse string
  while (currDigit >= endReverse):
      digitToPut = strX[currDigit]
      currDigit -= 1
      if ((digitToPut == "0") and (not zerosOk)):
          continue
      else:
          strXRev += digitToPut
          zerosOK = True      # think 109000 = 901, not 19

  # 0 if overflow
  # but ints don't overflow in python!
  if ((int(strXRev) > maxInt) or (int(strXRev) < -maxInt)):
      strXRev = 0

  # convert back to int
  intToReturn = int(strXRev)

  return intToReturn
{% endhighlight %}

***runtime beats 94.85% of python submissions ;)***
^ Though that number fluctuated quite a bit depending on certain modifications and iterations, curiously enough.

**Slight note**: `int`s are automatically promoted to `long`s in python, so there isn't really
a maximum integer per se. We do have `sys.maxint` and `sys.maxsize`, but they
refer more so to arrays and strings that might hold integers (sort of!).

Also, for **checking integer overflows** - the current method used to check them
assumes that converting a string to an integer that overflows will not cause an
error. Obviously that is incorrect. If we were being more rigorous here, and
performing the check while our number was still a string, something like the following
would make more sense:

{% highlight python %}
def isOverflowing(strN):
  maxInt = 2147483648

  # not even 10 digits!
  if (len(strN) < 10):
    return False

  # positive overflows:
  if ((strN[0] >= 2) and (strN[1:] > 147483648)):
    return True

  # negative overflows:
  if ((strN[1] >= 2) and (strN[2:] > 147483648)):
    return True

  return False
{% endhighlight %}

**UPDATE**:

Just for the heck of it, here's a cooler solution I adapted from one I found while trawling
through the LeetCode boards. It avoids using strings and all:

{% highlight python %}
def reverse(self, x):
  num = 0
  negtv = (x < 0)     # True or False
  if (negtv):
      x *= -1

  while (x > 0):
      # print "x: ", x, "num: ", num --> debug
      num = (num * 10) + (x % 10)
      x = x / 10

  if (num > 2**31):
      return 0
  if (negtv):
      num *= -1

  return num  
{% endhighlight %}


The debug output for `x = 123456` is as follows:

```
x:  123456 num:  0
x:  12345 num:  6
x:  1234 num:  65
x:  123 num:  654
x:  12 num:  6543
x:  1 num:  65432
```
