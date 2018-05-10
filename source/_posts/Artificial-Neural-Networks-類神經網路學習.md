title: Artificial Neural Networks 類神經網路學習
author: Willy Wang
tags:
  - Artificial Neural Networks
  - Gradient Decent
  - Perceptron trainning rule
  - ''
categories:
  - Machine Learning
date: 2018-04-25 22:48:00
---
# Artificial Neural Networks 類神經網路概論( 未完成 )

---


## 簡介

類神經網絡是一種受生物學啟發而產生的一種模擬人腦的學習系統。
[Youtube - 介紹神經元與類神經的關係](https://youtu.be/gcK_5x2KsLA)


















![neuralgraph](\blog\images\neuralgraph.png)











↑ 神經元示意圖( *synapse：比重 )

對於神經(neuron)我們有一個簡單的抽象：每個神經元是與其他神經元連結在一起的，一個神經元會受到多個其他神經元狀態的衝擊，並由此決定自身是否激發。

神經細胞透過輸入神經樹由其它神經細胞輸入脈波訊號後，經過神經細胞核的處理，其處理大約是：

1. 將收集到的訊號作**加總**
2. **非線性轉換**
3. 產生一個新的脈波信號

如果這個訊號夠強，則新的脈波信號會由神經軸傳送到輸出神經樹，再透過神經節將此訊號傳給其它神經細胞。值得注意的是：當訊號經過神經節後，由於**神經節加權值**的影響，其訊號大小值會改變。

神經網絡裡的結點相互連結決定了輸入的數據在裡面經過怎樣的計算。我們可以通過大量的輸入，讓神經網絡調整它自身的連接情況從而總是能夠得到我們預期的輸出。




![neuralgraph2](\blog\images\neuralgraph2.png)







**＜比較＞**電腦裡的模擬神經網路的架構需具備：

- 模擬頭腦神經的連結( 包含模擬突觸、細胞本體( 隱藏層 )、軸突 )
- 每個神經節點
- 實數的輸入與輸出
- 極大量的資訊
- 遷移學習( Transfer learning )
 	- [基礎概念 - 周莫烦 - 站在巨人的肩膀上, 迁移学习 Transfer Learning](https://www.youtube.com/watch?v=fCEHdyLkjNE)
 	- [實際應用( Python ) - 周莫烦 - 迁移学习 Transfer Learning](https://morvanzhou.github.io/tutorials/machine-learning/tensorflow/5-16-transfer-learning/)
 	- [什么是迁移学习 (Transfer Learning)？这个领域历史发展前景如何？](https://www.zhihu.com/question/41979241)

## 感知器 ( Perceptron )

設有 $n$ 維輸入的單個感知機( 從其他類神經元接收到的資訊 )，$a_1$ 至 $a_n$ 為 $n$ 維輸入向量的各個分量，$w_1$ 至 $w_n$ 為各個輸入分量連接到感知機的權值( 比重 )，$w_0$ 為偏置( 常數 )，一個神經元( Cell Body )分成兩個步驟，第一個 $\sum$ 為**彙總**資料，後面那個 $f(.)$ 為**傳遞函數**( 圖上的函數是"Sign function" )，判斷最後輸出的值， 最後以**純量輸出( 1 or -1 )**。

$$
Input: x_1, x_2, …,x_n
$$

$$
Output: 1 or  -1
$$










![perceptrongraph](\blog\images\perceptrongraph.png)







- **類神經元**示意圖
 	- **＜注意＞**$w_0$不是伴隨其他的資訊傳進神經元的，而是因為某些演算法的需求，而另外多加的一個**閾值**( 正負常數值 )。
 	- $ \sum_{i = 0}^n W_i X_i$又稱為**淨輸入( Net Input )**，可處理線性組合的假說空間( Hypotheses )。



![sgn_triggerfunction](\blog\images\sgn_triggerfunction.png)



上圖為此神經元判斷的觸發函數( 每個神經元的判斷都不盡相同，此為其中一種 )，帶入剛剛所算的淨輸入，計算輸出( -1就是判斷為無反應的狀況 )。

- **權值(**$w$**)**：如果當前神經元的某個輸入值權值為零，則當前神經元激發與否與這個輸入值無關；如果某個輸入值的*權重為正*，它對於當前神經元的激發值產生*正影響*。反之，如果*權重為負*，則它對激發值產生*負影響*。

- **偏移量(**$w_0$**)**：它定義了神經元的激發臨界值在空間上，它對決策邊界(decision boundary) 有*平移作用*，*就像常數作用在一次或二次函數上的效果*。感知器表示為**輸入向量與權向量內積**時，偏置被引申為**權量**，而對應的輸入值為 1。

- **決策邊界(decision boundary)**：設輸入向量與權向量的**內積為零**，可得出 n+1 維的**超平面**。平面的法向量為 w，並經過 n+1 維輸入空間的原點。法向量指向的**輸入空間**，其輸出值為**+1**，而與法向量**反向的輸入空間**，其輸出值則為**−1**。故可知這個超平面**定義了決策邊界**，並把輸入空間劃分為二。















![decisionboundery](\blog\images\decisionboundery.png)









- **激勵函數(activation function)**：激勵函數代表神經元在什麼輸入情況下，才觸發動作。




![activationfunction](\blog\images\activationfunction.png)



### 感知器可以「學習」的函數





![singlelayertwoinputperceptron](\blog\images\singlelayertwoinputperceptron.png)






Consider a 2-input perceptron ( 感知器 ) : 
It outputs 1 iff

$$
o( x_1, x_2 ) = ( w_0+w_1 \cdot x_1+w2 \cdot x2 > 0 )?
$$

<div style="text-align: center"> equivalent to </div>

$$
o( x_1, x_2 ) = sgn( w_0+w_1 \cdot x_1+w2 \cdot x2 )
$$



#### What weights represent $AND (x1, x2)$? 


$w_0 = -0.8, w_1 = w_2 = 0.5$<br>$o( x_1, x_2 ) \Rightarrow sgn(-0.8  + 0.5 \cdot x_1 + 0.5 \cdot x_2 )$


![weighttorepresentAND](\blog\images\weighttorepresentAND.png)






#### What weights represent $OR (x1, x2)$? 

$w_0 = 0.3, w_1 = w_2 = 0.5$<br><br>$o( x_1, x_2 ) = sgn(0.3  + 0.5 \cdot x1 + 0.5 \cdot x2 )$


![weighttorepresentOR](\blog\images\weighttorepresentOR.png)







#### What weights represent $NOT (x1, x2)$? 
$w_0 =0.0, w_1 = -1.0, w_2 = 0$<br><br>$o(x_1) = sgn( 0.0 –1.0x_1)$

![weighttorepresentNOT](\blog\images\weighttorepresentNOT.png)





#### What weights represent $XOR (x1, x2)$?

Not possible.

![possibletorepresentXOR](\blog\images\possibletorepresentXOR.png)










**＜NOTE＞**Not linearly separable $\rightarrow$ Can not be represented by a **single percepton**.<br><br>
**Solution: use multilayer networks.**



![multilayertorepresentXOR](\blog\images\multilayertorepresentXOR.png)







## How to Determine a Weight Vector? <br>如何決定權重向量？

------

在類神經網路學習的過程中，最重要的就是權重向量( Weight Vector )，因為這就是決定到時候感知器( Perceptrons )能不能做出正確預測( correct $\pm 1$ output )的關鍵依據。

通常來說，都會給定一組訓練範例( Trainning example )，而且，每個元素裡必定會含有輸入( Input )與輸出( Output )。

- $( x_1,  x_2, x_3, \ldots , x_{n-1}, x_n )$ 是訓練範例中會給的資訊。
- $+1 \; or \; -1$ 為 $( x_1,  x_2, x_3, \ldots , x_{n-1}, x_n )$ 的已知輸出( Target value )。

而我們的目標就是將 $( w_1,  w_2, w_3, \ldots , w_{n-1}, w_n )$ 訓練出來。

解決的演算法有很多，在這邊只討論其中兩個：

- The perception trainnin rule
- Gradient decent ( or call the delta rule )





### Perceptron Training Rule

- $t  = c(x_1, x_2, x_3, ..., x_n )$ 是我們**已知道**的結果( 1 or -1 )。
- $o：$對於訓練資料$ (x_1, x_2, x_3, ..., x_n ) $以感知器( Perceptron )測試後出來的結果( 1 or -1 )。
**＜Note＞：**Here o is the output of Perceptron, not the target value.

- 所以 $ ( t - o ) $ 為此時感知器( Perceptron )的誤差，然後藉由我們設定的 $\eta$ 函式判斷要對目前的 $w$ 修正多少值。


#### 演算法

- Initialize weights (w0, w1, w2, x3, ...,wn )  to random values
- Loop through training examples：
 $w_i \leftarrow w_i + \Delta w_i$
 Where $\Delta w_i = \eta (t-o) \cdot x_i$ and $\eta$ is a learning rate (small positive value, e.g., 0.1)<br><br>

- Given training data set

$$
D = \{ ( \vec{x}, t ) \}
$$

```cpp
//Initialize all weights w_i to random values
w[] <- random values
WHILE not all examples correctly predicted DO
	FOR each training example 
	x = (x_1, x_2, x_3, ..., x_n ) in D
		Compute current output o ( x_1, x_2, x_3, ..., x_n ) 
		FOR i = 0 to n
		    // always let x_0=1
			w_i  w_i + eta(t - o) * x_i   
```

**＜Note＞** If (t-o) = 0, no change in weight.

輪過一遍所有訓練資料，稱之為一個時代( Epoch )，若一個時代過後還有 $w_i$ 是錯誤的就繼續修改 $w_i$ ，直到某個時代所有的 $ w_i $ 可以讓 $ x_i $ 輸出正確。
**＜注意＞：**如果訓練資料是**線性可分離**( XOR就不可線性分離 )，且$\eta$是小於1的很小的值，那麼一定最後可以在**有限的世代**找到最後的感知器( Perceptron )。<br><br>

倘若今天的資料是無法被線性分離的改如何處理？
- **Approach 1:** 建立一個演算法可以努力找到逼近值。
   E.g. gradient descent method ( 梯度下降法 )
- **Approach 2:** 建立不同架構或多層( Multilayer networks )結構的神經網路以突破限制。




### Gradient Descent

[Youtube - Gradient Decent 介紹](https://www.youtube.com/watch?v=_-02ze7tf08)

我們需要在 $(n+1)$ 維的假說向量空間( Hypotheses Space )中搜索最合適( Best fit )的權值向量，我們需要有一定的規則指導我們的搜索，採用沿著梯度**反方向**往下走的方法，就稱為「梯度下降法」(Gradient Descent)。這種方法可以說是一種「貪婪演算法」(Greedy Algorithm)，因為它每次都朝著最陡的方向走去，企圖得到最大的下降幅度。即使訓練資料是不可線性分離的( Not lineary separable )，最後這個演算法還是會收斂在極趨近於目標想法的銓重向量停止。

**＜注意＞：** *Least square*為最常用來檢測誤差的方法。

為了要計算梯度，我們不能採用不可微分的 sign() 步階函數，因為這樣就不能用**微積分**的方式計算出**梯度**了，而必須改用可以微分的連續函數 sigmoid()，這樣才能夠透過微分計算出梯度。



$$
E(w) = \frac{1}{2} \sum_{d \in D} ( t_d - o_d )
$$



上面公式中$D$代表了所有的輸入案例( 或者說是樣本 )，$d$代表了一個樣本實例，$o_d$表示感知器的輸出，$t_d$代表我們預想的輸出。



首先，我們先看看權重( Weight vector )向量 $w$ 的梯度( Gradient )為何：





$$
\bigtriangledown E( w ) = \frac{\partial E}{\partial w} = ( \frac{\partial E}{\partial w_0}, \frac{\partial E}{\partial w_1}, \ldots, \frac{\partial E}{\partial w_n} )
$$



**＜注意＞：**  *梯度* 是一個裡面所有元素為對 $E$ 以對每個 $w_i$ 偏微分的**向量**。且這個向量指向的地方為最上坡之處。( 如下圖紅色處顯示，而下方黑色箭頭則表示該梯度投影下來所對應的方向 )






![gradient](\blog\images\gradient.png)














所以 **Gradient Descent** 就是該點梯度的 *反方向* ，也就是最下坡的方向，$i.e. \; -\bigtriangledown E(w)$。

這樣目標就明確了，欲在假說空間找到一組權值 $w$ 讓這個誤差的值最小，顯然我們用**誤差對權值**求導將是一個很好的選擇，導數的意義是提供了一個方向，沿著這個方向改變權值，將會讓總的**誤差變大**，更形象的叫它為梯度。

既然梯度確定了E最陡峭的上升的方向，那麼梯度下降的訓練法則是：

$$
\vec{w_i} \leftarrow \vec{w_i} + \Delta \vec{w_i}, \quad where \; \Delta \vec{w_i} = \eta \frac{\partial E}{\partial w_i}
$$









![gradientgraph](\blog\images\gradientgraph.png)






#### $E.x.$

- Example: two weights: $w = (w_0, w_1)$

  - Error surface $E$ is parabolic (by definition)
  - Single global minimum
  - Arrow: negated gradient at one point
  - Steepest descent along the surface











![examplegradient](\blog\images\examplegradient.png)











For the least square error function, gradient is easy to calculate:

$$
\bigtriangledown E( w ) = \frac{\partial E}{\partial w} = \frac{1}{2} \cdot \frac{\partial \sum_{d \in D} (t_d - o_d)^2}{\partial w_i} = \frac{1}{2} \sum_{d \in D}\frac{\partial (t_d - o_d)^2}{\partial w_i}
$$



$$
\Rightarrow \frac{1}{2} \cdot \sum_{d \in D} (2 \cdot (t_d - o_d)\frac{\partial( t_d - o_d )}{\partial w_i}) = \sum_{d\in D}((t_d - o_d)\frac{\partial(t_d - w\cdot x_d)}{\partial w_i})
$$

$$
\Rightarrow \sum_{d\in D} ((t_d - o_d)(-x_{id}))
$$

依上述，公式就可以簡化成：

$$
\Delta w_i = -\eta \frac{\partial E}{\partial w_i}
$$

$$
and
$$

$$
\frac{\partial E}{\partial w_i} = \sum_{d \in D}((t_d - o_d)(-x_{id}))
$$



最後公式變成：

![gradientdecentfinalformula](\blog\images\gradientdecentfinalformula.png)



# 參考

---
[Mr' opengate - AI - Ch16 機器學習(4), 類神經網路 Neural network](http://mropengate.blogspot.tw/2015/06/ch15-4-neural-network.html)

[Wikipedia - 人工神經網路](https://zh.wikipedia.org/wiki/%E4%BA%BA%E5%B7%A5%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C)

[基礎概念 - 周莫烦 - 站在巨人的肩膀上, 迁移学习 Transfer Learning](https://www.youtube.com/watch?v=fCEHdyLkjNE)

[實際應用( Python ) - 周莫烦 - 迁移学习 Transfer Learning](https://morvanzhou.github.io/tutorials/machine-learning/tensorflow/5-16-transfer-learning/)

[什么是迁移学习 (Transfer Learning)？这个领域历史发展前景如何？](https://www.zhihu.com/question/41979241)