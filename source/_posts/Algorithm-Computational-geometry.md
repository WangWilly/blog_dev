title: Algorithm - Computational geometry
author: Willy Wang (willywangkaa)
tags:
  - Computational geometry
categories:
  - Algorithm
date: 2018-10-15 18:24:00
---
# Computational geometry



## The rank of a node

![rankofnode](\willywangkaa\images\rankofnode.png)

- Dominate and rank
  - $P_1 = (x_1, y_1)$ dominates $P_2 = (x_2, y_2)$：$x_1 > x_2$ and $y_1 \geq y_2$
  - Rank ($P_1$)：The number of node that $P_1$ dominated



- Input
  - 2-D 平面上的點集合 S
- Output
  - 每個底的「Rank」



- Native approach
  - 對任意點檢查其它有幾個點可以「Dominate」
  - Time complexity：$\Theta(n^2)$



### Divide and conquer




![rankcomputation](\willywangkaa\images\rankcomputation.png)

1. 先令 m 是集合 S 中每個元素之 x 座標的**中位數 ( median )**，將 S 分成：
   - $S_L$：S 集合中 x 座標小於 m 之元素
   - $S_R$：S 集合中 x 座標大於 m 之元素
2. 將 $S_L$ 和 $S_R$ 中每個點的「Rank」求出 ( 遞迴 )
   - **終止條件**：若平面上只有一點，則設其「Rank」為零
3. 對於每個在 $S_R$ 中的點 P 修正其 Rank 
   - Rank(P) = Rank(P) + 「在 $S_L$ 中 y 座標比 P 小的點個數」
4. 回傳 $S_L$ 中點之 Rank 加 $S_R$ 中所有點之 Rank



- Time complexity
  - Step1：$\Theta(n)$
  - Step2：$2T(\frac n2)$
  - Step3：$\Theta(n)$
  - $T(n) = 2T(\frac n2) + \Theta(n) = \Theta(n\log n)$ 



## Maximal points




![maximalpoint](\willywangkaa\images\maximalpoint.png)

「沒有被任何點 dominate」的點稱為 Maximal point



![maximalpointalgorithm](\willywangkaa\images\maximalpointalgorithm.png)

1. 先令 m 是集合 S 中每個元素之 x 座標的中位數 ( median )，將 S 分成：
   - $S_L$：S 集合中 x 座標小於 m 之元素
   - $S_R$：S 集合中 x 座標大於 m 之元素
2. 將 $S_L$ 和 $S_R$ 中每個點的「Maximal point」求出 ( 遞迴 )
   - **終止條件**：若平面上只有一點，則此點即為 Maximal point
3. 將所有在 $S_L$ 和 $S_R$ 中的「Maximal point」投影到 L 上。並且依照其 Y 值由大到小用線性搜尋的方式找出每一個在 $S_L$ 中的「Maximal point」且其 y 座標比某個 $S_R$ 中的「Maximal point」的 y 座標小者，將其捨去
4. 回傳 $S_R$ 中「Maximal point」與 $S_L$中剩下的「Maximal point」



- Time complexity
  - Step1：$\Theta(n)$
  - Step2：$2T(\frac n2)$
  - Step3：$\Theta(n)$
  - $T(n) = 2T(\frac n2) + \Theta(n) = \Theta(n\log n)$ 



## Closest pairs



2-D 平面上的點集合 S，找 S 中距離最小的距離

- 概念
  - 計算「有可能Closest pair」兩點的距離，**不計算完全不可能的兩點之距離**



### Divide and conquer



1. 先令 m 是集合 S 中每個元素之 x 座標的中位數 ( median )，將 S 分成：
   - $S_L$：S 集合中 x 座標小於 m 之元素
   - $S_R$：S 集合中 x 座標大於 m 之元素
2. 將 $S_L$ 和 $S_R$ 中的 Closest pair 的距離 $d_L$, $d_R$ 求出 (遞迴)
   - **終止條件**：若平面上只有一點，則「Closest pair」的距離為無限大。




![closestpairalgorithm](\willywangkaa\images\closestpairalgorithm.png)

3. 令 d = $min(d_L, d_R)$ (不跨 $S_L、S_R$) 
   （1）對於在 $S_L$ 中且 **x 座標在 m-d ~ m 中的所有點** p = $(x_p, y_p)$
   （2）與在 $S_R$ 中且 **x 座標在 m ~ m+d、y 座標在** $y_p -d$ ~ $y_p + d$ **的所有點** g 計算 $min_p$ = min( dist(p, q)$\forall q \in S_R$)
   （3）最後令 d' = min( $min_p, \forall p \in S_L$ )

4. 回傳 min(d, d')



- Time complexity
  - Step1：$\Theta(n)$
  - Step2：$2T(\frac n2)$
  - Step3：$\frac n2 \times \Theta(1) = \Theta(n)$
  - $T(n) = 2T(\frac n2) + \Theta(n) = \Theta(n\log n)$

> - Step3 **根據鴿籠原理**
>
> ![closestpointnote_1](\willywangkaa\images\closestpointnote_1.png)
>
> ![closestpointnote](\willywangkaa\images\closestpointnote.png)

## Convex hull



算出 2D 平面上所有點的最小凸多邊形。



- 名詞
  - 順時針：Clockwise              ；Right-turn
  - 逆時針：Counterclockwise；Left-turn



### Naive Algorithm



1. 令目前平面上 x 座標最小的點為 P ( 必定在凸包上 )

2. P 與平面上每一點算出向量
3. 取其與 x 軸夾角最大的點 V

4. **旋轉平面使 V 的 x 座標最小化（使 (P,V) 垂直）成為 P'，重複第一步**

- 直到回到最一開始的 P 為止



- 演示
  - 左：P
  - 次：V




![convexhull_1](\willywangkaa\images\convexhull_1.png)


![convexhull_2](\willywangkaa\images\convexhull_2.png)


![convexhull_3](\willywangkaa\images\convexhull_3.png)


![convexhull_4](\willywangkaa\images\convexhull_4.png)


![convexhull_5](\willywangkaa\images\convexhull_5.png)


![convexhull_6](\willywangkaa\images\convexhull_6.png)



- Time complexity
  - 若有 n 點必須重複上述動作 n 遍，其中每一遍需要：
    - Step1 算出與其餘點的所有向量：$O(n)$
    - Step2 找出與 x 軸夾角最大的 V 點：$O(n)$
    - Step3 旋轉平面：$O(n)$
  - $\Rightarrow O(n^2)$



### Graham scan



1. 找出點 $p_0$ ：2D 平面上做有點中 y 座標最小者 ( 若有多個最小點取其中最左者 )

2. 找出有序集 $＜P_1, P_2, \ldots, P_n＞$：依照從 $P_0$ 對每個點取向量之**角度**由小到大排列

   - 若兩個以上的點角度相同，則保留離 $p_0$ 最近的點

3. 建構一個空的「堆疊( Stack )」 $S$ ( **最後凸包的接點** )

   - 建構完後 **push(**$p_0, S$**)、push(**$p_1, S$**)、push(**$p_2, S$**)**

```cpp
// 4. (第四步)
for(int i = 3; i <= n; i++) {
	a = second(S);
	b = top(S);
	while(cross_product(vector(p_i, a), vector(p_i, b)) != "left_turn") {
		pop(S);
	}
	push(p_i, S);
}
```



> - 外積 ( Cross porduct )
>
> 給定兩向量 $\vec{p_1} = (x_1, y_1), \vec{p_2} = (x_2, y_2)$
>
> 其外積為 $\vec{p_1} \times \vec{p_2} = det \begin{bmatrix}x_1 & x_2\\ y_1 & y_2 \end{bmatrix}$ 
>
> $\vec{p_1} \times \vec{p_2} > 0$：稱 $\vec{p_1}$ 往 $\vec{p_2}$ 為「Right turn」
>
> $\vec{p_1} \times \vec{p_2} < 0$：稱 $\vec{p_1}$ 往 $\vec{p_2}$ 為「Left turn」



- Time complexity
  - Step1：$\Theta(n)$
  - Step2：$\Theta(n\lg n)$
  - Step3：$\Theta(1)$
  - **Step4**：$\Theta(n)$
    - 每一個點一定會被「push stack」一次
    - 而最多會被「pop stack 」 一次，且再也不會重新「push stack」
    - 所以對於 stack 來說「push」會做 n 次，「pop」最多 n 次，所以最多 2n 次




#### 應用



- (92 台大) 在 2-D 空間中給定點集合 $S = ｛(x_1, y_1), (x_2, y_2), \ldots, (x_n, y_n)｝$ 一判斷是否有三點為共線 ( collinear )，使用一個 $O(n^2 \log n)$ 演算法解之



```cpp

bool collinear (G) {
	for(int i = 1; i <= n; i++) {
		set A = {};
        // O(n)
		for(int j = 1; j <= n ; j++) {
			if(j != i) {
				vector_2D tmp = vec(S[i], S[j]);
				push(tmp, A);
			}
		}

		sort(A); // 以斜率排序 O(nlogn)
		
        // O(n)
		for(int j = 1; j <= n ; j++) {
			if( slope(A[j]) == slope(A[j+1]) )
				return true;
		}
	}

	return false
}
```





- (97 台大)找到 farthest pair
  - 2D 平面上最遠的兩點必為在 Convex hull 上的某兩點



最遠的兩點必定「互相排斥」，意思是說通過該兩點的兩條平行線會使的其他所有點都在這個之內
( 紅點、綠點；紅點、黃點 )


![farthestpair_1](\willywangkaa\images\farthestpair_1.png)

- 如何找到「凸包上面哪個點會和哪個點**互相排斥**」？


![farthestpair_2](\willywangkaa\images\farthestpair_2.png)

在該點 ( 紅點 ) 兩鄰邊 ( 黃邊、綠邊 ) 之「最遠點」之間( **黃點與綠點之間的藍點** )**的所有點都有可以與紅點互相排斥**


![farthestpair_3](\willywangkaa\images\farthestpair_3.png)


![farthestpair_4](\willywangkaa\images\farthestpair_4.png)

> 如何求邊的最遠點？
>
> - 名詞
>   - 「Bitonic sequence」：在碰到「Bitonic point」之前的序列「單調上升」；之後「單調下降」
>
> ( 假設要幫黃邊找最遠點**黃點** ) 將除了黃邊上之外的點對黃邊做*點到邊的距離*並且依序記錄成一個「Bitonic sequence」，接著在該序列中找到中找「Bitonic point」( 也就是我們要找的最遠點黃點 )
>
> - Time complexity
>   - 在「Bitonic sequence」找「Bitonic point」其實只要使用「Binary search」$O(\log n)$ 即可
>     - $finBT(arr, L, R)\xrightarrow{mid = \frac{L + R}{2}}\left\{\begin{matrix} mid, if \; arr[mid-1] < arr[mid] \; ＆ \; arr[mid] > arr[mid+1] \\findBT(arr, mid+1, R), if\;arr[mid] < arr[mid+1] \\findBT(arr, L, mid), if\;arr[mid] > arr[mid+1]\end{matrix}\right.$
>
>
>
> - 相鄰兩頂點共用 $O(1)$ 個「排斥點」( 下圖**綠邊**上的藍點與紅點有共同「排斥點」**綠點**；有可能不只一個 )
>
> ![farthestpair_6](\willywangkaa\images\farthestpair_6.png)



演算法 (假設平面上有 n 個點)

1. 使用「Graham scan algorithm」將**有 m 個頂點的凸包**找出
2. 在使用「Find bitonic point algorithm」將每邊的**最遠點**找出
3. 使用最遠點將凸包上的頂點**分成 m 片段**
4. 使用凸包片段列出 $O(m)$ 對「互相排斥」的點並找出最遠的一個
5. 使用 $O(m)$ 線性找出最遠的一組

Time complexity

Step1 $O(n\log n)$

Step2 $O(n\log n)$

加總 $O(n\log n)$





- (100 政大) 判斷多邊形是 Convex 或 Concase
  - 使用 Graham scan 判斷連續三點是否形成凹角即可