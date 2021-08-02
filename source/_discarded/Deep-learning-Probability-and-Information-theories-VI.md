title: Deep learning - Probability and Information theories VI
author: Willy Wang (willywangkaa)
tags:
  - Probability
  - Information theories
categories:
  - Deep learning
date: 2019-04-24 12:33:00
---
# Deep learning - Probability and Information theories VI



## Application：Decision tree（決策樹） and random forest（隨機森林）

- 廣泛使用的一種**歸納推理演算法**
- **對於干擾直相當敏感**



### Impurity measure（亂度的測量）

$$
\arg \underset{j,v}{\max}\left(\mathrm{Impurity(\mathbb X^{parent})-Impurity(\mathbb X^{left},\mathbb X^{right})}\right)
$$

> **亂度為熵的非正式定義**
>
> - 給定一組**標記過**的樣本資料組
>   - $\mathbb X = \{(\boldsymbol x^{(i)}, y^{(i)})\}_{i = 1}^N$
>   - $\boldsymbol x$ 為一個向量， $j–th$ 維度該樣本的一個**特徵（Attribute）**
>     - 向量元素 $x_j$ 代表著一個該資料對應該特徵的一個**狀態（State of j-th attribute）**
>   - $y$ 為 $\boldsymbol x$ 經過一個隨機變數 $X$ 所對應的一個**標記狀態**
>     - 由於隨機變數 $X$ 是人決定的，所以可以將這個對應稱為一種**標記**
> - 欲經一樹狀函數 $f$（*規則*），將樣本的**特徵狀態** $\boldsymbol x^{(i)}$ 可以映射到**人為標記狀態** $y^{(i)}$ （Label）
>   - $f(\boldsymbol x^{(i)}) = y^{(i)}$
>
> ![1556032997160](C:\Users\hp\Desktop\筆記\吳尚鴻深度學習\deeplearning_randomvariables_probabilitydistribution_VI\1556032997160.png)
>
> - $\mathbb X^{\mathrm{parent}}$ 為決策樹裡其中一個**父節點**
>   - 代表一組**子樣本資料**
>     - $\mathbb X^{\mathrm{parent}} \subseteq \mathbb X$
> - $\mathbb X^{\mathrm{left}}、\mathbb X^{\mathrm{right}}$ 為對應於上述**父節點**的左右子節點
>   - 代表一組**子樣本資料**
>     - $\mathbb X^{\mathrm{left}} \subseteq \mathbb X \\\mathbb X^{\mathrm{right}} \subseteq \mathbb X$
>     - $\mathbb X^{\mathrm{left}}+\mathbb X^{\mathrm{right}}=\mathbb X^{\mathrm{parent}}$



- $\mathrm{Impurity}(\mathbb X^{\mathrm{parent}}) = \mathrm H[Y～\mathrm{Empirical(\mathbb X^{\mathrm{parent}})}]$
  - 「父節點的亂度」為 $Y～\mathrm{Empirical(\mathbb X^{\mathrm{parent}})}$ 的**熵**
  - $P_Y(y)=\mathbb X^{\mathrm{parent}} 中的被標記比例為\;y \;的比例$

- $\mathrm{Impurity(\mathbb X^{left},\mathbb X^{right})} = \sum_{i = \mathrm{left,right}}\frac{\vert\mathbb X^{(i)}\vert}{\vert\mathbb X^{\mathrm{parent}}\vert}\mathrm H[Y～\mathrm{Empirical(\mathbb X^{(i)})}]$

  - $\vert\mathbb X^{(i)}\vert$ 為該樣本資料組的數量
  - 「子節點的亂度」為左右子點對 $Y～\mathrm{Empirical(\mathbb X^{(i)})}$ 的熵，並依比對父點的資料量比例加總
    - **「子節點的亂度」≦「父節點的亂度」**
    - 目標是找到一種父節點的資料分類方法，將「子節點的亂度」最小化

- $\mathrm{Impurity(\mathbb X^{parent})-Impurity(\mathbb X^{left},\mathbb X^{right})}$ 稱作**「資訊獲利」（Information gain）**

  - 越大代表「子節點的亂度」越小
  - 資料分類方法的一種效能評鑑

- $(j,v)$ ：斷點

  - 在 $\boldsymbol x$ 還沒判斷的**特徵空間**中找到 $j–th$ 維度，選擇一個定值 $v$
  - 節點中的資料必須符合 $\mathrm{Rule}$ 的規則
  - 將父節點 $\mathbb X^{\mathrm{parent}}=\{(\boldsymbol x^{(i)}, y^{(i)})：\mathrm{Rule}\}$ 分支為兩個節點

    - $\mathbb X^{\mathrm{left}} = \{(\boldsymbol x^{(i)}, y^{(i)})：\mathrm{Rule}\cup\{x_j^{(i)}<v\}\}$
    - $\mathbb X^{\mathrm{right}} = \{(\boldsymbol x^{(i)}, y^{(i)})：\mathrm{Rule}\cup\{x_j^{(i)}\geq v\}\}$

![1556075449719](C:\Users\hp\Desktop\筆記\吳尚鴻深度學習\deeplearning_randomvariables_probabilitydistribution_VI\1556075449719.png)



### Decision tree



**Training a decision tree**

1. 建構一根節點 $\{(\boldsymbol x^{(i)}, y^{(i)})：\mathrm{Rule = \O}\}$
   - 節點中的資料必須符合 $\mathrm{Rule}$ 的規則
   - 最開始不設定規則 $\O$，所以此點資料等於 $\mathbb X$
2. 往下分支兩個子節點 $\mathbb X^{\mathrm{left}}、\mathbb X^{\mathrm{right}}$
   - 以 $\arg \underset{j,v}{\max}\left(\mathrm{Impurity(\mathbb X^{parent})-Impurity(\mathbb X^{left},\mathbb X^{right})}\right)$ 為依據，找到符合的 $\mathrm{Rule}$
   - 對其子節點遞迴此步驟，直到該節點的亂度等於 $0$（Pure），也就是其節點的**人為標記狀態**全部相等



### Random forests

> 隨機森林為多個決策樹所構成
>
> - 決策樹模型可能會被訓練得非常高
>   - 越高的節點其 $\mathrm{Rule}$ 會越嚴格
>   - 在**訓練資料樣本**可以判斷很好，但是在**測試資料樣本**最壞可能完全無法利用
>   - 解決此問題讓決策樹可以更廣泛應用（Generalizability）
>     - 剪枝（Pruning）：限制決策樹的高度，但可能葉節點中的**標記狀態**不會全部相等
>     - **隨機森林**：多棵剪枝過的決策樹之**集成**
> - 機器學習中的「集成」（Ensemble）
>   - 一般機器學習演算法只使用單一模型來學習，可能在該樣本之外會無法利用
>   - 結合多個**弱機器學習**以建構一個**強穩的模型**，避免**過適現象（Overfit）**的發生



**Training a random forest**

1. 取得一個**自助樣本（Bootstrap sample）**
   - **自助樣本**：在原始的訓練資料集中**隨機挑選** $M$ 個樣本（挑選過可以重新再挑選）
2. 使用自助養本，訓練過程中
   1. **隨機挑選** $K$ **個特徵**
   2. 在該範圍找到最好的斷點 $(j,v)$ 來分支
3. 重複第一第二步得到 $T$ 個決策樹



- 由 $T$ 個決策樹以**多數決（Majority vote）**來判斷**測試資料樣本**
  - 每個決策樹對樣本有不同的見解，取其多數以判斷
- 隨機森林的訓練與決策樹的訓練有些許的不同
  - 第一步與第二之一步

![1556079393095](C:\Users\hp\Desktop\筆記\吳尚鴻深度學習\deeplearning_randomvariables_probabilitydistribution_VI\1556079393095.png)

![1556079400721](C:\Users\hp\Desktop\筆記\吳尚鴻深度學習\deeplearning_randomvariables_probabilitydistribution_VI\1556079400721.png)