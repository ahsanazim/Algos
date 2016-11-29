---
layout: post
title:  "LeetCode # 9	-  Palindrome Number"
date:   2016-11-25 15:13:00
categories: LeetCode Math
---
**Question:**
Determine whether an integer is a palindrome. Do this *without extra space*.

*Some hints*:
Could negative integers be palindromes? (ie, `-1`)

If you are thinking of converting the integer to string, note the restriction of using extra space.

You could also try reversing an integer. However, if you have solved the problem "Reverse Integer", you know that the reversed integer might overflow. How would you handle such case?

There is a more generic way of solving this problem.

**Solution:**

{% highlight python %}
def reverseInt(self,n):
  """ helper func to reverse 32-bit int """
  rev = 0
  isNegative = (n < 0)
  if (isNegative):
      n = -n

  # reverse
  while (n > 0):
      rev = (rev * 10) + (n % 10)
      n = n / 10

  # overflow, negative ints --> not palindromes
  # return anything isn't a palindrome
  if ((rev > 2**31) or (isNegative)):
      return 12   

  return rev

def isPalindrome(self, x):
  return (x == self.reverseInt(x))
{% endhighlight %}
***runtime beats 93.53% of python submissions ;)***

Although the above is clearly fast, and should suffice, the "more generic
solution" hinted at above is interesting to say the least. Our current implementation
employs the fact that if you reverse a palindrome - be it a string or integer -
the reversed and original sequences are both equivalent.

**Note** that above I consider negative numbers to *not* be palindromes - this will
obviously be up to the direct discretion of the interviewer, and is open to clarification.

**Note pt. II** the restriction for "no extra space", I think, refers to
only being allowed *constant* extra space (otherwise the question would be ridiculously
impossible). Again, bad wording, and something to be clarified.

For the purposes of a generic solution, I created the code below. Note that it's
*extremely* slow, but again it has a dual purpose - I wanted more so to have some
fun implementing some of the helper functions shown. They've been helpful in other questions
of this sort.

{% highlight python %}
def getNthDigit(self,num, n):
    """ gets the nth digit in num int """
    limit = 10 ** n
    while (num >= limit):
        num = num / 10
    return num % 10

def getLength(self,num):
    """ returns number of digits in passed in int """
    if ((num >= 0) and (num <  10)):
        return 1
    len = 0
    while (num > 0):
        len += 1
        num = num / 10
    return len

def palindOverflows(self,num):
    """ checks if the reversed form of num is > 2^32 """
    maxInt = 2 ** 31        # limit
    lenMax = self.getLength(maxInt)
    lenNum = self.getLength(num)
    if (lenNum < lenMax):
        return False
    for i in range(lenMax):
        maxDigit = self.getNthDigit(maxInt,i+1)
        numDigit = self.getNthDigit(num,lenMax - i)
        if (numDigit < maxDigit):
            return False  
    return True  


def isPalindrome(self, x):
    # preliminary checking
    if (x < 0):         # negatives
        return False
    if (self.palindOverflows(x)):   # overflow pt.2
        return False
    lenX = self.getLength(x)
    lastIdx = lenX - 1

    if (lenX == 1):
        return True

    # move from start, check if digits (i) and (end-i) equal
    # if not then we have a non-palindromic num
    for i in range(lenX / 2):
        curr = self.getNthDigit(x,i+1)
        compl = self.getNthDigit(x,lenX - i)
        if (curr != compl):
            return False

    return True
{% endhighlight %}

For a super-efficient generic solution, look [here](https://discuss.leetcode.com/topic/40845/9-ms-java-beats-99-5-java-solutions-easy-to-understand).
