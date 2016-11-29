---
layout: post
title:  "LeetCode # 6	-  ZigZag Conversion"
date:   2016-11-25 10:52:00
categories: LeetCode String
---
**Question:**
The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this:

```
P   A   H   N
A P L S I I G
Y   I   R
```
And then read line by line: `"PAHNAPLSIIGYIR"`
Write the code that will take a string and make this conversion given a number of rows:
```
string convert(string text, int nRows);
```
`convert("PAYPALISHIRING", 3)` should return `"PAHNAPLSIIGYIR".`


**Solution:**

{% highlight python %}
def convert(self, s, numRows):
  if (numRows == 1):
      return s

  rows = [[] for _ in range(numRows)]
  step = (2 * (numRows - 1))
  finalStr = ""

  i = 0
  pos = 0
  while (i < len(s)):
      for j in range(numRows):
          pos = i + j
          if (pos >= len(s)):
              break
          rows[j].append(s[pos])
      if (pos >= (len(s) - 1)):
          break  
      for j in range(numRows-2):
          if ((pos + j) >= (len(s) - 1)):
              break
          rows[numRows-2-j].append(s[pos+j+1])
      i += step

  for idx, _ in enumerate(rows):
      for j in rows[idx]:
          finalStr += j

    return finalStr
{% endhighlight %}

This isn't a particularly fast, exciting, or clean solution, but basically works like
so:

- You create an array of arrays, each corresponding to a certain "row"
- The arrays will be filled with the characters that belong in said row
- You will then fill these arrays (i.e. the while-for constructs above)
  - Do so by first filling the downward path (first for loop - containing
    `numRows` elements)
  - And then filling the upward (which contains `numRows-2` elements)
- And then read from the arrays, in order - hence you're firstly reading from the
  characters in row I, then those in row II, and so on - thus giving you the
  desired pattern.
