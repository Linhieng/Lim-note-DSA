## 🍕 数据结构

### 数组/列表
```py
# 创建
arr = [0] * alen

# 交换元素
arr[i], arr[j] = arr[j], arr[i]

# 获取数组大小
len(arr)

```

### 栈
```py
# 创建
stack = []

# 入栈
stack.append(val)

# 出栈
stack.pop()

# 栈大小
len(stack)

# 查看栈顶元素
stack[-1]
```

### 队列
```py
# 导包
from queue import Queue

# 创建
q = Queue()

# 入队列
q.put(val)

# 出队列
q.get()

# 查看队列大小
q.qsize()

# 判断队列是否为空
q.empty()
```

```py
q = [1]

# 入队列
q.append(2)

# 出队列
q.pop(0)

# 查看队列大小
len(q)

# 判断队列是否为空
0 == len(q)  # 或者直接将 q 作为布尔值进行判断，如果 q 为空，布尔值就是 false

```

### 哈希表 map
```py
# 创建
m = { }

# 添加 / 修改
m[key] = val

# 删除
m.pop(key)

# 获取
m.get(key) # 不能用于判断

# 判断 / 查询
key in m
```

### 哈希表 set
```py
# 创建。如果创建时初始化值，则通过 s = {v1, v2, v3, ..} 创建
s = set()

# 添加
s.add(hashableKey) # list 无法被添加

# 判断  / 查询
hashableKey in s

# 删除
s.remove(hashableKey)

# 弹出
s.pop()
# 查看长度
len(s)
```

### 优先级队列
```py
# 导入
from queue import PriorityQueue

# 创建
pq = PriorityQueue()

# 添加
pq.put(val)

# 判断是否为空
pq.empty()

# 取出最小值元素值
pq.get()

# 传入对象时, 需要实现比较方法
def __lt__(self, other):
    return self.val[1] < other.val[1] # 从小到大
```

### 小根堆

``` py
# 导入
from heapq import heapify, heappush, heappop, heappushpop, heapreplace

# 原地修改可迭代对象为小根堆
heapify(iterableObj)

# 插入新元素, 确保该元素处于正确的位置
heappush(iterableObj, val)

# 弹出最小元素, 同时维持堆
heappop(iterableObj)

# 插入然后返回最小元素
heappushpop(iterableObj, val)

# 弹出最小元素然后插入
heapreplace(iterableObj, val)
```

## 🍕 符号或函数

- `0 <= x < LEN` py 支持这种写法！

- `ord()` 函数
    - 字符转 ASCII 码

- `chr()` 函数
    - 数字转字符

- `//` 运算符
    - 整数除法, 向下取整, 等同于 `int(a / b)`

- `**` 阶乘
    - `a ** 0.5` 求根号很方便

- `[::-1]` 反转列表/数组/字符串
    - 比如 `a[::-1]` 将会返回一个反转后的a
    - 比如 `a[x:] = a[x:][::-1]`
    - 比如 `a[x:y] = a[x:y][::-1]`

- `sorted(..., key=lambda x: 0-x[0])` 降序
    - 直接通过 lambda 表达式指定排序的升序降序, 适用于对元组的排序

- ` ','.join(...) `
    - 拼接数组, 使用方法比较奇葩, 是在分隔符后面调用 `join()`

- `list(range(..))`
    - 创建连续数组的列表

- `[0] * num`
    - 创建数组, 注意如果元素是对象, 则不能使用乘号, 得使用推导式

- `enumerate(sequence, start=0)`
    - 遍历过程中, 需要获取下标时非常有用。

## 🍕 读取输入

- `input()` 获取一行输入
- `strip()` 清除两边空格
- `split()` 根据空格划分
- `list()` 转换为列表（数组）
- `list(map(int, input().strip().split()))` 输入一行输入的数字
