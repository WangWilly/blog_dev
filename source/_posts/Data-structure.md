title: Data structure - Graph
author: Willy Wang (willywangkaa)
tags:
  - Graph
categories:
  - Data structure
date: 2018-12-02 19:41:00
---
# The structure of graph



## [Note] Adjacency list



有一圖 G = (V, E), |V| = n, |E| = e，需要 n 條相鄰串列（vertex[1...n] : pointer）來表達，其中 vertex[i] 代表頂點 i 的相鄰串列，紀錄所有與頂點 i 相鄰的頂點編號



- 以「相鄰串列」表示無向圖時，因為 邊(i,j) = 邊(j,i)（一條邊有兩個頂點），所以串列需要 2×e 個節點來儲存這張圖
  - **當圖的邊少時，可以取得優勢**



Example（無向圖）

求頂點 i 的「Degree」需要的時間複雜度為？

即為 vertex[i] 的串列長度，所以需要的時間複雜度為 O($V_i$串列長度) ≦ O(e)



Example（無向圖）

求圖上有多少邊的時間複雜度為？

$e = \frac 12\sum_{i = 1}^n (V_i \;Degree)\\ \therefore e = \frac 12 \sum_{i = 1}^n (V_i \;串列長度)\\ = O(n+e)$

> 無向圖上最多可以有 $\binom {n}{2} = \frac{n(n-1)}{2} = \frac{n^2-n}{2}$ 條邊，從上述又可以得知如果要追蹤以「相鄰串列」表示的無向圖上之邊，最多的時間複雜度為 $O(n + \frac{n^2-n}{2}) = O(n^2)$ 



Example

若以「相鄰串列」表示有向圖，要求頂點的「Out-degree」與「In-degree」，分別需要的時間複雜度為？

「Out-degree」：vertex[i] 的串列長度，所以也為 O(e)

「In-degree」：因為必需追蹤所有點以找到指向 i 的頂點為何，所以全部的串列都要確認，時間複雜度為 $O(n^2)$



## 相鄰串列與相鄰陣列比較表



|                              | Adjacency matrix                     | Adjacency list            |
| ---------------------------- | ------------------------------------ | ------------------------- |
| 空間需求                     | $O(n^2)$                             | **O(n+e)**                |
| 表示頂點數多邊數少之圖       | 需要$O(n^2)$且為**稀疏矩陣**，不適合 | 適合                      |
| 表示邊數多（e=O$(n^2)$）之圖 | 適合                                 | 需要$O(n+O(n^2))$，不適合 |
| 判斷兩頂點間是否存在邊       | 時間複雜度 O(1)，適合                | 時間複雜度 O(e)，不適合   |
| 求圖上的邊數（判斷連通...）  | 時間複雜度 $O(n^2)$，不適合          | 時間複雜度 O(n+e)，適合   |



## Adjacency multilist（相鄰多元串列）

欲表示圖 G = (V,E), |V| = n, |E| = e

- 每一條**邊**以一個**節點**表示

![1543649110616](\willywangkaa\images\1543649110616.png)

- 指標陣列（vertex[1...n] : pointer）
  - Vertex[i] **指向第一個包含** $V_i$ **的邊節點**

![1543649696312](\willywangkaa\images\1543649696312.png)

- 空間複雜度：O(n+e)



## Index array

欲表示圖 G = (V,E), |V| = n, |E| = e

- 一個一維陣列（Array[1... 2×e]）**紀錄所有點之相鄰頂點編號**

- 一個一維陣列為「Index」（Index[1...n]）表示頂點 i 在 Array 中紀錄的**起始位置**

![1543650099846](\willywangkaa\images\1543650099846.png)



- 空間複雜度：O(n+e)



## Incident matrix （離散數學）

欲表示圖 G = (V,E), |V| = n, |E| = e

- 一個二維陣列（A[1...n, 1...e] : boolean）
  - 若圖中存在邊 $e_k$=(i,j)，則 A[i, $e_k$]=A[j, $e_k$]=1，其餘為 0

![1543650560718](\willywangkaa\images\1543650560718.png)



# 圖的追蹤

G = (V,E), |V| = n, |E| = e

- 比較表
  - 在演算法書籍中，使用「Adjacency list」作深度追蹤與廣度追蹤時，**時間複雜度分析考慮了 visit[1...n] 陣列初值的設定**，所以為 O(n+e)，而資料結構書籍則無考慮

|                                           | DFS        | BFS        |
| ----------------------------------------- | ---------- | ---------- |
| 佐結構                                    | Stack      | Queue      |
| Adjacency Matrix（時間複雜度）            | $O(n^2)$   | $O(n^2)$   |
| Adjacency List（時間複雜度）[DS版]        | O(e)       | O(e)       |
| Adjacency List（時間複雜度）[Algorithm版] | **O(n+e)** | **O(n+e)** |



## DS 書籍版本



### Depth first search

> 以無項圖為主，此演算法使用「相鄰矩陣」

> 深度優先追蹤法以「Stack」實作，所以使用遞迴法實現



- Algorithm

```cpp
// Global variable:
//                    G: adjacency matrix
//          visit[1..n]: 頂點搜尋紀錄，且全部元素已經重設為 false
// Parameter:
//          v: 起始頂點
void DFS(v) {
    visit[v] = true;
    for(int w = 0; w <= n; w++) {
        if(G[v,w])
            if(!visit[w])
                DFS(w);
    }
}
```

> - Depth first search 的路線並不唯一
> - 通常頂點編號小者優先走訪，考試時路線才會唯一



### Breath first search

> 廣度優先追蹤以「Queue」實作
>



- Algorithm

```cpp
// Global variable:
//                    G: adjacency matrix
//          visit[1..n]: 頂點搜尋紀錄，且全部元素已經重設為 false
// Parameter:
//          v: 起始頂點
int BFS(v) {
    visit[v] = true;
    CreateQueue(q);
    Enqueue(q, v);
    while(!q.empty()) {
        u = Dequeue(q);
        for(int w = 0; w <= n; w++) {
            if(G[u,w])
                if(!visit[w]){
                    visit[w] = true;
                    Enqueue(q, w);
                }
        }
    }
}
```



![1543651440091](\willywangkaa\images\1543651440091.png)



> 無向圖二元樹
>
>![1543651513624](\willywangkaa\images\1543651513624.png)
>
> - (見上圖) 在圖中執行 DFS(1) 類似在二元樹中作「前序追蹤」
> - (見上圖) 在圖中執行 BFS(1) 類似在二元樹中作「層序追蹤」



Example （實現「Level-order」追蹤演算法）

```cpp
// v: 根節點
void Level_order(v) {
    print(v);
    Createqueue(q);
    Enqueue(q, v);
    while(!q.empty()) {
        u = Dequeue(q);
        if(u->lchild != null) {
            print(u->lchild);
            Enqueue(q, u->lchild);
        }
        if(u->rchild != null) {
            print(u->rchild);
            Enqueue(q, u->rchild);
        }
    }
}
```



## Algorithm 書籍版本



### Depth first search



- Algorithm
  - u.Color
    - White：尚未拜訪
    - Gray：已拜訪，但尚未拜訪完相鄰的頂點
    - Black：拜訪完相鄰的頂點
  - u.d
    - 從起點拜訪 u 點的時間
  - u.f
    - u 點拜訪完所有相鄰頂點的時間
  - u.parent
    - 在 DFS 拜訪樹中，為 u 的父節點

```cpp
// time: Global variable 用來記錄追蹤的路徑

int DFS_visit(G, u) {
    time++;
    u.color  = gray;
    u.d      = time;
    for each w in G.adj[u] {
        if(w==white) {
            w.parent = u;
            DFS_visit(G, w);
        }
    }
    time++;
    u.color = black;
    u.f     = time;
}

int DFS(G) {
    // O(|V|)
    // Initialize
    for each u in G.V {
        u.color  = white;
        u.parent = null;
    }
    time = 0;
    for each u in G.V {
        if(u.color==white) {
            DFS_visit(G, u);
        }
    }
}
```



#### 有向圖

使用 DFS 追蹤時，會將圖分為四種邊（詳見演算法文章中的圖論）

1. Tree edge：DFS 拜訪經過的邊
2. Back edge：指向祖先的邊
3. Forward edge：指向後代的邊
4. Cross edge：**跨不同子樹的邊**

> 在無向圖上，使用 DFS 追蹤只會將圖分成
>
> 1. Tree edge
> 2. Back edge



### Breath first search



- Algorithm
  - u.Color
    - White：尚未拜訪
    - Gray：已拜訪，但尚未拜訪完相鄰的頂點
    - Black：拜訪完相鄰的頂點
  - u.d
    - 從起點到 u 點路徑的邊數（深度）
  - u.parent
    - 在 BFS 拜訪樹中，為 u 的父節點

```cpp
// G: adjacency list
// s: 起始頂點
BFS(G, s) {
    // Time complexity: O(|v|)
    // Initialize
    // G.V: Vertex set for G
    for each u in G.V-{s} {
        u.color  = white;
        u.d      = oo;
        u.parent = null;
    }
    s.color  = gray;
    s.d      = 0;
    s.parent = null;
    CreateQueue(q);
    Enqueue(q, s);
    while(!q.empty()) {
        u = Dequeue(q);                    // Line A
        // G.adj[u]: Adjacency list for G
        for each w in G.adj[u] {           // ↑
            if(w.color==while) {           // ︳ Loop A
                w.color  = gray;           // ︳
                w.d      = u.d+1;          // ︳
                w.parent = u;              // ︳
                Enqueue(q, w);             // ︳
            }                              // ↓
        }
        u.color = black;
    }
}
```



- Time complexity
  - Line A：作「Dequeue」需要 O(1)，共有 |V| 個頂點
    - O(1)×|V| = O(|V|)
  - Loop A ：找到每個頂點的相鄰頂點
    - O(2|E|)
  - 總時間複雜度
    - O(|V|+|E|)

> **無向圖若邊上無權重**，求某起點到各頂點之**「最短路徑」**可以使用**廣度優先追蹤之生成樹**即可



## 追蹤的應用



Example

**無向圖 G**，判斷是否有**連通**？

- Algorithm

  1. 使用 DFS（[DS 版]） 或 BFS 追蹤 G

  2. 若 visit 陣列中**有頂點尚未被拜訪**即為**不連通**

- Time complexity

  - 相鄰矩陣：$O(|V|^2) + O(|V|) = O(|V|^2)$
  - 相鄰串列：O(|V|+|E|) + O(V) = O(|V|+|E|)



Example

**有向圖 G**，判斷是否有**環路**？

- Algorithm
  - 用 DFS，若存在「Back edge」即存在環路
- Time complexity
  - 相鄰矩陣：$O(|V|^2)$
  - 相鄰串列：O(|V|+|E|) （當圖上存在「Hamilton cycle」，為最好的情況，因為只要搜尋過所有點即可找到）



**Example**

**無向圖 G**，判斷是否有環路？（找「Back edge」即可）

> **如何判斷「Back edge」？**
>
> - 修改程式（因為無項圖的邊可來回，**為了防止同一邊追蹤兩次**須加上 `w.parent != u`）
>
> ```cpp
> ....
> for each w in G.adj[u] {
>     if(w.color==gray && w.parent!=u) // 為「Back edge」
> ....
> ```



# Spanning tree

> 針對無向圖

- 若 G 為非連通圖，則無「Spanning tree」
- 若 G 為連通圖，則「Spanning tree」≧ 1



**Example （True or False）**

一個連通無向圖 G，則 G 上任意的「Spanning tree」，必定包含 ≧ 1 條「共同的邊」？

**False（見下圖）**

![1543652365094](\willywangkaa\images\1543652365094.png)



## Minimum spanning tree

- 若圖中有多條邊具有相同的值，則「Minimum spanning tree」≧ 1
- 若每條邊權重皆不同則「Minimum spanning tree」唯一



- 應用
  - 電路布局成本最小化
  - 連結 n 城市之最少建設成本



### Kruskal's algorithm [Algorithm 書上版本]

G = (V,E), |V| = n, |E| = e

- 最多作 |E| 回合，而每一回合主要有兩個工作
  1. 挑選最小成本的邊 (u, v)
     - 邊的權重集合若使用「Heap」維護，則時間複雜度則為「Delete min」的成本（O(log |E|)）
  2. 判斷邊 (u, v) 加入到 s 之中是否形成環路（O(1)）
     - 使用一個「Disjoint set」維護「Minimum spanning tree」上的點，在加入新的邊時，使用邊上兩頂點在「Disjoint set」中尋找（`Find(u)==Find(v)?`），如果兩點都存在於**同一個集合**中，則形成環路，不加入該邊，反之將該邊加入（`Union(u,v)`）「Minimum spanning tree」中
     - 若「Disjoint set」採用「Union by height」與「Find with path compression」，則 `Union(u,v)` `Find(u)` `Find(v)` 需要的時間複雜度為 O(1)
- Time complexity
  - **O( |E|×log|E|)**



- Algorithm

```cpp
Mst_kruskal(G, W) {
    S = ∅; // MST邊集合
    D = ∅; // Disjoint set for vertex
    for each vertex x in G.V {           //  ↑
        D.Make_set(x);                   //  | O(|V|)
    }                                    //  ↓
    Sort(G.E); // 針對權重由小到大作排序           O(|E|log|E|)
    // 由小到大取邊
    for each (u,v) in G.E {              //  ↑
        if(D.Find(u)!=Find(v)) {         //  |
            S = S ∪ {(u,v)};             //  | O(|E|)
            D.Union(u,v);                //  |
        }                                //  |
    }                                    //  ↓
    return S;
}
```



- Time complexity
  - O(|V|) + O(|E|log|E|)+O(|E|) = O(|E|log|E|)
  - 因為 |E| ≦ |V|×|V|，所以 **O(|E|log|E|) = O(|E|log|**$V^2$**|) = O(|E|log|V|)**



### Prim's algorithm [DS 書上版本]



- Time complexity
  - O(||)



- Algorithm

```cpp
// G: graph
// W: weight of G.E
// r: 起點
Mst_prim(G, W, r) {
    for each u in G.V {             //   ↑
        u.key = ∞;                  //   ︳  Initialize
        u.parent = null;            //   ︳  O(|v|)
    }                               //   ↓

    r.key = 0;
    q = Build_priorityqueue(G.V);   // 可以使用「Binary heap」或「Fib. heap」，
                                    // O(|V|)
    while(q != ∅) {
        u = Extract_min(q);         //   ↑  LineA:O(log|V|)
        for each v in G.adj[u] {    //   ︳
            if(v.key > W[u,v]) {    //   ︳ LoopA
                v.parient = u;      //   ︳
                v.key     = W[u,v]; //   ︳ LineB:不斷更新「Prioryty queue」的鍵值
            }                       //   ︳
        }                           //   ︳
    }                               //   ↓
}
```



- Time complexity （用「Binary heap」作為「Priority queue」）

  1. 初值建立 O(|V|)

  2. 建立「Priority queue」 O(|V|)

  3. Line A：`Extract_min(q)` 作一次需要 O(log|V|)

     - 共作 |V| 次，需要 **O(|V|log|V|)**

  4. Loop A：總共會檢視 2 ×|E| 個頂點

     - Line B：`v.key = W[u,v]` 相當於是作「Decrease key of node in heap」

     - 因為為「Binary heap」，所以需要 O(log|V|) 的時間複雜度
     - 迴圈總共的時間複雜度為 **O(|E|log|V|)**

總共需要 O(|V|) + O(|V|) + **O(|V|log|V|) + O(|E|log|V|)** ，**因為 |E|≧ |V|-1 ，所以時間複雜度為 O(|E|log|V|)**

> - 與「Kruskal」的複雜度一致
> - 可以使用「Fibonacci heap」再進行加速



![1543730410752](\willywangkaa\images\1543730410752.png)



- Time complexity （用「Fibonacci heap」作為「Priority queue」）

  1. 初值建立 O(|V|)

  2. 建立「Priority queue」 O(|V|)

  3. Line A：`Extract_min(q)` 作一次需要 O(log|V|)

     - 共作 |V| 次，需要 **O(|V|log|V|)

  4. Loop A：總共會檢視 2 ×|E| 個頂點

     - Line B：`v.key = W[u,v]` 相當於是作「Decrease key of node in heap」

     - 因為為「Fibonacci heap」，所以需要 O(1) 的時間複雜度
     - 迴圈總共的時間複雜度為 O(|E|)

總共需要 O(|V|) + O(|V|) + O(|V|log|V|) + **O(|E|)** ，**因為 |E|≧ |V|-1 ，所以時間複雜度為 O(|V|log|V|+|E|)**



### Sollin's algorithm



步驟：（開始時先將每個頂點視為獨立的數根）

1. 針對每棵樹，各自挑出「Minimum cost tree edge」
2. 刪除重複挑出的邊
3. 重複第一步與第二步直到成為「最小生成樹」



# Shortest path problem

|            | Dijkstra's algorithm                                         | Bellman-ford algorithm             | Folyed-warshall algorihm |
| ---------- | ------------------------------------------------------------ | ---------------------------------- | ------------------------ |
| 欲解問題   | Single source                                                | Single source                      | All pair shortest        |
| 策略       | Greedy                                                       | Dynamic programming                | Dynamic programming      |
| 允許負邊？ | NO                                                           | **YES**                            | **YES**                  |
| 允許負環？ | NO                                                           | NO                                 | NO                       |
| 時間複雜度 | Adjacency matrix: O($V^2$)                                   | Adjacency matrix: O($V^3$)         | O( $V^3$ )               |
|            | Adjacency list<br />Heap：O(\|E\|log\|V\|)<br />Fib. heap：O(\|V\|log\|V\|+\|E\|) | Adjacency list<br />O(\|V\|×\|E\|) |                          |



## Dijkstra's algorithm [DS 書上版本]

適用於有向圖或無向圖皆可以



Data structure

- Cost matrix：為一 n × n 的矩陣，n = |V|

$cost[i, j] = \left\{\begin{matrix} 邊成本值 & ,if \;＜i,j＞ \in E \\ 0 & ,if \; i=j \\ \infty & ,if \;＜i,j＞ \notin E\end{matrix}\right.$ 

- S[1...n]：Boolean

$S[i] = \left\{\begin{matrix}False & , 起點到 \;i \;點的最短距離尚未決定 \\True & , 起點到 \;i \;點的最短距離已決定\end{matrix}\right.$ 

- Dist[1...n]：Integer

Dist[i] ： 起點到 i 點的最短路徑長（初值為「Cost matrix」起點到該點值）



- 概念



![1543730847726](\willywangkaa\images\1543730847726.png)

```cpp
if(Dist[i]>Dist[u]+Cost[u,i]) {
    Dist[i] = Dist[u] + Cost[u,i];
}
```



> 圖中不可有負邊，否則無法求初正確的最短路徑
>
> ![1543731055463](\willywangkaa\images\1543731055463.png)
>
> 以（1）作為起點，使用「Dijkstra's algorithm」則 `Dist[2] = 5`，但其實應為 `Dist[2] = 2`
>
> 不得對所有邊以加權種方式使得負邊為正，因為若有一路徑經過點越多，則該路徑權重也倍數成長，如下：
>
> ![1543731311974](\willywangkaa\images\1543731311974.png)
>
> 在路徑＜1,2＞路徑上只有增加權重 6 ，但是在路徑＜1,3,2＞上路徑增加權重為 12



Example



![1543736203059](\willywangkaa\images\1543736203059.png)



- Algorithm

```cpp
// s: 起點
Initialize(G, s) {              //  ↑
    for each vertex v in G.V {  //  ︳
        v.d = ∞;                //  ︳
        v.parent = null;        //  ︳  O(|V|)
    }                           //  ︳
    s.d = 0;                    //  ︳
}                               //  ↓
Relax(u,v,W) {
    if(v.d > u.d+W[u,v]) {
        v.d = u.d + w[u,v];   // 不斷更新「Fib. heap」的鍵值:O(1)
        v.parent = u;
    } 
}
// W[]: 邊權重集合
Dijkstra(G, W, x) {
    Initialize(G, x);
    S = ∅; // 已確定為最短路徑之點集合
    // 以點的d值建立「Priority queue」（Fib. heap）: O(|V|)
    q = Build_priorityqueue(G.V); 
    
    while(q != ∅) {
        u = extract_min(q);              // O(log|V|)×|V| = O(|V|log|V|)
        S = S ∪ {(u)};
        for each vertex v in G.adj[u] {  //  ↑
            Relax(u, v, W);              //  ︳O(|E|)
        }                                //  ↓
    }
}
```



- Time complexity
  - Binary heap：O(|V|) + O(|V|) + O(|V|log|V|) + **O(|E|log|V|)** = **O(|E|log|V|)**
  - Fibonacci heap：O(|V|) + O(|V|) + O(|V|log|V|) + **O(|E|)** = **O(|V|log|V|+|E|)**
    - 等同於「Prim's algorithm」

> 1. 與「Prim's algorithm」的時間複雜度相同
> 2. 「Prim's algorithm」與「Dijkstra's algorithm」都採用「廣度優先搜尋法」



## Bellman-Ford algorithm



Data structure

- $Dist^k[1...n]$：起點到 i 點的最短路徑長且**經過邊數 ≦ k**
  - $Dist^1$ ：初值，「Cost matrix」之起點到各點的權重值
  - 依序求出 $Dist^2、Dist^3、\ldots、Dist^{n-1}$，且 $Dist^{n-1}$ 即為最後結果



![1543736615438](\willywangkaa\images\1543736615438.png)

$Dist^k[i] = min_{\forall u}｛Dist^{k-1}[i], min｛Dist^{k-1}[u]+Cost[u, i]｝｝$，**u 為有邊指向 i 的頂點**



Example

![1543737721578](\willywangkaa\images\1543737721578.png)



- Algorithm

```cpp
// Cost: cost matrix
// n:    |V|
// s:    起點
BellmanFord(Cost, n, s) {
    for(int i = 1; i <= n; i++) {
        Dist[i] = Cost[s,i];
    }
    
    for(int k = 2; k <= n-1; k++) {                   //       ↑
        tmp_Dist[1...n];                              //       ︳
        for(int i = 1; i <= n; i++) {                 //  ↑    ︳
            for each u that has edge(u,i) {           //  ︳   O(|V|)
                if(Dist[i]>Dist[u]+Cost[u,i]) {       //  ︳   ︳
                    tmp_Dist[i] = Dist[u]+Cost[u,i];  //  ︳   ︳
                } else {                              // O(|E|) 
                    tmp_Dist[i] = Dist[i];            //  ︳   ︳
                }                                     //  ︳   ︳
            }                                         //  ↓    ︳
        }                                             //       ↓
        copy(Dist[1...n], tmp_Dist[1..n]);
    }
}
```



> 採用「Adjacency list」時間複雜度為 O(|V|×|E|) = O(|V|×|$V^2$|)



Example

如何利用「Bellman Ford algorithm」判斷圖上是否有負環？

1. 先執行完一次正常的「Bellman Ford」（求到 $Dist^{n-1}$ 為止）
2. 再求 $Dist^n$，若有值比 $Dist^{n-1}$ 還小，則必存在負環

```cpp
for(int i = 1; i <= n; i++) {
    tmp_Dist[1...n];
    for each u that has edge(u,i) {
        if(Dist[i] > Dist[u]+Cost[u,i]) {
            if(i==n) {
                return "存在負環"; 
            }
            tmp_Dist[i] = Dist[u]+Cost[u,i];
        } else {
            tmp_Dist[i] = Dist[i];
        }
    }
    copy(Dist[1...n], tmp_Dist[1..n]);
}
```



## Floyd-Warshell algorithm



假設 G = ＜V, E＞

Data structure

- $A^k$ ：n × n 矩陣，n = |V|
  - $A^k[i, j]$：i 到 j 之最短路徑且**途中經過頂點的編號必須 ≦ k**
  - $A^0$：原始的「Cost matrix」，依序求出 $A^1, A^2, \ldots, A^n$



概念

$A^k[i,j]= min｛A^{k-1}[i,j], A^{k-1}[i,k]+A^{k-1}[k,j]｝$，針對點（k）作討論，如果使用此頂點作為中介點路徑是否可以縮短

![1543738191981](\willywangkaa\images\1543738191981.png)



Example

![1543739868201](\willywangkaa\images\1543739868201.png)



- Algorithm

```cpp
// Cost[1..n, 1..n]: 邊的權重集合
// n: 頂點數
// A[1..n,1..n]: A^k[1..n,1..n]
FloydWarshell(Cost, n, A) {
    // Initialization
    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= n; j++) 
            A[i,j] = Cost[i,j];
    
    // 求取 A^1, A^2, A^3, ..., A^n
    for(int k = 1; k <= n; k++) {              // ↑
        tmpA[1..n,1..n];                       // ︳
        for(int i = 1; i <= n; i++)            // ︳
            for(int j = 1; j <= n; j++)        //  O(|V|^3)
                if(A[i,j]>A[i,k]+A[k,j])       // ︳
                    tmpA[i,j] = A[i,k]+A[k,j]; // ︳
        copy(tmpA, A);                         // ︳
    }                                          // ↓
}
```



# Activity on vertex network and topological sort



令 G = ＜V,E＞ 為有向圖，若為一個「Activity on vertex network」，則：

1. Vertex：代表「工作」（Activity）
2. Edge：工作之間的**「先後次序關係」**
   - 若（頂點 i）→（頂點 j），代表**「工作（i）必須要在工作（j）之前作完」**



- 應用
  - 判斷**計畫工作先後的執行是否合理**，即至少要有 ≧ 1 組合理的工作順序（使用拓樸排序來判斷）



- Topological sort（Order）
  -  給一個**不具有環路（使用 DFS 檢查是否有「Back edge」）**的「AOV network」，則至少可產生 ≧ 1 組「頂點的拜訪順序」，且若「AOV network 中（i）→（j）」，則最後輸出的拜訪順序中，（i）必定出現在（j）之前



步驟：

1. 找出「In-degree」等於 0 的頂點（i）
2. 輸出該點，並將（i）點指出去的邊從圖中移除
3. 重複第一、第二步直到所有頂點皆已輸出，或是圖上無「In-degree」等於 0 的頂點
4. 確認是否所有頂點皆輸出，若否表示該圖上無法建立一個「Topological sort」（因為該圖上有環路）



> 若該圖不含有環路，則「Topological sort」 ≧ 1 組；反之，則無法建立「Topological sort」



Example

![1543740383474](\willywangkaa\images\1543740383474.png)



- Algorithm [Algorithm 書上版本]

```cpp
int DFS_visit(G, u, p) {
    time++;
    u.color  = gray;
    u.d      = time;
    for each w in G.adj[u] {
        if(w==white) {
            w.parent = u;
            DFS_visit(G, w, p);
        }
    }
    time++;
    u.color = black;
    u.f     = time;
    insertFront(u, p);           // 當一個頂點完成了 DFS 的搜尋，將該頂點插入連結串列的前面
}

void DFS(G, p) {
    // O(|V|)
    // Initialize
    for each u in G.V {
        u.color  = white;
        u.parent = null;
    }
    time = 0;
    for each u in G.V {         //  ↑
        if(u.color==white) {    //  |
            DFS_visit(G, u, p); //  | O(|V|+|E|)
        }                       //  |
    }                           //  ↓
}

ptr Topologicalsort(G) {
    ptr p = null;
    DFS(G, p);
    return p;                   // 回傳建立好的「Topological sort」的鍊結串列
}
```



Example

![1543740792975](\willywangkaa\images\1543740792975.png)



- Time complexity
  - （等同於 DFS 的時間複雜度）O(|V|+|E|)



> **Topological sort（DFS 應用）**
>
> 在「有向圖」上找出「Strongly connected component」
>
> - Algorithm
>
> ```cpp
> int DFS_visit(G, u, p) {
>     time++;
>     u.color  = gray;
>     u.d      = time;
>     for each w in G.adj[u] {
>         if(w==white) {
>             w.parent = u;
>             DFS_visit(G, w, p);
>         }
>     }
>     time++;
>     u.color = black;
>     u.f     = time;
>     insertFront(u, p);           // 當一個頂點完成了 DFS 的搜尋，
>                                  // 將該頂點插入連結串列的前面
> }
> 
> int DFS_visit(G, u) {
>     time++;
>     u.color  = gray;
>     u.d      = time;
>     for each w in G.adj[u] {
>         if(w==white) {
>             w.parent = u;
>             DFS_visit(G, w, p);
>         }
>     }
>     time++;
>     u.color = black;
>     u.f     = time;
> }
> 
> void DFS(G, p) {
>     // O(|V|)
>     // Initialize
>     for each u in G.V {
>         u.color  = white;
>         u.parent = null;
>     }
>     time = 0;
>     for each u in G.V {         //  ↑
>         if(u.color==white) {    //  |
>             DFS_visit(G, u, p); //  | O(|V|+|E|)
>         }                       //  |
>     }                           //  ↓
> }
> 
> graph Stronglyconnectedcomponent(G) {
>     ptr p = null;
>     reverse(G_T, G);                        // 將原本 G 上的邊＜u,v＞，改為邊＜v,u＞
>     DFS(G, p);
>     
>     // Initialize，要對 G_T 作 DFS 的事前準備
>     for each u in G_T.V {
>         u.color  = white;
>         u.parent = null;
>     }
>     time = 0;
>     for(ptr i = p; i->link != null; i = i->link) {  
>                                             // 沿用「Topological sort」的鍊結串列，
>         DFS_visit(G_T, i);                  // 因為此順序恰巧是「Finish time」由大到
>     }                                       // 小排序下來，依照此順序對 G_T 作 DFS
>     
>     return G_T;                             // 在 G_T 上的 DFS forest 即為各個
>                                             // 「Strongly connected component」
> }
> ```
>
>
>
> Example
>
> ![1543741711989](\willywangkaa\images\1543741711989.png)



> **有向無環路圖（Direct acyclic graph）**上求「Single source all shortest paths」
>
> 可以利用「Topological sort」（O(|V|+|E|)）來減少維護「Fibonacci heap」需要的時間複雜度（每回合的Delete-min：O(|V|log|V|)）
>
> - Algorithm
>
> ```cpp
> // s: 起點
> Initialize(G, s) {              //  ↑
>     for each vertex v in G.V {  //  ︳
>         v.d = ∞;                //  ︳
>         v.parent = null;        //  ︳  O(|V|)
>     }                           //  ︳
>     s.d = 0;                    //  ︳
> }                               //  ↓
> 
> Relax(u,v,W) {
>     if(v.d > u.d+W[u,v]) {
>         v.d = u.d + w[u,v];
>         v.parent = u;
>     } 
> }
> 
> // W[1..n,1..n]: 邊權重集合
> // s: 起點
> void DAG_shorestpath(G,W,s) {
>     ptr p = Topologicalsort(G);
>     // Dijkstra's algorithm
>     Initialize(G, s);
>     for(ptr u = p; u->link != null; u = u->link) {
>         for each vertex v in G.adj[u] {
>             Relax(u,v,W)
>         }
>     }
> }
> ```
>
> Example
>
> ![1543742596379](\willywangkaa\images\1543742596379.png)
>
>
>
> - Time complexity
>   - O(|V|+|E|) + O(|V|) + O(|V|) + O(|E|) = O(|V|+|E|)
>   - 「Topological sort」+「Initialization」+「Trace adjacency list」



# Activity on edge network, critical path and critical task

應用於「計畫的專案管理」、CPM（Critical path management）



G = ＜V,E＞ 為有向圖且為「AOE network」，則：

1. Vertex：**代表「事件」（Event）**、「里程碑」（Milestone）、「查核點」
2. Edge：**代表「工作」（Activity）**
   - 權重：**代表該「工作」需要完成的工作時數**



- 所有指向某「事件頂點」的工作全部完成時，該事件才會發生

- 「事件頂點」一旦觸發後，從此頂點指出的「工作邊」才可以開工

![1543742794508](\willywangkaa\images\1543742794508.png)

Example

![1543743405702](\willywangkaa\images\1543743405702.png)

1. 完成計畫**最快**需要幾天？
   - 求 s 至 e 之最常路徑，也就為「Critical path」的長度（24天）

![1543743598858](\willywangkaa\images\1543743598858.png)

2. 列出所有「Critical path」
   - ＜s,e＞路徑長為 24 的皆為「Critical path」

![1543744208911](\willywangkaa\images\1543744208911.png)

3. 列出所有「Critical task」（不可延遲的工作）
   - 在「Critical path」上的**工作邊**｛$a_1, a_6, a_7, a_8, a_{10}, a_{12}$｝



4. 那些工作（瓶頸）的時數縮短後，可有效的縮短整體計畫之工作時數？
   - 所有「Critical path」上的「共同工作」（交集邊）｛$a_1, a_6, a_{12}$｝

![1543744339082](\willywangkaa\images\1543744339082.png)

5. 那些工作可以延遲？可以延遲多久不影響進度？
   - 不在「Critical path」上的工作邊可以有延遲

> 1. 求各個**事件頂點**點之「最早觸發時間」（起點至各點的「最長路徑長」）
>
> | 事件頂點       | s    | t    | u    | v    | w    | x    | e    |
> | -------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
> | 最早觸發時間點 | 0    | 8    | 13   | 16   | 19   | 12   | 24   |
> | 最晚觸發時間點 |      |      |      |      |      |      |      |
>
> 2. 求各個**事件頂點**點之「最晚觸發時間」（終點回推各點的最小值）
>
> ![1543745099778](\willywangkaa\images\1543745099778.png)
>
> 綜合以上可以發現事件頂點 x 必定要在第 7 天觸發
>
> 查看指向自己的頂點以確認最晚觸發的時間，也可以確認**該工作邊**是否可以延遲
>
> ![1543747954832](\willywangkaa\images\1543747954832.png)
>
> | 事件頂點       | s    | t    | u    | v    | w    | x      | e    |
> | -------------- | ---- | ---- | ---- | ---- | ---- | ------ | ---- |
> | 最早觸發時間點 | 0    | 8    | 13   | 16   | 19   | **12** | 24   |
> | 最晚觸發時間點 | 0    | 8    | 13   | 16   | 19   | **14** | 24   |
>
> **事件頂點 x 的觸發**可以延遲
>
> 3. 求各個工作之「最早開工」與「最晚開工」時間點
>    - 最早開工等價於**「需觸發之事件點」**之「最早觸發」時間點
>    - 最晚開工等價於**「欲觸發之事件點」**之「最晚觸發」時間點
>
> ![1543748398923](\willywangkaa\images\1543748398923.png)
>
> | 工作邊         | a1   | a2   | a3   | a4   | a5   | a6   | a7   | a8   | a9   | a10  | a11  | a12  |
> | -------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
> | 最早開工時間點 | 0    | 0    | 0    | 8    | 8    | 8    | 13   | 13   | 12   | 16   | 12   | 19   |
> | 最晚開工時間點 | 0    | 8    | 9    | 10   | 9    | 8    | 13   | 13   | 14   | 16   | 19   | 19   |
>
> 在「Critical path」上的**工作邊**｛$a_1, a_6, a_7, a_8, a_{10}, a_{12}$｝



# Articulation point



Example

![1543748935456](\willywangkaa\images\1543748935456.png)

橘色即為「Articulation point」



## Biconnected graph

一個連通無向圖，且無「Articulation point」，則稱為「Biconnected graph」

- 應用
  - 網路節點設置



- Biconnected component
  - 子圖
  - 連通
  - 為極大的「Biconnected」子圖

![1543749321235](\willywangkaa\images\1543749321235.png)



- 求出「Articulation point」（以上圖為例）

1. 從任意頂點作「深度優先查找」（這裡作DFS(3)），求出各點的「DFN」（DFS拜訪順序）

| 頂點 | 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| DFN  | 3    | 2    | 4    | 1    | 5    | 6    | 7    | 8    | 9    | 10   |
| LOW  |      |      |      |      |      |      |      |      |      |      |

畫出「DFS spanning tree」並標出「Back edge」

![1543749964256](\willywangkaa\images\1543749964256.png)

2. 求個頂點的「low」值
   - $low(x) = min｛dfn(x), ｛dfn(y)|為x的後代頂點最多經過一條「Back\;edge」所到之頂點｝｝$

| 頂點 | 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| DFN  | 3    | 2    | 4    | 1    | 5    | 6    | 7    | 8    | 9    | 10   |
| LOW  | 3    | 1    | 1    | 1    | 1    | 6    | 6    | 6    | 9    | 10   |

3. 判斷「Articulation point」
   - 針對**樹根頂點**：若有 ≧ 2 個子頂點，則必為「Articulation point」
     - （3）有 ≧ 2 個子頂點，所以為「Articulation point」
   - 針對**非樹根頂點**：任意一個 x 之**子頂點 y（非所有後代頂點）**只要符合`low(y) >= dfn(x)`，則為「Articulation point」
     - （1）之子頂點（0）：`dfn(1)<=low(0)`，為「Articulation point」
     - （2）之子頂點（4）：`dfn(4)>low(2)`，非「Articulation point」
     - （5）之子頂點（6）：`dfn(5)>low(6)`，非「Articulation point」
     - （6）之子頂點（7）：`dfn(6)>low(7)`，非「Articulation point」
     - （7）之子頂點（8）、（9）：`dfn(7)>low(8)&&dfn(7)>low(9)`，非「Articulation point」
   - 針對**葉頂點**：皆非「Articulation point」
     - （0）、（4）、（8）、（9）