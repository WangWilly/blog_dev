title: Algorithm - Graph algorithm
author: Willy Wang (willywangkaa)
tags:
  - Graph
categories:
  - Algorithm
date: 2018-10-15 16:51:00
---
# Graph algorithm



## Depth first search



- 所需變數：考慮圖中一點 u
  - color(u)：目前 u 節點的狀態
    - White：初始值，尚未訪查過
    - Gray：已被訪查過，但未訪查完其子節點
    - Black：已經訪查完其子節點
  - d(u)：發現時間點( Discover time )，即為第一次被訪查的時間點
  - f(u)：完結時間點( Finish time )，即為訪查完其子節點的時間點



- 演算法 (P.4-6)
  - **DFS(G)**：自 G 中任一點開始做 DFS
    - dfs_visit(u)：訪查 u 節點



```cpp
DFS(G)
{
	// initialize
	foreach u in G.V          // G 的點集合
		color(u) <- white
	time <- 0                 // 紀錄目前的時間

	foreach u in G.V
		if(color(u) == white) // 找到一個尚未被訪查過的節點
			dfs_visit(u)
}

dfs_visit(u)
{
	color(u) <- gray
	d(u) <- ++time

	foreach v in adj(u)
		if(color(v) == white)
			dfs_visit(v)

	color(u) <- black
	time++
	f(u) <- time
}
```



- Time complexity
  - $\Theta(\vert V\vert+\vert E\vert)$ ：點數加上邊數

> **將整個圖的所有點與邊訪查一次。**



### 應用



#### DFS tree - Edge



深度優先搜尋時，可以將圖中的邊分成四類：

1. **Tree edge**：u 透過 (u, v) 邊以訪查 v 節點。

> - 可在訪查的過程表示為一個「DFS tree」。

2. **Back edge**：不為「Tree edge」，而在「DFS tree」上由**子孫節點至祖先節點**的邊。

> - **自旋邊( Self loop )亦為「Back edge」**。

3. **Forward edge**：不為「Tree edge」，而在「DFS tree」上由**祖先節點至子孫節點**的邊。
4. **Cross edge**：無祖孫關係的邊。



在實現中，通常在做 DFS 時就會直接以**節點的狀態**判斷邊的種類。



1. **Tree edge**

$$
u_{灰} \rightarrow_{Tree–edge} v_{白}
$$

2. **Back edge**

$$
u_{灰} \rightarrow_{Back–edge} v_{灰}
$$

3. **Forward edge**

$$
u_{灰} \rightarrow_{Forward–edge} ｛v_{黑} \;＆＆ \; d(u)<d(v) ｝
$$


![forwardedge](\willywangkaa\images\forwardedge.png)



4. **Cross edge**

$$
u_{灰} \rightarrow_{Cross–edge} ｛v_{黑} \;＆＆ \; d(v)<d(u) ｝
$$

![crossedge](\willywangkaa\images\crossedge.png)

- Ex (96 年台大資工)



```cpp
DFS(G)
{
	// initialize
	foreach u in G.V          // G 的點集合
		color(u) <- white
		edge(u) <- Null       // DFS tree 初始化邊( 雙向邊 )
	time <- 0                 // 紀錄目前的時間

	foreach u in G.V
		if(color(u) == white) // 找到一個尚未被訪查過的節點
			dfs_visit(u)
}

dfs_visit(u)
{
	color(u) <- gray
	d(u) <- ++time

	foreach v in adj(u)
		if(color(v) == white)
			edge(v) <- u                     // 相連節點設為 u
            edge(v).attribute <- tree
			dfs_visit(v)
		else if(color(v) == gray)
			edge(v) <- u                     // 相連節點設為 u
            parent(v).attribute <- back
			dfs_visit(v)
		else                                 // 黑節點
			edge(v) <- u                     // 相連節點設為 u
			if(d(u) > d(v))                  // 無祖孫關係
				edge(v).attribute <- cross
            else if(d(u) < d(v))             // 有祖孫關係
				edge(v).attribute <- forward
			dfs_visit(v)
	color(u) <- black
	time++
	f(u) <- time
}
```



#### DFS tree - Acyclic



> **存在「Back edge」時，代表有迴圈產生。**



- 演算法 ( 94台大 ) ( 99台大資工 )

```cpp
Acyclic(G)
{
	// initialize
	foreach u in G.V          // G 的點集合
		color(u) <- white
	time <- 0                 // 紀錄目前的時間

	foreach u in G.V
		if(color(u) == white) // 找到一個尚未被訪查過的節點
			dfs_visit(u)
	return acyclic
}

dfs_visit(u)
{
	color(u) <- gray
	d(u) <- ++time

	foreach v in adj(u)
		if(color(v) == white)
			dfs_visit(v)
		else if(color(v) == gray) // back-edge is found
			return G is cyclic

	color(u) <- black
	time++
	f(u) <- time
}
```



- Time complexity
  - **有向圖** (94 台大)
    - $\Theta(V+E)$
  - **無向圖** (99 台大)
    - $\Theta(V)$

> **在無向圖中，因為鴿籠原理，最多只要檢查 V 個邊即可知道此圖是否為「Cyclic」。( 且如果邊的數量大於點的數量也根據鴿籠原理，此圖必含有迴圈 )** $\therefore \Theta(V+V) = \Theta(V)$
>
> **另外，在無向圖中只有**
>
> - **Tree edge**
> - **Back edge**

> 但是，在**有向圖**則不能以上述判斷。
>
> - Ex
>   - 兩個節點：A、B
>   - 五條邊皆從 A 節點至 B 節點
>   - **此圖不包含「Cycle」**



## Minimum spainning tree



### 問題探討



- 假設 (u, v) 為 G 中「權重 ( weight )」最小的邊，則 (u, v) 必在 G 的某一個「最小生成樹」中。

( **反證法** )


![minimumspainingtree_1](\willywangkaa\images\minimumspainingtree_1.png)
$$
假設 \;\forall T \in MST \;of \;G \quad edge\;(u, v) \notin T.E \\
將 \;(u, v)\; 加至 \; T \; ，會使\;T \;會產生一個 \;cycle \; C \\
令 \;e\; 為\;C\;中除了 \;(u, v)\; 之外權重最大的邊 \\
將 \;e\; 自\;C\;移除，使 \;C\; 會形成一個新的生成樹 \;T' \\
且 \;weight(T') \leq weight(T)\; 產生了一個更小的生成樹，\\
所以一開始假設的生成樹並非為最小生成樹。(矛盾)
$$

> 上述情況會有兩種矛盾的現象產生
>
> - 當 $Weight(T')<Weight(T)$ 時，代表原本的生成樹不為最小生成樹。
> - 當 $Weight(T') = Weight(T)$ 時，代表該最小生成樹的集合中，必有一個樹包含 (u, v) 邊。



> 定義：「Cycle」由三條邊組成的迴路。

- ☆ 假設 (u, v) 為 G 中「權重 ( weight )」**第二小**的邊，則 (u, v) 必在 G 的某一個「最小生成樹」中。

( **反證法** )

> 前提為該圖 G 各個權重不相等否則反例如下：
>
> 
![minimumspainingtree_2](\willywangkaa\images\minimumspainingtree_2.png)
$$
假設 \;\forall T \in MST \;of \;G \quad edge\;(u, v) \notin T.E \\
將 \;(u, v)\; 加至 \; T \; ，會使\;T \;會產生一個 \;cycle \; C \\
\because 一個「Cycle」一定由三個邊組成 \\
\therefore C \;中必有一個邊 \;e \;，其權重"大於等於" (u, v) 的權重 ....(*) \\
將 \;e\; 自\;C\;移除，使 \;C\; 會形成一個新的生成樹 \;T' \\
且 \;weight(T') \leq weight(T)\; 產生了一個更小的生成樹，\\
所以一開始假設的生成樹並非為最小生成樹。(矛盾)
$$


> G 中**權重第三小**的邊不一定存在於 G 的某一個「最小生成樹」中
>
> - Counter example
>
>
![minimumspainingtree_3](\willywangkaa\images\minimumspainingtree_3.png)



#### Kruskal's algorithm 的正確性



- 概念
  - T：使用 Kruskal 找出的生成樹
  - T'：真正的最小生成樹

1. 若 $T = T'$ ，得證。
2. 若 $T \ne T'$ ，T 必有邊不包含在 T' 之中，所以在 T.E - T'.E ( 差集合 ) 中挑一個權重最小的邊 e。

將 e 加入 T' 後，會形成一個 Cycle 「C」，
**在 {C.V - e} 中任取一邊 e' 必會大於等於 e，**
**因為如果該邊比較小，在 Kruskal 執行時必優先選擇 e'** ( 然而如此一來導致矛盾所以不可能發生 )，
最後我們可以造一個生成樹 T'' = ( G.V, **(T'.E - {e'})** $\cup$ {e} )，
且 $Weight(T'') \leq weight(T')$ ；

重複上述步驟 $T' \xrightarrow{用\;T\;有而\;T'\;沒有的邊做替換} T'' \xrightarrow{用\;T\;有而\;T''\;沒有的邊做替換} ... \rightarrow T$ ，
並且其過程中生成樹的權重不會增加，所以可以證明 $weight(T') = weight(T)$，
所以 T 是最小生成樹。



## Flow network



一個**有向圖** G = (V, E) 滿足

1. 只含有唯一一個 **in-degree = 0** 的點 S ( Source )
2. 只含有唯一一個 **out-degree = 0** 的點 T ( Sink )
3. 對於 G.E 中每個邊 e ，定義一個**容量** c(e) ≧ 0



另外，定義一個**流量**的函數：f(e)，滿足

1. 對每個點而言，**流入水量等於流出水量。**
2. 對每個邊而言，**0 ≦ f(e) ≦ c(e)**



### Max flow problem



- Input
  - 一個「Flow network」 G = (V, E)
- Output
  - 此網路的最大流量，也就是源頭最大能流出多少水量



![flownetwork](\willywangkaa\images\flownetwork.png)



#### Ford-Fulkerson algorithm



1. 使用**「Residual network」**表示法
2. 自 s → t 找一條路徑 P，令 a 為 P 上最小的權重
   - $\overrightarrow{P}$ 上的每一邊 ← 權重 **- a**
   - $\overleftarrow{P}$ 上的每一邊 ← 權重 **+ a**
   - 重複此操作直到找不到最小的權重 a 可以做上述動作。
3. 將最後指向 S 的邊之權重和輸出



- Time complexity
  - $O(|f^＊|\cdot E)$
    - $|f^＊|$ 為最大流量的值
    - **效能差**

> **如果每次在第二個步驟找到的最小流量 a 為 1，那勢必要重複** $|f^＊|$ **來找到最後的答案。**



> 可以利用「已經求取完最大流量」的 Residual network 得到「原本流量網路」的 Minimum cut。
>
> - Minimum cut：有 (S, T) 兩個子圖，滿足
>   - S $\cup$ T = V 且 S $\cap$ T = $\phi$
>   - 從子圖 S 連至子圖 T 之所有邊的權重和為所有「Cut」中最小
>
> 在求完最大流量的「Residual network」中，令
>
> - S：｛自 S 可以到達的節點｝
> - T：｛自 T 可以到達的節點｝
>
> 則 (S, T) 是原本「Flow network」的「Minimum cut」



#### Edmond-Karp algorithm



1. 使用**「Residual network」**表示法
2. 自 s 做 **Breadth first search** ，**當找到 t 時建立一條 s → t 路徑 P，令 a 為 P 上最小的權重**
   - $\overrightarrow{P}$ 上的每一邊 ← 權重 **- a**
   - $\overleftarrow{P}$ 上的每一邊 ← 權重 **+ a**
   - 重複此操作直到找不到最小的權重 a 可以做上述動作。
3. 將最後指向 S 的邊之權重和輸出



- Time complexity
  - $O(VE^2)$：**只和網路的大小有關**



Example (P.4-80 ex20)

**Escape problem**


![escapeproblem](\willywangkaa\images\escapeproblem.png)



將問題轉換成「邊與點均有 **Capacity** 的最大流量問題」 ( 將問題「Reduce」至「Flow network」中解決 )

![escapeproblem_sol](\willywangkaa\images\escapeproblem_sol.png)



給定一個「Escape problem」的格子圖 ( Grid；有 m 個**起點**與 4n - 4 個**出口** )



建立「Flow network」G = ( V, E )：

1. 建立一個「Source」節點 S 並與逃生問題中的**起點** ( 藍色節點 ) 相接。
2. 建立一個「Sink」節點 T 並與逃生問題中的**邊界點** ( 黃色區塊 ) 的點相接。
3. 「Grid」上的每一條無向邊 ( u, v ) ，在 G 中建立相對應的有向邊 ( u, v ) 與 ( v, u )。
4. 將每一個節點與邊的流量接設定為 1。



若在 G 中能找到最大流量為 m，則亦可以在「Grid」中找到一個逃生的方法。



> 點與邊均有「Capacity」 的流量網路可以用傳統的流量網路實現。
>
> 
![escapeproblem_solconti](\willywangkaa\images\escapeproblem_solconti.png)





## 其它問題



- 問題的要求即使有小變化，可能使其難度改變很大
  - Shortest path problem (Polynominal)與 Longest path problem (Non-deterministic polynominal)
  - Minimum cut (Polyniminal) 與 Maximum Cut (Non-deterministic polynominal)
  - Euler circuit (P) 與 Hamiltan Cycle (NP-Complete)
- 同一個問題，若給的環境不同，難度亦可能相差很多

|                          | Graph | Tree                    |
| ------------------------ | ----- | ----------------------- |
| Longest path problem     | NPC   | Linear time (P4-66 ex6) |
| **Minimum vertex cover** | NPC   | Linear time (P)         |

> Vertex cover：會和圖上所有邊相連的一個**點集合**
>
> 下圖的「Vertex cover」為｛b, d, f｝
>
> 
![vertexcover](\willywangkaa\images\vertexcover.png)