title: Operating System - Deadlock
author: Willy Wang
tags:
  - Deadlock
categories:
  - Operating System
date: 2018-07-10 21:54:00
---
# Deadlock

---



系統中存在一組 processes，彼此形成 *循環等待* 的情況，**造成這些 processes 皆無法往下執行，並降低產量 ( throughput ) 的現象。**




![1530163891325](\willywangkaa\images\1530163891325.png)



- 死結成立的四個必要條件 ( 所以有一個不成立，則死結必不發生，**但是若四個條件全部成立時，死結不一定會發生。** )<br>If there are 4 conditions are true, then the deadlock **will/can** arise. $\rightarrow$ **false/true.**
  - Mutual exclusion：**對於 Resource 而言，**具有此性質的 resource ，在**任何時間點最多只允許一個 process 持有/使用**，不可多個 processes 同時持有/使用。<br>$Ex. $ 大多數的資源皆具有此性質。如：CPU, memory, disk, printer...<br>$Counter \; Ex. $ **Read-only file 不具此性質。**
  - Hold and wait：**Process 持有部分的資源，且又在等待其他 processes 所持有的資源。**
  - No preemption：**Process 不可以任意剝奪其他 processes 所持有的資源**，必須等待對方釋放資源才有機會取得資源。<br>**＜Note＞：**若可 Preemption 則必無 Deadlock 頂多只有 Starvation。
  - Circular waiting：**系統中存在一組 Processes 形成循環等待之情況。 **




![1530163425234](\willywangkaa\images\1530163425234.png)



＜Note＞<br>1. (恐龍版本)Circular waiting  代表 Hold and wait。<br>2. (其他版本)Circular waiting  代表 Mutual exclusion、Hold and wait、No preemption。<br>3. **為何 Single-process 必不會造成 Deadlock ？ 因為 Circular waiting 不存在，所以 Deadlock 不發生。**



- 交通十字路口 Deadlock
  - **路口：資源**
  - **車子：Process**




![deadlock](\willywangkaa\images\deadlock.png)



- 與 Starvation 比較。
  - 相同之處為都是 *資源分配管理機制設計不恰當所導致。*



|                           Deadlock                           |                          Starvation                          |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| 一組 Processes 形成 Circular waiting ，造成這些 Process 皆無法往下執行。 | Process 因為長期無法取得完工所需的各式資源，造成遲遲無法完工。 |
|                      不可能有機會完工。                      |       有完工的機會，但是機會渺茫、Indefinited block。        |
|                 會連帶造成 Throughput 低落。                 |   與 Throughput 高低無關聯。(其中 SJF、SRTF 效能示好的。)    |
|              一定會在 Non-preemption 的環境下。              |                容易發生在 Preemption 的環境。                |
|  解決方案：Prevention、Avoidance、Detection and recovery。   |                   使用「Aging」技術解決。                    |



## Resource allcation graph ( R. A. G. ) 資源分配圖



令 $G= ＜V, E＞$ 為一有向圖，代表 R.A.G.，其中：

- Vertex ( 頂點 )
- Process

  


![1530164365297](\willywangkaa\images\1530164365297.png)



- Resource




![1530164408843](\willywangkaa\images\1530164408843.png)



其中「。」數目代表該項資源的數目。

- Edge ( 邊 )：
- Allocation edge




![1530164614789](\willywangkaa\images\1530164614789.png)



- Request edge




![1530164672585](\willywangkaa\images\1530164672585.png)

- $Ex. $




![1530166364004](\willywangkaa\images\1530166364004.png)



### 結論

- No cycle $\rightarrow$ No deadlock。

- **Cycle 不一定有 deadlock。**




![1530166810415](\willywangkaa\images\1530166810415.png)



此圖雖然有 Cycle，但因為 P3 必可以完工且會釋出一個 R2 Resource，再配置給 P2 使用，所以無死結。




![1530166364004](\willywangkaa\images\1530166364004.png)

因為 P4 必可以完工且會釋出 R3 Resource，再配置給 P3 使用，所以無死結。



- **除非每一類型的資源皆為 single-instance ( 單一數量 )，則有 Cycle 必有死結。**





## 解決 Deadlock



Prevention、Avoidance

- Pros
  - **保證系統為 deadlock free。( 不可能進入「Deadlock state」)**
- Cons
  - **對資源的使用與限制與取得限制多，所以 Resource utilization 偏低，連帶 throughput 偏低。**
  - **不可能造成 Starvation。**

Detection and Recovery

- Pros
  - **Resource utilization 相對較高， throuput 也較高。**
- Cons
  - **System 可能進入 deadlock state。**
  - **Detection and recovery 的成本很高。**



### Deadlock prevention



**破除四個必要條件之其中一個，則死結必不發生。**

- 針對「Mutual exclution」
  - **因為 Resource 本來就有的性質，以致無法解決「Mutual exclution」帶來的問題。**
- 針對「Hold and wait」
  - 方法一：作業系統可以決定**除非該 Process 可以一次取得全部所需資源，才允許持有資源，否則不得持有任何資源。**
  - 方法二：**Process 可先持有部分資源，但當該 Process 要申請其他資源時，必須先釋出所持有的資源，才可以提出申請。**
- 針對「No preemptive」
  - **將該法則改為可強奪( Premptive )，並以優先權作為搶奪基準。**( 但可能會造成 Starvation )



| Process | 持有的資源 | 欲申請的資源         |
| ------- | ---------- | -------------------- |
| P1      | R1         | R3 ( 可以申請 )      |
| P2      | R5         | R3 ( 必須先釋出 R5 ) |
| P3      | R1、R5     | R3  (必須先釋出 R5 ) |



- *針對「Circular waiting」
  - 「Resource ordering」：**作業系統賦予每一類型資源一個唯一的 Resource ID，再規定 Process 必須按照 Resource ID asending (遞增) 的方式對資源提出申請。**
  - 證明「Resource ordering」：<br>假設在這樣的規定下，系統仍存在一組 Processes 形成 Circular waiting 如下圖，依規定，我們可以推導出資源 ID 大小關係如：$r_0 < r_1 < r_2 \ldots < r_n \quad , r_i \; is \; unique.$，但又 $r_0 < r_n < r_0$ ，矛盾，所以 Circular waiting 必不存在。




![1530168967415](\willywangkaa\images\1530168967415.png)



### Deadlock aviodance



當某個 process  提出某些資源申請時，則作業系統必須執行「Banker's algorithm」以確認**倘若分配給 Process 其申請資源後，系統未來是否處於「Safe state」；若為「Saft state」，則核准申請，否則為「Unsafe state」系統會否決該資源申請，**Process 必須等一段時間後再重新申請資源，系統會再確認當時的狀態是否安全。




![1528970642055](\willywangkaa\images\1528970642055.png)

Deadlock 是 Unsafe 集合的 Subset。



#### Banker's Algorithm



- 資料結構
  - n：Process 個數。
  - m：Resource 種類數。
  - $Request_i：[1 \ldots m] \; of \; int \Rightarrow P_i$ 提出的各式資源申請量。
  - $Allocation：n \times m \; matrix \Rightarrow$ 各個 Process 目前持有的各式資源數量。
  - $Max：n \times m \; matrix \Rightarrow$ 各 Process 完工所需之各式資源最大數量。
  - $Need：n \times m matrix = Max - Allocation \Rightarrow$ **各 Process 還需各式資源才能完工。**
  - $Available：[1 \ldots m] \; of \; int = 資源總量 - Allocation\Rightarrow$ **系統目前可用的各式資源數量。**


![1528971381784](\willywangkaa\images\1528971381784.png)



- 演算步驟
  1. 確認 $Request_i \leq Need_i$<br>**若成立進入第二步，否則因為申請不合理導致終止** $P_i$。
  2. 確認 $Request_i \leq Available$<br>**若成立進入第三步，否則** $P_i$ **一直等待直到資源可以使用時。**
  3. ( 演算結果 ) $Allocation_i = Allocation_i + Request_i \quad Need_i = Need_i - Request_i \quad Available = Available - Request_i$
  4. **依照第三步的演算值執行「Safety Algorithm」，若回傳「Saft state」則可以核准該申請，但若回傳「Unsafe state」則核駁該次申請。**<br>$P_i$ **必須等待一段時間再重新提出申請。**



#### Safty algorithm



- 資料結構
  - n：Process 個數。
  - m：Resource 種類數。
  - $Request_i：[1 \ldots m] \; of \; int \Rightarrow P_i$ 提出的各式資源申請量。
  - $Allocation：n \times m \; matrix \Rightarrow$ 各個 Process 目前持有的各式資源數量。
  - $Max：n \times m \; matrix \Rightarrow$ 各 Process 完工所需之各式資源最大數量。
  - $Need：n \times m matrix = Max - Allocation \Rightarrow$ **各 Process 還需各式資源才能完工。**
  - $Available：[1 \ldots m] \; of \; int = 資源總量 - Allocation\Rightarrow$ **系統目前可用的各式資源數量。**
  - $Work：[1\ldots m] \; of \; int \Rightarrow$ **代表系統目前可用的資源累積數量。**
  - $Finish：[1\ldots n] \; of \; int$ <br>$\Rightarrow \{\begin{matrix}True \quad 代表P_i可完工 \\False \quad 尚未完工  \end{matrix}$
- 演算步驟
  1. 設定初值<br>$Work = Availabe \quad Finish[i] \; \forall i, 1 \leq i \leq n \; 設定為 \; false  $
  2. **試找一個** $P_i$ 滿足：<br>$Finish[i] 為 False$<br>$Need_i \leq Work$<br>若可以找到進入第三步，否則進入第四步。
  3. **設定**$Finish[i] = True \quad Work = Work + Allocation_i$，**接著進入第二步。**
  4. 確認 $Finish$ 陣列，若全部皆為 True 回傳「Safe state」，否則回傳「Unsafe state」。

- Safe sequence / Safe stete
  - **至少可以找出大於一組 「Safe sequence」稱為 Safe state 否則稱為 Unsafe state ，代表作業系統未來依此 Processes 順序可分配各 Process 所需的資源使得大家皆可以順利完工。**



#### 實際演練一

- 5 個 Processes：$P_0, \ldots, P_4$
- 3 種類型的 Resource：$A, B, C$
- 起始資源量：$(A, B, C) = (10, 5, 7)$<br>求取 $Need[]$ 與$Available[]$<br>$P_1$ 提出 $(A, B, C) = (1, 0, 2)$ 之資源申請，請問是否予以核准或是核駁，請說明。




![1531184487179](\willywangkaa\images\1531184487179.png)


![1531184825933](\willywangkaa\images\1531184825933.png)

- **Banker's algorithm**

  - $Request_1 = (1, 0, 2)$

  1. 確認 $ Request_1 (1, 0, 2) \leq Need_1(1, 2, 2)$，OK ( goto step2 )。
  2. 確認 $ Request_1 (1, 0, 2) \leq Available(3, 3, 2)$，OK ( goto step3 )。
  3. Safty algorithm。

- Safty algorithm

  1. Initial value：$Work = Available = ＜2, 3, 0＞$
  2. 因為可以找到一個 Process $P_1$ 供應 Resource 並使之得以完成該工作。<br>滿足：$Finish[1] = False\quad Need_1 \leq Work$，OK ( goto step3 )。
  3. **設定** $Finish[1] = True \quad Work = Work + Allocation_i$，**goto step2。**<br>...**在 Step2 不斷重複的尋找是否有 $P_i$ 尚未完工且目前資源可以予以完工**...
  4. 最後確認 $Finish[]$ 全皆為 $True$ 後回傳「Safe state」。

- 列出其 Safe sequence

  - $＜P_1, P_3, P_4, P_0, P_2＞$




#### 實際演練二



- 5 個 Processes：$P_0, \ldots, P_4$
- 3 種類型的 Resource：$A, B, C$
- 起始資源量：$(A, B, C) = (10, 5, 7)$<br>求取 $Need[]$ 與$Available[]$<br>$P_4$ 提出 $(A, B, C) = (3, 3, 0)$ 之資源申請，請問是否予以核准或是核駁，請說明。



![1531184487179](\willywangkaa\images\1531184487179.png)

![1531184825933](\willywangkaa\images\1531184825933.png)

- Banker's algorithm
  1. 確認 $ Request_4 (3, 3, 0) \leq Need_4(4, 3, 1)$，OK ( goto step2 )。
  2. 確認 $ Request_1 (3, 3, 0) \leq Available(3, 3, 2)$，OK ( goto step3 )。
  3. Safty algorithm。
- Safty algorithm
  1. Initial value：$Work = Available = ＜0, 0, 2＞$
  2. 因為不能找到一個 Process $P_i$ 供應 Resource 並使之得以完成該工作 Fall ( goto step4 )。
  3. 最後確認 $Finish[]$ 非全皆為 $True$ 後回傳「Not safe state」。



#### Banker's algorithm 的 Time complexity



- 令 n：process 數目、m：resource 種類數。
  - Step1 需要 $O(m)$。(檢查 request 是否大於 need)
  - Step2 需要 $O(m)$。(檢查目前剩下資源是否充足可以給予 request)
  - Step3 需要 $O(m)$。(若可以給予，將原本的 need、allocation、available)
  - Safty algorithm 的 time complexity。
- Safty algorithm
  - 初值設定、每次工作需要 $O(m)$、將所有 processes 確認一遍需要 $O(n)$。
  - **最多檢查 **$n + (n-1) + (n-2) + \ldots + 1 = \frac{(n+1) \cdot n}{2}$ 個 process 每次檢查 $Need_i \leq work$ 需要 $O(m)$ ，加總起來為 $\rightarrow  O(n^2 \cdot m) $。
  - 使用 $ O(n) $ 確認所有 processes 都做完檢查。



#### Single-instance resource algorithm



- 可以化簡較為簡易的 avoidance 檢查方法。
- 利用 RAG，加上**「Claim edge」使用。**




![1530172024919](\willywangkaa\images\1530172024919.png)



- clam edge (虛線)：代表 $P_i$ **未來**會對 $R_j$ 提出申請。(即為 Max/Need 的意義)



- 演算法 ( 當 $P_i$ 提出 $R_j$ 申請 )

  1. 檢查原本有無該宣告邊之存在，若有進入第二步，否則終止 $P_i$。
  2. 確認 $R_j$ 是否可供使用，若可以進入第三步，否則 $P_i$ 等待該資源 ( 由 Claim edge 轉為 Request edge )。
  3. **( 演算結果 ) 暫時將宣告邊改為配置邊( Allocation edge )，進入第四步**。
  4. **確認途中是否有 Cycle 存在，若無則為「Safe state (核准)」，若有 cycle 為「Unsafe ( 核駁 )」**。
- $Ex .$




![1530172534581](\willywangkaa\images\1530172534581.png)

- 若$P_1$ 對 $R_2$ 提出申請是否核准？**因為沒有 Cycle 存在，所以 Safe 核准申請。**




![1530172797528](\willywangkaa\images\1530172797528.png)

- 若為 $P_2$ 對 $R_2$ 提出申請是否核准？**因為有 cycle 存在，所以核駁申請。**




![1530172966437](\willywangkaa\images\1530172966437.png)



- **＜Note＞：Deadlock 位於 Unsafe state 的集合之中，也就是說若目前的狀態是 unsafe state 有可能會導致死結，但也有可能不會導致死結。**





#### *＜Theorem＞ Deadlock free




![1530173438706](\willywangkaa\images\1530173438706.png)



系統若有 n 個 processes、m 個 resources intsance ( 單一種類 )，**且滿足下列條件**：

1. $1 \leq Max_i \leq m$ ，單一 Process 不得要求超過該種類資源的上限。
2. $\sum_{i = 1}^n Max_i < n + m \Rightarrow \sum_{i = 1}^n Max_i - n < m$ ，假設目前所有的 processes 都剩下一個資源未取得，而剩下的資源小於 m 代表的意思就是 $ m - (\sum_{i = 1}^n Max_i - n ) \geq 1$，**代表我們還有至少一個以上的資源可以使某些 processes 可以先結束**，接著回收的資源就可以再分配給其他 processes。

則**系統是不可能會有死結的 ( Deadlock free )**。



- $Ex1 .$ 有 6 部印表機現在正被 Process 使用，每個 process 最多需要 2 部印表機才可以完工，則系統最多允許 **幾個** process 執行以確保 Peadlock free ？
  - m = 6, $Max_i = 2$。<br>(1)  $1\leq Max_i \leq m \Rightarrow 1\leq 2 \leq 6$，OK。<br>(2)  $\sum_{i = 1}^n Max_i < n+m \Rightarrow 2n < n+6 \Rightarrow n < 6 \Rightarrow$ **最多 5 個 Processes**。
- $Ex2 . 有 10 部印表機現在正被 Process 使用，每個 process 最多需要 3 部印表機才可以完工，則系統最多允許 **幾個** process 執行以確保 Peadlock free ？
  - $Max_i = 3, m = 10$。<br>$3n < n+10, 2n < 10 \Rightarrow n < 5 \Rightarrow$ $最多 4 個 \; Processes。$
- Proof<br>假設資源全部配置出去即為 $\sum_{i = 1}^n Allcation_i = m​$，又 $\sum_{i = 1}^n Need_i = \sum_{i = 1}^n Max_i - \sum_{i = 1}^n Allocation_i \Rightarrow \sum_{i = 1}^n (Max_i) - m \Rightarrow \sum_{i = 1}^n Max_i  = \sum_{i = 1}^n Need_i + m​$。<br>因為依照定理第二點 $\sum_{i = 1}^n Max_i < n+m \Rightarrow \sum_{i = 1}^n Need_i + m < n+m​$<br>，所以 $\sum_{i = 1}^n Need_i < n​$，**<br>此式代表至少有大於等於 1 個process 之 Need_i 為 0 代表 Process_i 可以完工，**且 P_i 至少會釋出超過 1 個 Resource (**因為按照定理第一點可以知道每個 process 只少會占用大於 1 個資源 Max_i 大於等於 1**)，使得剩下的 processes 中又會有大於等於 1 個 processes 可以取得資源並完工。



### Deadlock detection and recovery



如果放任 Resources 無限制的使用，雖然 Utilization 高，**但是系統有可能進入死結而不自知，所以需要有一個死結偵測的演算法以及解決 ( Recovery ) 死結的方法。**



#### Detection



- **＜Note＞**
  - Avoidance ( Banker's algorithm ) 含有**未來**的資訊 (Max, Need)。
  - Detection 只有**目前的資訊 ( Current infomation )**。
- 資料結構
  - n：Process 個數。
  - m：Resource 種類數。
  - $Allocation：n \times m \; matrix \Rightarrow$ 各個 Process 目前持有的各式資源數量。
  - $Available：[1 \ldots m] \; of \; int = 資源總量 - Allocation\Rightarrow$ **系統目前可用的各式資源數量。**
  - $Work：[1\ldots m] \; of \; int \Rightarrow$ **代表系統目前可用的資源累積數量。**
  - $Finish：[1\ldots n] \; of \; int$ <br>$\Rightarrow \{\begin{matrix}True \quad 代表P_i可完工 \\ False \quad 尚未完工  \end{matrix}$
  - *$Request：n \times m \; matrix \Rightarrow$ **各 Process 目前對各式資源提出的申請量。**
- 演算法
  1. 初值設定<br>$Work = Available$，因為目前沒資源的 process 不會導致 deadlock，因為不會有 **hold** *and wait* 的問題所以$Finish[i] = \{\begin{matrix}True \quad if \; Allocation_i = 0 \\ False \quad if \; Allocation_i \neq  0 \end{matrix}$
  2. 試找一個 $P_i$ 滿足<br>$Finish[i] = False \; AND \; Request_i \leq Work$<br>若找到進入第三步，否則進入第四步。
  3. 設定 $Finish[i] = True \quad Work = Work + Allocation_i$，**回到第二步。**
  4. 確認 $Finish$ **陣列，若全部皆為 true，而我們可以得知目前沒有死結的可能。<br>否則我們可以得知目前有死結的存在，而且** $Finish[i] = false$ **的 Process 會陷入該死結之中。**
- **Time complexity**
  - $O(n^2 \cdot m)$：**死結偵測一次的時間需求很高，還要再乘上偵測的頻率，所以總體的成本很可觀。**



#### 實際演練一




![1531188159444](\willywangkaa\images\1531188159444.png)



- (1) $Work = Available (0, 0, 0)$
- (2) 可以找到 $P_0$ 予以 Resource 後可完工且 $Finish[0] = False$，$Request_0 \leq Work$ 前往第三步。
- (3) 設 $Finish[0] = True$ ，$Work' = Work + (0, 1, 0) = (0, 1, 0)$ 前往第二步。
- (2) 可以找到 $P_2$ 予以 Resource 後可完工且 $Finish[2] = False$，$Request_2 \leq Work$ 前往第三步。
- (3) 設 $Finish[2] = True$ ，$Work = Work + (3, 0, 3) = (3, 1, 3)$ 前往第二步。
- 在不斷的 (2) $\Leftrightarrow$ (3) 之下，可以再找到 $P_1, P_3, P_4$ 可以完工進入第四步。
- (4) 最後確認 $Finish[]$ 全皆為 $True$ 後得知目前「Deadlock free」。



#### 實際演練二


![1531188115016](\willywangkaa\images\1531188115016.png)



- $Work = Available = (0, 0, 0)$
- (2) 可以找到 $P_0$ 予以 Resource 後可完工且 $Finish[0] = False$，$Request_0 \leq Work$ 前往第三步。
- (3) 設 $Finish[0] = True$ ，$Work' = Work + (0, 1, 0) = (0, 1, 0)$ 前往第二步。
- (2) **找不到** $P_i$ **可以利用目前資源完工，進入第四步。**
- (4)  最後確認 $Finish[]$ 非全皆為 $True$ 後**得知目前有「Deadlock」，而在死結內的程序有** $P_1, P_2, P_3, P_4$。





#### Single instance resource algorithm



- 令 $G = ＜V, E＞$  有向圖代表「Wait-for Graph」，其中<br>1. Vertex：**只有 Process 而已，無 Resource 頂點**。<br>2. Edge：Wait-for edge。


![1530174622303](\willywangkaa\images\1530174622303.png)

- 從 RAG 簡化而成，即為：


![1530174725109](\willywangkaa\images\1530174725109.png)


![1530174756704](\willywangkaa\images\1530174756704.png)



- 較為 Detection 演算法**簡單**
  - **使用「Wait-for graph」：在「Wait-for graph」中，若有 cycle 存在則目前有死結存在，否則目前無死結存在。**




![1529287297237](\willywangkaa\images\1529287297237.png)

- 此「Wait-for Graph」有 Cycle，所以有死結存在。


![1529287327536](\willywangkaa\images\1529287327536.png)





#### Recovery



- Kill processes in the deadlock
  - **Kill 「all」 processes in the deadlock**：先前的工作成果全部作廢，成本太高。
  -  **Kill process one by one**，刪除一個 process 之後再使用**偵測死結演算法，若死結仍存在，再多刪除一個 process。( 因為這種做法需要：O(loop 刪除次數**$ \times$**偵測成本 )，時間成本太高不值得 )**
- **Resource preemption (資源剝奪)**
  1. 選擇受害 Process。
  2. 剝奪該 Process 的資源。
  3. **回復該受害 Process 當初未被剝奪資源的狀態。(太困難，成本太高也可能會有 Starvation 的問題)**



