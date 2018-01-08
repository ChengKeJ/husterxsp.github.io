---
layout: post
title: 单源最短路径问题
date: 2017-10-31
author: "Xsp"
catalog: true
tags:
    - Algorithm
---
关于图的单源最短路径的C++ 实现，以[POJ 2387:Til the Cows Come Home](http://bailian.openjudge.cn/practice/2387/) 为例AC。

### Bellman-Ford算法
采用动态规划思想的解法。要求没有负圈，如果有负圈的话就没有解了。
对所有边进行松弛操作，共V−1次，其中 V 是图节点的数量。时间复杂度O(VE)。V和E分别是节点和边的数量。

另外可以用SPFA算法进行优化，[因为松弛操作必定只会发生在最短路径前导节点松弛成功过的节点上，用一个队列记录松弛过的节点，可以避免了冗余计算。](https://zh.wikipedia.org/wiki/%E8%B4%9D%E5%B0%94%E6%9B%BC-%E7%A6%8F%E7%89%B9%E7%AE%97%E6%B3%95)

考虑：为什么要循环V-1次？
[参考](http://www.wutianqi.com/?p=1912)

因为最短路径肯定是个简单路径，不可能包含回路的，
如果包含回路，且回路的权值和为正的，那么去掉这个回路，可以得到更短的路径
如果回路的权值是负的，那么肯定没有解了
图有n个点，又不能有回路
所以最短路径最多n-1边
又因为每次循环，至少relax一边
所以最多n-1次就行了

```cpp
#include <iostream>
#include <vector>

using namespace std;

class Edge {
public:
    int u, v, weight;
    Edge(int u, int v, int w)
    :u(u), v(v), weight(w) {}
};

int BellmanFord(vector<Edge*> edges, vector<int>& distance, vector<int>& predecessor, int N, int T) {
    for (int i = 1; i <= N - 1; i++) {
        bool flag = 0;
        for (int j = 1; j <= T; j++) {
            int u = edges[j]->u, v = edges[j]->v, weight = edges[j]->weight;

            // 此处写两次是因为 POJ 2387 是无向图
            
            if (distance[u] > distance[v] + weight) {
                distance[u] = distance[v] + weight;
                flag = 1;
            }
            if (distance[v] > distance[u] + weight) {
                distance[v] = distance[u] + weight;
                flag = 1;
            }
        }
        // flag=0表示一次遍历中没有松弛操作，提前直接退出
        if (!flag) break;
    }
    // 检查是否有负圈。但是在 POJ 2387 中加入以下代码无法AC？
//    for (Edge *edge : edges) {
//        int u = edge->u, v = edge->v, weight = edge->weight;
//        if (distance[u] > distance[v] + weight || distance[v] > distance[u] + weight) {
//            return 0;
//        }
//    }
    return 1;
}
// 假设问题是从节点1到N的最短路径
int main () {
    int N, T;
    cin >> T >> N;

    vector<Edge*> edges(T + 1);
    // 注意这个地方的初始化，如果初始化为INT_MAX，会出现加法溢出。POJ 2387中写的1-100范围有问题可忽略
    vector<int> distance(N + 1, 1e9), predecessor(N + 1);

    distance[1] = 0;

    for (int i = 1; i <= T; i++) {
        int u, v, weight;
        cin >> u >> v >> weight;
        edges[i] = new Edge(u, v, weight);
    }

    if (BellmanFord(edges, distance, predecessor, N, T)) {
        cout << distance[N] << endl;
    }

    return 0;
}
```

### Dijkstra算法
采用贪心思想的解法。不过条件要求比Bellman-Ford算法更严格一点，要求没有负边。从这也能看出贪心和动规的相似性。
另外，当图中所有的边的权值都为1时，Dijkstra算法其实就退化成 广度优先搜索 了。
一般情况下，对于每一个贪心算法，几乎总是有一个对应的动态规划解，当然这种情况下，贪心的效率会更高一些。\\
每次选取未被处理的具有最小权值的节点，然后对它的出边进行松弛操作。因而需要一个最小优先队列来保存节点集合。
一开始想直接用`priority_queue`, 但是之后发现这个C++容器不能遍历，故不可用。

用数组实现优先队列的Dijkstra解法如下。用数组实现的话，时间复杂度O(N^2)。可以用二叉堆，二项式堆以及斐波拉契堆优化，参看[卜东波老师的课件](http://bioinfo.ict.ac.cn/~dbu/AlgorithmCourses/Lectures/Lec7-Heap.pdf)。不过卜老师讲完一整节斐波拉契堆之后也说...这个数据结构比较复杂，一般情况下不建议用哈哈。
另外还有如何处理负边的情况，对每一个边加上一个权值。。。

```cpp
// POJ有点恶心的就是WA之后没有任何提示，以下的代码始终无法AC。。。
// 参考可以AC的 http://blog.csdn.net/zxy_snow/article/details/6108418
#include <iostream>
#include <vector>

const int maxint = 1000000;

using namespace std;
void Dijkstra(vector<vector<int>> graph, vector<int>& distance, vector<int>& prev, int N, int T) {
    // 判断是否已存入该点到S集合中，其中源节点到S集合中的每个结点之间的最短路径已经被找到。
    vector<bool> s(N + 1, 0);

    // 外层循环总共添加N次节点
    for (int i = 1; i <= N; i++) {
        // 从结点集V-S中选取最短路径估计最小的结点u加入S
        int u = 1, dist = maxint;
        for (int j = 1; j <= N; j++) {
            if (!s[j] && distance[j] < dist) {
                u = j;
                dist = distance[j];
            }
        }
        // 将u加入S集合
        s[u] = 1;
        // 对所有从u出发的边进行松弛
        for (int v = 1; v <= N; v++) {
            if (!s[v] && graph[u][v] != maxint) {
                int newDist = distance[u] + graph[u][v];
                if (distance[v] > newDist) {
                    distance[v] = newDist;
                    prev[v] = u;
                }
            }
        }
    }
}

// distance数组 表示当前点到源点的最短路径
// prev记录当前点的前一个节点
// N是顶点数，T是边数
int main () {
    int N, T;
    cin >> T >> N;

    vector<vector<int>> graph(N + 1, vector<int>(N + 1, maxint));
    vector<int> distance(N + 1, maxint), prev(N + 1);

    distance[1] = 0;

    for (int i = 0; i < T; i++) {
        int x, y;
        cin >> x >> y;
        cin >> graph[x][y];

        graph[y][x] = graph[x][y];
    }
    for (int i = 1; i <= N; i++) {
        graph[i][i] = 0;
    }

    Dijkstra(graph, distance, prev, N, T);

    cout << distance[N] << endl;

    return 0;
}
```

参考：
+ [https://zh.wikipedia.org/wiki/贝尔曼-福特算法](https://zh.wikipedia.org/wiki/%E8%B4%9D%E5%B0%94%E6%9B%BC-%E7%A6%8F%E7%89%B9%E7%AE%97%E6%B3%95)
+ [https://zh.wikipedia.org/wiki/戴克斯特拉算法](https://zh.wikipedia.org/wiki/%E6%88%B4%E5%85%8B%E6%96%AF%E7%89%B9%E6%8B%89%E7%AE%97%E6%B3%95)
+ 算法导论第24章