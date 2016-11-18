# Linked List Cycle

## [Linked List Cycle I](https://leetcode.com/problems/linked-list-cycle/)

We use two pointers, one moves step by step, the other moves two steps at a time. If either pointer becomes null, then there is no cycle in the list, otherwise if there is a cycle, the two pointers will meet at some point.

> Why do the two pointers must meet together when there is a cycle?
> <br/><br/>
> The `fast` pointer first step into the cycle. When the `slow` pointer step into it, imagine that `slow` is in front of `fast` inside the cycle, and `fast` moves one step faster than `slow`/one step closer to `slow` each time. So they must meet at some time or there is no possibility that `fast` could jump over `slow`.

```java
	public boolean hasCycle(ListNode head) {
        if (head == null)   return false;
        ListNode fast = head, slow = head;
        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (fast == slow)   return true;
        }
        return false;
    }
```

This problem has many [follow-up questions](http://www.cnblogs.com/hiddenfox/p/3408931.html). Here's some versions of them.

**1. what is the length of the cycle?**

<img align="center" src="https://github.com/TongZhangUSC/LeetCode-Summary/blob/master/pic_explanation/linkedlist%20cycle.jpg"/>

In this picture, let's say X is the 1st node of list, Y is the 1st node in the cycle, Z is the node where `slow` and `fast` meet at the first time.

***Solution 1 (most simple)***

Since `fast` moves twice as fast as `slow`, then we have `2(a+b)=a+b+c+b`, namely, `a=c`.

Thus, `the length of cycle = a+b`. That is to say, the iteration time from the 1st node in the list X to the 1st node Y where they met is equal to the length of the cycle.

***Solution 2***

Let `slow` and `fast` continue to move after their first meet. Record the iteration times from their first meet to their second meet, and this is going to be exactly the length of the cycle.

***Solution 2***

Let `slow` continue to move after their first meet and let `fast` stay there. Record the iteration times from their first meet to their second meet, and this is going to be exactly the length of the cycle.


**2. how to find the 1st node of the cycle（See `@Linked List Cycle II` )?**

## [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

```java
	public ListNode detectCycle(ListNode head) {
        if (head == null)   return null;
        boolean hasCycle = false;
        ListNode fast = head, slow = head;
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (slow == fast) {
                hasCycle = true;
                break;
            }
        }
        
        if (hasCycle == false)  return null;
        slow = head;
        while (slow != fast) {
            slow = slow.next;
            fast = fast.next;
        }
        return fast;
    }
```


