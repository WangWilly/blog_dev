title: Machine learning - 機器學習地圖
author: Willy Wang
tags:
  - Map
categories:
  - Machine Learning
date: 2018-04-20 10:51:00
---
# 機器學習地圖

---


## 類別


- **監督學習 (Supervised learning)**：從**給定的訓練數據集**中學習出一個模式（函數 / learning model），當新的數據到來時，可以**根據這個模式預測**結果。監督學習的訓練集**要求**是包括**輸入**和**輸出**，也可以說是**特徵和目標**。訓練集中的目標是由**「人」**標註的。常見的監督學習算法包括**回歸分析**和**統計分類( Classify )**。

- **無監督學習 (unsupervised learning)**：與監督學習相比，訓練集**沒有人為標註的結果**。常見的無監督學習算法有**聚類( Cluster )**。

- **半監督學習 (Semi-supervised learning)**：介於監督學習與無監督學習之間。

- **增強學習 (reinforcement learning)**：通過**觀察**來學習做成如何的動作。每個動作都會對環境有所影響，學習對象根據**觀察到的周圍環境**的**反饋( 獎勵 )**來做出判斷。

## 機器學習演算法種類

- 構造條件機率：回歸分析和統計分類
	 - 人工神經網絡
	 - 決策樹
	 - 高斯過程回歸
	 - 線性判別分析
	 - 最近鄰居法
	 - 感知器
	 - 徑向基函數核
	 - 支持向量機
- 通過再生模型構造機率密度函數：
	 - 最大期望算法
	 - graphical model：包括貝葉斯網和Markov隨機場
	 - Generative Topographic Mapping
- 近似推斷技術：
	 - 馬爾可夫鏈
	 - 蒙特卡羅方法
	 - 變分法
- 最優化：大多數以上方法，直接或者間接使用最優化算法。




![machinelearningmap](\willywangkaa\images\machinelearningmap.png)





















![kindofmachinelearning](\willywangkaa\images\kindofmachinelearning.png)























# 參考

---
[Mr' OpenGate - AI - Ch13 機器學習(1), 機器學習簡介與監督式學習 Introduction to Machine Learning, Supervised Learning](http://mropengate.blogspot.tw/2015/05/ai-supervised-learning.html)