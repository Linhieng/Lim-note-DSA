## [230809 Largest prime factor](https://practice.geeksforgeeks.org/problems/largest-prime-factor2601/1)

【题意】： 求最大的质因数

【 Excepted 】：
-  Time Complexity: O(sqrt(N))
-  Space Complexity: O(1)

连续三天做不出题真的让我怀疑人生了。

### Python3

【✔️[数论-分解质因数](https://zhuanlan.zhihu.com/p/415361242)】：

```py
class Solution:
    def largestPrimeFactor (self, N):
        i = 2
        lpf = 2
        while i <= N//i:
            if N % i == 0:
                lpf = max(lpf, i)
                k = 0
                while N % i == 0:
                    N //= i
                    k += 1
            i += 1
        if N > 1:
            lpf = max(lpf, N)
        return lpf
```

【❌埃拉托斯特尼筛法 - 195/1120】：
```py
class Solution:
    def eratosthenes(self, N):
        is_prime = [True] * (N+1)
        sqrt = int(N ** 0.5) + 1
        for i in range(2, sqrt):
            if is_prime[i]:
                for j in range(i * i, N+1, i):
                    is_prime[j] = False
        return [x for x in range(2, N+1) if is_prime[x]]

    def largestPrimeFactor (self, N):
        next_prime = self.eratosthenes(N // 2 + 1)

        while len(next_prime) != 0:
            prime = next_prime.pop()
            if N % prime == 0:
                return prime

        return N
```

【❌常规做法 - 161/1120 】：
```py
class Solution:
    def is_prime(self, num):
        sqrt = int(num ** 0.5) + 1
        for i in range(2, sqrt):
            if num % i == 0:
                return False
        return True

    def next_prime(self, cur):
        while not self.is_prime(cur+1):
            cur += 1
        return cur + 1

    def largestPrimeFactor (self, N):
        if self.is_prime(N):
            return N

        lpf = 2
        while True:
            while N % lpf == 0:
                N //= lpf
            if N == 1:
                return lpf

            lpf = self.next_prime(lpf)
```