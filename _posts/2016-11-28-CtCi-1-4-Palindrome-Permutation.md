---
layout: post
title:  CtCi 1.4 - Palindrome Permutation
date:   2016-11-28 00:40:00
categories: Array String
---
**Question:** Given a string, determine if it is a permutation of a palindrome.

**Solution:**

An basic analysis of the problem would make a few things clear:  

- for even length strings that are permutations of palindromes, all characters
must be present an even number of times  
- for odd length strings that are permutations of palindromes, one character is present
an odd number of times, while the rest are present an even number of times.

Thus the following solution will seem intuitive:

{% highlight python %}
def isPalinPerm(s):
    even = ((len(s) % 2) == 0)    # True if even length
    hashmap = {}

    # insert character-count mappins in to hashmap
    for ch in s:
        if (ch in hashmap):
            hashmap[ch] += 1
        else:
            hashmap[ch] = 1

    # check if counts are correct
    if (even):           
        for keys in hashmap:
            if ((hashmap[key] % 2) != 0):
                return False
    else:
        oddSeen = False
        for key in hashmap:
            if ((hashmap[key] % 2) != 0):
                if (oddSeen):
                    return False
                else:        # multiple odds not allowed
                    oddSeen = True
    return True
{% endhighlight %}

You'll note that the separation between *even* and *odd* cases isn't really
necessary - the condition that *for a string to be a permutation of a palindrome,
it can have no more than one character with odd count* actually suffices. Clearly, the following code will
do:

{% highlight python %}
def isPalinPerm(s):
    even = ((len(s) % 2) == 0)    # True if even length
    hashmap = {}
    oddSeen = False

    # insert character-count mapping in to hashmap
    for ch in s:
        if (ch in hashmap):
            hashmap[ch] += 1
        else:
            hashmap[ch] = 1

    for key in hashmap:
        if ((hashmap[key] % 2) != 0):
            if (oddSeen):     # multiple odds not allowed
                return False
            else:        
                oddSeen = True
    return True
{% endhighlight %}

This is because for even length strings, when we see one odd count character, then
it necessitates the presence of another odd count character (otherwise the string
wouldn't be of even length, would it?). Hence ensuring that the `return False` will (eventually) be
triggered if we see an odd count character in an even length string.
Thus the single test iteration takes care of both even and odd length strings.

This solution runs in **O(n) time**, with **O(1) space**.

*Note* that the space complexity might be a point of confusion here - although we
might intuitively say O(n), the space does *not* actually increase along with input
string size. It caps at whatever the size of our character set (ASCII, etc) is. Hence the space
complexity could also be considered `O(k)`.

There are other possible solutions for such a problem (using a bit vector would reduce space
requirements - although such solutions should be employed with care)
