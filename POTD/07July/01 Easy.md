## [230701 - Number of 1 Bits](https://practice.geeksforgeeks.org/problems/set-bits0143/1)

- 【题意】 打印二进制形式的 N 有多少位上是 1
- 【要求】
    - Time Complexity O(logN)
    - Auxiliary Space O(1)
- 【Constraints】
    - 1 <= N <= $10^9$;

不断取出最右侧位上的值，直到 0。 看完题到做出来不到三分钟。

### Python3 代码

【我的】
```py
class Solution:
	def setBits(self, N):
		bits = 0
		while N != 0:
		    if 1 == (N & 1):
		        bits += 1
	        N >>= 1
        return bits
```