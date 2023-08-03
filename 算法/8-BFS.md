BFS 的核心思想应该不难理解的，就是把一些问题抽象成图，从一个点开始，向四周开始扩散。一般来说，我们写 BFS 算法都是用「队列」这种数据结构，每次将一个节点周围的所有节点加入队列。

BFS 相对 DFS 的最主要的区别是：**BFS 找到的路径一定是最短的，但代价就是空间复杂度可能比 DFS 大很多**

-------

### 算法框架

要说框架的话，我们先举例一下 BFS 出现的常见场景好吧，**问题的本质就是让你在一幅「图」中找到从起点 start 到终点 target 的最近距离，这个例子听起来很枯燥，但是 BFS 算法问题其实都是在干这个事儿**，把枯燥的本质搞清楚了，再去欣赏各种问题的包装才能胸有成竹嘛

```go
// 计算从起点 start 到终点 target 的最近距离
func BFS(start Node, target Node) int {
    q := make([]Node, 0) // 核心数据结构
    visited := make(map[Node]bool) // 避免走回头路

    q = append(q, start) // 将起点加入队列
    visited[start] = true

    for len(q) > 0 {
        sz := len(q)
        /* 将当前队列中的所有节点向四周扩散 */
        for i := 0; i < sz; i++ {
            cur := q[0]
            q = q[1:]
            /* 划重点：这里判断是否到达终点 */
            if cur == target {
                return step
            }
            /* 将 cur 的相邻节点加入队列 */
            for _, x := range cur.adj() {
                if _, ok := visited[x]; !ok {
                    q = append(q, x)
                    visited[x] = true
                }
            }
        }
    }
    // 如果走到这里，说明在图中没有找到目标节点
}
```

### 双向 BFS 优化

**传统的 BFS 框架就是从起点开始向四周扩散，遇到终点时停止；而双向 BFS 则是从起点和终点同时开始扩散，当两边有交集的时候停止**

双向 BFS 还是遵循 BFS 算法框架的，只是**不再使用队列，而是使用 HashSet 方便快速判断两个集合是否有交集**
