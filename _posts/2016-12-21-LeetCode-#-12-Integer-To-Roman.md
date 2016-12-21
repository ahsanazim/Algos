---
layout: post
title: "LeetCode # 12 Integer To Roman"
date:  2016-12-21 13:21:52
categories: Math String
---
**Question:**
Given an integer, convert it to a roman numeral.

Input is guaranteed to be within the range from 1 to 3999.


**Solution:**  
{% highlight python %}
def intToRoman(num):
  """
  :type num: int
  :rtype: str
  """
  roman = ""
  pos = 0     # tens?, hundreds?, thousandths?, etc

  # corresponding values for positions
  posZero = ["","I","II","III","IV","V","VI","VII","VIII","IX"]
  posOne = ["","X","XX","XXX","XL","L","LX","LXX","LXXX","XC"]
  posTwo = ["","C","CC","CCC","CD","D","DC","DCC","DCCC","CM"]  
  posThree = ["","M","MM","MMM"]

  while (pos <= 3):
    currVal = num % 10
    if (pos == 0):
      roman = posZero[currVal] + roman
    elif (pos == 1):
      roman = posOne[currVal] + roman
    elif (pos == 2):
      roman = posTwo[currVal] + roman
    else:
      roman = posThree[currVal] + roman
    pos += 1
    num /= 10

  return roman
{% endhighlight %}

A few points:

- [this](https://www.math.nmsu.edu/~pmorandi/math111f01/RomanNumerals.html) link is helpful in
understanding the roman numeral system for those unfamiliar
- [this](https://discuss.leetcode.com/topic/32333/share-my-python-solution-96ms/2) solution I saw
was exceptionally succinct.
