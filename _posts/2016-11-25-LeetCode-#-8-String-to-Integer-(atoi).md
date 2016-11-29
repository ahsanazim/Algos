---
layout: post
title:  "LeetCode # 8	- String to Integer (atoi)"
date:   2016-11-25 23:15:00
categories: LeetCode Math String
---
**Question:**
Implement `atoi` to convert a string to an integer.

*Hint*:  
Carefully consider all possible input cases. If you want a challenge, please do not see below and ask yourself what are the possible input cases.

*Notes*:  
It is intended for this problem to be specified vaguely (ie, no given input specs). You are responsible to gather all the input requirements up front.

*REQUIREMENTS*:  
The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned. If the correct value is out of the range of representable values, `INT_MAX (2147483647)` or `INT_MIN (-2147483648)` is returned.

**Solution:**

{% highlight python %}
def isSign(self,c):
    """ checks if char is +/- """
    return ((c == "+") or (c == "-"))

def myAtoi(self, str):

    firstIdxSign = -1
    firstNonWhiteSpace = -1
    firstIdx = -1
    lastIdx = -1
    intToReturn = None
    maxInt = 2147483647
    minInt = -2147483648

    # find bounds of possible int
    for idx, i in enumerate(str):
        if ((firstIdx == -1) and (firstIdxSign == -1) and (self.isSign(i))):
            firstIdxSign = idx
            lastIdx = idx
        elif ((firstIdx == -1) and (i != " ") and (not i.isdigit())):
            firstNonWhiteSpace = idx
            lastIdx = idx
        elif ((firstIdx == -1) and (i.isdigit())):
            firstIdx = idx
            lastIdx = idx
        elif ((firstIdx  != -1) and (str[firstIdx:idx+1].isdigit())):
            lastIdx = idx
        elif ((firstIdx  != -1) and (not str[firstIdx:idx+1].isdigit())):
            break

    # error checking
    if (firstIdx == -1):
        return 0
    afterSignNums = None
    if (firstIdxSign != -1):
        if (firstIdx == lastIdx):
            afterSignNums = str[firstIdxSign+1:lastIdx+1]
        else:
            afterSignNums = str[firstIdxSign+1:lastIdx+1]
    else:
        if (firstIdx == lastIdx):
            afterSignNums = str[firstIdx:firstIdx+1]
        else:
            afterSignNums = str[firstIdx:lastIdx+1]
    if not (afterSignNums.isdigit()):
        return 0

    if (firstIdxSign != -1):
        intToReturn = int(str[firstIdxSign:lastIdx+1])
    else:
        intToReturn = int(str[firstIdx:lastIdx+1])

    if (firstNonWhiteSpace != -1):  
        if not (str[firstNonWhiteSpace:lastIdx+1].isdigit()):
            return 0

    if (intToReturn > maxInt):
        return maxInt
    elif (intToReturn < minInt):
        return minInt
    else:
        return intToReturn
{% endhighlight %}

The above solution is really quite terrible, and speaks to how annoying this problem
was to get right. As is obvious, I didn't quite approach it in a truly planned way,
and made a few ad-hoc changes along the way. I'll definitely be revisiting this
in the future.

The main thing I took away from the problem is, as the question mentions, the importance
of clarifying requirements up front and thinking of all possible situations. Some of
the test cases presented in this really hammer this point home.

Some better solutions I've seen can be found [here](https://discuss.leetcode.com/topic/66928/python-simple-solution-beates-90-with-tips/3)
and [here](https://discuss.leetcode.com/topic/55866/3ms-java-really-easy-to-understand/2).

Just to re-emphasize point, I'll also quote [this](https://discuss.leetcode.com/topic/68397/so-many-conditions-to-check-a-simple-if-else-solution-using-flags) answer to end:

> Sometimes interviewers want to check if you can think of all the possibilities over the input. What about "+123 +", " - - 124", " -124 " and so on. Actually your program should not fail for such test cases. Nothing unexpected should happen with you code.
This question is fairly easy. Just you need to check for all the possibilities.
