# 230829 Delete nodes having greater value on right

【题意】：从单链表中删除节点的值小于其右侧最大值的节点

【Excepted】

- Time Complexity: O(N)
- Auxiliary Space: O(1)

## Python3

```py
class Solution:
    def compute(self,head):
        p = head
        arr = []
        while p is not None:
            arr.append(p.data)
            p = p.next

        m = float('-inf')
        ans = []
        for i in arr[::-1]:
            if i >= m:
                ans.append(i)
            m = max(i, m)
        p = head
        pre = None
        for i in ans[::-1]:
            p.data = i
            pre = p
            p = p.next

        pre.next = None
        return head
```
