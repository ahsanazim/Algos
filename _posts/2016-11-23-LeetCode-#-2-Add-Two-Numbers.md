---
layout: post
title:  "LeetCode # 2	- Add Two Numbers"
date:   2016-11-23 11:27:00
categories: LeetCode Math LinkedList
---

**Question:**
You are given two linked lists representing two nonnegative numbers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

**Example:**

```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
```

**Solution:**

{% highlight python %}
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

def calculateLength(self,start):
    curr = start
    len = 0
    while curr is not None:
        len += 1
        curr = curr.next

    return len

def addTwoNumbers(self, l1, l2):
    """
    :type l1: ListNode
    :type l2: ListNode
    :rtype: ListNode
    """

    # calculate and equalize lengths by filling in zeros
    curr_l1 = l1
    curr_l2 = l2
    len_l1 = self.calculateLength(l1)
    len_l2 = self.calculateLength(l2)

    if len_l1 > len_l2:
        diff = len_l1 - len_l2
        # set pointer to last element in l2
        curr_l2 = l2
        while curr_l2.next is not None:
            curr_l2 = curr_l2.next
        n = 0
        while n < diff:
            toInsert = ListNode(0)
            curr_l2.next = toInsert
            curr_l2 = curr_l2.next
            n += 1
    elif len_l2 > len_l1:
        diff = len_l2 - len_l1
        # set pointer to last element in l1
        curr_l1 = l1
        while curr_l1.next is not None:
            curr_l1 = curr_l1.next
        n = 0
        while n < diff:
            toInsert = ListNode(0)
            curr_l1.next = toInsert
            curr_l1 = curr_l1.next
            n += 1

    # now calculate sum
    toInsertVal = 0
    secondToLast = None
    carry = 0
    start = l1
    end = False
    while not end:
        if l1 is None:                  # i.e. carry != 0
            lastNode = ListNode(carry)
            secondToLast.next = lastNode
            end = True
        else:
            toInsertVal = l1.val + l2.val + carry
            carry = toInsertVal / 10
            toInsertVal = toInsertVal % 10
            l1.val = toInsertVal
            if l1.next is None:
                if carry == 0:
                    end = True
                else:
                    secondToLast = l1
            l1 = l1.next
            l2 = l2.next
    return start
{% endhighlight %}

Beat 68% of python times
