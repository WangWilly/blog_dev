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



「流量網路圖論模型」為一**有向圖** G = (V, E) 滿足：

- 含有唯一「**in-degree = 0**」的點 S（Source；源點）

- 含有唯一「**out-degree = 0**」的點 T（Sink；匯點）

- 對於 (u,v) ∈ E
  - 定義**容量 c：**$E→R^+$
    - 通常其加權有向邊 (u,v) 代表其**容量**
  - 定義**流量 f：**$E → R$
    - **每個點流入水量等於流出水量**
  - 容量限制（Capacity constraints）
    - $\forall(u,v )\in  E \Rightarrow f(u,v)\leq c(u,v)$



假設一流量網路 G=(V,E)，其「殘餘網路」（Residual network）為

- 有向圖 $G_f = (V, E_f)$
  - $E_f = ｛(u,v)︱^{（1）}0<f(u,v)<c(u,v) \;或\; ^{（2）}f(u,v)<0｝$
  - 上述意旨 $(u,v)\in E_f$ 代表**其邊「尚有容量可以自 u 至 v」**
    - 假設 $f(u,v)>0$ 代表有流量自 u 至 v，則其邊有容量的可能有二
      - **（1）其流量尚未達到容量（**$f(u,v) < c(u,v)$**）**
        - 殘餘容量：$c_f(u,v) = c(u,v)-f(u,v)$
      - **（2）流量可以使其產生「回推容量」**
        - 因為 $f(u,v)$ 代表**「u 至 v 有流量通過」**
          - 則**「v 至 u」**可將通過的流量**回推**，進而產生**容量**
        - 殘餘容量：$c_f(v,u) = f(u,v) = -f(v,u)$
  - 「流量網路」的邊在「殘餘網路」中**至多會被拆成兩條邊**
    - $∣E_f∣ \leq 2∣E∣$
- 假設在 $G_f$ 中，**從「源點」至「匯點」存在一條路徑**
  - **代表「源點」至「匯點」尚有容量**
  - 假設其路徑中可以產生最小流量 $f'$
    - 原流量網路的流量可增加為 $f+f'$

> **增廣路徑定裡**
>
> - **「一個網路達到最大流」**$\Leftrightarrow$ **「其殘餘網路中『源點』至『匯點』沒有路徑」**



### Max flow problem



- Input
  - 一個「Flow network」 G = (V, E)
- Output
  - 此網路的最大流量（源點最大流至匯點的水量）



![flownetwork](\willywangkaa\images\flownetwork.png)



#### Ford-Fulkerson algorithm



步驟

1. 先將「流量網路」G 轉換為「殘餘網路」$G_f$
2. 在 $G_f$ 中自 s → t 找一條增廣路徑 p（隨便的方法找一條路徑，如：DFS）
   - 令 f 為 p 上最小的權重
     - $\overrightarrow{P}$ 上每一邊的容量減少 f
     - $\overleftarrow{P}$上的每一邊容量增加 f**（回推容量）**
3. 重複第二步直到找不到增廣路徑 p
4. 指向頂點 s 邊，其容量總和即為「最大流量」



- Algorithm

```cpp
Graph fordfulkerson(Graph G, node s, node t) {
    G_f = residualnetwork(G);
    path = exist_path(G_f,s,t);
    while(path) {
        G_f = add_argumenting_path(G_f, path);
        path = exist_path(G_f,s,t);
    }
    return G_f;
}
```



- Time complexity
  - $O(∣f^＊∣\cdot E)$
    - $∣f^＊∣$ 為最大流量
    - **效能差**

> - **每次在第二個步找到的最小流量 f 為 1**
>   - 每次找增廣路徑的時間複雜度為 O(∣V∣+∣E∣)
>   - **必重複** $∣f^＊∣$ **回合找到最後總流量**
>     - $O(∣f^＊∣\cdot(∣V∣+∣E∣)) = O(∣f^＊∣\cdot E)$
>
> ![1549795349837](\willywangkaa\images\1549795349837.png)





> **利用「已經求取完最大流量」的 Residual network 可得到「原本流量網路」的 Minimum cut**
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



步驟

1. 先將「流量網路」G 轉換為「殘餘網路」$G_f$
2. 自 s 作「**Breadth first search**」至 t 找到路徑 p
   - 令 f 為 p 上最小的權重
     - $\overrightarrow{P}$ 上每一邊的容量減少 f
     - $\overleftarrow{P}$上的每一邊容量增加 f**（回推容量）**
3. 重複第二步直到找不到增廣路徑 p
4. 指向頂點 s 邊，其容量總和即為「最大流量」



- Time complexity
  - 以「Breadth first search」找最多 O(∣V∣∣E∣) 條「增廣路徑」
    - 「鄰接矩陣」：$O(∣V∣^2∣V∣∣E∣) = O(∣V∣^3∣E∣)$
    - 「鄰接串列」：$O((∣V∣+∣E∣)(∣V∣∣E∣)) = O(∣V∣∣E∣^2)$



##### 時間複雜度證明



**證明最短增廣路徑（Argument path）的長度「非遞減」（利用反證法）**

> $G_f = (V, E_f)$：**某次增廣前的「殘餘網路圖」**
>
> $G_{f'} = (V, E_{f'})$：**某次增廣後的「殘餘網路圖」**
>
> - $\forall x \in V$
>   - $\delta_f (s, x)$ 為源點 s 到 x 頂點在 $G_f$ **最短路徑**
>   - $\delta_{f'} (s, x)$ 為源點 s 到 x 頂點在 $G_f$ **最短路徑**

1. 假設 v 頂點是「在某次增廣後 $\delta_{f} (s, v)$ 長度變小的頂點」
   - $\Rightarrow \delta_f(s,v) > \delta_{f'}(s,v)$

2. 假設 u 頂點為最短路徑 $\delta_{f'}(s,v)$ 裡 v 的前一個頂點（**在該次增廣後其最短路徑有可能變長或不變**）
   - $\Rightarrow \delta_{f'}(s,v) = \delta_{f'}(s,u)+1$
   - 因為「增廣前 s 至 u 的最短路徑」≦「增廣後 s 至 u 的最短路徑」
     - $\Rightarrow \delta_f(s,u) \leq \delta_{f'}(s,u) $
       - 則 $\Rightarrow \delta_f(s,u)+1 \leq \delta_{f'}(s,u)+1 = \delta_{f'}(s,v) < \delta_f(s,v)\\ \Rightarrow \delta_f(s,v)> \delta_f(s,u)+1 \;......（1）$
     - 如果 $(u,v) \in E_f$
       - 由於三角定裡則「s 至 v 的最短路徑」必小於等於「s 至 u 的最短路徑」加上 (u,v) 邊
         - $\Rightarrow \delta_f(s,v) \leq \delta(s,u)+1$
     - 因為（1）則 $(u,v)\notin E_f$
3. 因為 $(u,v) \in E_{f'}$ 且 $(u,v) \notin E_f$
   - 所以可以知道**「此次增廣必經過 (v, u)」**
     - 則「s 至 u 的最短路徑」等於「s 至 v 的最短路徑」+ 1
       - $\Rightarrow \delta_f(s,u) = \delta_f(s,v)+1 \;.....（2）$
   - 將（2）帶入（1）中
     - $\Rightarrow \delta_f(s,v) > (\delta_f(s,v) + 1)+1 $（矛盾）
4. 所以 v 頂點不存在，證明**「最短增廣路徑」長度非遞減**



**證明增廣次數為 O(∣V∣∣E∣)**

> - 在一次增廣中
>   - **定義「Critical edge」為該增廣路徑中容量最小的邊（**若有多條取其一即可）

1. 假設 (u,v) 為 k-th 增廣中的「Critical edge」
   - 則 k-th 增廣後 $(u,v) \notin E_{f'}$
   - 若在 k-th 增廣之後的 i-th 增廣後 (u,v) 再度出現（i > k）
     - 則 i-th 增廣必定沿著 (u,v) 進行增廣
2. 對 k-th 與 i-th 進行討論
   - 在 k-th 增廣前，存在 $\delta_{f^{作 \;k–th \; 之前}}(s,v)+1 = \delta_{f^{作 \;k–th \; 之前}}(s,u)$
   - 在 i-th 增廣前，存在 $\delta_{f^{作 \;i–th \; 之前}}(s,u)+1 = \delta_{f^{作 \;i–th \; 之前}}(s,v)$
   - 然而已知「最短增廣路徑長度非遞減」
     - 故 $\delta_{f^{作 \;k–th \; 之前}}(s,v)+2 \leq \delta_{f^{作 \;i–th \; 之前}}(s,u)$
3. 最短增廣路的長度不超過 ∣V∣-1
   - 因為一條邊為「Critical edge」時，其增廣路徑會不斷累加 2
     - 故一條邊成為「Critical edge」的次數不超過 $\frac{∣V∣-1}{2}$
   - 每次增廣最少有一條「Critical edge」
     - 由定義可以知道 $∣E_f∣ \leq 2∣E∣$ 
   - 所以最差的情況就是每條邊都被選成為增廣路徑
     - $O(\frac{∣V∣-1}{2}\cdot 2∣E∣) =O(∣V∣∣E∣) $



#### Dinic algorithm

「Shortest augmenting path algorithm」改良版，作一次可以找到所有「一樣長的最短擴充路徑」



步驟

1. 計算「剩餘網路」各點到源點（匯點）的最短距離
2. 建立「容許網路」（Admissible network）
   - 尋找「阻塞流」（Blocking flow），並擴充其流量
3. 重覆步驟 1、2 最多 V-1 次，直到無法擴充流量



> - 在「剩餘網路」中
>   - 以阻塞流擴充流量，就斷絕了所有「一樣長的最短擴充路徑」
>
> - 在「容許網路」中
>   - 所有「由源點到匯點的最短路徑」都被阻塞
>     - 在「剩餘網路」中，在「Edmond-Karp algorithm」中已證明源點到匯點的最短距離會增加
>
> - 「擴充路徑」的長度範圍是 1 到 V-1 （Simple path）
>   - 故最多找 V-1 次阻塞流



- 時間複雜度
  - O(V²E)
    - 找一個阻塞流：O(VE) 
    - 最多找：O(V) 



##### 容許網路

在「剩餘網路」上以源點（匯點）作為起點，計算源點（匯點）到每一點的**最短距離**



- 在剩餘網路中
  - 一條由**源點往匯點方向**的邊
    - 若兩其端點最短距離相差一
      - 稱作「容許邊」（Admissible edge）
- 所有容許邊，整體視作一張圖
  - 稱作「容許網路」（Admissible Network）



「流量網路」G = (V,E)

![1549793031130](\willywangkaa\images\1549793031130.png)



其「剩餘網路」$G_f = (V, E_f)$

![1549793298592](\willywangkaa\images\1549793298592.png)



一種由源點開始的「容許網路」

![1549793355647](\willywangkaa\images\1549793355647.png)



一種由匯點開始的「容許網路」

![1549793388148](\willywangkaa\images\1549793388148.png)



> - 容許網路
>   - 為有向無環圖（DAG）或稱作分層圖（Level graph）
>     - 容許網路可以畫成一層一層的模樣，只有相鄰的層有邊
>   - 任意一條由源點到匯點的路徑
>     - 為最短擴充路徑
>   - 藉由容許網路，可以迅速找到所有「一樣長的最短擴充路徑」
>
> 容許網路就是剩餘網路的「最短路徑圖」
>
> ![1549793497499](\willywangkaa\images\1549793497499.png)
>
> （左圖為源點開始的「容許網路」分層圖；右圖為匯點開始的「容許網路」分層圖）



##### 阻塞流

在容許網路中一個源點到匯點的流，無法再擴充流量稱作「阻塞流」（通常會出現許多種選擇，不必選其最大流）



1. 逐次建立的「容許網路」中
   - **會找到所有「一樣長的最短擴充路徑」**
2. 在該「容許網路」中**讓擴充的流量到達瓶頸**
   - 整體形成「阻塞流」



![1549794137394](\willywangkaa\images\1549794137394.png)

> - 容許網路上尋找最短擴充路徑
>   - 不必作溯洄沖減（溯洄沖減會增加路徑長度，最後得到的不是最短擴充路徑）
>
> - 源點隨意往匯點走，若遇到死胡同，就重頭開始走，下次避免再走到死胡同
>   - **改由匯點隨意往源點走，就不會遇到死胡同**
> - 若順利走到匯點，就形成一條最短擴充路徑，並且擴充流量
>   - **一條最短擴充路徑**
>     - 至少有**一條邊**是瓶頸
> - 容許網路最多只有 E 條邊能作為瓶頸
>   - 所以一個阻塞流最多只有 E 條最短擴充路徑
>   - 從源點走到匯點並擴充流量需時 O(V)
>     - 最多有 O(E) 條最短擴充路徑，所以找出一個阻塞流的時間複雜度為 O(VE)





#### 補充例題



Example（1）（P.4-80 ex20）

**Escape problem**


![escapeproblem](\willywangkaa\images\escapeproblem.png)



> 將問題轉換成「邊與點均有 **Capacity** 的最大流量問題」**（將問題「Reduce」至「Flow network」中解決）**
>

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





**Example（2）（101交通大學資料結構與演算法）**

This question is about the max flow problem

- **Which of the following statements is wrong?**
  - （A）By the Ford-Fulkerson algorithm we can find the maximum flow.
  - （B）Given a flow network G=(V,E), Edmond-Karp algorithm has time complexity $O(∣V∣∣E∣^2)$.
  - （C）The time complexity of Ford-Fulkerson algorithm depend on the capacity.
  - **（D）If each edge has a different capacity, then there exists a unique minimum cut.**
  - （E）The maximum flow is equal to the capacity of a minimum cut.

![1549709412486](\willywangkaa\images\1549709412486.png)

- **Which statement is wrong for a flow network G=(V,E)?**
  - （A）If f is a maximum flow in G, **then the corresponding residual network contains no augmenting path.**
  - （B）For any cut (S,T), the capacity of the cut is not smaller than the value of the flow crossing this cut.
  - （C）The value of any flow f in G is bounded above by the capacity of any cut of G.
  - **（D）If all edges of G have different capacities, then there exists a unique flow f that gives the maximum flow.**
  - （E）The capacity of each edge of G can be any non-negative number.



![1549708938056](\willywangkaa\images\1549708938056.png)



- **Let G=(V,E) be a bipartite graph, where V = L ∪ R. Which statement is wrong about finding a maximum  bipartite matching?**
  - （A）It can be solved by constructing a corresponding flow network and finding the maximum flow.
  - （B）The corresponding flow network can be obtained by adding two vertices s, t and edges from s to vertices in L, and edges from vertices in R to t.
  - （C）The capacity of each edge in the corresponding flow network is set to 1.
  - **（D）The maximum flow of the corresponding flow network is always integral and the flow value of each edge is integral as well.**
  - （E）The cardinality of a maximum matching of G is equal to the maximum flow of the corresponding flow network. 

**「Maximum flow」必為整數，但是 Edge 上的「Flow」未必**

![1549709744238](\willywangkaa\images\1549709744238.png)





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



# 補充例題



Example（100 交通大學資料結構與演算法）

- Suppose there are several cities along a highway (from the left to the right on the map), which has no forks
- Given the distances between the neighboring cities, we can compute the distance between any two cities
  - For example, given 4 cities, in order, (A, B, C, D) and the distances between neighboring cities are: distance(A, B) = 1, distance(B, C) = 2, distance(C, D) = 4
  - Then we can compute the distance matrix
- In this problem, we consider the reverse problem
  - Given the distance between all pair of the cities, we want to recover the order of the cities along the highway
  - Suppose there are N cities and we only know the distances between all pairs of cities, that is, there are $\frac{N(N-1)}{2}$ number in arbitrary order
  - For convenience, let the leftmost city be the first city and the rightmost city be the last one and order cities accordingly

|      | B    | C    | D    |
| ---- | ---- | ---- | ---- |
| A    | 1    | 3    | 7    |
| B    |      | 2    | 6    |
| C    |      |      | 4    |

> （中譯）
>
> - 假設有數個在「無岔路高速公路」旁的城市（在地圖上為由左至右）
> - 當每個相鄰城市的距離得知時，我們可以計算出任兩個城市之間的距離
>   - 舉例來說，依序給定四個城市（A、B、C、D）與每個相鄰城市的距離：
>     - A 與 B 距離 = 1
>     - B 與 C 距離 = 2
>     - C 與 D 距離 = 4
>   - 則我們可以計算出其距離矩陣
> - 在此問題中要考慮的是「逆向工程問題」
>   - 給定任兩個城市之間的距離（未給出是哪兩個城市，只知道距離資訊），欲將原始相鄰城市的距離求出
>   - 假設知道 N 個城市與城市的距離，也就是說會給定一串大小為 $\frac{N(N-1)}{2}$ 且無序的數字數列
>   - 為了方便計算，讓最左邊的城市當作第一個城市最右邊的城市為最後一個城市

**Which of the following is false?**

1. The largest value must be the distance of **the first city and last city along the highway**
2. It can be solved by **searching**
3. If the input has an answer, **then it is unique**
4. The second largest value can be the distance of the first city and second to the last city along the highway
5. The second largest value can be the distance of **the second city and the last city along the highway**

**Ans: (3)**

**Suppose there are 6 cities and the distance between each pair of the cities are: 9, 8, 8, 7, 6, 6, 5, 5, 3, 3, 3, 2, 2, 1, 1. Which of the following is correct ?**

1. There is no solution for this input
2. The distance of the second city and the third city is 3
3. The distance of the fourth city and the fifth city is 1
4. The distance of the third city and the fourth city is 3
5. The distance of the second city and the third city is 8

|      | 2    | 3    | 4    | 5    | 6    |
| ---- | ---- | ---- | ---- | ---- | ---- |
| 1    | 1    | 3    | 6    | 8    | 9    |
| 2    |      | 2    | 5    | 7    | 8    |
| 3    |      |      | 3    | 5    | 6    |
| 4    |      |      |      | 2    | 3    |
| 5    |      |      |      |      | 1    |

**Ans: (4)**