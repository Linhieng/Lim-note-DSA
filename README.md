# 数据结构与算法

本次学习是跟着 [视频](https://www.bilibili.com/video/BV13g41157hK/) 来的。

记录笔记时, 暂时不打算记录过于生涩, 或者过于严格化的定义, 力求能听懂就行。 所以一些自己已经熟知的知识, 可能就不会表现在笔记上。 但保不齐未来某个时间段就忘了现在这个时间段上, 自认为已经"熟知"的知识。

人的优势在于使用工具。不要认为只靠想就做出一道算法题有多厉害，真正厉害的应该是能画出来，理清逻辑，并且能给别人讲明白。所以要多画图！

## 学习的知识

- 时间复杂度, (额外)空间复杂度
- 位运算
- 对数器
- 递归(recursion), master 公式
- 二分法, partition
- 哈希表, Map, Set
- 有序表, Map, Set

## 基础内容, 必须掌握

- 排序算法(SortAlgorithm)
    - 选择排序(SelectSort)
    - 冒泡排序(BubbleSort)
    - 插入排序(InsertSort)
    - 归并排序(MergeSort)
        - 小和问题
    - (随机)[快速排序(QuickSort)](https://practice.geeksforgeeks.org/problems/quick-sort/1)
        - 二项分布(two-way partition)
        - 三项分布(three-way partition)
    - [堆排序(HeapSort)](https://practice.geeksforgeeks.org/problems/heap-sort/1)
        - heapInsert
        - heapify
    - 桶排序
        - 计数排序(CountSort)
        - 基数排序(RadixSort)
- 链表(LinkedList)
    - 反转链表(reverse)
    - 链表回文(palindrome)
    - [链表 partition](https://practice.geeksforgeeks.org/problems/given-a-linked-list-of-0s-1s-and-2s-sort-it/1)
    - 链表随机复制
- 二叉树(BinaryTree)
    - 递归遍历, 非递归遍历。 (前序、中序、后序和层序)
    - [最大宽度](https://practice.geeksforgeeks.org/problems/right-view-of-binary-tree/1)
    - 搜索二叉树 BST(BinaryTreeSearch)
    - 完全二叉树, 满二叉树
        - 堆
    - 平衡二叉树
    - 递归套路解决二叉树 DP(DynamicProgramming)
    - [最近公共祖先节点](https://practice.geeksforgeeks.org/problems/lowest-common-ancestor-in-a-bst/1)
    - 后继节点
    - (递归, 非递归)序列化和反序列化(Serialize & Deserialize)
- 前缀树
    - 存储字符串数组
- 图(Graph)
    - BFS 广度优先遍历
    - DFS 深度优先遍历
    - 拓扑排序
    - MST 最小生成树
        - Kruskal
        - Prim
    - Dijkstra 最短路径
- 贪心算法
    - 安排会议
    - 最小字典序
    - 哈夫曼编码
    - 项目利润
- 暴力递归 (Violent recursion) (这是动态规划的基础)
    - 汉诺塔问题
    - 打印字符串的全部子序列, 全部排列
    - 先手后手
    - 递归逆转栈
    - 数字转换26进制的所有结果
    - 暴力背包
    - N 皇后
- 哈希
    - 哈希函数的特性(离散,均匀)
    - 找出出现次数最多的数字 (哈希值求模)
    - 布隆过滤器 (位图)
    - 一致性哈希 (虚拟节点技术)
- 并查集
    - 并行处理岛问题(map reduce)
- KMP 算法
    - KMP 搜索所有字符串
    - next 数组和 lps 数组的求取
- manacher 算法
    - 回文半径/直径数组
    - 所到达的最右侧的回文右边界和它的中心点
- 双端队列 - 窗口问题
- 单调栈
- 二叉树 Morris 遍历
    - 前序中序后续遍历
    - 判断搜索二叉树
- 树型DP
    - 求一棵树的最大距离
    - 最大快乐值（最大收益）
- 大数据算法
    - 找出出现次数最多的数字 (哈希值求模)
    - 布隆过滤器 (位图)
    - 一致性哈希 (虚拟节点技术)
    - 并查集结构做岛问题的并行计算
    - 位图解决某一范围上数字的出现情况
    - 利用分段统计思想、并进一步节省大量空间
    - 利用堆、外排序来做多个处理单元的结果合并

## 我的想法

[ ] 改写可以那些使用递归实现的算法

初次编写代码时, 先考虑特殊情况/边界, 再考虑通用情况。
优化代码结构时, 先考虑通用情况(提取出核心代码), 再考虑特殊情况/边界。

多动笔画一画, 学会这一个技巧不仅仅有助于理解 DSA, 也能帮助你给别人讲解, 从而写出好的笔记。
