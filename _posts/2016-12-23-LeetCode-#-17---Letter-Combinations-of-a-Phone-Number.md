---
layout: post
title: "LeetCode # 17 - Letter Combinations of a Phone Number"
date:  2016-12-23 13:21:32
categories: Backtracking String
---
**Question:**
Given a digit string, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given [here](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png).

```
Input:Digit string "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```
*Note*: Although the above answer is in lexicographical order, your answer could be in any order you want.

**Solution:**

***Recursive solution***

{% highlight python %}
def letterCombinations(self, digits):
  mapping = {'2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl',
             '6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz'}
  ret = []
  if len(digits) == 0:
    return []
  if len(digits) == 1:
    return list(mapping[digits[0]])

  prev = self.letterCombinations(digits[:-1])     # e.g. "23" out of "234"
  additional = mapping[digits[-1]]                # e.g. "4" out of "234"

  # prev = arr of combinations of digits[:-1]
  # add = mapping corresponding to digits[-1] (e.g. 'ghi' for '4')
  for str in prev:
    for char in additional:
      comb = str + char
      ret.append(comb)

  return ret
{% endhighlight %}


Some helpful debug output:

```
==> func(digits) w/ digits = " 234 "
==> func(digits) w/ digits = " 23 "
==> func(digits) w/ digits = " 2 "
	base case len==1 w/ digits = 2
BACK IN ==> func(digits) w/ digits = " 23 "
	prev: ['a', 'b', 'c'] additional: def
	a ~~~
		a + d = ad
		a + e = ae
		a + f = af
	b ~~~
		b + d = bd
		b + e = be
		b + f = bf
	c ~~~
		c + d = cd
		c + e = ce
		c + f = cf
BACK IN ==> func(digits) w/ digits = " 234 "
	prev: ['ad', 'ae', 'af', 'bd', 'be', 'bf', 'cd', 'ce', 'cf'] additional: ghi
	ad ~~~
		ad + g = adg
		ad + h = adh
		ad + i = adi
	ae ~~~
		ae + g = aeg
		ae + h = aeh
		ae + i = aei
	af ~~~
		af + g = afg
		af + h = afh
		af + i = afi
	bd ~~~
		bd + g = bdg
		bd + h = bdh
		bd + i = bdi
	be ~~~
		be + g = beg
		be + h = beh
		be + i = bei
	bf ~~~
		bf + g = bfg
		bf + h = bfh
		bf + i = bfi
	cd ~~~
		cd + g = cdg
		cd + h = cdh
		cd + i = cdi
	ce ~~~
		ce + g = ceg
		ce + h = ceh
		ce + i = cei
	cf ~~~
		cf + g = cfg
		cf + h = cfh
		cf + i = cfi
```

***Recursive Backtracking***

This solution employs recursive backtracking. It's an amalgam of a few solutions I've encountered and translated from Java. [This](https://discuss.leetcode.com/topic/25559/simple-python-backtracking) is a more succinct, native python solution. I prefer the solution below, though. Needless to say, this is also the preferred solution in an interview setting. 

{% highlight python %}
def backtracking(self, ret, tempSolution, digits, mapping, pos):
  # We get the set of corresponding letters for the current digit
  letters = mapping[int(digits[pos])]

  # Now we loop through the set of letters
  for char in letters:
    # Append each one of the letters to our temporary solution
    tempSolution += char
    # have we finished?, if so, add this temp combination to our result
    if (len(tempSolution) >= len(digits)):
      ret.append(tempSolution)
    # otherwise call recurse but now with the next digit as current digit   
    else:
      self.backtracking(ret, tempSolution, digits, mapping, pos + 1)
    # remove the last digit, prepare to change to another letter  
    tempSolution = tempSolution[0:-1]

def letterCombinations(self, digits):
  if (len(digits) == 0):
    return []
  # digits used as an index to get the corresponding set of letters
  mapping = ["","","abc","def",'ghi',"jkl","mno","pqrs","tuv","wxyz"]
  ret = []

  # tempSolution is used to build the solution on the fly
  tempSolution = ""

  # here we call dfs function starting with digit at position zero
  self.backtracking(ret, tempSolution, digits, mapping, 0)

  return ret
{% endhighlight %}

and without the intense commenting:

{% highlight python %}
def backtracking(self, ret, tempSolution, digits, mapping, pos):
  letters = mapping[int(digits[pos])]

  for char in letters:
    tempSolution += char
    if (len(tempSolution) >= len(digits)):
      ret.append(tempSolution)  
    else:
      self.backtracking(ret, tempSolution, digits, mapping, pos + 1)
    tempSolution = tempSolution[0:-1]

def letterCombinations(self, digits):
  if (len(digits) == 0):
    return []
  mapping = ["","","abc","def",'ghi',"jkl","mno","pqrs","tuv","wxyz"]
  ret = []

  tempSolution = ""

  self.backtracking(ret, tempSolution, digits, mapping, 0)
  return ret
{% endhighlight %}

Also, since this is a nice place to bring the topic up, it helps to read [this](http://robertheaton.com/2014/02/09/pythons-pass-by-object-reference-as-explained-by-philip-k-dick/) link when getting one's head around python's *pass by object reference* third-way approach to the
pass-by-reference vs pass-by-value argument.
