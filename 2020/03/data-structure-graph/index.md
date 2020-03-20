# 数据结构-图


链表是一种一对一的关系，树是一种一对多的关系，图则是一种多对多的关系。实际上，我们可以将链表和树都看作图的一部分。

## 1. 图的定义

用 V(Vertex) 表示顶点的集合，用 E(Edge) 表示边的集合，则图可以看作由一个非空的有限顶点集合V和一个有限边的集合组成，记作G(V, E)。其中

- 边可以表示为顶点对：(v, w) ∈ E，其中 v, w ∈ V
- 小括号包含的两个顶点表示无向边，有向边可以用 <v, w> 表示
- 不考虑重边和自回路

因为图涉及的概念比较多，因此我们不会像树那样一次性全部列出来，而是用到的时候进行解释，下面是一个图的例子和上面提到的几个概念。

![](https://s1.ax1x.com/2020/03/16/8Y0u9K.png)

关于图的操作集有很多，但最基本的如下

- Create()：建立并返回空图
- InsertVertex(Graph G, Vertex V)：将顶点 V 插入图 G
- InsertEdge(Graph G, Edge E)：将边 E 插入图 G
- DFS(Graph G, Vertex V)：从顶点 V 出发深度优先遍历图 G
- BFS(Graph G, Vertex V)：从顶点 V 出发广度优先遍历图 G
- ShortestPath(Graph G, Vertex V, int Dist[])：计算图 G 中顶点 V 到任意其它顶点的最短距离
- MST(Graph G)：计算图的最小生成树

## 2. 图的表示

图的表示有**很多种方法**，但最常用的是邻接矩阵和邻接表

### 2.1 邻接矩阵

通过邻接矩阵$G[N] [N]$表示图，首先将 N 个顶点从0到 N-1 编号，然后按如下公式填入数值，即如果两个顶点有边连接，填入0，如果没有边，则填入
$$
G[N][N] = \begin{cases} 0& 若<v_i,v_j>是G中的边 \\\ 1& 否则 \end{cases}
$$
以第一部分的示例无向图为例，其邻接矩阵形式如下图

![图的邻接矩阵表示](https://s1.ax1x.com/2020/03/16/8Y03Bd.png)

实际编程时，通常使用二维数组的形式存储。但对于无向图，邻接矩阵是对称的，通过只存储下三角矩阵或上三角矩阵的形式，可以节省一半的存储空间。

![下三角邻接矩阵](https://s1.ax1x.com/2020/03/16/8Y0U9f.png)

对于有向图来讲，邻接矩阵并不是对称的，因此不能采用这种方式。

下面介绍一个新的概念：顶点的度。对于无向图，顶点连接的边的数量称作顶点的度，对有向图来讲，从顶点发出的边数称作「出度」，指向该点的边数称作「入度」。对应到邻接矩阵中

- 无向图：度是对应行（或列）非0元素的个数
- 有向图：对应行非0元素的个数是「出度」，对应列非0元素的个数是「入读」

### 2.2 邻接表

如果是稠密图（边很多），使用邻接矩阵比较合适。如果是稀疏图（点很多而边很少），存在大量的无效元素，使用邻接矩阵会浪费大量的存储空间，这种情况可以使用邻接表来存储。

将所有顶点用一个指针数组$G[N]$表示，指向该顶点所有相邻顶点构成的链表（顺序不重要，可以随意），同样以第一部分的示例无向图为例，邻接表如下

![邻接表](https://s1.ax1x.com/2020/03/16/8Y0DBj.png)

对于网络，链表节点中需要增加表示权重的域。邻接表方便寻找任一顶点的所有邻接点，可以节省存储空间，但对有向图无法计算顶点的出度，需要构造「逆邻接表」

## 3. 图的构建

我们以邻接矩阵形式存储，定义图的结构体如下

```go
type Graph struct {
	VNum      int     // the number of Vertices
	ENum      int     // the numver of Edges
	AdjMatrix [][]int // adjacency matrix
}

```

为了方便测试，不建立 CreateVertex() 和 CreateEdge() 函数，而是直接对结构体进行初始化从而创建图，创建了一个无向图和一个有向图

```go
func CreateUndirectedGraph() *Graph {
	g := &Graph{}
	g.VNum, g.ENum = 6, 6
	for i := 0; i < 7; i++ {
		//为了便于操作和理解，从下标为1开始
		g.AdjMatrix = append(g.AdjMatrix, make([]int, 7))
	}
	g.AdjMatrix[1][2], g.AdjMatrix[1][3], g.AdjMatrix[1][4] = 1, 1, 1
	g.AdjMatrix[2][1], g.AdjMatrix[2][5] = 1, 1
	g.AdjMatrix[3][1], g.AdjMatrix[3][5] = 1, 1
	g.AdjMatrix[4][1] = 1
	g.AdjMatrix[5][2], g.AdjMatrix[5][3], g.AdjMatrix[5][6] = 1, 1, 1
	g.AdjMatrix[6][5] = 1
	return g
}

// 初始化一个图，顶点和边的数量、权值都预设好
func CreateDirectedGraph() *Graph {
	g := &Graph{}
	g.VNum, g.ENum = 7, 12
	for i := 0; i < 8; i++ {
		//为了便于操作和理解，从下标为1开始
		g.AdjMatrix = append(g.AdjMatrix, make([]int, 8))
	}
	g.AdjMatrix[1][2], g.AdjMatrix[1][4] = 2, 1
	g.AdjMatrix[2][4], g.AdjMatrix[2][5] = 3, 10
	g.AdjMatrix[3][1], g.AdjMatrix[3][6] = 4, 5
	g.AdjMatrix[4][3], g.AdjMatrix[4][5], g.AdjMatrix[4][6], g.AdjMatrix[4][7] = 2, 2, 8, 4
	g.AdjMatrix[5][7] = 6
	g.AdjMatrix[7][6] = 1
	return g
}
```

## 4. 图的遍历

有深度优先（Depth First Search, DFS）和广度优先（Breadth First Search, BFS）两种，前者类似于树的先序遍历，后者类似于树的层次遍历。下面图的遍历算法以下图为例

![图的邻接矩阵表示](https://s1.ax1x.com/2020/03/16/8Y03Bd.png)

### 4.1 深度优先遍历

递归解法的程序实现如下

```go
func DepthFirstSearch(g *Graph, vertex int, result []int) []int {
	g.AdjMatrix[0][vertex] = 1
	result = append(result, vertex)
	for k, v := range g.AdjMatrix[vertex] {
		if v > 0 && g.AdjMatrix[0][k] != 1 {
			result = DepthFirstSearch(g, k, result)
		}
	}
	return result
}
```

以结点0为入口，深度优先的遍历结果为[0 1 4 2 5 3]

### 4.2 广度优先遍历

```go
func BreadthFirstSearch(g *Graph, vertex int) []int {
	result := []int{}
	g.AdjMatrix[0][vertex] = 1
	queue := list.New()
	queue.PushBack(vertex)
	for queue.Len() != 0 {
		vertex := queue.Remove(queue.Front()).(int)
		result = append(result, vertex)
		for k, v := range g.AdjMatrix[vertex] {
			if v > 0 && g.AdjMatrix[0][k] != 1 {
				g.AdjMatrix[0][k] = 1
				queue.PushBack(k)
			}
		}
	}
	return result
}
```

以结点0为入口，广度优先的遍历结果为[0 1 2 3 4 5]

## 5. 最短路径

最短路径问题可以抽象为：在网络中，求两个不同顶点之间的所有路径中，边的权值之和最小的那一条路径，这条路径就是两点之间的最短路径（Shortest Path）。其中，第一个顶点为源点（Source），最后一个顶点为终点（Destination）。

最短路径问题不是一个单独的问题，而是一系列问题的综合，包括

1. 单源最短路径问题：从某固定源点出发，求其到所有其它顶点的最短路径
   - （有向）无权图
   - （有向）有权图
2. 多源最短路径问题：求任意两顶点间的最短路径

最短路径使用的示例图如下

![](https://s1.ax1x.com/2020/03/19/8yn7VA.png)

### 5.1 单源最短路径

在理解最短路径算法前有两个问题需要注意

1. 无向图和有向图都适用，虽然多数示例是有向图
2. 无权图是有权图的特例（权值为1），因此不单独介绍
3. 图中不可以存在权值为负的边，否则 Dijkstra(迪杰斯特拉)算法不起作用

如第3条所述，单源最短路径的典型算法称为 Dijkstra(迪杰斯特拉)算法，算法的基本思想为以起始点为中心层层向外扩展，直到扩展到终点为止。因此，该算法和广度优先搜索有一定的相似性。

输入上面的示例图，Dijkstra算法的输出为：[0 2 3 1 3 6 5]

```go
func DijkstraShortestPath(g *Graph, vertex int) {
	count := 1                          // 已收录的顶点数目，用于控制循环
	find := make([]bool, g.VNum+1)      //标记已访问过的结点
	prevVertex := make([]int, g.VNum+1) //当前节点的前驱结点
	distance := make([]int, g.VNum+1)   //当前结点的最短路径

	//初始化
	for i := 1; i <= g.VNum; i++ {
		if g.AdjMatrix[vertex][i] > 0 {
			distance[i] = g.AdjMatrix[vertex][i]
			prevVertex[i] = vertex
		} else {
			distance[i] = MAX_INT
			prevVertex[i] = -1
		}
	}

	distance[vertex] = 0
	find[vertex] = true
	v, d := vertex, 0 //用来迭代顶点的变量和初始距离

	for count < g.VNum {
		d = MAX_INT
		for i := 1; i <= g.VNum; i++ {
			if !find[i] && distance[i] < d {
				d = distance[i]
				v = i
			}
		}
		find[v] = true
		count++
		for k, t := range g.AdjMatrix[v] {
			if !find[k] && t > 0 && distance[v]+t < distance[k] {
				distance[k] = distance[v] + t
				prevVertex[k] = v
			}
		}

	}
	fmt.Println(distance[1:])
}
```

### 5.2 多源最短路径

多源最短路径使用Floyd算法，这是一个经典的动态规划算法，核心思想是：从任意节点 i 到任意节点 j 的最短路径不外乎2种可能，1是直接从 i 到 j，2是从 i 经过若干个节点 k 到 j。所以，我们假设 Distance(i,j) 为节点u到节点v的最短路径的距离，对于每一个节点k，我们检查 Distance(i,k) + Distance(k,j) < Distance(i,j) 是否成立，如果成立，证明从 i 到 k 再到 j 的路径比 i 直接到 j 的路径短，我们便设置Distance(i,j) = Distance(i,k) + Distance(k,j)，这样一来，当我们遍历完所有节点 k，Distance(i,j) 中记录的便是 i 到 j 的最短路径的距离。

整个过程可以描述为两个步骤

1. 从任意一条单边路径开始，所有两点之间的距离是边的权，如果两点之间没有边相连，则权为无穷大。
2. 对于每一对顶点 u 和 v，看看是否存在一个顶点 w 使得从 u 到 w 再到 v 比己知的路径更短，如果是，更新它。

程序实现如下

```go
func FloydShortestPath(g *Graph, vertex int) {
	var D [][]int
	var path [][]int
	var i, j, k int
	for i := 0; i < g.VNum; i++ {
		D = append(D, make([]int, g.VNum))
		path = append(path, make([]int, g.VNum))
	}
	for i = 1; i <= g.VNum; i++ {
		for j = 1; j < g.VNum; j++ {
			D[i][j] = g.AdjMatrix[i][j]
			path[i][j] = -1
		}
	}
	for k = 1; k <= g.VNum; k++ {
		for i = 0; i < g.VNum; i++ {
			if D[i][k]+D[k][i] < D[i][j] {
				D[i][j] = D[i][k] + D[k][j]
				path[i][j] = k
			}
		}
	}
}
```

## 6. 拓扑排序

如果图中从 v 到 w 有一条有向路径，则 v 一定排在 w 之前。满足此条件的顶点序列称为一个**拓扑序**，获得一个拓扑序的过程就是**拓扑排序**。

一个最典型的例子是排课表，一个专业很多课程都有先修课，因此排课时必须考虑先修课的存在，以每门课程为结点，若课程间存在先修课关系则有边，这样构成的网络叫做AOV（Activity On Vertex）网，也是拓扑排序使用的网络。

拓扑排序用一句话描述就是「每次删除入度为0的顶点并输出它」，以下图为例，拓扑排序的结果为：V1,V2,V3,V4,V5。拓扑排序的结果是不唯一的。

![拓扑排序](https://s1.ax1x.com/2020/03/19/8yKVYt.png)

拓扑排序必定是一个有向无环图（DAG），因此，该算法也可以用于判断一个图是否为有向无环图。程序实现如下

```go
func TopologicalSort(g *Graph) []int {
	result := []int{} //拓扑排序的结果数组
	count := 1        //判断图中是否有环

	//计算各结点的入度并存储
	indegree := make([]int, g.VNum+1)
	for i := 1; i <= g.VNum; i++ {
		for j := 1; j <= g.VNum; j++ {
			if g.AdjMatrix[i][j] > 0 {
				indegree[j]++
			}
		}
	}

	queue := list.New()

	//入度为0的结点入队
	for i := 1; i <= g.VNum; i++ {
		if indegree[i] == 0 {
			queue.PushBack(i)
		}
	}

	for queue.Len() != 0 {
		vertex := queue.Remove(queue.Front()).(int)
		result = append(result, vertex)
		count++
		for k, v := range g.AdjMatrix[vertex] {
			if v > 0 {
				indegree[k]--
				if indegree[k] == 0 {
					queue.PushBack(k)
				}
			}
		}
	}
	if count != g.VNum {
		fmt.Println("This is a DAG!")
	}
	return result
}
```

## 7. 关键路径

拓扑排序应用在AOV网络上，每个顶点表示一个活动或任务。如果每条边表示一个活动或任务，就是AOE（Activity On Edge）网络，多用在安排一个庞大生产流程的工序上，工序之间有先后关系。

如下图所示，在AOE网络中，事件 i 发生后，其后继活动 a(i,*) 都可以开始，但只有所有先导活动 a( *,j ) 都结束后，事件 j 才可以发生。

![](https://s1.ax1x.com/2020/03/19/8y1p4A.png)

假设一个工程的 AOE 网如下，最常求的就是 a) 整个工程完工需要多长时间？ b) 哪些活动影响工程进度？或求关键路径。图中的虚线表示事件有先后关系，但是这个活动不存在。

![AOE网络](https://s1.ax1x.com/2020/03/19/8y3Exx.png)

对事件（顶点）i，令最早发生时间为 ve(i)，最晚发生时间为 vl(i)；

对活动（边）a(i,j)，令最早开始时间为 e(i,j)，最晚开始时间为 l(i,j)。

那么整个工程的完工时间就是终点的最早发生时间；关键路径就是路径长度最长的路径。求关键路径的算法如下：

1. 将所有顶点进行拓扑排序；
2. 计算 ve(j)， $ve(j) = max\{ve(*) + a(*,j)\}$ ，其中*为任意前驱事件，有ve(1) = 0；
3. 计算 vl(i)， $vl(i) = min\{vl(*) - a(i,*)\}$ ，其中*为任意后继事件，有vl(n) = ve(n)；
4. 计算 e(i,j) 和 l(i,j)，$e(i,j) = ve(i)$，$l(i,j) = vl(j)-a(i,j)$
5. 结论：工程总用时 ve(n)，关键活动是 e(i,j) = l(i,j) 的活动 a(i,j)

如果只求工程总用时，那么只需要第1，2步。关于两个核心公式可以这样理解：事件 j 在所有前驱活动都完成后发生，所以其最早发生时间为 $ve(j) = max\{ve(*) + a(*,j)\}$ ，即取决于最慢的前驱活动。另一方面，事件 i 发生后所有后继活动都可以开始了，所以其最晚发生时间为  $vl(i) = min\{vl(*) - a(i,*)\}$，即不耽误最慢的后继活动。

简单理解的话，就是按照拓扑有序排列顶点，然后从前往后计算事件的最早发生时间得到总时间，再从后往前计算事件的最晚发生时间，最后计算活动的最早和最晚开始时间得到关键活动和关键路径。求上面示例图的关键路径过程如下表

| 事件 | 最早发生时间ve | 最晚发生时间vl | 活动   | 最早开始时间e | 最晚开始时间l |
| ---- | -------------- | -------------- | ------ | ------------- | ------------- |
| v1   | 0              | 0              | a(1,2) | 0             | 0             |
| v2   | 6              | 6              | a(1,3) | 0             | 2             |
| v3   | 4              | 6              | a(1,4) | 0             | 1             |
| v4   | 5              | 6              | a(2,5) | 6             | 6             |
| v5   | 7              | 7              | a(3,5) | 4             | 6             |
| v6   | 7              | 7              | a(4,6) | 5             | 6             |
| v7   | 12             | 13             | a(5,6) | 7             | 7             |
| v8   | 11             | 11             | a(5,7) | 7             | 8             |
| v9   | 15             | 15             | a(5,8) | 7             | 8             |
|      |                |                | a(6,8) | 7             | 7             |
|      |                |                | a(7,9) | 12            | 13            |
|      |                |                | a(8,9) | 11            | 11            |

最终得到工程完工需要时间为15，关键路径是 1,2,5,6,8,9

程序实现如下



## 8. 最小生成树

未完待续……