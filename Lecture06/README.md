## 🍕 图

### 认识图

图的组成(Components):
- 顶点集合(Vertices)
- 边集合(Edges)

图分为两种类型:
- 有向图(Directed Graph)
- 无向图(Undirected Graph)

图常见的两种表示方式:
- 邻接表(Adjacency List): 用链表或数组表示。对于每个顶点，我们都维护一个与之相邻的顶点列表。
- 邻接矩阵(Adjacency Matrix): 用一个二维数组来表示图中顶点之间的连接关系。 假设图有 $N$ 个顶点, 则邻接矩阵大小为 $N×N$

图的其他常见概念:
- 权重(Weight): 表示边的相关值, 或者成本
- 入度(In-degree): 指向该节点的边的数量
- 出度(Out-degree): 该节点指向其他节点的边的数量
- 邻接点: (Adjacent) 该节点能直接访问的节点。 或者说, 从该节点出发, 能直接到达(直走一条边)的节点。
- 邻接边: (Adjacent) 属于该节点的边(有方向性)。

### 创建自己的图结构

图本身的算法不难, 图的难点在于图的表示方式可以非常丰富, 当遇到一个特殊的表示方式时, 如何将一个已有的算法应用到新的图中, 才是难点。
技巧: 定好一个图结构, 根据这个图结构熟悉它所对应的图算法。 后续遇到特殊结构的图时, 将其转换为自己定好的图的结构, 这样一来图算法就可以复用了, 我们只需要实现转换接口函数就行了。

老师的图结构:

```java
class Graph {
    /*
    nodes 的 key 一般是 待转换图结构中节点的表示方式, 利用它们所给定的节点作为 key, 能够保证唯一性, 并且后续新增同样的待转换结构时, 也可以方便的判断是否已存在该节点
             value 就是转换后的, 属于我们自己的图的节点。
    */
    HashMap<Integer, Node> nodes; //
    HashSet<Edge> edges; // 边集合
}
class Node {
    int value; // 节点的值
    int in; // 节点入度
    int out; // 节点出度
    // nexts 和 nodes 不一定是得 set 集合, 也可以是数组列表。 具体情况具体分析。 如果使用数组能够实现需求时, 数组是更好的选择。 虽然 set 的操作也是常数时间, 但这个常数时间比起数组的还是比较大的。
    ArrayList<Node> nexts; // 邻接点集合
    ArrayList<Edge> edges; // 邻接边集合
}
class Edge {
    int weight;
    Node from;
    Node to;
}
```

### 广度(宽度)优先遍历 BFS(Breath-First Search)

思路类似于二叉树的层序遍历, 图不同于二叉树, 图是可以成环的, 所以需要稍微改一下层序遍历的代码。
具体体现在: 利用 set 和 queue 实现 BFS。 set 是为了防止一个节点重复进入到 queue 中。

【步骤介绍】:
- 初始时 queue 中一定有一个节点, 遍历将从这个节点开始
- 从 queue 中取出一个节点, 将该节点的 nexts 中所有节点都添加到 queue 中。
- 为了保证一个节点不会重复加入 queue 中, 每次添加时都先检查 set 中是否存在这个节点
- 不断的从 queue 中取出节点, 重复上面步骤, 直接所有节点都曾进过 queue。

```java
void bfs(Node node) {
    if (node == null) {
        return;
    }
    Queue<Node> queue = new LinkedList<>();
    HashSet<Node> set = new HashSet<>();
    queue.add(node);
    set.add(node);
    while (!queue.isEmpty()) {
        Node cur = queue.poll();
        print(cur.value); // 任意操作
        for (Node next: cur.nexts) {
            if (!set.contains(next)) {
                set.add(next);
                queue.add(next);
            }
        }
    }
}
```

### 深度优先遍历 DFS(Deep-First Search)

广度优先遍历时, 是先将一个节点中所有 next 加入到 queue 中。
由于加入到 queue 的顺序类似于层序遍历, 所以在弹出 queue 时进行处理, 就可以实现广度优先遍历。

深度优先遍历而不是, 它是不断从一个节点的 nexts 中取出一个 next, 然后逮着它薅(方向是向下的),
直到薅完了, 才回退到上一个 nexts 取出另一个 next 继续薅。 如此反复, 所以节点都将被薅完(处理)
所以深度优先遍历中, 利用的是 stack, 不断压栈(薅), 薅完了才回退(出栈), 然后逮着另一个继续薅(压栈)。
处理的时机也变成了压栈的那一刻。

```java
void dsf(Node node) {
    if (node == null) {
        return;
    }
    Stack<Node> stack = new Stack<>();
    HashSet<Node> set = new HashSet<>();
    stack.add(node);
    set.add(node);
    print(node.value);
    while (!stack.isEmpty()) {
        Node cur = stack.pop(); // 薅完了 cur
        for (Node next : cur.nexts) { // 看看 cur 的 nexts 中还有谁没薅过
            if (!set.contains(next)) { // 找到一个没薅过的 cur 邻接点
                stack.push(cur); // 说明此时 cur 还有继续薅的价值, 所以要重新把 cur 压栈
                stack.push(next); // 将邻接点 next 放到 stack 薅
                set.add(next); // 薅过的点加到 set 中
                print(next.value); // 处理时刻
                break; // 每次只从 nexts 中逮着一个 next 薅
            }
        }
    }
}
```

### 拓扑排序

场景: 文件 a 的编译, 会需要很多依赖, 并且保证这些依赖不会是循环依赖。 这些依赖之间的关系, 就是一张图。 请问要按怎样的顺序编译这些依赖, 才能完成 a 的编译。
> 循环依赖, 比如 a 依赖 b, b 又依赖 a, 这就是循环依赖, 这种情况下是无法编译成功的。

【实现】:
- 因为没有循环依赖, 所以一定会有一个入度为 0 的节点。
- 只编译入度为 0 的节点, 然后将该节点的影响消除: 将该节点的所有 next 节点的入度数 - 1。
- 如此反复, 剩下的最后一个节点, 一定会是 a 节点。 表示现在所有依赖都准备好了, 可以编译 a 了。

【核心】: 拓扑排序中, 核心就是一个节点的 **入度** 和一个节点的 **邻接点集合**。
每次都将入度为 0 的节点取出, 然后利用邻接点集合将其他节点的入度 -1。
周而复始, 就可以实现拓扑排序。

```java

List<Node> sortedTopology(Graph graph) {
    HashMap<Node, Integer> inMap = new HashMap<>(); /* key: 节点;   value; 该节点剩余的入度 */
    Queue<Node> zeroInQueue = new LinkedList<>(); /* 只存储剩余入度为 0 的节点 */
    // 初始化 zeroInQueue
    for (Node node : graph.nodes.values()) {
        inMap.put(node, node.in);
        if (node.in == 0) {
            zeroInQueue.add(node);
        }
    }
    List<Node> result = new ArrayList<>();
    while (!zeroInQueue.isEmpty()) {
        // 编译入度为 0 的节点
        Node cur = zeroInQueue.poll();
        result.add(cur); // 按编译顺序对节点排序
        // 消除该节点的影响
        for (Node next: cur.nexts) {
            inMap.put(next, inMap.get(next) - 1);
            if (inMap.get(next) == 0) {
                zeroInQueue.add(next);
            }
        }
    }
    return result;
}

```


### 最小生成树 MST(Minimum Spanning Tree)

MST: 指在一个连通无向图中找到一棵包含所有顶点并且边的权值之和最小的树。 在最小生成树中，任意两个顶点之间有且仅有一条路径。

【注意】: 最小生成数只适用于连通无向图, 对于有向图, 可以参考最小生成森林

常见的求解最小生成树的算法有:
- Kruskal 算法
- Prim 算法

【注意⚠️】: 图可以是"森林", 所以遍历时要注意遍历到所有的节点。

#### Kruskal 算法

【算法核心】: 将图中的边按照权值从小到大进行排序, 然后依次加入生成树中。 如果加入的边不会形成环路, 则确定加入该边, 否则删除该边。

【判断环路】:
- 在将一条边加入到生成树中前, 先判断该边的 from 和 to 是否在一个集合中, 不在一个集合则表示不成环。
- 将一条边加入到生成树后, 需要将 from 所在集合和 to 所在集合合并为一个集合。
- 实现判断环路的有很多种方法, 最知名也最快的就是 "并查集"(常数级别)。

```java

// 实现 "并查集" 功能, 但没有 "并查集" 快
class EasyUnionFind {
    HashMap<Node, List<Node>> setMap;

    EasyUnionFind(List<Node> nodes) {
        for (Node cur : nodes) {
            List<Node> set = new ArrayList<Node>();
            set.add(cur);
            setMap.put(cur, set);
        }
    }

    boolean isSameSet(Node from, Node to) {
        List<Node> fromSet = setMap.get(from);
        List<Node> toSet = setMap.get(to);
        return fromSet == toSet;
    }

    void union(Node from, Node to) {
        List<Node> fromSet = setMap.get(from);
        List<Node> toSet = setMap.get(to);

        for (Node toNode : toSet) {
            fromSet.add(toNode);
            setMap.put(toNode, fromSet);
        }
    }
}

Set<Edge> kruskalMST(Graph graph) {

    // UnionFind unionFind = new UnionFind();
    // unionFind.makeSets(graph.nodes.values());
    EasyUnionFind unionFind = new EasyUnionFind(graph.nodes.values()) // 使用上面的结构也能实现并查集功能, 只是没有并查集快
    // 有一个有序表来存储边, 实现边的有序存储。
    PriorityQueue<Edge> priorityQueue = new PriorityQueue<>(new EdgeComparator());
    for (Edge edge : graph.edges) {
        priorityQueue.add(edge);
    }
    Set<Edge> result = new HashSet<>();
    while (!priorityQueue.isEmpty()) {
        Edge edge = priorityQueue.poll();
        if (!unionFind.isSameSet(edge.from, edge.to)) {
            result.add(edge);
            unionFind.union(edge.from, edge.to);
        }
    }
    return result;
}
```



#### Prim 算法

【算法核心】: 从一个节点开始, 逐步扩展生成树。 每次选择与当前生成树连接的边中(不包含生成树内部的边)权值最小的边, 直到生成树包含所有节点为止。

【过程】:
1. 初始时, 随机选取一个节点, 然后将该节点的邻接边解锁, 此时生成树只包含这一个节点。
2. 从生成树的邻接边中选取权重最小的那一条边, 然后将边上的节点纳入生成树中, 此时将会有新的边被解锁。 (对于多个相同的最小权重边, 任选一个都可以)
3. 重复上面步骤, 直到将所有节点纳入生成树中。
- 注意⚠️, 生成树的邻接边不包含生成树内部的边。
- 不选到生成树内部的边的方法:, 看看边的另一侧是否是生成树所包含的节点。


【Kruskal 和 Prim 区别】:
- Kruskal 算法需要判断成环。 因为每次连接的不是两个节点, 而是两个集合, 直接将两个集合相连时可能出现环, 所以需要判断成环。
- Prim 算法不需要判断成环。 因为 Prim 算法每次都只会将一个节点拉入到生成树中, 这是点并入集合的过程, 所以它不需要判断成环。

```java
class EdgeComparator implements Comparator<Edge> {
    @Override
    int compare(Edge o1, Edge o2) {
        // 从小到大排序
        return o1.weight - o2.weight;
    }
}

Set<Edge> primMST(Graph graph) {
    PriorityQueue<Edge> priorityQueue = new PriorityQueue<>( new EdgeComparator() ); // 存储被解锁(可供选择)的边
    HashSet<Node> set = new HashSet<>(); // 存储生成树内的部节点
    Set<Edge> result = new HashSet<>(); // 存储生成树的边

    // 这个 for 循环是用来处理 图 是森林的情况, 这样才能够为每个不连通的区域, 生成各自的最小生成树
    for (Node node : graph.nodes.values()) {

        if (!set.contains(node)) {
            set.add(node);
            for (Edge edge : node.edges) {
                priorityQueue.add(edge); // 新增一个点, 解锁一些边
            }
            // 这一个 for 循环就能够生成属于 node 节点的 MST 了
            while (!priorityQueue.isEmpty()) {
                Edge edge = priorityQueue.poll(); // 每次取出一条最小权重边。
                Node toNode = edge.to;
                if (!set.contains(toNode)) { // 如果这个边是生成树内部的边, 则需要重新选取
                    set.add(toNode);
                    result.add(edge);
                    // 新增一个点, 解锁一些边
                    for (Edge nextEdge : toNode.edges) {
                        // 这里好把一些已经删除的边重新放入到有序表中, 但不会影响最后结果, 因为他们放进去后迟早再次被删除。
                        priorityQueue.add(nextEdge);
                    }
                }
            }
        }

    }
    return result;
}
```

### Dijkstra 算法 - 最短路径

【过程】:
- 选定起始点, 然后初始化起始点与其他所有节点的距离, 除了起始点与起始点本身的距离是 0 外, 其他距离都是无穷大∞ , 用一个 map 保存这些信息。
- 从 map 中选取距离最短的节点 node, 最短距离是 d。 然后查看 node 的邻接边和邻接点,
计算 d 加上 node 邻接边后到达邻接点的距离, 然后和 map 中的距离比较, 如果新产生的距离更短, 则更新 map 中的距离。
依次计算万 node 的所有邻接边后, 将 map 中 node 节点的距离锁定, 目的是让下一次从 map 中选取最近节点时, 不要再次选取到 node 节点。
原因是, 不存在负数距离的情况下, 这个 node 节点的距离将不会再更新了, 所以它也没什么用了。
- 重复上面步骤, 直到 map 没有节点可以选择。

【适用范围】: 不能出现累加和为负值的情况。 这样导致距离无穷小(死循环)

比如, 下面例子中, Dijkstra 算法会不断在其中循环, 因为每转一圈, 都会发现距离边的更短了, 所以会一直转下去。
```
    -3
A ------ B
 \      /
 1\    /1
   \  /
    C
A 到 B 距离 -3
B 到 C 距离 1
C 到 A 距离 1
```

```java
HashMap<Node, Integer> dijkstra1(Node head) {
    // distanceMap 存储节点 head 到其他节点的最小距离
    HashMap<Node, Integer> distanceMap = new HashMap<>();
    distanceMap.put(head, 0);
    // selectedNodes 存储被锁定的节点, 即该节点的距离不可能边的更小。
    HashSet<Node> selectedNodes = new HashSet<>();
    // getMinDistanceAndUnselectedNode 从 distanceMap 中选取未被锁定的最小距离的节点
    Node minNode = getMinDistanceAndUnselectedNode(distanceMap, selectedNodes);
    while (minNode != null) {
        int distance = distanceMap.get(minNode);
        // 遍历该节点与其他节点的距离, 看看该节点与其他节点距离 + 该节点在 map 中的距离, 是否能够比 map 中的距离更小, 如果可以则更新 map
        for (Edge edge : minNode.edges) {
            Node toNode = edge.to;
            if (!distanceMap.containsKey(toNode)) {
                distanceMap.put(toNode, distance + edge.weight);
            } else {
                distanceMap.put(edge.to, Math.min(distanceMap.get(toNode), distance+edge.weight)); // 如果新距离更短, 则更新距离
            }
        }
        // 遍历完 minNode 节点后, 锁定 minNode 节点。
        selectedNodes.add(minNode);
        // 然后重新继续选取最小距离的节点
        minNode = getMinDistanceAndUnselectedNode(distanceMap, selectedNodes);
    }
    return distanceMap;
}

/*
getMinDistanceAndUnselectedNode 函数可以继续优化。
    这个函数的功能就是从 distanceMap 中返回一个不在 selectedNode 的最小值
    所以可以利用 堆结构 让 distanceMap 维持有序的状态。
    不过主要注意的一点是, 不能使用系统提供的堆。
    原因在于 distanceMap 中的元素并不是不变的, 之前的某个值可能会突然变小, 这个时候需要更新堆。
    但系统提供的堆, 如果要让修改你之前添加到的数据, 系统堆能够做到, 但是它会将所有元素重新进行排序。
    即相当于重新创建一个堆。 这种模式下使用堆排序时间复杂度更高。
    所以需要自己定义一个堆结构, 并且自己实现: 当某个节点值变小时, 要能够执行 heapInsert 或 heapify 操作。
 */
Node getMinDistanceAndUnselectedNode(
    HashMap<Node, Integer> distanceMap,
    HashSet<Node> selectedNodes,
) {
    Node minNode = null;
    int minDistance = Integer.MAX_VALUE;
    for (Entry<Node, Integer> entry : distanceMap.entrySet()) {
        Node node = entry.getKey();
        int distance = entry.getValue();
        if (!selectedNodes.contains(node) && distance < minDistance) {
            minNode = node;
            minDistance = distance;
        }
    }
    return minNode;
}
```
