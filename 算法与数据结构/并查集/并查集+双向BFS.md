# 并查集+双向bfs



> 给你一个大小为 n x n 的二元矩阵 grid ，其中 1 表示陆地，0 表示水域。
>
> 岛 是由四面相连的 1 形成的一个最大组，即不会与非组内的任何其他 1 相连。grid 中 恰好存在两座岛 。
>
> 你可以将任意数量的 0 变为 1 ，以使两座岛连接起来，变成 一座岛 。
>
> 返回必须翻转的 0 的最小数目。

使用「并查集」将两个岛标记出来，然后将两个岛的点分别入队，再运用「双向 BFS」来找最短通路。

p[N] 表示 并查集 

创建并查集

```java
int find(int x) {
    if (p[x] != x) p[x] = find(p[x]); //寻找最外面的节点 并且更新当前节点的根
    return p[x];
}
void union(int x, int y) {
    p[find(x)] = p[find(y)];
}
```

为了方便，我们将 grid 记为 g。

对于所有满足 g[i][j] = 1g[i][j]=1 的节点与其四联通的方向，值同为 11 的节点进行并查集连通性维护。

随后建立两个队列 d1 和 d2 分别存储两个岛的节点（以二元组 (x, y)(x,y) 的方式出入队），并使用两个哈希表 m1 和 m2 来记录从两岛屿出发到**达该节点所消耗的步数（以节点的一维编号为 key，以消耗步数为 value）。** 

最后是使用「双向 BFS」来求解使两岛屿联通的最小通路：每次从队列中较少的进行拓展，只有尚未被处理过的节点（没有被当前哈希表所记录）才进行入队并更新消耗步数，**当拓展节点在另外一个队列对应的哈希表表中出现过，说明找到了最短通路。**

```java
class Solution {
    static int N = 10010;
    static int[] p = new int[N];
    static int[][] dirs = new int[][]{{1,0},{-1,0},{0,1},{0,-1}};
    int n;
    int getIdx(int x, int y) {
        return x * n + y;
    }
    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }
    void union(int x, int y) {
        p[find(x)] = p[find(y)];
    }
    public int shortestBridge(int[][] g) {
        n = g.length;
        for (int i = 0; i <= n * n; i++) p[i] = i;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (g[i][j] == 0) continue;
                for (int[] di : dirs) {
                    int x = i + di[0], y = j + di[1];
                    if (x < 0 || x >= n || y < 0 || y >= n) continue;
                    if (g[x][y] == 0) continue;
                    union(getIdx(i, j), getIdx(x, y));
                }
            }
        }
        int a = -1, b = -1;
        Deque<int[]> d1 = new ArrayDeque<>(), d2 = new ArrayDeque<>();
        Map<Integer, Integer> m1 = new HashMap<>(), m2 = new HashMap<>();
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (g[i][j] == 0) continue;
                int idx = getIdx(i, j), root = find(idx);
                if (a == -1) a = root;    
                else if (a != root && b == -1) b = root;
                if (root == a) {
                    d1.addLast(new int[]{i, j});
                    m1.put(idx, 0);
                } else if (root == b) {
                    d2.addLast(new int[]{i, j});
                    m2.put(idx, 0);
                }
            }
        }
        while (!d1.isEmpty() && !d2.isEmpty()) {
            int t = -1;
            if (d1.size() < d2.size()) t = update(d1, m1, m2);
            else t = update(d2, m2, m1);
            if (t != -1) return t - 1;
        }
        return -1; // never
    }
    int update(Deque<int[]> d, Map<Integer, Integer> m1, Map<Integer, Integer> m2) {
        int sz = d.size();
        while (sz-- > 0) {
            int[] info = d.pollFirst();
            int x = info[0], y = info[1], idx = getIdx(x, y), step = m1.get(idx);
            for (int[] di : dirs) {
                int nx = x + di[0], ny = y + di[1], nidx = getIdx(nx, ny);
                if (nx < 0 || nx >= n || ny < 0 || ny >= n) continue;
                if (m1.containsKey(nidx)) continue;
                if (m2.containsKey(nidx)) return step + 1 + m2.get(nidx);
                d.addLast(new int[]{nx, ny});
                m1.put(nidx, step + 1);
            }
        }
        return -1;
    }
}

```

