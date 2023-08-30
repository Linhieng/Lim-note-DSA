# [230830 Delete a Node in Single Linked List](https://practice.geeksforgeeks.org/problems/delete-a-node-in-single-linked-list/1)

【题意】：删除单链表的第 n 个节点

【Excepted】

- 时间复杂度：O(n)
- 额外空间复杂度：O(1)

## Python3

```py
def delNode(head, k):
    if k == 1:
        return head.next
    i = 0
    p = head
    while p is not None:
        i += 1
        if i == k-1:
            if p.next is not None:
                p.next = p.next.next
            break

        p = p.next

    return head
```
