## 🍕 总结排序(时间复杂度, 空间复杂度, 稳定性)

| 排序算法(典型)   | 时间复杂度(通常情况) | (额外)空间复杂度(通常情况) | 稳定性(通常情况) |
|---------------|----------------------|----------------------------|----------------|
| 选择排序         | $O(N^2)$             | $O(1)$                     | ❌不稳定          |
| 冒泡排序         | $O(N^2)$             | $O(1)$                     | ✔️稳定           |
| 插入排序         | $O(N^2)$             | $O(1)$                     | ✔️稳定           |
| **归并排序**     | $O(N * logN)$        | $O(N)$                     | ✔️稳定           |
| **随机快速排序** | $O(N * logN)$        | $O(logN)$                  | ❌不稳定          |
| **堆排序**       | $O(N * logN)$        | $O(1)$                     | ❌不稳定          |
| 计数排序         | $O(N)$               | $O(N)$                     | ✔️稳定           |
| 基数排序         | $O(N)$               | $O(N)$                     | ✔️稳定           |

### 稳定性

稳定就是: 当存在相等元素时, 排序后它们的相对顺序不变。

- 选择排序: 每次选择最小的数与第一个数交换。
    - 排序后不稳定🌰:
        - [ $3_a$, $3_b$, 1 ]
        - [ 1, $3_b$, $3_a$ ] 第一轮遍历时, 选到最小值 1, 将它与第一个数交换
        - 很明显, 排序后的 $3_b$ 和 $3_a$ 位置互换了。

- 冒泡排序: 如果左值 > 右值, 就交换。
    - 排序后稳定🌰:
        - [ $3_a$, $3_b$, 1 ]
        - [ $3_a$, 1, $3_b$ ]
        - [ 1, $3_a$, $3_b$ ]
    - 注意⚠️, 如果冒泡排序的实现写成左值 >= 右值时交换, 则冒泡排序会变成不稳定：
        - [ $3_a$, $3_b$ ]
        - [ $3_b$, $3_a$ ] 因为此时左值 == 右值, 所以还会继续交换, 此时就会导致不稳定

- 插入排序: 当左值 >= 右值时, 停止插入(交换)。
    - 排序后稳定🌰:
        - [ $3_a$, $3_b$, 1 ]
        - [ $3_a$, 1, $3_b$ ]
        - [ 1, $3_a$, $3_b$ ]
    - 同样注意⚠️, 如果当两个值相等时, 还要继续插入, 则会变成不稳定：
        - [ $3_a$, 1, $3_b$ ]
        - [ 1, $3_a$, $3_b$ ] 插好 1
        - [ 1, $3_a$, $3_b$ ] 插好 $3_a$
        - [ 1, $3_b$, $3_a$ ] 插好 $3_b$

- 归并排序: 先二分后依次排序, 然后合并。 合并过程中若两个数相等, 先合并左边
    - 排序后稳定🌰:
        - merge [ 1, $3_a$ ]  [ $3_b$ ]
        - merge result: [ 1 ]
        - merge result: [ 1, $3_a$ ]
        - merge result: [ 1, $3_a$, $3_b$ ]
    - 同样注意⚠️, 如果先合并右边, 则会不稳定。 而前面介绍过的 "小和问题", 就是先合并右边的, 所以那时的归并是不稳定的
        - merge [ 1, $3_a$ ]  [ $3_b$ ]
        - merge result: [ 1 ]
        - merge result: [ 1, $3_b$ ]
        - merge result: [ 1, $3_b$, $3_a$ ]

- 快速排序: 不断的随机选取 pivot(划分值), 然后执行 partition。partition 核心: 将数值划分进对应区域。 比如, 将 < pivot 的值划分进 "< 区", 具体做法是将该值域 "< 区" 边界交换, 然后扩大边界。
    - 排序后不稳定🌰:
        - [ $3_a$, $3_b$, 1 ], 选取的 pivot 是 2
        - [ 1, $3_b$, $3_a$ ], 识别到 1 时, 将它与 "< 区" 的边界+1 值交换, 故 $3_a$ 跨过了 $3_b$
    - 不管是二项划分还是三项划分, 结果都是不稳定的。


- 堆排序: 利用堆的特性(可以是大根堆或小根堆)进行排序
    - 在构造堆的过程中就已经打乱了顺序, 举个小根堆打乱🌰:
        - 原数组: [ $3_a$, $3_b$, 1 ]
        - 对 1 进行 heap insert (或者对 $3_a$ 进行 heapify) 的过程中, 将会交换 1 和 $3_a$ 的位置, 结果如下
        - 小根堆: [ 1, $3_b$, $3_a$ ]

- 计数排序, 基数排序, 他们的算法思路本身就不是基于比较的, 他们是利用 "桶" 的结构来实现排序的, 所以只要确保 "入桶" 和 "出桶" 的过程中元素的相对顺序不变, 那么他们就是稳定的

### 总结

前面的几种排序, 可以分为三大类, 选择冒泡插入作为一类, 归并快排堆作为一类, 计数基数作为一类。
对于计数和基数排序, 只适用于特定情况。
选择冒泡插入排序, 可以作为简单排序算法的学习, 只适用简单数据。
归并快排堆排序, 适用范围最广。

归并, 快排, 堆 如何选取?
- 一般都是选择 **随机快排** 进行排序。 虽然快排和堆的时间复杂度相同, 但实践过程中发现快排的常数时间更短。
- 当有稳定性需求时, 选择归并排序
- 当要压榨空间时, 选择堆排序

排序算法常见的坑
- 归并排序的额外空间复杂度可以变成 O(1)。—— 非常难实现, 不需要掌握。有兴趣可以搜索"归并排序 内部缓存法"。 而且虽然空间复杂度降了, 但同时它也变得不稳定了, 那为什么不用堆排序呢?
- 原地归并排序可以实现空间复杂度为 O(1)。—— 虽然空间复杂度降低了, 但它的时间复杂度变成了 O(N^2), 那为什么不用简单排序呢
- 快速排序可以做到稳定。 —— 难。 而且虽然做到了稳定性, 但空间复杂度会变成 O(N), 那为什么不用归并排序呢? 有兴趣可以搜索 "01 stable sort"。
- 面试题: 要求奇数放在数组左边, 偶数放在数组右边, 同时相对次序不变, 并且时间复杂度为 O(N), 空间复杂度为 O(1)。
    - 回答: 经典的快排做不到稳定性, 而经典快排的 partition 又是 01 标准的, 它和这个奇偶问题其实是一种调整策略, 快排做不到, 所以我也不知道怎么做。 然后把问题反抛给面试官。
    - partition 过程, 能够将 "≤ pivot" 的放左边, "> pivot" 的放右边, 这其实就是 "01标准"。 它能做到时间复杂度 O(N), 空间复杂度 O(1), 但它做不到稳定。 所以这道面试题目前是无解的。

### 工程上对排序的改进

- 充分利用 O(N*logN) 和 O(N^2) 各自的优势。
比如在大数据量的时候使用快排, 在小数据量的时候使用插入。 所以在快排的代码上, 经常会看到在排序前, 先用 if 判断一下待排序的数据, 如果小于一定 60 个, 则使用的是插入排序。　

- 稳定性问题。
对于基本数据, 不需要考虑稳定性问题。 比如系统提供的排序黑盒, 当数据是基本类型时, 它会使用快排。 但数据是对象时, 它会使用归并。


## 🍕 简单了解哈希表

哈希表有两种, Map 和 Set, 这两种本质上是一样的。 Map 是 key-value 的形式, Set 是只有 key 的形式。

哈希表的增删改查操作都是 O(1) 级别的。 需要注意的是, 哈希表的常数时间是比较大的, 它相比数组寻址肯定是比较慢一点的。

注意一个概念： 值传递还是引用传递。 通常情况是, 哈希表的规则是:
- 当 key 类型是基础类型时, 哈希表内部是值传递的; 此时哈希表内部会开辟一块空间存储具体的值, 这块空间的大小由具体的值决定。
- 当 key 类型是复杂类型时, 哈希表内部是引用传递时; 此时哈希表内部只会存储引用, 不会重新拷贝一份数据。 引用所占用的空间的大小是固定的。

> java 中的哈希表是 HashSet, HashMap 结构
> C++ 中的哈希表是 UnorderedSet, UnorderedMap 结构


## 🍕 简单了解有序表

有序表和哈希表类似, 也有 Map 和 Set 两种。
只不过有序表实现了对 key 进行排序。 所以可以按照特定顺序获取有序表中的 key-value。
注意, 如果传递给有序表的 key 无法直接比较, 则需要传递比较器。

有序表的增删改查操作都是 O(logN) 级别的。

红黑树, AVL 树, size-balance-tree 和跳表等都属于有序表结构。 只是底层具体实现不同。

> java 中的有序表是 TreeSet, TreeMap 结构
> C++ 中的有序表是 OrderedSet, OrderedMap 结构

## 🍕 链表

链表分为单链表和双链表:
- 单链表: 单向连接
- 双链表: 双向连接

### 小练习 - 反转链表

【题目】: 分别实现反转单链表和反转双链表的函数。
【要求】: 如果链表长度为 N, 要求时间复杂度为 O(N), 额外空间复杂度为 O(1)。

【⚠️重点】： 当你断掉某个链条时, 你要清楚是否需要使用到断开之前所指向的节点! 如果需要, 那么一定要先将该节点存储起来！

【我的代码】：

```py
def reversed_linked_list(head, is_double=False):
    # 判断边界条件
    if head is None or head.next is None:
        return head

    # 初始值
    pr = None
    p = head
    pn = p.next

    while True:
        p.next = pr # 反转
        if is_double:
            p.prev = pn # 反转双链表只多了这一步

        # 结束判断
        if pn is None:
            break

        # 更新
        pr = p
        p = pn
        pn = p.next

    return p
```


### 小练习 - 打印两个有序链表的公共部分

【题目】: 给定两个有序链表的头指针 head1 和 head2, 打印两个链表的公共部分。
【要求】: 如果两个链表的长度之和为 N, 时间复杂度要求为 O(N), 额外空间复杂度要求为 O(1)。

【算法思路】: 两个指针, 谁小谁移动。 相同时打印并一起移动。 直到某一指针越界。

【我的代码】:
```py
def linked_list_common(head1, head2):
    common = []
    while head1 is not None and head2 is not None:
        if head1.data < head2.data:
            head1 = head1.next
        elif head1.data > head2.data:
            head2 = head2.next
        else:
            common.append(head1.data)
            head1 = head1.next
            head2 = head2.next
    return common
```

### 面试时链表解题方法论

笔试和面试要求不一样。
- 笔试: 能过就行, 没人看你代码。 所以不用太在乎空间复杂度, 一切为了时间复杂度。
- 面试: 时间复杂度仍然是第一位, 但是空间复杂度一定要省。 因为此时看你的不是机器, 是面试官。

【技巧】:
- 额外数据结构记录(哈希表等)
- 快慢指针。 比如一个指针走一步, 一个指针走两步。

### 练习 - 判断单链表回文

【做法1(笔试)】: 先遍历一遍链表, 并把值依次放入栈中。 再遍历一遍链表, 此时每次遍历都将栈的内容弹出, 然后比较是否相等。 直到不相等或者栈为空。

【做法2(笔试)】:
做法 1 中遍历了两遍链表, 而且栈占用的空间大小为 N。
但实际上, 当我们遍历到链表中间之后, 就可以将栈中的元素 pop 出来和链表进行比较了。
问题在于, 如何确定中间位置? 方法就是 **快慢指针**。
一个指针走一步, 一个指针走两步。 当快指针到头时, 就说明到中间附近了。

【做法3(面试)】:不借助栈实现:
1. 找到中间位置
2. 从中间, 将后面的链表节点反转。
3. 从链表两端往中间一一比较, 右链表在比较的过程中再次将链表反转

【注意⚠️】: 思路都很简单, 需要注意的只有长度奇偶数情况下, 中点的选取。 根据题意的不同, 选取的方式也不同。

#### 老师代码

```java
// 方法1
public static boolean isPalindrome1(Node head) {
    Stack<Node> stack = new Stack<Node>();
    Node cur = head;
    // 将链表所有节点入栈
    while (cur != null) {
        stack.push(cur);
        cur = cur.next;
    }
    // 再次遍历链表, 并从栈中元素一一比较
    while (head != null) {
        if (head.value != stack.pop().value) {
            return false;
        }
        head = head.next;
    }
    return true;
}

// 方法2
public static boolean isPalindrome2(Node head) {
    if (head == null || head.next == null) {
        return true;
    }
    Node right = head.next;
    Node cur = head;
    // 找到中点位置
    while (cur.next != null && nul.next.next != null) {
        right = right.next;
        cur = cur.next.next;
    }
    // 从中点位置再开始将元素入栈
    Stack<Node> stack = new Stack<Node>();
    while (right != null) {
        stack.push(right);
        right = right.next;
    }
    // 再次遍历, 一一比较
    while (!stack.isEmpty()) {
        if (head.value != stack.pop().value) {
            return false;
        }
        head = head.next;
    }
}

// 方法3
public static boolean isPalindrome3(Node head) {
    if (head == null || head.next == null) {
        return true;
    }
    Node n1 = head;
    Node n2 = head;
    // 快慢指针找到中间
    while (n2.next != null && n2.next.next != null) {
        n1 = n1.next;
        n2 = n2.next.next;
    }
    // 反转后半链表
    n2 = n1.next;
    n1.next = null;
    Node n3 = null;
    while (n2 != null) {
        n3 = n3.next;
        n2.next = n1;
        n2 = n3;
    }
    // 从链表两端向中间一一比较
    n3 = n1;
    n2 = head;
    boolean res = true;
    while (n1 != null && n2 != null) {
        if (n1.value != n2.value) {
            res = false;
            break;
        }
        n1 = n1.next;
        n2 = n2.next;
    }
    // 完成后再次把后半链表反转
    n1 = n3.next;
    n3.next = null;
    while (n1 != null) {
        n2 = n1.next;
        n1.next = n3;
        n3 = n1;
        n1 = n2;
    }
}

```

#### 我的代码


```py
def is_palindrome_linked_list2(head):
    if head is None:
        return False
    if head.next is None:
        return True
    # 利用快慢指针, 走到中点就开始出栈比较
    slow = head
    quick = head.next
    mid_r = None  # 偶:偏右; 奇:中点右边
    half_stack = []  # 只会填充左侧。 偶:填一半; 奇:不包含中点
    while True:
        half_stack.append(slow.data)
        if quick.next is None:  # 偶
            mid_r = slow.next
            break
        if quick.next.next is None:  # 奇
            mid_r = slow.next.next
            break
        slow = slow.next
        quick = quick.next.next
    while mid_r is not None:
        if (mid_r.data != half_stack.pop()):
            return False
        mid_r = mid_r.next
    return True


def is_palindrome_linked_list3(head):
    if head is None:
        return False
    if head.next is None:
        return True
    # 不借助栈, 过了中点后将链表反转
    # 然后从链表两侧往回一一比较, 往回走时再次反转链表
    slow = head
    quick = head.next
    mid_r = None
    while True:
        if quick.next is None:
            mid_r = slow.next
            break
        if quick.next.next is None:
            mid_r = slow.next.next
            break
        slow = slow.next
        quick = quick.next.next
    # 从 mid_r 节点开始进行反转
    pr, p, pn = None, mid_r, mid_r.next
    while True:
        p.next = pr
        if pn is None:
            break
        pr = p
        p = pn
        pn = p.next
    # 此时的 p 是右链表头节点, mid_r 是尾节点
    # 现在再次反转右链表, 在反转的过程中与左链表一一比较
    left = head
    pr, p, pn = None, p, p.next
    is_palindrome = True
    while True:
        if left.data != p.data:
            is_palindrome = False
        p.next = pr
        if pn is None:
            break
        pr = p
        p = pn
        pn = p.next
        left = left.next
    return is_palindrome
```

### 练习 - 对单链表的值进行划分

【题目】: 给定一个单链表的头节点 head, 节点的值类型是整型, 再给定一个整数 pivot。
实现一个调整链表的函数, 将链表调整为左部分都是值小于 pivot 的节点, 中间部分都是值等于 pivot 的节点, 右部分都是值大于 pivot 的节点。
【进阶】: 要求实现稳定性, 同时时间复杂度为 O(N), 额外空间复杂度为 O(1)。

【做法 1(笔试)】: 把单链表转换为数组, 然后做快排的 partition。 做完后再把数组转换为单链表

【做法 2(面试)】:
定义三个区域的空链表。 遍历链表, 将每次遍历到的节点放到正确的区域内。 遍历完成后将三个区域的链表连接起来就可以了。

由于链表中节点的移动代价是 O(1), 所以仅需有限的几个变量就实现链表 partition。

【代码思路】：
1. 定义 6 个变量, SH 表示 "< 区" 的头节点, ST 表示 "<" 区域的尾节点。 同理还有 "= 区" 的两个节点 EH 和 ET,   "> 区" 的两个节点 BH 和 BT。 他们的初始值都是空。
2. 遍历链表
3. 当节点 < 划分值时, 将节点放到 "< 区" 的链表上。具体做法是: 第一个小于划分值的节点, 将它同时赋值给 SH 和 ST, 然后 SH 就不用变了。 后续再遇到符合条件的节点时, 将它作为 ST 的下一节点, 同时 ST 向下移动, 以此类推。
4. 当节点 == 划分值或节点 > 划分值 时, 都是同样的做法。
5. 结束时, 将三条链表连接起来。 具体做法:
    - 在 ST 不为空时: 若 EH 不为空, 则 ST 连接 EH, 否则连接 BH。 不需要再判断 BH 是否为空。
    - 在 ET 不为空时, ET 只会连接 BH。
    - 在 BT 不为空时, BT 必须连接"空"。
    - 连接好各自的尾部后, 只需要放回三条链表中的一个头节点即可, 三个头节点的优先级是 SH > EH > BH。

#### 老师代码

```java
// 方法1
public static Node listPartition1(Node head, int pivot) {
    if (head == null) {
        return head;
    }
    Node cur = head;
    // 获取链表长度
    int i = 0;
    while (cur != null) {
        i++;
        cur = cur.next;
    }
    // 将链表转换为数组
    Node[] nodeArr = new Node[i];
    i = 0;
    cur = head;
    for (i = 0; i != nodeArr.length; i++) {
        nodeArr[i] = cur;
        cur = cur.next;
    }
    // 在数组上执行 partition
    arrPartition(nodeArr, pivot);
    // 将数组重新连接成链表
    for (i = 1; i != nodeArr.length; i++) {
        nodeArr[i - 1].next = nodeArr[i];
    }
    nodeArr[i - 1].next = null;
    return nodeArr[0];
}

public static void arrPartition(Node[] nodeArr, int pivot) {
    int small = -1;
    int big = nodeArr.length;
    int index = 0;
    while (index != big) {
        if (nodeArr[index].value < pivot) {
            swap(nodeArr, ++small; index++);
        } else if (nodeArr[index].value == pivot) {
            index ++;
        } else {
            swap(nodeArr, --big, index);
        }
    }
}

// 方法2
public static Node listPartition2(Node head, int pivot) {
    // 创建三条链表, 分别存放三类值
    Node sH = null; // small head
    Node sT = null; // small tail
    Node eH = null;
    Node eT = null;
    Node bH = null;
    Node bT = null;
    Node next = null; // save next node

    // 将节点分别放入三类链表上
    while (head != null) {
        next = head.next;
        head.next = null;
        if (head.value < pivot) {
            if (sH == null) {
                sH = head;
                sT = head;
            } else {
                sT.next = head;
                sT = head;
            }
        } else if (head.value == pivot) {
            if (eH == null) {
                eH = head;
                eT = head;
            } else {
                eT.next = head;
                eT = head;
            }

        } else {
            if (bH == null) {
                bH = head;
                bT = head;
            } else {
                bT.next = head;
                bT = head;
            }
        }
        head = next;
    }


    if (sT != null) {
        // 若有 s, s 直接连 eH
        sT.next = eH;
        // 此时要是 eH 没有, 下一个 if 会捕获到, 并把 s 重新连到 b
        // 要是 e 有,                       那么 e 就需要连到 b
        // 也就是, 不管 e 有没有, 都需要有人连接到 b, 要么是 s 连, 要么是 e 连
        eT = eT == null ? sT : eT;
        // 此时的 eT 可能是 s, 也可能是 e
    }
    // 如果上一个 if 进去了, 那么这里的 if 也一定会进去
    // 如果上一个 if 没进去, 那么这里的 if 可能会进去
    if (eT != null) {
        eT.next = bH;
    }

    // 连接顺序是 s-e-b, 要是没有 sH, 那就返回 eH, 要是 eH 也没有, 那就返回 bH
    // 所以前面两个 if 只需要确保三个区域能连接起来就可以。
    return sH != null ? sH : (eH != null ? eH : bH);
}

```

#### 我的代码
```py
def linked_list_partition2(head, pivot):
    # 稳定
    if head is None or head.next is None:
        return head

    # 创建三条链表, 分别存放大于等于小于
    lH, lT = None, None
    eH, eT = None, None
    mH, mT = None, None
    p = head
    while p is not None:
        if p.data < pivot:
            if lH is None:
                lH = lT = p
            else:
                lT.next = p
                lT = p
        elif p.data == pivot:
            if eH is None:
                eH = eT = p
            else:
                eT.next = p
                eT = p
        else:
            if mH is None:
                mH = mT = p
            else:
                mT.next = p
                mT = p
        p = p.next

    # 将三条链表连在一起。 只需要对 tail.next 进行操作
    if lT is not None:  # lT 要么连 eH, 要么连 mH。 (自动包含了 None 的情况)
        lT.next = eH if eH is not None else mH
    if eT is not None:  # eT 只会连 mH (自动包含了 None 的情况)
        eT.next = mH
    if mT is not None:  # mT 只会连 None
        mT.next = None

    # 只需返回三个头中的一个, 优先级是 lH > eH > mH
    return lH if lH is not None else (eH if eH is not None else mH)

```


### 练习 - 复制含有随机指针节点的链表

【题目】: 一种特殊的单链表节点类描述如下
```java
class Node {
    int value;
    Node next;
    Node rand;
    Node(int val){
        value = val;
    }
}
```
rand 指针是单链表节点结构中新增的指针, rand 可能指向链表中的任意一个节点, 也可能指向 null。
给定一个由 Node 节点类型组成的无环单链表的头节点 head, 请实现一个函数完成这个链表的复制, 并返回复制的新链表的头节点
```
   ╭ ---------- rand -------╮
   ↓                        ↑
╭  ─  ╮     ╭  ─  ╮      ╭  ─  ╮
│     │ --> │     │ -->  │     │ --> ....
╰  ─  ╯     ╰  ─  ╯      ╰  ─  ╯

```
【要求】: 时间复杂度 O(N), 额外空间复杂度 O(1)

【解释题目】: 这个链表和普通链表的区别在于, 他多了 rand 这个属性。 当我们复制链表时, 要求把每个节点上的 rand 的指向也成功复制。

【做法 1(笔记)】: 创建一个 Map, 它的 key 是旧链表节点, value 是新节点(即旧节点的克隆体)。 先遍历一遍链表, 完成所有节点值的拷贝, 此时的拷贝并不包含 next 和 rand。 再遍历旧链表, 通过旧链表获取 next 节点和 rand 节点, 将旧链表的节点作为 key, 可以获取就节点所对应的新节点的地址, 然后将新节点的地址赋值给 新节点的 next 和 rand。

【做法 2(面试)】:
如果不借助 Map 保存克隆出来的新节点, 那么就需要思考: 克隆出来的新节点要放在哪里能不占用额外空间, 并且还能根据旧节点找到对应的克隆节点?
答案就是将克隆节点插入到旧节点之后! 具体做法如下:
1. 第一次遍历链表, 创建克隆节点并将其插在对应旧节点之后
2. 第二次遍历链表, 为克隆节点找到他们的 rand 节点所对应的克隆节点。 (克隆 rand 节点 在 rand 节点的 next 中)
3. 第三次遍历链表, 将克隆节点从中抽出出来
- 注意: 第二次遍历链表和第三次遍历链表不能合并在一次遍历中。 因为 rand 节点是随机的, 如果找到一个克隆节点的 rand 节点后就将该克隆节点抽离出来, 那么后面的节点就可能找不到它的克隆 rand 节点了。

```
╭  ─  ╮                 ╭  ─  ╮                 ╭  ─  ╮                 ╭  ─  ╮                 ╭  ─  ╮
│old1 │      -->        │old2 │      -->        │old3 │      -->        │old4 │      -->        │old5 │      -->        ....
╰  ─  ╯                 ╰  ─  ╯                 ╰  ─  ╯                 ╰  ─  ╯                 ╰  ─  ╯
将新克隆的节点放到旧链表上
╭  ─  ╮     ╭  ─  ╮     ╭  ─  ╮     ╭  ─  ╮     ╭  ─  ╮     ╭  ─  ╮     ╭  ─  ╮     ╭  ─  ╮     ╭  ─  ╮     ╭  ─  ╮
│old1 │ --> │ new1│ --> │old2 │ --> │ new2│ --> │old3 │ --> │ new3│ --> │old4 │ --> │ new4│ --> │old5 │ --> │ new5│ ---> ....
╰  ─  ╯     ╰  ─  ╯     ╰  ─  ╯     ╰  ─  ╯     ╰  ─  ╯     ╰  ─  ╯     ╰  ─  ╯     ╰  ─  ╯     ╰  ─  ╯     ╰  ─  ╯
```

#### 老师代码:

```java
// 做法1
public static Node copyListWithRand1(Node head) {
    HashMap<Node, Node> map = new HashMap<Node, Node>();

    // 第一次遍历: 拷贝所有旧节点
    Node cur = head;
    while (cur != null) {
        map.put(cur, new Node(cur.value));
        cur = cur.next;
    }

    // 第二次遍历: 为新节点的 next 和 rand 赋值新节点的引用
    cur = head;
    while (cur != null) {
        // 将旧节点的 next 节点 所对应的新节点复制给新节点的 next
        map.get(cur).next = map.get(cur.next);
        mpa.get(cur).rand = map.get(cur.rand);
        cur = cur.next;
    }
    return map.get(head);
}

// 做法2
public static Node copyListWithRand2(Node head) {
    if (head == null) {
        return null;
    }

    // 1. 将新创建的克隆节点放到旧链表上。
    Node cur = head;
    Node next = null;
    while (cur != null) {
        next = cur.next;
        cur.next = new Node(cur.value);
        cur.next.next = next;
        cur = next;
    }

    // 2. 为新创建的克隆节点的 rand 赋值
    cur = head;
    Node curCopy = null;
    while (cur != null) {
        next = cur.next.next;
        curCopy = cur.next;
        curCopy.rand = cur.rand != null? cur.rand.next : null;
        cur = next;
    }

    // 3. 将克隆链表抽离出来
    Node res = head.next;
    cur = head;
    while (cur != null) {
        next = cur.next.next;
        curCopy = cur.next;
        cur.next = next;
        curCopy.next = next != null ? next.next : null;
        cur = next;
    }
    return res;

}
```

#### 我的代码

```py
def copy_random_node_linked_list2(head):
    if head is None:
        return None
    # 1. 创建克隆节点并将它们插入到源节点后
    p = head
    while p is not None:
        p_copy = Node(p.data)
        p_copy.next = p.next
        p.next = p_copy
        p = p_copy.next
    # 2. 找到克隆节点的 rand 所对应的克隆 rand 节点
    p = head
    while p is not None:
        p_copy = p.next
        pn = p.next.next
        if p.rand is not None:
            p_copy.rand = p.rand.next
        p = pn
    # 3. 将克隆节点抽离出来
    p = head
    copy_head = p.next
    while p is not None:
        p_copy = p.next
        pn = p.next.next
        p_copy.next = pn.next if pn is not None else None
        p.next = pn
        p = pn
    return copy_head
```

###