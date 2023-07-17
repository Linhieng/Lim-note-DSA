## [230717 - First non-repeating character in a stream](https://practice.geeksforgeeks.org/problems/first-non-repeating-character-in-a-stream1216/1)

- 【题意】 第一个不重复字符
- 【要求】
    - Time Complexity O(26*N)
    - Auxiliary Space O(26)
- 【Constraints】
    - 1 ≤ N ≤ $10^5$
- 【Example】:
    ```
    Input: A = "aabc"
    Output: "a#bb"
    Explanation: For every character first non
    repeating character is as follow-
    "a" - first non-repeating character is 'a'
    "aa" - no non-repeating character so '#'
    "aab" - first non-repeating character is 'b'
    "aabc" - first non-repeating character is 'b'
    ```

总感觉很简单, 但一弄就是弄了 90 分钟!
最开始还误解了题意, 以为一个字母最多出现两次, 后面提交过一次才发现并不是这样。
差不多90分钟, 中间的弯路就不说了, 这些不是说了就能怎么样的, 不浪费这些时间。
还是提取一下最终正确的思路吧！

首先, 提取一下题意的信息就是: 遍历字符串, 每个位置的字符, 是前面字符串中 **第一个** **只出现一次** 的字符, 否则为 # 符号。
这样一来, 这道题就很简单了。
那么重点就在于如何存储前面 **第一个** **只出现一次** 的字符。
我的做法是, 先利用一张表 occ_table 存储每个字母出现的次数, 然后利用一个数组 sequence 存储字母出现的次序, 再利用一个变量存储当前的有效次序。
现在, sequence 指针只会往前走, 什么时候走呢? 只有当 sequence 当前指针指向的字母出现次数大于 2 次时, 才往前走。
当走到头时, 就说明没有只出现一次的字符了, 于是赋值为 #。

但应该还可以优化, 因为题意要求的空间只有 26, 很明显是再说只能利用一个字母数组。
然后它的时间复杂度是 26*N, 说明最坏情况是每判定一个位置, 都需要遍历 26 字母表。
虽然我现在做出来了, 但是我的空间复杂度达不到要求。

只出现一次, 这个简单, 重点在于, 第一个! 这个信息要存储在哪里?
数组的下标已经被用于定位字符了, 如果利用数组的位置存储这个信息, 那么查看某个元素出现次数就变得复杂了。

只利用一个 26 大小的数组, 一轮遍历, 就能找到第一个只出现一次的字符?
字母所在位置不能改, 元素上的值只能是单个数字, 这个数字应该就是出现次数呀。
就算我还能利用字符串的前一个字符, 判断当前的第一个只出现一次的字符, 但是这只能要出一个字母的次序,
无法存储所有字母的次序呀!

想不出, 评论区也找不到。

【用到的 py 基础知识】:
- `ord()` 字符转 ASCII
- `chr()` 数字转字符
- `[*A]` 字符串转字符串数组

容易出错的地方:
- 缩进不对
- `=` 写成 `==`
- 函数`()` 写成 `[]`, 最典型的错误就是 `arr.append[i]`
- 字符串无法修改指定位置的值

### Python3 代码

【我的】
```py
class Solution:
	def FirstNonRepeating(self, A):
		fnrc = ''

		occ_table = { chr(x+ord('a')): 0 for x in range(26) }  # occ_table = [0] * 26
		sequence = []
		si = 0

		for c in A:

		    occ_table[c] += 1  # occ_table[ ord(c)-ord('a') ] += 1
            if c not in sequence:
                sequence.append(c)

            while si < len(sequence) and occ_table[ sequence[si] ] > 1: # occ_table[ ord(sequence[si])-ord('a') ]
                si += 1

	        if si < len(sequence):
	            fnrc += sequence[si]
	        else:
	            fnrc += '#'

	    return fnrc
```