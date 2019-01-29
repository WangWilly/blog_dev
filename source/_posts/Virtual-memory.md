title: Operating System - Virtual Memory
author: Willy Wang (willywangkaa)
tags:
  - Virtual Memory
categories:
  - Operating System
date: 2018-09-10 14:06:00
---
# Virtual memory



- Pros
  - **允許 Process 大小超過實體記憶體可用空間大小時，Process 仍可以正確執行**，主要是作業系統要解決之議題，開發者無須擔心。

> - Additional pros
>   - RAM 的利用度 ( Utilization ) 高
>   - 儘可能地提高系統的多工度 ( Multiprogramming degree )，亦也提高 CPU 的利用度( Utilization )
>     - **＜Note＞ Thrashing 除外**
>   - **I/O Transfer time** ( I/O 傳輸時間 )**較小** (Response)：每次程式要執行時從原本要抓取該程式全部頁面( page )，變成只需要抓取足夠頁面即可
>     - 傳輸時間 $\propto$ 傳輸量
>     - **＜Note＞ 但是整個 Process 在執行的時間作 I/O 傳輸的次數上升(還是要將方才剩餘的頁面抓進頁框之中)，導致整體作 I/O 傳輸的時間是上升的**
>   - 開發者只需專注於程式的開發，無須將心思置於程式過大而無法執行之問題，因為虛擬記憶體是作頁系統管理的一環。( 開發者不需要使用「Overlay」的技術 )



## 實現虛擬記憶體



### Demand paging ( 需求分頁技術 )




![demandpaging](\willywangkaa\images\demandpaging.png)



- 架構於 Page memory management 的基礎之上

  - **＜Note＞ 但在虛擬記憶體之中，與傳統 Page memory management 最大的差異在於以前的 Process 必須等到所有程式載入到頁框之中才可執行；但是在虛擬記憶體裡採用「Lazy swapper」，意指在程式開始時，無須載入所有與該 Process 相關之頁面也可正確的執行 Process，等到該 Process 需要該頁面時才載入至頁框。**(若初始時，若該 Process 在記憶體無任何頁面，則稱之為「Pure demand paging」 )
  - 當 Process 於執行階段企圖存取不在頁框( RAM frame )中的頁面時，則稱為發生了「Page fault」並對作業系統發出該錯誤中斷，接著作業系統會將 Process 所需的「Lost page」從硬碟載入至 RAM 中，Process 方可執行。

- 在分頁中，需要一個欄位：「Valid/ Invalid bit」，用來區分此分頁是否存在於 RAM 中。

  - $\left\{\begin{matrix} 1 & ： & 在 \; RAM 中 \\ 0 & ： & 不存在於 RAM 中 \end{matrix}\right.$

  - |                | Valid bit                                                    |
    | -------------- | ------------------------------------------------------------ |
    | 作業系統       | 設定、修改 ( 作業系統在建立分頁表時會決定哪些頁面的進出，所以會設定該 Valid bit 的狀態，另外作業系統在處理「Page fault」時會將頁面抓入 RAM 中並修改該 Valid bit ) |
    | 記憶體管理單元 | 讀取 Valid bit                                               |




### 處理 Page fault




![handlingpagefault](\willywangkaa\images\handlingpagefault.png)



- **處理時間長**



1. MMU 會遇到「Address error」並對作業系統傳出「interrupt」。
2. 作業系統收到中斷後，必須暫停該 Process 的執行且保存其狀態資訊於 PCB 中。
3. 作業系統檢查 Process 之存取位址是否合法。若為非法，則終止該 Process；**若合法，則作業系統判定為「Page fault」所導致。**
4. 作業系統吸到 RAM 中檢查有無空閒頁框
   - 若無，則必須執行「Page replacement」工作，以空出一個頁框，以便將頁面移入記憶體之中。
5. 作業系統再到硬碟中找出「Lost page」所在的位址，啟動 I/O 運作，將 Lost page 載入到 free frame 中。
6. **最後作業系統修改分頁表**，紀錄該分頁位於何頁框碼中，**另外將 Invalid 改為 Valid。**
7. **作業系統恢復中斷之前 Process 的執行。**



> Ref P8.7 步驟六 ...作業系統**可**將 CPU 分給...



### 計算 Effective memory access time ( Virtual memory )



當 P 是「Page fault ratio」時，有效的記憶體存取時間為：
$$
(1-p) \times Memory \;access \;time + p \times ( Page\; fault \;process \;time+Memory\;access\;time)
$$

- Ex

  - Memory access time：200 ns
  - page fault process time：5ms

  若 page fault ratio = 10 %，求出 Effective memory access time。


$$
Memory \;access \;time + p \times Page\; fault \;process \;time = 200\;ns + 0.1 \times 5ms = 500180 \;ns
$$


  若希望 Effective memory access time 不超過 2ms 則 page fault reatio 為？


$$
\begin{matrix}
&Memory \;access \;time + p \times Page\; fault \;process \;time &\leq& 2 \;ms \\
\Rightarrow& 200 \;ns + p\times5 \;ms &\leq& 2 \;ms \\
\Rightarrow& p\times5 \;ms &\leq& 1.9998 \;ms \\
\Rightarrow& p &\leq& 0.39996
\end{matrix}
$$


> 令 P 代表 TLB hit ratio，q 代表 page fault ratio
> $$
> p \times (TLB \;time + Memory \;access \;time) + (1-p) \times (TLB \;time + \\ Memory\;access\;time_{查Page table} + Memory\;access\;time_{存取資料} + \\q \times Page\; fault \;process \;time)
> $$
>





- 欲降低有效記憶體存取時間降低，提升虛擬記憶體的效能，關鍵在於**降低「Page fault ratio」。**



### 影響 Page fault ratio 因素



#### Page replacement (頁面替換)



當 **Page fault 發生**且 **RAM 無空閒之頁框**，作業系統必須執行此工作，**即要選擇一「Victim page」( 或 replaced page )** 將它搬出 **( swap out ) 至硬碟保存**，以騰出一個空閒的頁框，所以**遇到需要「Page replacement 」的情況時，會額外多出一個硬碟的 I/O 運作 ( Page fault process time 更久 )。**



- 如何**降低**「Swap out」此一額外的「I/O運作」？
  - 在分頁表中多一個欄位**「Modification bit」(Dirty bit)**，用以表示上次分頁載入後到現在，內容是否被修改過。<br>$\left\{\begin{matrix} 1 & ： & 曾經修改過 \\ 0 & ： & 不曾修改過 \end{matrix}\right.$ <br>**所以作業系統可以檢查 Victim page 的 Dirty bit。若為 0 ，則無需將該分頁「swap out」**以降低 I/O 次數；**反之，則要將該分頁「Swap out」至硬碟之中。**




> |                        | Dirty bit                              |
> | ---------------------- | -------------------------------------- |
> | Memory management unit | set $(0 \rightarrow 1)$                |
> | Operating system       | Reference and reset$(1 \rightarrow 0)$ |



- Ex 

  - Page fault process time：8 ms ( 有可用頁框或是該犧牲頁框不用「swap back」至硬碟中 )
  - Page fault process time：20 ms ( 該犧牲頁框**需**「swap back」至硬碟中 )
  - Memory access time：100 ns
  - 選定已修改過犧牲頁框之機率：70 %

  若要使有效記憶體存取時間$\leq 200 \;ns$ ，求 page fault ratio $\leq ？$

$$
100 \;ns + p\times page\;fault\;process\;time \leq 200 \;ns \\
\Rightarrow 100 \;ns + p\times (0.3 \times 8\;ms+0.7\times20\;ms) \leq 200 \;ns \\
\Rightarrow 100 \;ns + p\times 16.4 \;ms \leq 200 \;ns \\
\Rightarrow p \leq \frac{1}{164000}
$$



- Ex

  - 1 次 I/O time：10 ms
  - Page fault ratio：10 %
  - Victim page is modified ratio：60 %
  - Memory access time：200 ns

  求有效記憶體存取時間為何？

$$
200 \;ns + 0.1 \times page\;fault\;process\;time \\
 = 200 \;ns + 0.1 \times ( 0.4\times 10\;ms+0.6\times(10 \;ms \times 2) )......*需要兩次I/O運作
$$



#### Replacement policy



- **Local** replacement policy ( 多數使用 )
  - 作業系統只能從發生「Page fault」的 Process 之所有分頁 ( in frame ) 之中挑選一個**犧牲頁面**，**不可在其他 Process 之所有分頁 ( in frame ) 裡挑選犧牲頁面。**
  - **Cons**
    - 無法從其他**不活躍的 Process** 之所有分頁中挑選一個犧牲頁面，**導致記憶體的利用度差。**
  - **Pros**
    - 若該 Process 發生「Page fault」的頻率極高，但也不會搶奪其它 Process 的頁框，**所以可限縮「Thrashing」的範圍。**
- Global replacement policy
  - 作業系統可從其他 Process 挑選犧牲頁面。
  - **Cons**
    - 若該 Process 發生「Page fault」的頻率極高，因為可以搶奪其它 Process 的頁框，**所以「Thrashing」的範圍會一直擴散。**
  - **Pros**
    - 可從其他**不活躍的 Process** 之所有分頁中挑選一個犧牲頁面，**記憶體的利用度佳。**



#### Page replacement schema ( 法則 )



###### Belady anomaly

> **Process 分配到的頁框數增加**，其「Page fault ratio」卻**不減反增**之異常現象。
>
> - Ex ( *FIFO法則 )
>
> $$
> 1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5
> $$
>
>
>
> 配置**三個**頁框：
>
> 
![FIFO3frame](\willywangkaa\images\FIFO3frame.png)
>
>  9 次「Page fault」
>
> 配置**四個**頁框：
>
> 
![FIFO4frame](\willywangkaa\images\FIFO4frame.png)
>
>  **10 次「Page fault」**
>
> - **Stack property**
>   - **n 個頁框所包含的「Page set」保證是 n+1 個頁框所包含的「Page set」之子集合**。
>   - **若具有「Stack property」保證不會有「Belady anomaly」。**
>     - 若在 n 個頁框的「Page set」不會發生「Page fault」，那在 n+1 個頁框自然也不會發生「Page fault」；而如果在 n 個頁框的「Page set」**會發生**「Page fault」，那在 n+1 個頁框可能不會發生「Page fault」，這樣之下至少 n+1 個頁框會比 n 個頁框**少一次「Page fault」。**
>   - 具有 Stack property 的法則 ( 不會發生「Belady anomaly」 )
>     - Optimal 法則
>     - Least recently used 法則
>
> 
![stackproperty](\willywangkaa\images\stackproperty.png)



##### FIFO 法則



最早載入的頁面**( Loading time 最小 )**，作為犧牲頁面。

- Pros
  - 簡單、容易實作。
- Cons
  - 可能有「Belady anomaly」現象。
  - 效能不是很好，Page fault ratio 相當高。
  - **＜Note＞ Page replacement 法則之中，只有最佳沒有最差 <br>( 在任何情況之下都要使 FIFO 的「Page fault」最高才能稱為最差 )。**



- Ex
  - 給予 3 個頁框並且初始時全部皆為空 ( 又稱為「Pure demand paging」)，以下是「**Page reference string**」，請求取「Page fault」次數。


$$
7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2, 1, 2, 0, 1, 7, 0 ,1
$$

> 或是以頁框大小為 100 kB，目前有三個空頁框，存取下列位址：
> $$
> 731, 008, 117, 258, 039, 331, 047 ...
> $$
> 或是「Logical address」為 12 bits，「Page no.」佔有 3 bits，存取位址如下：
> $$
> FAF, 1DC, 21E, 147, 747, 0AF, 5D7... 
> $$
>




![FIFOpagereplace](\willywangkaa\images\FIFOpagereplace.png)

**總共 15 次。**



##### Optimal 法則 ( OPT )



選擇**將來長期不會使用到的分頁**作為犧牲頁面。



- Pros
  - **「Page fault ratio」最低，所以稱之為「Optimal」。**
  - **不會有「Belady anomaly」現象。**
- Cons
  - 因為需要未來的資訊，**所以無法被實作出來。**
    - 通常做為理論研究時當作比較對象來使用。



- Ex
  - 給予 3 個頁框並且初始時全部皆為空 ( 又稱為「Pure demand paging」)，以下是「**Page reference string**」，請求取「Page fault」次數。

$$
7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2, 1, 2, 0, 1, 7, 0 ,1
$$


![pagereplacement_opt](\willywangkaa\images\pagereplacement_opt.png)

**總共 9 次。**



##### Least recently used ( LRU ) 法則



**選擇過去不常使用的分頁作為犧牲頁面**，即挑選**最後一次存取時間點最小的分頁**，也就相當於是「反向 Optimal 法則」( 依照歷史資訊而作出決定的 OPT 法則 )。



- **Pros**
  - **「Page fault ratio」可以被接受。**
  - **不會發生「Belady anemaly」現象。**
- **Cons**
  - **LRU 的製作需要大量硬體支持，所以成本很高。**
    - 進而衍生出「LRU 近似法則」，以降低成本。



###### LRU 的製作方法

> - 以 Counter 製作
>
>   - 程序
>
>     1. 每次發生存取該頁面時，累進 Counter 的值。
>
>     2. 將 Counter 的值複製至該存取頁面之「last reference time」欄位之中。
>
>     3. 作業系統在挑選 LRU 分頁時，就挑選「last reference time」最小的分頁。
>
>
>
> - 以 Stack 製作
>   - 最後一次存取之分頁必定置於 Stack **頂端**。
>   - Stack 之**底端即為 LRU 分頁。**
>   - **Stack 大小 = 頁框數量。**
>
> 
![LRU_stack](\willywangkaa\images\LRU_stack.png)
>
> -  3 個頁框有 10 次「Page fault」
>   - **注意：此時「FIFO」法加上三個頁框才 9 次「Page fault」**
> - 4 個頁框有 8 次「Page fault」



- Ex

  - 給予 3 個頁框並且初始時全部皆為空 ( 又稱為「Pure demand paging」)，以下是「**Page reference string**」，請求取「Page fault」次數。


$$
7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2, 1, 2, 0, 1, 7, 0 ,1
$$


![pagereplacement_LRU](\willywangkaa\images\pagereplacement_LRU.png)

**總共 12 次。**



##### Least recently used 近似法則



主要以「Reference bit」為基礎 $\left\{\begin{matrix} 0 & ： & 此分頁未曾被修改過 \\ 1 & ： & 此分頁曾經被修改過 \end{matrix}\right.$，但不確定何時被存取( 與 LRU 的差異 )。

 

###### Additional reference bit



> **每個分頁有一個欄位 ( 或暫存器； 8 bit ... )，當發生記憶體存取時，該分頁之「Reference bit」會被設置為 1，系統每隔一段時間會將各分頁的暫存器值「右移」一位 ( 空出最高位元並將最右側位元捨去 )，並將各分頁之「Reference bit」值複製到暫存器之「最高」位元並重設各個分頁的「Reference bit」。**
>
> 將來要挑選犧牲頁面時，**挑選「暫存器最小值」之分頁**，若多個分頁具有相同的值，則以「FIFO 法則」加以篩選。
>
>
>
> - Ex
>   - 給予 3 個頁框並且初始時全部皆為空
>
> $$
> 1, 2, 1, 1 ,1 ,1, 3, 1, 3, 3, 3, 3, 5
> $$
>
> 
![pagereplacement_addbit](\willywangkaa\images\pagereplacement_addbit.png)
>
> 遇到「Page 5」時，因為**「Page 2」**的暫存器值最小所以選擇該分頁為犧牲分頁。



###### Second chance 法則



以 FIFO 為基礎，搭配「Reference bit」以挑選犧牲頁面。

- 步驟
  1. **先以 FIFO 順序挑出一個分頁。**
  2. **檢查此分頁的「Reference bit」值，**<br>$\left\{\begin{matrix}  case \;1： & 0 & 則該分頁即為犧牲者處理完後回到第一步。 \\ case\;2： & 1 &  跳至第三步。 \end{matrix}\right.$
  3. **給予該分頁機會 ( 不挑選該分頁作為犧牲者 )**
  4. **重設該分頁之「Reference bit」為 0。** 
  5. **☆將該分頁之「載入時間」設置成現在時間 ( FIFO 性質 )。**<br>( 載入時間有兩個地方會被更改，$\left\{\begin{matrix} 1. & 該分頁真的被載入至記憶體之中。 \\ 2. & 給予該分頁機會不挑選為犧牲者時。\end{matrix}\right.$ )
  6. 回到第一步。
- ☆Ex ( 四個頁框中，依目前狀態挑一頁框為犧牲頁框 )




![pagereplacement_secondchance](\willywangkaa\images\pagereplacement_secondchance.png)

挑「Page 3」為犧牲頁框。

- Ex ( 三個頁框 )


$$
1, 2, 3, 4, 2, 5, 2, 6, 1, 2
$$


![pagereplecement_secondchoice_2](\willywangkaa\images\pagereplecement_secondchoice_2.png)



**8 次「Page fault」。**



> - 當所有分頁之「Reference bit」皆相同時，則退化成 FIFO。
> - **也稱為「Clock algorithm」。**



###### Enhanced second chance 法則



以 ＜Reference bit, Modification bit＞ 配對值作為挑選犧牲頁面的依據，值最小者之分頁作為犧牲頁面。若多個分頁具有相同的值，則以 FIFO 為準。

- Modificaion bit
  - 該分頁曾經被修改過，若該分頁為犧牲品，要將該分頁寫回硬碟保存 ( Swap out )。

- 大小依據 ( 大小由上到下 )



$$
＜Reference bit, Modification bit＞\\
＜0, 0＞\\
＜0, 1＞\\
＜1, 0＞\\
＜1, 1＞
$$


##### LFU 與 MFU 法則 ( 無計算題 )



以分頁的**累計總參考次數**為挑犧牲頁面的依據。

- **Cons**
  - **「Page fault reatio」相當高。**
  - **有「Belady anomaly」。**
  - **製作需大量硬體支持，所以成本非常高。**



###### Least frequently used



挑選參考次數最小的分頁為犧牲品。

- 若多個分頁具有相同值，也是以 FIFO 為準。



###### Most frequently used



挑選參考次數最多的分頁為犧牲品。

- 若多個分頁具有相同值，也是以 FIFO 為準。



##### 問題與討論



- Ex

| Page   | Loading time | Last reference time | Reference bit | Modification bit | 參考次數 |
| ------ | ------------ | ------------------- | ------------- | ---------------- | -------- |
| Page 1 | 493          | 800                 | 0             | 0                | 410      |
| Page 2 | 172          | 700                 | 1             | 1                | 235      |
| Page 3 | 333          | 430                 | 0             | 1                | 147      |
| Page 4 | 584          | 621                 | 1             | 0                | 875      |
| Page 5 | 256          | 564                 | 0             | 1                | 432      |

則下面各法則之犧牲頁面為何？

- FIFO：Page 2
- LRU：Page 3
- Second chance：~~Page 1~~ Page 5
  - **要從最早載入( FIFO )的開始檢查。**
- Enhanced second chance：~~Page 1~~ Page 1
  - **以 ＜Reference bit, Modification bit＞ 配對值作為挑選犧牲頁面的依據。**
- LFU：Page 3
- MFU：Page 4



#### Page buffering 機制



挑選出犧牲頁面後，如果該分頁被**修改**過，則會

1. **將犧牲頁面 Swap out 至硬碟之中。**
2. **載入「Lost page」。**
3. **讓 Process 恢復執行。**

而以上處理的時間過於冗長，導致 Process 要恢復執行所需的時間拖太久而欲趕改善。



##### Free frame pool

( 作業系統的預留頁框，平常不配置給 Process 所用 )

- **主要由作業系統維護。**
- 當作業系統挑到**曾修改過之分頁**之後：
  1. **作業系統從「Free frame pool」中取出一個 Free frame，提供 Lost page 載入。**
  2. **載入完成之後，Process 即可恢復執行。**
  3. 作業系統稍後將犧牲頁面寫回硬碟，空出之頁框**再還給作業系統，加入「Free frame pool」之中**




![freefreampool](\willywangkaa\images\freefreampool.png)



##### Modification list



- 作業系統維護一條**「Modification list」，記錄所有曾被修改過的分頁資訊 ( 即為Modification bit 值 = 1 )**
  - 作業系統等到「Paging I/O device」利用度低時 ( 裝置閒置；有空 )，將此串列中某些分頁寫回硬碟之中，**同時自此串列移除這些分頁，接著重設該「Modification bit」為 0。**
  - 可以使選擇犧牲分頁時，**會有較小的機率選到曾被修改過的分頁**，也就能讓 Process 有較高的機會可以快速的恢復執行。



##### ☆ 強化 Free frame pool



**以「Free frame pool」為基礎，針對 Pool 中每一個頁框記錄放的是哪個 Process 的哪個分頁** ( ＜Process ID, Page No.＞ )，因為這些分頁內容必定為最新值 ( 該值與存於硬碟中的值目前**同調** )。



- 流程
  1. 作業系統選擇完犧牲頁面之後 ( 該頁面曾被修改過 )。
  2. **作業系統在「Free frame pool」中尋找有無該需要的「Lost page」存在，**<br>**（a）若存在**則直接將該頁框加入至「Resident frame pool」之中，Process 即可恢復執行 ( **不需任何一次的硬碟的 I/O 傳輸** )。<br>**（b）若不存在**則再**從硬碟取出該分頁寫入至「Free frame pool」**並將該頁框加入至「Resident frame pool」中(**需一次的硬碟的 I/O 傳輸**)。
  3. 當作業系統將犧牲分頁寫回 ( Swap back ) 至硬碟之後，再將該頁框加入至「Free frame pool」中。



##### 問題與討論



- Ex (P. 8-90 ex89)



- 舉例說明 LFU 之「Page fault」次數**少於** LRU 的可能。
  - 給三個頁框一開始為空。



- 舉例說明 LFU 之「Page fault」次數**多於** LRU 的可能。
  - 給三個頁框一開始為空。



#### 頁框數量分配多寡之影響



- 一般而言，Process 分配到的頁框數增加，*其「Page fault ratio」理應下降*。

- **作業系統分配 Process 頁框數量時，必須滿足最少及最多數量限制**。( 由硬體限制，作業系統無權 )

  - **最大數量限制**

    - 實際頁框多寡( 實體記憶體大小 )

  - **最少數量限制**

    - **CPU 在「完成機器指令執行過程中」，可能最多對 RAM 讀寫的次數。**( 否則 CPU 在讀取機器指令階段可能永遠無法完成。若讀寫需 3 次，則配給所有 Process 至少要 3 個頁框 )
    - Ex 

    **假設指令的存取不需跨頁面。**

    **存在於記憶體的運算元採用直接定址模式**

    則最多可能需要幾次的記憶體存取？ 3 次，**則作業系統至少要給 Process 頁框數量大於等於三**。

    - Ex

    **假設指令的存取需跨頁面。**

    **存在於記憶體的運算元採用間接定址模式**

    則最多可能需要幾次的記憶體存取？ 6 次，**則作業系統至少要給 Process 頁框數量大於等於六**。



### Thrashing 現象




![thrashing](\willywangkaa\images\thrashing.png)



**若 Process 分配到的頁框不足時，**則此 Process 會經常「Page fault」，所以作業系統要作 Page replacement，若作業系統採用「Global replacement policy」，可能會挑選到其他 Process 所把持的頁框當作犧牲頁面，而如此一來也會造成其他 Process page fault 使得其他 Process 也發生 Page fault，最後又去搶奪其他 Process 的頁框來使用，導致所有 Process 皆發生 Page fault 且**等待 Paging I/O 裝置運作完成 ( Swap out / Swap in )，此時 CPU 利用度下降，作業系統會企圖調高「Multiprogramming degree」**，將導入更多 Process 進入執行，**但是記憶體原本就不夠，**所以這些 Process 也立刻發生「Page fault」，而作業系統又在調高「Multiprogramming degree」，**不斷循環下去**，此時系統呈現：

- **CPU 利用度急速下降。**
- **Paging I/O 設備異常忙碌。**
- **Processes 花在 Page fault process time ( Blocked state )遠大於正常執行時間 ( Running state )。**



#### 解決與預防 Thrashing



##### Decrease multiprogramming degree - 解決



當 Thrashing 發生時，作業系統必須將「Multiprogramming degree」降低。

- 選擇低優先權或完成度低的 Process 作 Swap out。



##### 使用 Page fault frequency control 機制 - 預防




![pagefaultfrequencycontrol](\willywangkaa\images\pagefaultfrequencycontrol.png)



使用 **Page fault frequency contro**l 機制，來防止 Thrashing 發生。

- **作業系統會制定合理的「Process page fault ratio 上限與下限」**
  - 作業系統如果發現 Process 的「Page fault ratio」
    - **高於「上限值」**：作業系統應該多**分配一些額外的頁框給該 Process，降低其「Page fault ratio」回到合理的區間。**
    - **低於「下限值」**：作業系統應該**收回一些該 Process 的頁框，分配給其他有需要之 Process 。**
  - **若作業系統能夠控制所有 Process 之「Page fault ratio」在合理的區間，則理當不會發生「Thrashing」。**



##### ☆Working set model 技術



運用 Working set model 技術預估 Process 在不同時間執行時，所需之頁框數量，作業系統根據此資訊，分配 Process 足夠的數量，以防止 Thrashing。



此技術是基於「Locality model」( 集中模型 )之理論基礎之上。


![workingsetwindow](\willywangkaa\images\workingsetwindow.png)

- 相關名詞解釋
  - Working set window：$\Delta$
    - 表示以「$\Delta$ 次的 Page reference」作為依據參考。
  - Working set
    - 在「$\Delta$ 次的 Page Reference 」中**所「Reference」到不同分頁之集合。**
  - Working set size ( WSS )
    - Working set 之「元素個數」。
    - **代表 Process 此時所需要頁框數量。**
- **☆☆ 作業系統應用的方法**
  - 假設 *n* 為 Processes 個數。
  - $WSS_i$**：**$Process_i$ **在此時期的「Working set size」。**
  - **求** $D = \sum_{i = 1}^n WSS_i = 頁框的總需求量$ ( Demand )。<br>**令 M = 實際記憶體頁框的總數**
    - **若**$D \leq M$**：則作業系統可以依照** $WSS_i$ **之值分配給** $P_i$ **足夠的頁框數量，也不會造成 Thrashing。**
    - **若**$D > M$**：則作業系統會選擇一些 Process 並對他們作「Swap out」，以降低 D 直到** $D \leq M$ **為止**，作業系統再進行分配頁框。
- Pros
  - 可以防止「Thrashing」。
  - 對於「Prepaging」有助益：**事先猜測哪些資源 Process 會使用到哪些分頁，並預先載入至記憶體之中，如果猜測精準，則可以避免初期之大量「Page fault」**。
- Cons

  - 不易制定精確的「Working set」。
  - 若前後期的「Working sey」內容分頁差異很大，則 I/O 次數會上升。



- **Ex (P.8-58 39)**
  - 下列狀況增加「Multiprogramming degree」是否有助於提高「CPU utilization」？
    - （1）CPU 利用度：13%、硬碟利用度：97%
    - （2）CPU 利用度：87%、硬碟利用度：3%
    - **（3）**CPU 利用度：13%、硬碟利用度：3%



（1）**(thrashing)**、（2）維持現狀；



- **Ex (P.8-63 49)**
  - CPU 利用度：20 %
  - Paging disk：97 %
  - 下列哪些措施**必 ( will )、可能 ( is likely to )、絕不 ( never ) 增進 CPU 的利用度？**
    - （1）Increase multiprogramming degree
    - （2）Decrease multiprogramming degree
    - （3）Install *faster CPU*
    - （4）Install more main memory
    - （5）Install bigger disk
    - （6）Install faster disk
    - （7）Local replacement policy used
    - （8）Prepaging used
    - （9）Use bigger page size
    - （10）Use smaller page size

目前正在「Thrashing」：

**(will)**

(2)；(4)；(9)

**(is likely to)**

(6)：**因為 Page fault process time 可以降低**；**(7)**；**(8)**

**(never)**

(1)；(3)；(5)；**(10)：更差**



- Example（106清華大學資工計算機系統）
  - Is it possible for a process to have two working sets, one representing data and another representing code? Explain your answer.

「Working set」是由多個**正在執行的**「Virtual memory page」組成，**且並不包含不用執行的「Page」**，所以不可能有兩個「Working set」分別記錄「Data」與「Code」



###### ☆Locality model

> 
![localittmodel](\willywangkaa\images\localittmodel.png)
>
> Process 執行時，對於所存取之「記憶體區塊」，**並非是均勻的，而是具有某種 局部/集中 區域存取之特性。**
>
> - Temporal locality ( 時間局部性 )
>   - 目前所存取的區域**不久後**又會再度被存取。( **或者是此區域在最近經常被存取**，如上圖所示 )
>   - 可能導致的因素
>     - **Loop 敘述**
>     - **Subroutine ( function, pure code... )：常被使用的一段程式碼片段。**
>     - **Counter ：經常存取之變數。**
>     - **Stack**：頂端之元素。
>
> - Spatial locality ( 空間區域性 )
>   - 目前所存取區域**之鄰近區域**也極有可能被再次存取。
>   - **可能導致的因素**
>     - **陣列**
>     - **Sequential code execution ( 鄰近程式碼 )**
>     - **Common data area ( 集中的共享變數 )**
>     - Linear search
>     - **Vector 的運算** ( 早期將向量視為陣列 )
>
> 只要 Program 中用到的指令、資料結構、演算法符合「Locality model」，**則此程式對於記憶體是友善的 ( Page fault ratio 應下降 )**；若不符合則對記憶體不友善。
>
> - **不友善的因素**
>   - **Hashing：希望資料不要靠太近，會發生碰撞。**
>   - **Binary search：會在記憶體跳來跳去。**
>   - **Link list**
>   - **goto , jump 指令**
>   - **間接定址模式：會容易跨頁面存取。**
> - Ex (p.8-55 ex32)



###### Page size 的影響



> - 若 Page size **愈小**，則
>   - Pros
>     - **Page fault ratio ( 效能的關鍵，一旦發生「Page fault」必須要執行「Swap」 )**：越大<br>(因為跨不同分頁的機率增加)
>     - Page table size：愈大
>     - I/O 次數 ( Total time )：**增加** ( 因為會有更多頁面需要載入 )
>   - Cons
>     - 內部碎裂：**輕微**
>     - I/O Transfer time ：越小 ( 因為單一頁面小，I/O的**傳輸量**就可以降低 )
>     - **Locality：越佳**
>
>
>
> **＜主流趨勢＞** 朝向大的 Page size 設計。



### Program structure 對於記憶體存取之影響




![workingset](\willywangkaa\images\workingset.png)



- 若 Program 中使用的指令、資料結構、演算法符合「Locality model」，則有助於降低「Page fault ratio」。

  - 反之，則無助於降低「Page fault ratio」。
- **程式中對於陣列元素的處理程序最好與陣列元素在記憶體中的儲存方式 ( Row-major 或 Column-major )對應，有助於降低「Page fault ratio」**



- Ex

  - A：array [1... 128, 1 ... 128] of int

  - 每個 int 佔有 1 B

  - A 以 Row-major 方式存於記憶體中

  - Page size = 128 B

  - 給該 Process 三個頁框，且程式已經在記憶體之中，採用 FIFO replacement policy 求下列程式碼的「Page fault」次數。

  - （1）

    ```
    for (i = 1 to 128) {
        for (j = 1 to 128) {
            A[i, j] = 0
        }
    }
    ```




  每一列發生 1 次「Page fault」，共 128 列，所以總共發生 128 次「Page fault」。



  - （2）

    ```
    for (j = 1 to 128) {
        for (i = 1 to 128) {
            A[i, j] = 0
        }
    }
    ```



  每一行發生 128 次「Page fault」，共 128 行，所以總共發生 128 $\times$ 128 次「Page fault」。



- Ex

  - A：array [1... 100, 1 ... 100] of int

  - 每個 int 佔有 1 B

  - A 以 Row-major 方式存於記憶體中

  - Page size = 200 B

  - 給該 Process 三個頁框，且程式已經在記憶體之中，採用 **LRU** replacement policy 求下列程式碼的「Page fault」次數。

  - （1）

    ```
    for (i = 1 to 100) {
        for (j = 1 to 100) {
            A[i, j] = 0
        }
    }
    ```



  每兩列發生 1 次「Page fault」，共 100 列，所以總共發生 100 / 2 次「Page fault」。



  - （2）

    ```
    for (j = 1 to 100) {
        for (i = 1 to 100) {
            A[i, j] = 0
        }
    }
    ```



  每一行發生 100 / 2 次「Page fault」，共 100 行，所以總共發生 50 $\times$ 100 次「Page fault」。



### Copy-on-write



主要談論到三種 `fork()` 的差異。

![copyonwrite](\willywangkaa\images\copyonwrite.png)

- `fork()` without「copy-on-write」
  - Parent process 使用 `fork()` 建立 child process，**作業系統會配置新頁框給予 Child process**。( Child 與 parent 占用不同的記憶體空間 )
  - **同時，作業系統會複製 Parent process 內容( Code section, data section )給 Child process。**
  - Cons
    - **記憶體頁框需求量大增。( 與 Parent process 頁框一致 )**
    - **建立 Child process 的時間慢。**( 需複製 Parent process 資料 )
    - ☆ **在 Child 生出之後立即執行** `execkp()` **作其他的工作時，更加顯得不必要作上面兩件事。**
- `fork()` with「copy-on-write」
  - 當 Parent process 建立 Child process 時，**作業系統讓 Child 一開始共享 Parent process 之記憶體頁框空間，所以不需要配置給 Child process 新的頁框與複製 Parent 的內容給 Child process。**
  - **若 Child process 想要更改某分頁的內容，則作業系統會配置一個新的分頁給 Child ，且複製該分頁內容到新頁框中( 並修改 Child 的分頁表指向新頁框 )，供 Child 使用與修改，**就不會影響 Parent process 的資料。
  - 有可能會修改 ( modified ) 的 分頁需標示「copy-on-write bit」為 1。<br>( Read-only code / data 無須標示 )
  - Pros
    - **可降低頁框需求量**	
    - **加速 Process 的建立。**

- `vfork()` ( Virtual memory `fork()` )
  - Parent process 建立 Child process 時，**Child process 會共享 Parent process  相同的頁框，但是並無提供「Copy-on-write 技術」，所以任何一方改變了某分頁內容會使另一方受到影響。( 須小心使用 )**
  - **特別適合用於 Child process 的建立是要立刻執行** `execlp()` **要去作其他工作時**，只需要將 Child process 的分頁表指向新頁框的位址即可讓效率提升。
  - 通常用於
    - Command interpreter 的製作。( UNIX shell )



### TLB reach



經由 TLB 的對應，所能存取到的「記憶體空間」大小，則「TLB reach」愈大愈好。
$$
TLB \; reach = TLB \;entry \;數量 \times Page \;size
$$

- Ex
  - TLB 有 8 個 entry
  - Page size = 16 kB

TLB reach = 128 kB



- 增加「TLB reach」的方式
  - 提高 TLB entry 數量
    - **Pros**
      - **TLB reach 增加**
      - **TLB hit ratio 提高**
    - **Cons**
      - **價格成本高**
      - 有時候 TLB entry 的提升仍不足以涵蓋 Process 的「Working set」
  - 增加「Page size」
    - Pros
      - **TLB reach 增加**
      - **成本可以接受**
    - Cons
      - **內部碎裂愈嚴重** <br>解決方法：現代的硬體均會**提供不同大小的分頁( multiple page size )**來使用；<br>E.g. 提供 2 組不同大小的「Page size」 - 4 kB、2 kB<br>所以 TLB 的紀錄項目就會增加一個「Page/Frame size」的欄位紀錄；<br>此外，目前 TLB 的管理從**硬體**改成**作業系統管理**，雖然會因為**中斷使效能降低，但獲得的好處可以抵消。**