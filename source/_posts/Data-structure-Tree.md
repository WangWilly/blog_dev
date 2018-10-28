title: Data structure - Tree
author: Willy Wang (willywangkaa)
tags:
  - Tree
categories:
  - Data structure
date: 2018-10-28 20:11:00
---
# Binary tree



## 節點數量計算



- 二元樹之**第 i 階**之節點個數為 $2^{i-1}$，i ≧ 1
- 在高度為 k 的二元樹，全部節點之個數為 $2^k-1$，k ≧ 1

- 對於任一非空二原樹 T ，如果令 Leaf 節點的個數為 $n_0$，
  而 Degree 為 2 的節點的個數為 $n_2$，則 $n_0 = n_2+1$

$$
令\; n_1 代表 \;Degree\;為\; 1 \; 的節點個數，n\; 表示節點總數 \\
\Rightarrow n = n_0 + n_1 + n_2 ......(1)\\
又因為只有\; Degree \; 為 \; 1、2 \;的節點才會有分支，令分支總數為 \;B \\
\Rightarrow B = n_1 + 2\cdot n_2 \\
樹的總節點個數又為分支總數加 \;1 \\
\Rightarrow n = n_1 + 2\cdot n_2 + 1 ......(2)\\
By \;(1)、(2)\Rightarrow n_0 = n_2 + 1
$$

> **Example**
>
> 有一樹的 Degree 為 4 ，Non-leaf 必有 4 個子節點，若 Leaf 節點個數為 $n_0$ ，求總結點數？
> $$
> 令\; n \;為樹的節點總數 \\
> n = n_0 + (n_1 + n_2 + n_3) + n_4 \\
> 而非葉節點必有四個子節點，所以 \;n_1、n_2、n_3\; 個數皆為零 \\
> \Rightarrow n = n_0 + n_4 ......(1)\\
> 令\; B \;為樹的總分支數量 \\
> B = 4\cdot n_4 \\
> 樹的總節點個數又為分支總樹加 \;1 \\
> n = 4\cdot n_4 + 1 ......(2)\\
> By \;(1)、(2) \Rightarrow n_4 = \frac {n_0 -1}{3} \\
> \Rightarrow n = n_0 + \frac {n_0 -1}{3} = \frac {4\cdot n_0 -1}{3}
> $$
>

> Example
>
> Completed binary tree 有 1000 個節點 (邊號為 1 ~ 1000)
>
> - 節點編號 747 之 Grandparent 為
>
> $\frac{\frac{747}{2}}{2} = 186$
>
> - 節點編號 i 之 Grandchilden 為
>
> 4i +1、4i + 2、4i + 3、4i+4
>
> - 節點編號 512 之左子節點為
>
> 512 × 2 = 1024 ≧ 1000 ，不存在
>
> - **樹高為**
>
> $2^k-1 = 1000 \\ \Rightarrow k = \lceil \lg 1001 \rceil = 10$
>
> - 節點編號 537 位於哪一層
>
> $2^k - 1 = 537 \\ \Rightarrow k = \lceil \lg 538 \rceil = 10$
>
> - 第七層的第一個節點編號為
>
> 前六層之節點總數 $2^6 - 1 = 63 \\ \Rightarrow 64$ 
>
> - **最後一個父節點之編號**
>
> $1000 / 2$ = 500
>
> - **有幾個 Leaf 節點**
>
> 1000 - 500 = 500
>
> - Degree 為 1 之節點個數為；該節點編號為
>
> $n = n_0 + n_1 + n_2 \\ \Rightarrow n = 500 + n_1 + (n_0 -1 )\\ \Rightarrow n = 1$
>
> 編號為 500 
>
> - (True or false) 在一個「Complete binary tree」，$n_1$的數量 = 0 or 1？
>
> TRUE
>
> - (True or false) 在一個「Complete binary tree」，任何節點之左右子樹必也為「Complete binary tree」？
>
> **TRUE**
>
> - (True or false) 在「Binary tree」中，根節點之左右子樹若皆為「Complete binary tree」，則此樹為「Complete binary tree」？
>
> False



# Strict binary tree



二元樹中，任何**非葉節點**必有兩個子節點 ( $n_1$ = 0 )




![strictbinarytree](\willywangkaa\images\strictbinarytree.png)



- Example
  - 「Full binary tree」必為「Strict binary tree」？ **True**
  - 「Complete binary tree」必為「Strict binary tree」？ **False**
  - 「Strict binary tree」必為「Full binary tree」？ **False**
  - 「Strict binary tree」必為「Complete binary tree」？ **False**



> Example
>
> 針對下列非空二元樹，選出正確敘述
>
> - （1）$n_0 = n_2 + 1$
> - （2）$n = 2\cdot n_0 - 1$
> - （3）高 = $\lceil \lg (n+1)\rceil$
> - （4）$n_0 + n_2 \leq n \leq n_0 + n_2 + 1$
>
> 任意「Binary tree」--- （1）
>
> 「Full binary tree」--- （1）（2）（3）**（4）**
>
> 「Complete binary tree」 --- （1）（3）（4）
>
> **「Strict bianry tree」**--- （1）（2）

> **Link list tree**
>
> n 個節點之「Binary tree」總共有 2n 個鍊節，其中 n-1 條鍊節是有用的
>
> **n - (n-1) = n + 1 為 Nil (浪費)**



# Tree travesal



> 若只有前序與後序的資訊時狀況如下：
>
> - Preorder：abc
> - Postorder：cba
>
> ![treetra_1](\willywangkaa\images\treetra_1.png)
>
> ![treetra_2](\willywangkaa\images\treetra_2.png)
>
> ![treetra_3](\willywangkaa\images\treetra_3.png)
>
> ![treetra_4](\willywangkaa\images\treetra_4.png)



Example

- Preorder：ABCDEFG
- Postorder：DCBFGEA



**A**(BCD)(EFG)

(DCB)(FGE)**A**


![traex_1](\willywangkaa\images\traex_1.png)

討論 **＜EFG＞**


![traex_2](\willywangkaa\images\traex_2.png)

討論 **＜BCD＞**


![traex_3_1](\willywangkaa\images\traex_3_1.png)


![traex_3_2](\willywangkaa\images\traex_3_2.png)


![traex_3_3](\willywangkaa\images\traex_3_3.png)


![traex_3_4](\willywangkaa\images\traex_3_4.png)

Example 

決定下列二元樹各為何？

- 前序等於中序
  - Right skew tree、Empty、Single node tree
- 後序等於中序
  - Left skew tree、Empty、Single node tree
- 前序等於後續
  - Single node tree、Empty



Example

下列哪些可決定唯一的二元樹？

- （1）Levelorder + Preorder
- （2）Levelorder + Postorder
- **（3）Levelorder + Inorder**



Levelorder：abc

Preorder：cba


![traex_3_1](\willywangkaa\images\traex_3_1.png)


![traex_3_2](\willywangkaa\images\traex_3_2.png)


![traex_3_3](\willywangkaa\images\traex_3_3.png)


![traex_3_4](\willywangkaa\images\traex_3_4.png)



Example

下列何者可以決定唯一的二元樹？

- **（1）Complete binary tree + 前序**
- **（2）Complete binary tree + 中序**
- **（3）Complete binary tree + 後序**
- **（4）Complete binary tree + 中序**



**皆可**，因為「Complete binary tree」可以視為一個已經建好的樹狀表格，在依照各不同的「Order」填入即可



## 應用



- 葉節點個數計算

```cpp
int Myst( T:pointer of root ) {
    if(T == nil) {
        return 0;
    } else {
        n_L = Myst( T->leftchild );
        n_R = Myst( T->rightchild );
        if( n_L + n_R == 0 ) return 1;
        else return n_L + n_R
    }
}
```



- Swap a binary tree

```cpp
void Swap ( T:pointer of root ) {
    if(T != nil) {
        Swap(T->leftchild);
        Swap(T->rightchild);
        Tmp = T->leftchild;
        T->leftchild = T->rightchild;
        T->rightchild = R->leftchild;
    }
    return;
}
```



# Binary search tree



Example

n 個節點的**「Full binary search tree」**問平均比較次數？



> 樹高 k = $\lceil \lg (n+1) \rceil$
>
> $\Rightarrow n = 2^k -1$



令 S 為全部的比較次數，若比較到第一層需要比較一次比較次數，若到第二層需要兩次比較次數....
$$
\begin{matrix}
 S = & 2^0 \cdot 1 + & 2^1 \cdot 2 + 2^2 \cdot 3 + \ldots + 2^{k-1}\cdot k..................(1)\\ 
 2S= & & 2^1 \cdot 1 + 2^2 \cdot 2 + \ldots + 2^{k-1}\cdot (k-1) + 2^k \cdot k .....(2)
\end{matrix} \\
By (2)-(1) \Rightarrow S = -2^0 -2^1 - 2^2 -\ldots - 2^{k-1} + 2^k \cdot k \\
\Rightarrow \frac{S}{n} = \frac{2^k\cdot k-\frac{2^k-1}{2-1}}{2^k-1} = \frac {2^k\cdot(k-1)+1}{2^k -1} \approx \frac {2^k\cdot(k-1)}{2^k} = (k-1) = \lg(n+1)-1
$$
所以平均只要比較樹高的量即可



**Example** 

下列何者可決定唯一的「Binary tree」？

- **（1）Binary search tree + Preorder**
- （2）Binary search tree + Inorder
- **（3）Binary search tree + Postorder**
- **（4）Binary search tree + Levelorder**

> 「Binary search」本身就會有「Inorder」



**Example (P.212 ex28)**



有一個「Binary search tree」，找資料 2006 時經過了

1000、5566、5203、k、1314、1510、2381、2006



雖然知道 K 是多少，但可以藉由 k 之後查詢的資料判斷一定是小於 5203，所以

![treeex_1](\willywangkaa\images\treeex_1.png)

還有兩種狀況需要討論

- 若 1314、1510、2381、2006 比 k 還要小的情況


![treeex_2](\willywangkaa\images\treeex_2.png)

**則 2381 < k < 5203**

- 若 1314、1510、2381、2006 比 k 還要大的情況


![treeex_3](\willywangkaa\images\treeex_3.png)

**則 1000 < k < 1314**

所以 2381 < k < 5203；1000 < k < 1314



# Thread binary tree



> 一棵 n 個結點並以鏈結串列製作之二元樹中，會有 2n - (n-1) = n+1 條鏈結是「Nil」



- 若 x 的「Lchild」為「Nil」，則視為**左引線指向中序順序中 x 的前一個節點**
- 若 x 的「Rchild」為「Nil」，則視為**右引線指向中序順序中 x 的後一個節點**



- 節點結構



![threadbt](\willywangkaa\images\threadbt.png)



## Head node



![threadbthead](\willywangkaa\images\threadbthead.png)



- Insuccess (x) 找出 x 的中序後繼者
  - 若 x 的「Rthread」為 True，則 x 的「Rchild」即為後繼者
  - 若 x 的「Rthread」為 False，則 x 的右子點開始往左下找，直到「Lthread」為 True，該節點即為後繼者
- Algorithm

```cpp
Insuccess(x){
    temp = x->Rchild;
    if(x->Rthread == false) {
        while(temp->Lthread != false) {
            temp = temp->Lchild;
        }
    }
    return temp; 
}
```

- 簡化「Inorder travesel」

從「Head」開始不斷詢問「Insuccessor」並列出

```cpp
Inorder(head) {
    temp = head;
    do{
        temp = Insuccess(temp);
        if(temp != head) print(temp->data);
    } while(temp != head);
}
```



- Insert t node as the right child of s

```cpp
Insert(t, S) {
    t->Rthread = s->Rthread;
    t->Rchild  = s->Rchild;
    t->Lthread = true;
    t->Lchild  = s;
    s->Rthread = false;
    s->Rchild  = t;
    if(t->rthread == false) {
        temp         = Insuccess(t);  // 原本為 s 的中序後繼者，要將引線指向 t 節點
        temp->Lchild = t;
    }
}
```



# Extended binary tree



n 個節點之二元樹，如果以鏈結串列方式建立，則**有 n+1 條空鏈結**，在這些空鏈結上加上特殊結點，稱為「外部節點」( Extended node；Falure node )，其餘結點稱為「內部節點」( Internal node )




![extendbinarytree](\willywangkaa\images\extendbinarytree.png)



> 有些版本 ( 如離散數學 )：
>
> - 外部節點等價於葉節點
> - 內部節點等價於非葉節點



- I：Internal path length
- E：External path length

令 n 為內部節點數量，則有 n+1 個外部節點：

- I = $\sum_{i = 1}^n$（樹根至 內部節點$_i$ 之路徑長）
- E = $\sum_{j = 1}^n$（樹根至 外部節點$_j$ 之路徑長）



## Theorem

令 n 為內部節點個數
$$
E = I + 2n
$$
**數學歸納法**

1. 當內部節點數量為 0，E = I = 0，E = 0 + 2×0 = 0，成立
2. 假設 內部節點數量 ≦ n-1 時成立
3. 考慮內部節點數量 = n 時。令 $n_L$ 為樹根之左子樹具有的節點數量
   ，$I_L$ 為「Internal path length」，$E_L$ 為「External path length」，
   同理右子樹也假設 $n_R、I_R、E_R$；
   所以$I = I_L + I_R + (n_L + n_R) \\ E = E_L+E_R +(n_L +1 +n_R+1)\\ \because E_L = I_L + 2n_L , E_R = I_R + 2n_R \\ \Rightarrow E = (I_L + 2n_L) + (I_R + 2n_R) + (n_L +1 +n_R+1) \\ = I + 2N$





> - E 與 I 成正比
> - 樹高越小 E、I 值越小 



**Example**

**Skewed extended binary tree 具有 n 個內部節點，求 I 與 E 之值**

$E = I + 2n = \sum_{i = 0}^{n-1} i + 2n \\ = \frac {n^2 + 3n}{2} \\ I = \frac{n^2-n}{2}$



## Weighted external path length



給 n 個外部節點加權值 $q_i, 1 \leq i \leq n$，則 WEPL = $\sum_{i = 1}^n [樹根到「外部節點_i」路徑長 \cdot q_i]$



> 因為加權值的影響，不見得樹高越小 WEPL 越小 (Fall 的機率)



### Minimum WEPL



給 n 個外部節點加權值，要在 $\frac 1n \binom{2n-2}{n-1}$ 棵不同的「Extended binary tree」中找出 WEPL 最小的樹



- Huffman algorithm

令 w = ｛n 個外部節點之加權值｝

步驟：

1. 在 w 中取出兩個最小的加權值建立「Extended binary tree」
2. 將該「Extended binary tree」之加權值再加回 w
3. 重複上述，直到剩下一個值在 w 中 (執行 n-1 次)
4. 此樹稱為「Huffman tree」，其 WEPL 即為「Minimum WEPL」



```cpp
// w: set｛n 個外部節點之加權值｝
// n: w.size
Huffman(w, n) {
    for(int i = 1; i <= n-1; i++) {
        node t;
        t->Lchild = Del_min(w);
        t->Rchild = Del_min(w);
        t->weight = t->Lchild->weight + t->Rchild->weight;
        Insert(w, t);
    }
    return w[0];
}
```

- Time complexity
  - Priority queue (Min-heap)
  - 因為對「Min-heap」作了 n-1 次 Del_min 與 Insert(w, t) $\Rightarrow O(n\log n)$

