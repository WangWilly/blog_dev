title: Machine learning - Supervised learning
author: Willy Wang
tags:
  - Concept Learning
  - Supervised Learning
  - ''
categories:
  - Machine Learning
date: 2018-04-20 16:15:00
---
# Supervised learning 監督式學習
Concept Learning 概念式學習

![machinelearningconcept](\willywangkaa\images\machinelearningconcept.png)

## 機器學習簡介<br>( 節錄自 [Mr' OpenGate](http://mropengate.blogspot.tw/2015/05/ai-supervised-learning.html) )

機器學習是近20多年興起的一門多領域交叉學科，涉及**機率論**、**統計學**、**逼近論**、**凸分析**、**計算複雜性理論**等多門學科。機器學習理論主要是設計和分析一些讓計算機可以自動「學習」的演算法。機器學習算法是一類從資料中**自動分析獲得規律**，並利用規律對**未知資料進行預測**的算法。

機器學習已廣泛應用於**數據挖掘**、**計算機視覺**、**自然語言處理**、**生物特徵識別**、**搜尋引擎**、**醫學診斷**、**檢測信用卡欺詐**、**證券市場分析**、**DNA序列測序**、**語音和手寫識別**、**戰略遊戲**和**機器人**等領域。

**＜定義＞** 機器學習定義如下

```A computer program is said to learn from experience E with respect to some class of tasks T and performance measure P, if its performance at tasks in T, as measured by P, improves with experience E.```

### $Ex.$

- T(任務)：將郵件分類為垃圾或非垃圾。
- E(經驗)：觀察目前信箱的信是把哪些種類的郵件標記為垃圾，而哪些是非垃圾。
- P(效能)：被正確分類成垃圾或非垃圾的郵件的數量。









![supervisedlearningworkflow](\willywangkaa\images\supervisedlearningworkflow.png)








## 一種歸納的方式

- 從已知的現象訓練機器判斷常態的結果，而已知的現象只有**是非**之結論。(所以用這個方法訓練出來的智能只能判斷**對錯**)
- 所以以這種方法訓練的**智能**我們可以看待它是一個布林函數(Boolean-valued Funciotn)。
 - 輸入 - 欲判斷的狀態 (Attributes)。
 - 輸出 - 對(TURE)、錯(FALSE)。

![learningfunction](\willywangkaa\images\learningfunction.png)





## 環境狀態( Attribute )、<br>學習目標( Target Concept )

### Example from book：Enjoy sport






![conceptlearningenjoysportdata](\willywangkaa\images\conceptlearningenjoysportdata.png)




- 六個會影響的**環境狀態 ( Attribute )**
- 四個**案例 ( Instance )**
- 學習目標：**判斷**當時是否很享受運動。$EnjoySport = \{Yes, No\}$

#### 環境狀態( Attribute )

- 每個事件(instance)發生時會引響結果的*基本元素*。
- 倘若現在有一事件會被影響的環境狀態為 $N$ 個。
- 每個環境狀態可能出現的狀態個數為 $n_i$ 。(第 $i$ 個環境狀態)
- **(Nominal values; symbolic values; Discretized values)**(? 待了解)

#### 案例( Instance ) - 已知結果的一群環境狀態

- 令 $x$ 為已知結果的一群環境狀態。
- 那我們稱所有可能產生的案例為一個空間(Space)並稱它 $X$。
- 若 $M$ 等於此空間 $X$ 的大小，則 $ M = n_1 \cdot n_2 \ldots n_{N-1} \cdot n_N$。
- 我們其實可以將 $M$ 視為此環境狀態所有組合的方法數。

#### 學習目標( Target Concept )

- 要學到的想法(在這裡我們給機器訓練的想法也可視為一個函式)。
	 - $c(x)=1 \qquad if \; EnjoySport=Yes$
	 - $c(x)=0 \qquad if \; EnjoySport=No$

- $c$ 是一個定義再**案例空間(Instance Space)**的布林函數。 
	 - $c:X \rightarrow \left\{ 0, 1 \right\}$

- 訓練集合 "$D$"
	 - 所有已知想法的案例集合
	 - $Ex.$
     
 $$
 <x_1, c(x_1)>, <x_2, c(x_2)> \ldots <x_m, c(x_m)>
 $$

#### 假說( Hypotheses ) - 在這裡我們標記為 $H$

- 定義：所有**環境狀態( Attributes )**之**限制( Constraints )** 的**交集( Conjunction )**。
- 限制( Constraints )的種類
	 - Specific value ( **針對**值 ) $\qquad e.g. \; ( sky = sunny )$
	 - Don't care value ( **不在意**值 ) $\qquad e.g. \; ( sky = "？" )$
	 - No value allow ( **無**值 ) $\qquad e.g. \; ( sky = "\phi" )$

- 若今天有一**案例( Instance )**符合了我們的假說，也就是說它每個**環境狀態( Attributes )**全部都不逾越我們假說中的所有**限制( Constraints )**。
- $Ex.$

$$
h \leftarrow < Sunny, Warm, ?, Strong, ?, ? >
$$

- 假說空間( Hypotheses Space )的大小
	 - 語意上來說( Syntactically distinct number )

$$
M_H = ( n_1 + 2 ) \cdot ( n_2 + 2 ) \cdot \ldots \cdot ( n_{N-1} + 2 ) \cdot ( n_{N} + 2 )
$$

$$
( Two \; more \; "values" \; have \; been \; added, "?" and "\phi" )
$$

 - 實際上來說( Semantically distinct number )


$$
M_H = 1+ ( n_1 + 1 ) \cdot ( n_2 + 1 ) \cdot \ldots \cdot ( n_{N-1} + 1 ) \cdot ( n_{N} + 1 )
$$

因為如果該假說的限制交集裡，有一個以上的"\phi"存在於集合中，代表所有的案例( Instances )絕對都不可能不逾越我們的假說，全部的案例都會判定為錯誤(False)。

#### 小節論

- $c:EnjoySport : X \rightarrow \{  0, 1 \}$ 是我們的**學習目標( Target Concept )**。

- 六個**環境狀態( Attributes )**：

	- Sky ( 可能的變數有三種 )

    $$\{ Sunny, Cloudy, Rainy \}$$

	- Airtamp ( 可能的變數有兩種 )

    $$\{ Warm, Cold \}$$

	- Humidity ( 可能的變數有兩種 )

    $$\{ Normal, High \}$$

	- Wind ( 可能的變數有兩種 )
    
    $$\{ Strong, Light \}$$

	- Water ( 可能的變數有兩種 )
    
    $$\{ Cold, Warm \}$$

	- Forecast ( 可能的變數有兩種 )
    
    $$\{ Same, Change\}$$



- 案例空間的大小 $= 3 \cdot 2 \cdot 2 \cdot  2 \cdot 2 \cdot 2 = 96$
- 假說空間的大小 $= 1 + ( 4 \cdot 3 \cdot 3 \cdot  3 \cdot 3 \cdot 3 ) = 973$ ( 實際上 )

現在知道目前的假說空間大小後，就要開始找到符合我們期望( Target )的假說( Hypotheses )，那要從何先下手呢？首先，我們可以先從現有的訓練資料使用，其中，我們還可以了解一個概念－－－**Inductive learning hypothesis**，意思是說，我們今天使用訓練的資訊來找到一個最靠近的假說時，我們也可以找到一些潛在的的規則包含在我們找到的假說之中，其中會有我們從為訓練過的**案例( Instance )**在內。

 **＜注意＞：**在實務上來說，有可能訓練的難度會急遽上升，有可能我們的假說空間會超級大，甚至於無限大也有可能，所以要一個個要從所有的假說找到我們需要的是不太可能的，**那怎麼辦呢？**我們可以利用假說空間的一個特性－－－Partial Ordering，也就是說，這個空間裡的元素，是可以依照一個順序大小排列的。

## 假說空間的廣至收斂<br>General-to-Specific Ordering over Hypotheses



首先定義幾個名詞，我們有：
- 案例( Instance )：$x$
- 假說( Hypothesis )：$h$
- 若今天 $h(x) = 1$ ，稱之 Positive ( True ) Outcome。

由廣至收斂，定義若 $H_1 \geq_g H_2$，則可以說 $H_1$ 比 $H_2$還要更廣( General )，舉例：

$$
\{ Sunny, ?, ?, ?, ?, ? \} \geq_g \{ Sunny, ?, ?, Strong, ?, ? \}
$$

![GtoSprap](\willywangkaa\images\GtoSpraph.png)














## Find S Algorithm

1. 將假說 $h$ 初始化為假說空間 $H$ 中的最特殊假說 $\{ \phi, \phi, \phi, \phi, \phi, \phi \} $
2. 對每個**正例** $x$ (  **＜注意＞**我們只使用正例( Positive Outcome )，不用反例！ )<br>
 - 對 $h$ 的每個環境狀態( Attribute )進行約束
   如果 $x$ 的該環境狀態滿足 $h$ 對應的環境狀態，那麼不做任何處理。
   否則將 $h$ 中該環境狀態一般化( Generalize ) 以滿足 $x$ 的環境狀態。
3. 重複直到所有正例都被尋遍。
3. 輸出最後唯一的假說 $h$ ，而這個假說正是我們使用訓練資料中的正例所能訓練出最收斂的假說。



![findsalgorithm](\willywangkaa\images\findsalgorithm.png)











## Version Space

### Definition: Consistent Hypotheses( 認同假說 )

若有假說 $h$ 以訓練集合所有的案例進行測試，輸出結果和我們的想法一致，就可以聲明假說 $h$ 為認同假說( Consistent Hypotheses )。( 下方為原始定義 )

A hypothesis $h$ is consistent with a set of training examples $D$ of **target concept** $c$ if and only if $h(x) = c(x)$ for each training example $<x, c(x)>$ in $D$.

$$
Consistent (h, D) \equiv ( \; \forall <x, c(x ) \in D \;)  h(x) = c(x))
$$






### Definition: Version Space ( 候選空間 )

候選空間就是對於該測試的資料集，所有的認同假說所組合的空間，因為每個假說都可以符合目前的訓練資料的期望，所以每個假說都等待我們再進一步驗證。( 下方為原始定義 )
The version space $VS_{H,D}$ , with respect to hypothesis space $H$ and training examples $D$, is the subset of hypotheses from $H$ consistent with all training examples in $D$.

$$
VS_{H,D} \equiv \{ h \in H \; | \; Consistent (h, D) \}
$$










![versionspacegraph](\willywangkaa\images\versionspacegraph.png)
















- Version space for a "rectangle" hypothesis language in two dimensions. Green pluses are positive examples, and red circles are negative examples. GB is the maximally general positive hypothesis boundary, and SB is the maximally specific positive hypothesis boundary. The intermediate (thin) rectangles represent the hypotheses in the version space.

Definition: General Boundary
General boundary $G$ of version space $VS_{H,D}$ : set of most general members

Definition: Specific Boundary
Specific boundary $S$ of version space $VS_{H,D}$ : set of most specific members

Version Space
Every member of the version space lies between $S$ and $G$

$$
VS_{H,D} \equiv \{ h \in H \; | \; (\exists s \in  S ) (\exists g \in  G ) (g \geq_g h \; \geq_g s) \}
$$

$$
where \geq_g \equiv more \; general \; than \; or \; equal \; to
$$



## The *List-Then-Elimination* Algorithm<br>列表消除演算法

1. 起始化：
候選空間( Version Space ) $\leftarrow(assign)$ 所有在假說空間的假說。

2. 對每個訓練案例$<x, c(x)>$
從候選空間消除所有 $h(x) \neq c(x)$ 的假說 $h$。

3. 對所有的訓練案例檢驗過，最後輸出候選空見剩下的**假說列表**。

### 優點

**保證**最後的假說列表裡的所有假說必定和訓練案例的期望相符( Consistent )。


### 缺點

- 若今天假說空間是**無窮大**，那這個方法就不能使用。
- 要將該假說空間的所有假說窮舉於列表之中。



## Candidate-Elimination Algorithm<br>候選消除演算法

**＜前情提要＞：**候選空間( Version Space )可以由 Most specific boundaries 與 Most general boundaries 界定出來。
- 正例可以將 Specific boundary 變的更一般化( General )。
Positive examples force specific boundary to become more general.
- 反例可以將 General boundary 變的更收斂( Specific )。
Negative examples force general boundary to become more specific.

最後，這個界定出來的假說集合可以符合所有的訓練資料。
In the end, all hypotheses which satisfy training data remain.

1. 起始化
G $\leftarrow$ 一組在假說空間 $H$ 最一般化的環境因素。標記為：$<?, \ldots, ?>$
S $\leftarrow$ 一組在假說空間 $H$ 最嚴苛的環境因素。標記為：$<\phi, \ldots, \phi>$

2. 對於每個訓練案例 $d$，進行以下操作：
 - 若 $d$ 為一正例：
 從 $G$ 中移除所有與 $d$ 不一致的假說。
 對 $S$ 中每個對 $s$ 不一致的假說$s$：
 1. 將 $s$ 從 $S$ 之中移去。
 2. 把 $s$ 的所有的極小泛化假說 $h$ 加入到S中，其中 $h$ 滿足 $h$與$d$一致，且$G$的其中一個元素必比起 $h$ 更泛化。
 3. 從$S$中移去所有符合這樣的假說：它比S中另一假設更泛化。

 - 若 $d$ 為一反例：
 從 $S$ 中移去所有 $d$ 不一致的假說。
 對 $G$ 中每個與 $d$ 不一致的假設 $g$：
  1. 從 $G$ 中移去 $g$ 。
  2. 把 $g$ 的所有的極小特化式 $h$ 加入到 $G$ 中，其中 $h$ 滿足 $h$ 與 $d$ 一致，而且 $S$ 的某個成員比 $h$ 更收斂。
  3. 從 $G$ 中移去所有符合這樣的假說：它比 $G$ 中另一假說更特殊。


### 範例 - $EnjoySport$

#### 起始化

Specific boundary to: 	$S_0 = \{(Ø,Ø,Ø,Ø,Ø,Ø)\}$
General boundary to:	$G_0 = \{(?,?,?,?,?,?)\}$

#### 1’st instance: (Sunny,Warm,Normal,Strong,Warm,Same) = Yes

**Positive** example *generalizes* Specific boundary

$S_1 = \{ (Sunny,Warm,Normal,Strong,Warm,Same)\}$

$G_1 = \{ (?,?,?,?,?,?)\}$

#### 2’nd instance: (Sunny,Warm,High,Strong,Warm,Same) = Yes

**Positive** example *generalizes* Specific boundary

$S_2 = \{(Sunny,Warm,?,Strong,Warm,Same)\}$

$G_2 = \{ (?,?,?,?,?,?)\}$

#### 3’rd instance: (Rainy,Cold,High,Strong,Warm,Change) = No

**Negative** example *specializes* General boundary

$S_3 = \{ (Sunny,Warm,?,Strong,Warm,Same) \}$ 

$G_3 = \{(Sunny,?,?,?,?,?), \quad	O.K.$ <br>

$(Cloudy,?,?,?,?,?), \quad Not \; more \; general \; than \; S_3$<br>

$(?,Warm,?,?,?,?),	\quad O.K.$<br>

$(?,?,Normal,?,?,?), \quad Not \; more \; general \; than \; S_3$<br>

$(?,?,?,Light,?,?), \quad Not \; more \; general \; than \; S_3$<br>

$(?,?,?,?,Cool,?), \quad Not \; more \; general \; than \; S_3$<br>

$(?,?,?,?,?,Same)\} \quad O.K.$<br>

$\Rightarrow G_3 = \{ (Sunny,?,?,?,?,?), (?,Warm,?,?,?,?), (?,?,?,?,?,Same)\}$<br>

#### 4’th instance: (Sunny,Warm,High,Strong,Cool,Change) = Yes

**Positive** example *generalizes* Specific boundary
$S_4 = \{ (Sunny,Warm,?,Strong,?,?) \}$
$G_4 = \{ (Sunny,?,?,?,?,?), (?,Warm,?,?,?,?) \}$

Final version space is all hypotheses, h such that:

$$
g \geq_ g h \; \geq_g s
$$

![candidate-eliminationalgorithm](\willywangkaa\images\candidate-eliminationalgorithm.png)

















![candidate-eliminationalgorithm2](\willywangkaa\images\candidate-eliminationalgorithm2.png)








What query should the learner make next?
How should these be classified?

$$
<Sunny, Warm, Normal, Strong, Cool, Change>
$$

$$
<Rainy, Cold, Normal, Light, Warm, Same>
$$

$$
<Sunny, Warm, Normal, Light, Warm, Same>
$$

### Inductive Bias 歸納偏置 ( 未完待補 )

需要某些的預先設定( 偏見 )。

# 參考

---

[Mr' OpenGate - AI - Ch13 機器學習(1), 機器學習簡介與監督式學習 Introduction to Machine Learning, Supervised Learning](http://mropengate.blogspot.tw/2015/05/ai-supervised-learning.html)

[《机器学习》第2章中find-s算法的python实现](https://willywangkaa.csdn.net/minvacai/article/details/20202687)

[WEKIPEDIA - Version space learning](https://en.wikipedia.org/wiki/Version_space_learning)

[robert_ai - ML一（概念学习和一般到特殊序）](http://www.cnblogs.com/robert-dlut/p/3977184.html)