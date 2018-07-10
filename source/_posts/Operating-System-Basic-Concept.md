title: Operating System - Basic Concept 1
author: Willy Wang
tags:
  - Basic Concept
  - ''
categories:
  - Operating System
date: 2018-07-10 10:51:00
---
# 基礎觀念 - Basic Concept (壹)

---



## 機器種類 - Machine type



- Bare Machine(裸機)
  - 只有Hardware Components所組成(eg. CPU, memory, I/O device)，沒有任何輔助使用者 ( User ) 的系統程式 ( System Program )存在。
- Extended Machine
  - 以 Bare Machine 為底加上輔助使用者的系統程式 - System Programs (eg. OS, Compiler, DBMS...)，即構成延伸機器。



### CPU 的等待時間( Idle time )



- 人類手動操作對於 CPU time 太慢。
  - 以「Automatic Job Sequence」的軟體**常駐於**在早期的電腦上，利用少量的記憶體執行**非常駐於的電腦上的軟體**，類似於現今的作業系統，稱之為「Resident Monitor」。
- I/O 裝置的工作速度遠比 CPU 的工作速度還慢，CPU 針對某些工作必須等待 I/O 工作完成後才能繼續執行，所以造成 CPU 等待時間 ( idle time ) 太久。
  - 以較快速的裝置介入 CPU 與 I/O 裝置之間作為緩衝。



#### 脫機 (Off-line 非即時運作 I/O)




![1529920981306](\blog\images\1529920981306.png)



不及時的將 CPU 演算完的資料給予讀取或是列印，利用專門的外圍控制機，將低速 I/O 設備上的數據傳送到高速磁碟 ( 磁帶 ) 上；或者相反，CPU 直接對於磁帶讀寫，使電腦加快讀取的速度。



- 以磁帶機緩解讀卡機( Input )與印表機( Output )造成過慢的等待( idle )：電腦中間以磁帶機的方式加速與讀卡機與印表機的溝通( 讀卡機與印表機不直接由 CPU 直接操作 )。
- 磁帶機實作
  - 以專用的 I/O 裝置對於磁帶讀寫。( CPU 直接控制 )
  - 以一個小型的子電腦對於磁帶讀寫，習慣稱之為衛星機( salellite processing )，主要負責磁帶拷貝的工作，CPU 不直接參與磁帶機的讀寫運作。
- Pros
  - CPU 不受到讀卡機或是印表機的速度限制( CPU 受到磁碟機的速度限制 )。
  - 已寫好的程式不必更動，只要把原本直接交付給裝置執行的指令存入磁帶中再給該裝置執行。
  - 裝置獨立性( Device Independent )：泛指同一程式可以在不同的 I/O 裝置上執行的能力。
- Cons
  - 因為要先讓電腦先將指令載入至磁帶，再交由該機器執行，導致設定時間需求長。
  - 磁帶只能以循序讀取( Sequential Acess )的方式讀寫。



#### 緩衝區 ( Buffering )



用以實現 CPU 能與 I/O 同時運作。可以想像成有一個超強者( CPU )，但是要跟一對弱者( I/O device )合作，是必要強者等待弱者將事情做完再交付給強者執行，而強者若要等到某一位弱者執行完後再執行太沒效率了，所以強者準備一個空間( 緩衝區 )給弱者將作完的工作丟入，再來就交由強者以不同策略使用緩衝區的資料達到最佳化。



- I/O Bound Job
  - 因為需要大量的 I/O 工作，速度受限於 I/O 裝置速度。
- CPU Bound Job
  - 因為需要大量的 CPU 計算，速度受限於 CPU 的處理速度。



#### 假脫機<br>線上同時周邊處理技術( Simultaneous Peripheral Operation On-Line, SPOOL )



又稱為排隊緩存技術。當系統中引入了多道程序技術( Multiprocess )後，完全可以利用其中的一道程序，來模擬脫機輸入時的外圍控制機功能，把低速 I/O 設備上的數據傳送到高速磁碟上；再用另一道程序來模擬脫機( Off-line )輸出時外圍控制機的功能，把數據從磁碟傳送到低速輸出設備上。這樣，便可在主機的直接控制( On-line )下實現脫機輸入、輸出功能。




![1529918563313](\blog\images\1529918563313.png)



- 外圍操作與 CPU 對數據的處理可以同時進行。
- SPOOL 的需求
  - 建立在具有多程序( Multiprocess )功能的作業系統。
  - 高速隨機外存。( 磁碟存儲技術 )
- 組成
  - 井：在磁碟上開闢的兩個大存儲空間。<br>輸入井：模擬脫機輸入時的磁碟設備，用於暫存I/O 設備輸入的數據。<br>輸出井：模擬脫機輸出時的磁碟，用於暫存用戶程序的輸出數據。
  - 緩衝區：為了緩解 CPU 和磁碟之間速度不匹配的矛盾，在內存( RAM )中要開闢兩個緩衝區。<br>輸入緩衝區：暫存由輸入設備送來的數據，一旦緩衝區要滿出來了，再傳送到輸入井。<br>輸出緩衝區：暫存從輸出井送來的數據，往後再傳送給輸出設備。
  - 「輸入程序 $SP_i$ 」 和「輸出程序 $SP_o$」。這裡利用兩個進程來模擬脫機 I/O 時的外圍控制機。<br>「程序 $SP_i$」：模擬脫機輸入時的外圍控制機，將用戶要求的數據從輸入機通過輸入緩衝區再送到輸入井，當CPU 需要輸入數據時，直接從輸入井讀入內存。<br>「程序 $SP_o$」模擬脫機輸出時的外圍控制機，把用戶要求輸出的數據先從內存送到輸出井，待輸出設備空閒時，再將輸出井中的數據經過輸出緩衝區送到輸出設備上。
- 達到共享 I/O 裝置的目的。
  - 印表機( 獨占設備 )：當用戶進程請求列印輸出時，SPOOLing系統同意為它列印輸出，但並不真正立即把印表機分配給該用戶進程，而只為它做兩件事：<br>1. 由「輸出程序」在輸出井中為之**申請一個空閒磁碟塊區**，並將要列印的數據送入其中。<br>2. 「輸出程序」再為用戶程序**申請一張空白的用戶請求列印表，並將用戶的列印要求填入其中，再將該表掛到請求列印隊列上**。<br>**如果還有進程要求列印輸出，系統仍可接受該請求，**也同樣為該進程做上述兩件事。如果印表機空閒，輸出進程將從請求列印隊列的隊首取出一張請求列印表，根據表中的要求將要列印的數據，從輸出井傳送到內存緩衝區，再由印表機進行列印。列印完後，輸出進程再查看請求列印隊列中是否還有等待列印的請求表。若有，又取出隊列中的第一張表，並根據其中的要求進行列印，如此下去，直至請求列印隊列為空，輸出進程才將自己阻塞( Blocked )起來。僅當下次再有列印請求時，輸出進程才被喚醒( Wake up )。
- Pros
  - **提高 I/O 的速度**：從對低速 I/O 設備進行操作，**演變為對輸入井或輸出井中數據的存取，如同脫機輸入輸出一樣**，提高了I/O 速度，緩解了 CPU 與 I/O 設備之間速度不匹配的矛盾。
  - **將獨占設備視為為共享設備**：SPOOLing 系統中，實際上並**沒為任何進程分配設備**，而只是在輸入井或輸出井中為進程分配一個存儲區和建立一張I/O 請求表。
  - **實現了虛擬設備功能**：宏觀上，雖然是多個程序在同時使用一台獨占設備，而對於每一個程序會認為自己獨占了一個設備( 邏輯上的設備 )。SPOOLing 系統**實現了將獨占設備變換為若干台對應的邏輯設備的功能。**



**＜Note＞：**Spool 與 Buffer 的差異性。

- Spool 允許有程序在執行 CPU 運算時，可以有其他的程序的 I/O 運算同時進行( overlay execution )
- Buffer 允許有程序執行 CPU 運算時同時運算( overlay execution )該程序的 I/O 運算。



## 系統種類 - System Type



### 多元程序系統 - Multiprogramming System




**允許系統 ( 或在記憶體 ) 中存在多個 Process ( 處理程序 ) 同時執行**。透過 CPU Scheduling 技術，當某個 Process 取得 CPU 執行時，若因為某些事件發生 ( 如：Wait for I/O completed 、Resource Not Available... ) 而無法往下執行時，作業系統可將 CPU 切換給其他 Process 使用，則CPU在各個 Processes 切換，可以使 CPU 總是為忙碌的狀態( Busy )。

- 主要用於避免 CPU 的空等( idle )，提高 CPU 效能( Utilization )。

- Multiprogramming Degree：
  - **系統中存在執行的 Process 個數。**
  - 通常 Multiprogramming Degree 越高，且非為 Thrashing 狀態，則 CPU 效能越高。

- 多個 Process 同時執行的方法：
  - Concurrent ( 並行 )：**一個 CPU，多個 Processes 共同使用。**

  
![1530087435637](\blog\images\1530087435637.png)

  

  - Parallel ( 平行 )：**多核心 CPU 共同執行多個 Processes。**

  
![1530087726413](\blog\images\1530087726413.png)



### 分時系統 - Time Sharing System



It's a **logical extension of multiprogramming system, the CPU in this kind of system switch highly frequently.**



- Multiprogramming 的一種，又可以稱為「Multitasking」。<br>**＜Note＞：與 Multiprogramming 的差異在於 CPU 切換的頻率極高。**

- **排班使用「RR(Round-Robin) 法則」**：作業系統規定一個 CPU Time Quantum ( 區段 )，**若 Process 在取得CPU後，未能於區段內完成工作，則必須被迫放棄CPU，等待下一次輪迴。**
  - **通常 Quantum 時間很短。( 小於 1 秒 )**
  - 對每個使用者 ( 程序 ) 是公平的。
- 適用在User Interactive System ( 互動式作業系統  ) 或 Response Time ( 反應時間 )要求較短的系統。
- 透過 Resource Sharing 技術 (如：CPU資源 - CPU scheduling、記憶體資源 - Memory sharing、I/O 裝置資源 - Spooling )，使得每個使用者 ( 程序 ) 皆認為享有專屬的資源。
  - **Virtual Memory 技術，擴展邏輯的記憶體空間 ( Virtual Memory Space )。**



![1530091014614](\blog\images\1530091014614.png)



### 分散式系統 - Distributed System



#### 多處理器系統 - Multiprocessors System ( 緊密耦合 - Tightly Coupled )



又稱為 Mulitprocessing、Parallel system。



- 在一台機器 ( 或是在主機板 ) 中，具有多顆 CPUs ( 或是 Processors )。
  - **共享此機器的資源：**<br>1. Memory<br>2. Bus<br>3. I/O - Devices<br>4. Power supplier
  - **通常聽從同一個時脈器 ( Clock )。**
  - **通常由通一個作業系統管理。**
  - **CPUs 溝通藉由「共享記憶體 - Shared memory」的方式完成。**
  - 可以進行平行運算 ( Parallel computing )。
- **Pros**
  - **增加吞吐量 ( Throughput )**：可以使多個程序在不同的 CPUs 上執行 ( Parallel computing )。<br>**＜Note＞：「 N 個 CPUs 之產能」<<「一個有 N 倍 CPU 的產能」，因為會有問題存在使得產能抵銷：**<br>1. Resource contention - 資源競爭。<br>2. Processor 之間的 communication。
  - **增加可靠度 ( Reliability )：若有一個 CPUs 在執行時失效，然而還有其餘的 CPUs 可以繼續執行，使得系統不會因此停頓。**<br>**＜Note＞**<br>1. **Graceful degradation** ( 漸進式滅亡 )：又稱為「Fail - soft」，**系統不會因為某些硬體或是軟體元件故障而停頓，仍然保持運作的能力。**<br>2. **Faild - Tolerent system ( 容錯系統 )：具有「Graceful degradation」特性的系統。**
  - **提升運算的經濟效益 ( Economy of scale )**：因為「N 個 CPUs 在同一部機器」**可共享該機器的資源 ( Memory、Bus、I/O - devices ... ) **，所以較於「N 部機器」更為便宜而寫有效率。


![1530093944139](\blog\images\1530093944139.png)



##### Symmtric Multiprocessor - SMP



**每個 Processors 的工作能力是相同的 ( Identical )，並且每個 Processors 皆有對等的權利存取資源。**

- **Pros**
  - **可靠度較為 ASMP 高。**
  - **效能較高。**
- **Cons**
  - **SMP 的作業系統開發互斥存取的機制，使得設計較為複雜。**



##### Asymmtric Multiprocessor - ASMP





![ASMP_2](\blog\images\ASMP_2.png)



**每個 Processors 的工作能力不盡相同，通常採取「Master-Slave ( Boss-Employee )」架構。Master Processor 負責工作分派、資源分配與監督 Slave Processor 等管理工作，**其餘的 Slave Processors 負責執行工作。



- Pros
  - **因為與開發「Single - CPU」的作業系統相似，所以相較為簡單。**
- Cons
  - **可靠度較為 SMP 低。**
  - **由於 Master Procrssor 是效能的瓶頸，所以效能較低。**



##### 「Multiprocessor」 V.S  「Multicore Processor」



- **依作業系統的設計觀點是沒有差異的。( 將一個 Core 視為一個 logical CPU 資源 )**
  - $Ex. $ 主機板中有 4 顆 Duel-core CPUs ，對於作業系統來說，視為 $4 \times 2 = 8$ 個 CPUs 可以使用。 

- Multiprocessor




![1530099598137](\blog\images\1530099598137.png)

- Multicore Processor
  - Pros<br>1. Power Saving<br>2. 因為在同一個晶片之內，傳輸的速度較快。




![multicore](\blog\images\multicore.png)



#### 分散式系統 - Distributed System



又**稱為「Loosely-Coupled system ( 鬆散耦合系統 )」。**



- 多部機器彼此透過網路( Network )、Bus 的方式相互串聯。
- 每部機器之 CPU 都有各個自有的 Memory、Bus、I/O-device ... ，**且並不共享，Clock time 也不盡相同**。
- 每部機器的作業系統也不盡相同。
- **溝通採用「Message Passing」的方式。**
  1. 建立連線 ( Communication Link )。
  2. Message 相互傳輸。
  3. 釋放連結 ( Link )。
- 實現「Client - Server Computing Model」
  - Server：提供服務的機器。<br>$Ex. $ Mail server、File server、DNS、Printer server、Computing server...。
  - Client：本身不提供服務，當需要某項服務時，向對應的 Server 發出請求，當 Server 服務完成，再將結果回傳至 Client。
  - **＜Note＞：「Peer-to-Peer Model」**<br>Peer：同時具有 Server 與 Client 的角色。
- **用以實現「Remote site communication」**
  - $Ex. $ 透過網際網路可以實現 Email、FTP。
- Pros
  - **Computation speed up - 增加吞吐量 ( Throughput )**：
  - **增加可靠度 ( Reliability )**：
  - **Resource Sharing - 提升運算的經濟效益 ( Economy of scale )**：所以成本低。



### 即時系統 - Real-time System



#### Hard Real-time System




![1530103496272](\blog\images\1530103496272.png)



**This system must ensure the critical task completed on time.**



**工作必須要再規定的時間內完成，否則會有重大危害的狀況發生。**



- 設計考量：
  - 所有時間延遲之因素皆須納入考量( 如：感測器資料傳輸速度、運算速度、Signal 傳輸速度... )，**並且確保這些時間加總能在 Deadline 前做完**。
  - **所有會造成時間過久或無法預測之設備或機制，最好少用或不要使用。**<br>$Ex. $ **磁碟 ( Disk ) 最好少用或不用、虛擬記憶體 ( Virtual Memory ) 絕不使用。**
  - 針對 CPU 排班的設計，**需要先考量是否可排程化 ( Schedulable )，在進行排程規劃 ( 如：Rate-monotonic、EDF scheduling )。**
- **Time sharing system 無法與 Hard real-time system 並存。**
- **現行的商用作業系統不支援 Hard Real-time 的特性**。( 通常保留於客製化的作業系統之中。 )
- **作業系統造成的「Dispatch latency」要盡量降低。**
  - **＜Note＞：一般實務上，Hard Real-time System 少有作業系統的存在。**

- 應用於：**軍事防衛系統、核能安控系統、工廠自動化生產、機器人控制 ...。**





#### Soft Real-time System



**This system must ensure the real-time process get the highest priority than the others and retain this priority level until it completed.**



- 應用於：**Multimedia system、Simulation system、VR system ...。**
- 針對 CPU 排班的設計
  - **必須支持「Preemptive priority scheduling - 可插隊排程」。**
  - **不可提供「Aging」功能。**
- 儘量降低核心 ( kernel ) 的 Dispatch latency time。
- **可支援 Virtual memory，但是要求 Real-time processes 的全部 pages 在完工前，必須置於記憶體之中。**
- **可以與「Time-sharing system」並存。**
  - $Ex. $ **Solaris**。
- **一般商用作業系統皆支持 Soft real-time system 的功能。**



### 批次系統 - Batch system



**將一些較不緊急、定期性、非交談互動性 ( non-interactive ) 的工作，**累積成堆，再分批送入系統處理。



- **主要目的：提高資源的利用度 ( Resource utilization )**
  - 利用冷門時段的時間批次將工作送入執行。
- **不適合用於 Real-time system、User-interactive application。**

- $Ex. $ **庫存盤點、報稅、掃毒、磁碟重組、清算系統 ...。**



### 掌上型系統 - Hand Held System



- $Ex. $ PDA、Smartphone (智慧型手機)、PAD (平板) ...。
- **硬體天生的限制會導致軟體必須配合的功能。**



| 硬體限制                                       | 軟體應對設計                                                 |
| ---------------------------------------------- | ------------------------------------------------------------ |
| 因為**電源供應、散熱不易**所以閹割處理器的性能 | **運算量宜簡單**                                             |
| **記憶體有限**                                 | 程式量宜小，並且要適當的管理記憶體，將不用的記憶體立刻釋放。 |
| **顯示器很小 ( 解析度、長寬比 )**              | **顯示內容精簡化** ( 手機網站須有所刪減 )                    |



## I/O 運作方式



### 詢問式 I/O - Polling I/O



又稱為 Busy-waitting I/O or programmed I/O 。



- 操作流程
  1. User process 發出 I/O 要求給作業系統。
  2. 作業系統收到請求後，( 有可能 )會暫停目前此 process 的執行，並執行對應的 System calls。
  3. Kernel 的「 I/O 子系統 ( subsystem )」會將該請求傳給「裝置驅動程式 ( Device driver )」。
  4. 裝置驅動程式依照此請求設定對應的「 I/O 指令參數( Commands )」給予「裝置控制器 ( Device Controller )」。
  5. 裝置控制器起動，監督 I/O 設備的運作進行，
  6. 在這個時候，作業系統 ( 可能 ) 將 CPU 交付給另一個 Process 執行。
  7. **但是 CPU 在執行 Process 工作的過程中，卻要不斷去 Polling 裝置控制器以確保 I/O 運作是否完成或有 I/O-Error。**
- **Cons**
  - **CPU 耗費大量時間用於 Polling I/O 裝置控制器上，並未全用於 Process 執行，所以 CPU 的效能 ( Utilization ) 低，產量 ( Throughput ) 低。**



![1530157733165](\blog\images\1530157733165.png)



### 中斷式 I/O - Interrupted I/O



- 操作流程
  1. User process 發出 I/O 要求給作業系統。
  2. 作業系統收到請求後，( 有可能 )會暫停目前此 process 的執行，並執行對應的 System calls。
  3. Kernel 的「 I/O 子系統 ( subsystem )」會將該請求傳給「裝置驅動程式 ( Device driver )」。
  4. 裝置驅動程式依照此請求設定對應的「 I/O 指令參數( Commands )」給予「裝置控制器 ( Device Controller )」。
  5. 裝置控制器起動，監督 I/O 設備的運作進行，
  6. 在這個時候，作業系統 ( 可能 ) 將 CPU 交付給另一個 Process 執行。
  7. **當 I/O 運作完成，裝置控制器會發出「I/O-completed」的中斷 ( Interrupt ) 通知作業系統 ( CPU )。**
  8. **作業系統收到中斷後 ( 可能 ) 會先暫停目前 Process 的執行。**
  9. **作業系統必須查詢「Interrupt Vector ( 中斷向量表 )」，確認是何種中斷發生，同時也要找到該中斷之服務處理程式的位置 ( ISR：Interrupt service routine )。**
  10. **跳至 ISR 位置並執行 ISR。**
  11. **ISR 完成後，交還使用權給 Kernel，**Kernel 可能會作通知的動作給 User process。
  12. **恢復 ( Resume ) 原先中斷前的中作執行 ( 或交由 CPU 排程器決定 )。**
- Pros
  - **CPU 不需耗費時間用於 Polling I/O 裝置上，而是可以用於 Process 執行上，所以 CPU 效能 ( Utilization ) 提升、產量 ( Throughput ) 提升。**
- Cons
  - 中斷 ( Interrupt ) 處理仍需耗費 CPU 的工作時間。
  - **＜Note＞：若「I/O 運作的時間」<「中斷處理時間」，則使用中斷 I/O 就不是一個好選擇，因此 Polling 還是有其存在必要性。**
  - 若*中斷發生的頻率太高*，則大量的中段處理會占用幾乎全部的 CPU 工作時間，導致系統效能很差。
  - **CPU 仍需耗費一些時間用於監督 I/O 裝置與記憶體之間的「資料傳輸」過程。**




![1530159007395](\blog\images\1530159007395.png)



### DMA (Direct Memory Access) I/O



DMA-controller 負責 I/O 裝製與記憶體之間的資料傳輸工作，過程中**無須 CPU 5 參與監督，所以讓 CPU 有更多時間用於 Process 執行上。**



- Pros
  - **CPU 效能 ( Utilization ) 更高。**
  - **適合用在「Block-transfer oriented I/O-device」上 ( 因為中斷發生頻率不至於過高 )，如：Disk**。<br>**＜Note＞：不適合用在「Byte-transfer oriented I/O-device」。**
- Cons
  - **因為 DMA 控制器( controller ) 會與 CPU 競爭記憶體與 Bus 的使用權，當控制器占用記憶體或是Bus時，CPU 要被迫等待，所以引進 DMA 控制器增加硬體設計的複雜度。**
- **DMA controller**
  - **通常採用「Cycle stealing (Interleaving)」技術，與 CPU 輪替使用記憶體以及 Bus，若 CPU 與 DMA controller 發生衝突 ( conflict：同時要使用記憶體或 Bus )，則會給予 DMA 較高的優先權。**
  - 通常系統會給予「對該資源需求量頻率較小」的對象**有較高的優先權，可以使得**<br>1. 平均等待時間較小。<br>2. 平均產能較高。




# 參考

---



- [SPOOLing技術 ](https://read01.com/zh-tw/6m34Gn.html)