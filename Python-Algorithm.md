## 🍕 其他

在做题过程中，涉及奇偶数，下标，中点时。牢记下面四种情况：
- `n//2`
    ```
       偶数(n=2)      奇数(n=3)
        0  1          0  1  2
           ↑             ↑
    ```
- `n//2 - 1`
    ```
       偶数(n=2)      奇数(n=3)
        0  1          0  1  2
        ↑             ↑
    ```
- `(n+1)//2`
    ```
       偶数(n=2)      奇数(n=3)
        0  1          0  1  2
           ↑                ↑
    ```
- `(n-1)//2`
    ```
       偶数(n=2)      奇数(n=3)
        0  1          0  1  2
        ↑                ↑
    ```

## 🍕 相关数据结构（对象）的方法和属性说明

- 全局函数
    - `len(..)`
    - `sorted(..)`
- `list`
    - `append(object)` 从尾部添加
    - `insert(index, object)` 从指定位置添加
    - `pop(index)` 弹出指定位置元素
    - `reverse()` 反转（可用 `[::-1]` 代替）
    - `index(val)` 找不到时会报错，想要判断是否存在，应该用 `in` 关键字
- `{}`
    - `pop(key)`
    - `keys()`
    - `values()`
    - `get()` 不存在时会报错，想要判断是否存在，应该用 `in` 关键字

- `from queue import Queue`
    - 完全可以用 `list` 代替
    - `put(val)`
    - `get()`
    - `empty()`
    - 不支持 `len()`
- `from queue import PriorityQueue`
    - `put(val)`
    - `get()` 取出最小值
    - `empty()`
    - 比较对象时，需要对象有提供比较方法（`__lt__`）
- `set`
    - `add(ele)`
    - `pop(ele)`
    - `remove(ele)` 不返回
- `import heapq`
    - `heapify(iterableObj)` 原地修改可迭代对象为小根堆
    - `heappush(iterableObj, val)` 插入新元素, 确保该元素处于正确的位置
    - `heappop(iterableObj)` 弹出最小元素, 同时维持堆
    - `heappushpop(iterableObj, val)` 插入然后返回最小元素
    - `heapreplace(iterableObj, val)` 弹出最小元素然后插入

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
