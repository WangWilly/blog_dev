title: Decision Tree Learning
author: Willy Wang
tags:
  - Decision Tree Lrearning
  - Data Mining
  - Machine Learning
  - ''
categories:
  - Machine Learning
date: 2018-04-18 12:06:00
---
# Decision Tree Learning 決策樹學習

---

## 簡介

- 最受歡迎的**歸納推理演算法( inductive inference algorithm )**。

- 廣泛且實務的方法。

- **對於干擾值( Noise )相當敏感**。

- 可用來學習如何以**聯集( Disjunctive )**表示**限制集( Constraints )**。
( [Concept Learning]() 以**交集( Conjunctive )**表示 )

- 呈現的方式相當簡單。

- 樹狀結構( Tree Structure )、若則表示式( If-Then rules )

## $Ex.$ Play Tennis

### 範例訓練資料( Training Example )

![decisiontreelearning_training example](\willywangkaa\images\decisiontreelearning_training example.png)

### 決策樹( Decision Tree )

![decisiontreelearning_exampletree](\willywangkaa\images\decisiontreelearning_exampletree.png)

## 決策樹( Decision Tree )的介紹

### 決策樹表示法( Decision Tree Representation )

- 每個**內節點 Internal node ( 包括根結點 Root node )**代表對一個**環境狀態( Attribute )**檢驗。
- 而**分支( Branch )**出來的意義我們可以視為是該**環境狀態( Attribute )**的一種可能**值( Attribute value )**。
- 每個**葉節點 Leaf node **給予一個適當的**分類結果( Classification )**。
 - 我們將每個**案例( Instances )**分類到一個離散的**類別( Categories )**之中。
 - 藉由**決策樹**由**根結點**至**葉節點**找到該**類別**。

### 決策樹引導的假說( Hypotheses )

- 先 AND 再 OR ( 原文：Disjunctions (OR’s) of conjunctions (AND’s) )。
- 經由根結點往葉節點走可視為是一種對於該環境狀態限制的交集( Conjunction of constraints on attributes )。
- 而連上兄弟節點( Sibling )的兩個邊( Edges )可視為是一種對於該環境狀態限制的聯集( Separate branches are disjunctions )。
- $Ex \; ( Cont. )$
<div style="text-align: center"> (Outlook=Sunny and Humidity=Normal) </div>
<div style="text-align: center"> or </div>
<div style="text-align: center"> (Outlook=Overcast) </div>
<div style="text-align: center"> or </div>
<div style="text-align: center"> (Outlook=Rain and Wind=Weak) </div>

#### 注意！

- 每個用來訓練的**案例 ( Instances )**都必須要以「因素-結果」( Attribute - value pairs )的方式給予訓練。
- 目標訓練函式 ( Target function )的值域是**離散的值 ( Discrete value )**。
- 這種方法最後呈現的**假說 ( Hypotheses )**有可能是一些**環境狀態限制( Constraints on attributes )**的聯集( Disjunctive )。
- 極有可能會被不乾淨的資料( Noise )擾亂了學習。
- 應用於：
 - 醫療或是設備的診斷。
 - 信用額度分析( 銀行 )。

### 決策樹的種類

世界上有許多有特殊的**決策樹演算法( decision-tree algorithms )**，比較著名的有：
- ID3 (Iterative Dichotomiser 3)
- C4.5, C5.0 (successor of ID3)
- CART (Classification And Regression Tree)
- CHAID (CHi-squared Automatic Interaction Detector).
- MARS: extends decision trees to handle numerical data better.

#### 注意

- ID3 is the algorithm discussed in textbook.( 在書本中有更詳細的介紹 )
 - Simple, but representative. ( 簡單但representative? )
 - Source code publicly available. ( 程式碼是開放的 )

### ID3演算法

- **概述**：Top-down, greedy search through **space of possible decision trees**.
( 在所有可以出現的決策樹中用貪心法由上而下找到較佳的那棵樹。 )<br>
ID3在建構決策樹過程中，以資訊獲利(Information Gain)為準則，並選擇最大的資訊獲利值作為分類屬性。這個算法是建立在奧卡姆剃刀的基礎上：越是小型的決策樹越優於大的決策樹。儘管如此，該算法也不是總是生成最小的樹形結構，而是一個啟發式算法。另外，C4.5算法是ID3的升級版。<br><br>
 - Decision trees represent hypotheses, so this is a search through hypothesis space.
( 決策樹亦也代表是一種假說，所以這個演算法也可以說是在所有的假說中找到一個較佳的假說。 )
 - **那個演算法該如何起手呢？**
決定甚麼**環境因素( Attribute )**應該放在**根結點( Root node )**？
 - 接著由上而下( Top-down )的建構決策樹，對每個後繼的節點( Successive node )使出一樣的決策手段選出該節點應該置入何種**環境因素( Attribute )**。
 - **注意！**千萬不要由下往上參考之前選過的值，因為我們以貪心法則，所以目前的最佳解決不可能出現在之前選過的**環境因素( Attribute )**之中，或是受其干擾。
( Never backtracks to reconsider earlier choices. )
 - 同上述，在每次的選擇之中，由於我們認知這種情況適用**貪心法( Greedy Method )**，所以我們每次**環境因素( Attribute )**的選擇都朝向我們最後最佳的**假說**靠近。

#### 虛擬碼( Pseudo Code )

```
1. 使用屬性計算與之相關的樣本熵值
2. 選取其中熵值最小的屬性(資訊獲利最大)
3. 生成包含該屬性的節點
4. 遞迴直到終止
```

![decisiontreelearning_builddecisiontree_algorithm](\willywangkaa\images\decisiontreelearning_builddecisiontree_algorithm.png)

![decisiontreelearning_howtochoosenode](\willywangkaa\images\decisiontreelearning_howtochoosenode.png)

**＜討論＞**
ID3演算法的終極目標，就是要將決策樹中每個節點都擺上最優的**環境因素( Attributes )**。<br>
$Question.$<br>
到底以甚麼條件決定甚麼因素要擺放於哪個節點？
$Answer.$
資訊獎賞 or 資訊獲利( Information gain )。
 - 資訊獲利( Information gain )
 統計該價值以檢視該環境因素置於何處來分類我們的資料，我們使用**熵( entropy 又稱"亂度" )**來定義這邊的**資訊獲利( Information gain )**。( 原文：Statistical quantity measuring how well an attribute classifies the data. Use entropy to define information gain. )


### ID3 和 C4.5 - Information gain ( 資訊獲利 ) 與 Gain ratio

#### 定義

關心其中一個環境因素( Attribute )$A$ 的資訊獲利( Information gain )我們標記為 $Gain( S, A )$，且我們關心的目標樣本群體為 $S$，其中：
$$Gain( S, A ) = Entropy( S ) - \sum_{ v \in Values(A) } ( \frac{S_v}{S}Entropy(S_v) )$$
- $v$ ranges over values of $A$
- $S_v$: members of $S$ with $A = v$
- $1^{st}$ term: the entropy of $S$
- $2^{nd}$ term: expected value of entropy after partitioning with $A$

#### Example： PlayTennies

- 四個環境變因
	- Outlook 	= {Sunny, Overcast, Rain}
 - Temperature     = {Hot, Mild, Cool}
	- Humidity	 = {High, Normal}
		 Wind     	 = {Weak, Strong}

- 欲看討的結果 - **開心**或是**不開心**( Target Attributes - Binary )
	- PlayTennis = 	{Yes, No}
- 今天有14組訓練資料
 - 9筆的結果是開心的 ( Positive )
 - 5筆的結果是不開心的( Negative )

- 訓練資料表

![decisiontreelearning_trainningdataform](\willywangkaa\images\decisiontreelearning_trainningdataform.png)


##### Step 1. 計算整體的亂度( Entropy )

$N_\oplus = 9, N_\ominus = 5, N_{Total} = 14$
$Entropy( S ) = -\frac{9}{14} \cdot \lg (\frac{9}{14}) - \frac{5}{14} \cdot \lg ( \frac{5}{14} ) = 0.940$

##### Step2. 不斷計算**資訊獲利( 找亂度比較低attribute的 )**，選擇最大值當作根結點

- Outlook
 - Outlook = Sunny

 $$N_\oplus = 2, N_\ominus = 3, N_{Sunny} = 5$$

 $$Entropy(S_{Sunny}) = -(\frac{2}{5})\cdot \log_2(\frac{2}{5}) - (\frac{3}{5}) \cdot \log_2(\frac{3}{5}) = 0.971$$

 - Outlook = Overcast
$$N_\oplus = 4, N_\ominus = 0, N_{Overcast} = 4$$
$$Entropy(S_{Overcast}) = -(\frac{4}{4})\cdot \log_2(\frac{4}{4}) - (\frac{0}{4}) \cdot \log_2(\frac{0}{4}) = 0.0$$
 - Outlook = Rain
$$N_\oplus = 3, N_\ominus = 2, N_{Rain} = 5$$
$$Entropy(S_{Rain}) = -(\frac{3}{5})\cdot \log_2(\frac{3}{5}) - (\frac{2}{5}) \cdot \log_2(\frac{2}{5}) = 0.971$$
 - 計算環境因素的 Outlook 之資訊獲利

$$Gain(S, Outlook) = Entropy(S) - (N_{Sunny} / N_{total}) * Entropy(S_{Sunny})$$

$$ - (N_{Overcast} / N_{total}) * Entropy(S_{Overcast})$$

$$ - (N_{Rain} / N_{total} ) * Entropy(S_{Rain})$$

$$\Rightarrow 0.940 - (5/14) \cdot 0.971 - (4/14) \cdot 0.00 - (5/14) \cdot 0.971 = 0.246$$


- Temperature
 - Repeat process over { Hot, Mild, Cool }
 $$ Gain( S, Temperature ) = 0.029 $$
- Humidity
 - Repeat process over { High, Normal }
$$ Gain( S, Humidity ) = 0.151 $$
- Wind
 - Repeat process over { Weak, Strong }
$$ Gain( S, Wind ) = 0.048 $$

再來，我們要找到最佳的資訊獲利( Information gain )，其中：

$$Gain(S, Outlook) = 0.246$$

$$ Gain( S, Temperature ) = 0.029 $$

$$ Gain( S, Humidity ) = 0.151 $$

$$ Gain( S, Wind ) = 0.048 $$

從亂度的點看來，似乎Outlook的亂度最低( 與宇亂度相減後剩餘比較多資訊獲利 )，所以我們選擇**Outlook**作為我們根結點( root node )，如下圖：


![decisiontreelearning_choosenode](\willywangkaa\images\decisiontreelearning_choosenode.png)

選擇了Outlook做為決策樹的根結點後，緊接著，我們可以將三種不同的Outlook作為分支，其中特別的是，Overcast狀態之中( 上圖中間綠色部分 )，全部皆為開心狀態( Positive outcome )，所以可以直接決定Overcast輸出為開心( Positive )。

##### Step 2. Conti. - 選擇下一個節點( 子樹的根結點 )

( 從何子節點開始建子樹？ I don't know yet. )

- Same steps as earlier but only examples sorted to the node are used in Gain computations.( 無法理解 )
- 選一個點( 隨機？ )繼續建子樹
 - Outlook = Sunny

$$Gain(S_{Sunny}, Humidity) = 0.97 - (3/5) \cdot 0 - (2/5) \cdot 0 = 0.97 bits$$

$$Gain(S_{Sunny}, Temperature) = 0.97 - (2/5) \cdot 0 - (2/5) \cdot 1 - (1/5) \cdot 0 = 0.57 bits$$

$$Gain(S_{Sunny}, Wind) = 0.97 - (2/5) \cdot 1 - (3/5) \cdot 0.918 = 0.019 bits$$

由上式可以看出來**Humidity**的亂度最小，所以選擇之為此子樹的根。

##### Final Decision Tree

![decisiontreelearning_finaldecisiontree](\willywangkaa\images\decisiontreelearning_finaldecisiontree.png)

# 熵、亂度 (Entropy)

---

## 介紹
在資訊理論中，熵被用來衡量一個**隨機變數出現的期望值**(機率與統計)。它代表了在被接收之前，訊號傳輸過程中損失的資訊量，又被稱為資訊熵。熵是對**不確定性**的測量。在資訊界，熵**越高**則能**傳輸越多的資訊**( 資訊越多意味著有**更多的可能性** )，熵**越低**則意味著**傳輸的資訊越少**( 資訊越少意味著有**更少的可能性** )。<br><br>
如果有一枚理想的硬幣，其出現正面和反面的機會相等，則拋硬幣事件的熵等於其能夠達到的最大值。我們無法知道下一個硬幣拋擲的結果是什麼，因此每一次拋硬幣都是不可預測的。( 越是不可預測的結果 $\rightarrow$ 亂度越大，而這種結果，正是造成人類**選擇障礙**的原因，所以我們希望熵越低越好，我們可以立即做出判斷 )

### $Ex1.$
使用一枚**正常硬幣**進行拋擲，這個事件的熵是一位元，若進行n次獨立實驗，則熵為$n$，因為可以用長度為 $n$ 的位元流表示。但是如果一枚硬幣的兩面完全相同，那個這個系列拋硬幣事件的熵等於**零**，因為結果**能被準確預測**。

### $Ex2.$
$Let \; y \; be \; a \; Boolean \; function, and \; let \; P \; denote \; Probability.$
What is the most pure (亂度低) probability distribution?
$$P(y = 0) = 1, P(y = 1) = 0$$
$$P(y = 0) = 0, P(y = 1) = 1$$


What is the most impure (亂度高) probability distribution?
$$P(y = 0) = 0.5, P(y = 1) = 0.5$$
意同於最大的亂度。

## 定義
首先，我們可以先從簡單的看討當目前的結果最多只有兩種情況，如拋硬幣，最多只有正面或是反面，下圖$x$軸$P_\oplus$代表擲出正面的機率函數，而$y$軸則是對應的熵值，而$P_\ominus$的機率軸則是會隨著$P_\oplus$下降而上升( 兩者互補 )，但是對應到的熵值會一樣大。


![decisiontreelearning_entropygraph](\willywangkaa\images\decisiontreelearning_entropygraph.png)

$S$ is a sample of training examples( 隨機變量 ).
當今天的結果只有正與反 ( 與硬幣一樣 )時，觀察目前的**隨機變量**
- 我們令：
 - $P_\oplus$ ( 就目前隨機變數產生的機率 ) is the portion of the positive examples ( 正面 ) in $S$.
 - $P_\ominus$ ( 就目前隨機變數產生的機率 ) is the portion of the negative examples ( 反面 ) in $S$.
Entropy ( 熵 ) measures the impurity ( 亂度 ) of $S$.

我們先定義熵值 ( Entropy ) 如下：
$$ Entropy( S ) = E( I( S ) ) = E(- \ln ( P ( S ) ) ) $$
其中，$E$為**期望函數**，$I( S )$是 $S$ 的**資訊量**（又稱為[資訊本體](https://zh.wikipedia.org/wiki/%E8%87%AA%E4%BF%A1%E6%81%AF)），$I( S )$也是一個**隨機變數**。
所以在取硬幣的樣本( $S$ )完後，我們可以將熵值寫成：






$$ Entropy( S ) = \sum_{i = 1}^{2} P(S_i)I(S_i)$$


$$ \Rightarrow -\sum_{i = 1}^{2} P(S_i)\log_{2} P(S_i) $$


$$ \Rightarrow  -P_{\oplus}\log_2 P_{\oplus} - P_{\ominus}\log_2 P_{\ominus}$$

**＜Note＞**
$$\sum_{i = 1}^N P_i = 1 \; and \;  0 \leq P_i \leq 1 $$

### **推廣至一般式**



![decisiontreelearning_generalentropygraph](\willywangkaa\images\decisiontreelearning_generalentropygraph.png)

當取自有限的樣本時，熵的公式可以表示為：
$$H(X) = \sum _{i} P(x_i) \, I(x_i)=-\sum_i P(x_i)\log _b P(x_i)$$

在這裏 $b$ 通常是$2$,自然常數 $e$，或是$10$。當$b = 2$，熵的單位是$bit$；當$b = e$，熵的單位是$nat$；而當$b = 10$,熵的單位是$Hart$。

**＜Note＞**
定義當$P_i = 0$時，對於一些 $i$ 值，對應的被加數 $0 \log_b 0$ 的值將會是 $0$，這與極限一致。
$$ \Rightarrow \lim_{p\to0+} ( p\log p ) = 0 $$

# CART (Classification and Regression Tree)

---

見 [Mr' opengate  - AI - Ch14 機器學習(2), 決策樹 Decision Tree](http://mropengate.blogspot.tw/2015/06/ai-ch13-2-decision-tree.html)


# 決策樹學習的常見問題

---

## 避免過度適配資料( Prevent Overfitting )

首先，相較於很冗長的樹，在機器學習中其實比較偏向於比較矮的樹，然而，為何？我們可以由[Occam’s Razor ( 奧坎剃刀 )](https://zh.wikipedia.org/wiki/%E5%A5%A5%E5%8D%A1%E5%A7%86%E5%89%83%E5%88%80)得知，若有兩個假說同時都能解釋該現象，我們偏向於比較沒那麼嚴個的假說( 可以表達比較廣的概念 )。<br>過度配適是指模型對於範例的過度訓練，導致模型記住的不是訓練資料的一般特性，反而是訓練資料的局部特性。對測試樣本的分類將會變得很不精確。

**＜注意＞**通常過度適配發生在訓練範例含有雜訊和離異值時，但當訓練數據沒有雜訊時，過度適配也有可能發生，特別是當訓練範例的數量太少，使得某一些屬性「恰巧」可以很好地分割目前的訓練範例，但卻與實際的狀況並無太多關係。

![decisiontreelearning_overfitting1](\willywangkaa\images\decisiontreelearning_overfitting1.png)

![decisiontreelearning_overfitting2](\willywangkaa\images\decisiontreelearning_overfitting2.png)

## 解決方案：修剪決策樹移除不可信賴的分支

- 事前修剪 (Prepruning) : 透過決策樹不再增長的方式來達到修剪的目的。選擇一個合適的臨界值往往很困難。
- 事後修剪 (Postpruning) : 
 - 子樹置換 (Subtree Replacement)：選擇某個子樹，並用單個樹葉來置換它。
 - 子樹提升 (Subtree Raising)：

![decisiontreelearning_subtreeraising](\willywangkaa\images\decisiontreelearning_subtreeraising.png)

## 合併連續值屬性

透過動態地定義新的離散值屬性來實現，即先把連續值屬性的值域分割為離散的區間集合，或設定門檻值以進行二分法。


## 屬性選擇指標的其他度量標準

- 訊息獲利 : 趨向於包含多個值的屬性
- 獲利比率 : 會產生不平均的分割，也就是分割的一邊會非常小於另一邊
- 吉尼係數 : 傾向於包含多個值的屬性，當類別個數很多時會有困難，傾向那些會導致平衡切割並且兩邊均為純粹的測試

**＜尚有其他的度量標準，也都各有利弊＞**

# 例題

---

- A data set has 4 Boolean variables. What is the maximum number of leaves in a decision tree?          $2^4$
- To each leaf in the decision, the number of corresponding rule is __1__
- If a decision tree achieves 100% accuracy on the training set, then it will also get 100% accuracy on the test set? __No__
- Using information gain to pick attributes, decision tree learning can be considered __A* search algorithm.__ __No__
- A decision tree can describe any Boolean function? __Yes__


# 補充

---

- C4.5 演算法
	- C4.5演算法利用屬性的獲利比率(Gain Ratio)克服問題，獲利比率是資訊獲利正規化後的結果。求算某屬性A的獲利比率時除資訊獲 利外，尚需計算該屬性的分割資訊值(Split Information) : SplitInfoA(S)=∑t∈T|Sj||S|×log2|Sj||S|

- C4.5的改善：對連續屬性的處理
	- 改善ID3傾向選擇擁有許多不同數值但不具意義的屬性：之所以使用獲利比率(Gain Ratio)，是因為ID3演算法所使用的資訊獲利會傾向選擇擁有許多不同數值的屬性，例如：若依學生學號(獨一無二的屬性)進行分割，會產生出許多分支，且每一個分支都是很單一的結果，其資訊獲利會最大。但這個屬性對於建立決策樹是沒有意義的。


- C5.0 演算法
	- C5.0 是 C4.5的商業改進版，可應用於海量資料集合上之分類。主要在執行準確度和記憶體耗用方面做了改進。因其採用Boosting方式來提高模型準確率，且佔用系統資源與記憶體較少，所以計算速度較快。其所使用的演算法沒有被公開。

- C5.0 的優點：
	- C5.0模型在面對遺漏值時非常穩定。 
    - C5.0模型不需要很長的訓練次數。 
    - C5.0模型比較其他類型的模型易於理解。 
	- C5.0的增強技術提高分類的精度。

# 參考

---

[Mr' opengate - AI - Ch14 機器學習(2), 決策樹 Decision Tree](http://mropengate.blogspot.tw/2015/06/ai-ch13-2-decision-tree.html)

[Wiki - 熵 - 資訊理論](https://zh.wikipedia.org/wiki/%E7%86%B5_%E4%BF%A1%E6%81%AF%E8%AE%BA)

[奧坎剃刀](https://zh.wikipedia.org/wiki/%E5%A5%A5%E5%8D%A1%E5%A7%86%E5%89%83%E5%88%80)