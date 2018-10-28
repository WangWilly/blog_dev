title: Operating System - Memory Management
author: Willy Wang (willywangkaa)
tags:
  - Memory Management
categories:
  - Operating System
date: 2018-09-08 15:46:00
---
# Memory Management



## Binding



**為決定程式( 或 Process ) 執行的起始位址的動作。**



- **時機分類**
  - **Compiling time**：由編譯器主導 Binding，**也稱為 Static binding。(通常用在作業系統的命令檔之中 )**
    - 產生的 Object code 又稱為**「Absolute object code」**；而在之後的 Loader 稱為**「Abslolute Loader」**，先做完 Loading 的工作之後，會將指定位址的程式碼對作業系統進行詢問( Allocation )，若該位址不可擺放，會對開發者丟出*錯誤回報*。
    - **Cons**
      - **Process 若要改變執行檔的起始位址，必須要重新對原始碼重新編譯，非常不便。**
    - Note：通常用於 .com ( 作業系統命令檔 )。
  - **Loading time 或 Linking loading time**：由 Linking loader 或 Linkage editor 主導 Binding**，也稱為 Static binding。**
    - Compiler 所產生出的 Object code **稱為 「Relocatable object code」( 可重新定位目的碼 )**
    - **Pros**
      - **程式起始位址若要修改，只要重新做「Relocation loading」，而無須重新編譯。**
    - **Cons**
      - 程式充新執行時，若「Module ( 外部參考 )」的數量很多，則會花很多時間在「Re-linking」上。
      - **Process 執行期間，不可以更改程式在記憶體的起始位址。**
  - **Execution time**：由作業系統動態決定起始位址**，也稱為 Dynamic binding。**
    - 決定 Process 起始位址之工作，延到**執行時期 ( execution time )才動態執行，也就是說 Process 在執行期間可以任意變更起始位址且 Process 仍能正確執行。**
    - **需要硬體額外的支持。**
    - **Pros：**Process 的起始位址可在執行期間任意更動且能正確地執行，**有助於作業系統在記憶體管理上的彈性度，像是「Compaction」、「Process 的 Swap out 與Swap in」都很需要這種技術才能完美的執行。**
    - **Cons：**需要硬體額外的支持；**Process 在執行時需時較久，效能較差**。



- Relocation


![relocation](\willywangkaa\images\relocation.png)



- Linking




![linking](\willywangkaa\images\linking.png)



- Relocation



![relocation_2](\willywangkaa\images\relocation_2.png)



- Dynamic relocation




![dynamicrelocation](\willywangkaa\images\dynamicrelocation.png)



- **＜補充＞**
  - Relocation－修正：**當程式執行起始位址改變，某些 Object code 內容必須隨之修正，**才能正確執行。
    - 當有採用「直接定址 ( Direct addressing mode )」指令時，當起始位址改變，就必須改變其地址。
  - **Linking－修正：解決「外部參考 (External symbol reference)」之修正。**
    - 屬於「外部符號；外部參考」：**副程式名稱、外部變數 ( entern )、函式庫 ( Librery )...。**
  - 「Linking loader」負責：
    - Allocation：向作業系統要求起始位址。
    - Loading：將目的碼載入到記憶體之中。
    - **Linking：依照編譯器所交辦的「Linking 修正資訊」，執行 Linking 修正。**
    - Relocation：做重新定位的目的碼相關修正。
- **＜Note＞凡是以「Static binding」的方式定位程式碼，皆無法在執行的期間更改該 Process 在記憶體的起始位址。**



- Relocatable object code

![relocatiableobjectcode](\willywangkaa\images\relocatiableobjectcode.png)



- **記憶體位址探討**
  - Logical address：generateby CPU
  - Phosical address：時竟去 Physical memory (  i.e. RAM ) 存取之位址。
  - 若發生$\left\{\begin{matrix}  Logical\; address = physical\; address \Rightarrow Static\; binding \\ Logical\; address \neq physical\; address \Rightarrow {\left\{\begin{matrix} Dynamic\; binding \\ Page \\ Segement \\ Page—segement \end{matrix}\right.} \end{matrix}\right.$ 
  - 當電腦要將「Logical address」要轉成「Physical address」時，會交由**硬體的 Memory Manegement Unit (MMU) 負責完成。( 不適合交給作業系統處理，因為一直不斷的中斷很浪費效能。 )**




![compilingflow](\willywangkaa\images\compilingflow.png)



## Dynamic Loading



也稱為「Load-on-call」，在執行期間時，若「Mudule」真正被呼叫到而且不在記憶體中，則當下「Loader」才能將它載入到記憶體中。



- **Pros**
  - **節省記憶體空間。**
  - **不需要作業系統額外的支持。**
- **Cons**
  - **Process 執行的時間比較久。**
- ＜Note＞
  - 早期稱為「Overlay 結構」技巧，是開發人員的責任，而作業系統無介入管理。
  - 現在的技術都已經交給作業系統提供的「虛擬記憶體 ( Vrtual memory )」。



## Dynamic Linking



在執行期間時，若「Module」被呼叫到，才將之載入並且與**其他「Module」進行「Linking - 修正」( 解決外部符號參考 )。**



- **Pros**
  - **適用在「函式庫鏈結 ( Library linking )」，如：Dynamic linking library。**
  - **節省不必要的 Linking time。**
- **Cons**
  - **需要作業系統的支援。**
  - 執行所需時間比較久。



## Memory Allocation



### External Fragmentation 情形



在連續性配置的要求下，目前 AV-list 中任何一個 Hole 的大小均小於某個 Process 的大小，**但是當這些 Holes 大小的加總時，卻大於等於 Process 大小，而這些零碎的可用記憶體空間因為不連續，所以仍然無法配置給 Process**，這種情況就稱為「External Fragmentation」。



- 空閒空間不能使用，記憶體使用度低 ( Low memory utilzation )。
- ＜Note＞
  - 一般而言，每當配置 N 大小的空間給 Process 時，會有大約 0.5N 的外部碎裂，所以外部碎裂的比例為 $\frac{0.5 \times N}{N + 0.5 \times N} = \frac{1}{3}$ 稱為「One third rule」，**顯現出外部碎裂是非常嚴重的問題。**



#### 解決方法



##### 使用「Compaction ( 壓縮；聚集 )」技術




![compaction](\willywangkaa\images\compaction.png)



移動**執行中的 Process**，使得原本非連續的可用記憶體空間 ( Hole )，可以聚集形成一個夠大的連續可用的記憶體空間。

- **演算法難以實作**
  - **不易在最段時間制定最佳的 Compaction 策略。**
  -  **Processes 必須為「Dynamic binding」才可於執行期間移動。**(大多數程式傾向於 Static binding)



##### ☆使用 Page Memory Management (分頁記憶體管理)



- 將「Physical memory」( RAM ) 視為一組 **Frame (頁框) 的集合**，且個各個 **Frame size 相同**。
  - **注意：Frame size 是硬體決定，作業系統只是配合**，Paging 使採用「Physical」的觀點。
- 「Logical memory」( User process 大小 ) 視為一組 **Paging ( 頁面 )**的集合，**且 Page size 要使它等於 Frame size。**
- 作業系統以**「非連續性」的配置原則**，即若 Process 大小等於 n 個 Pages ，則作業系統只需在「Physical memory」中找出**n 個可用的頁框(不一定要連續)，即可配置給 Process。**
- 作業系統會針對每一個 Processe 建立一個**Page table ( 分頁表 )，紀錄各個 Page 實際置於哪個編號的頁框，當 Process 大小等於  n 個頁面，則他的分頁表就有 n 個 entry。**
  - **注意：分頁表被儲存於 PCB ( Process control block ) 之中。**
- 圖示




![paging](\willywangkaa\images\paging.png)



- **Logical  address 轉譯 physical address ( 使用 Memory Manegemeny Unit )**
  1. Logical address 初始是**單一量**，CPU 自動拆解成 p d(如下圖)，**其中 P 代表分頁編號， d 代表 Page offset ( 偏移量 )。**( Note：$單一量位址 = Page\;size \times "p" + "d"$ )
  2. 依據 P 查詢分頁表，取得該 Process 的頁框編號 f 。
  3. f 與 d 合成為 f d (如下圖) 即為「Physical address」或 $ f \times Page \; size + d $。



![logical2physicaladdress](\willywangkaa\images\logical2physicaladdress.png)



- **Pros**
  - **沒有外部碎裂問題。**
  - **可以支援記憶體共享( Memory sharing )與記憶體保護( Memory protection )的實施。**
  - **可支持 Dynamic loading 、Dynamic linking 與 Virtual memory 的實現。**
- **Cons**
  - 由於 Process 的大小不一定會是分頁大小的整數倍數，所以**會發生內部碎裂的問題 ( Internal fragementation )。**
    - **＜Note＞：若分頁大小愈大，則內部碎裂愈嚴重。**
  - **需要額外的硬體支援。**
    - **分頁表的製作。**
    - 邏輯位址利用 **MMU** 轉換成實體位址。
    - 因為要轉換記憶體位址，所以**「Effective memory acess time」比較長( 與連續記憶體配置比相比)。**



###### Memory sharing




![memorysharing](\willywangkaa\images\memorysharing.png)



若多個 Process 彼此具有共通的**唯讀分頁( Code、Data )**，則我們可以藉由 Process 各自的分頁表將共通的分頁*映射* 到同一個頁框，如此可以節省記憶體空間。



- Copy on write



###### Memory protection



在分頁表中多加一個「Protection bit」欄位，$\left\{\begin{matrix} R：Page \; 唯讀。 \\ W： Page \; 可以讀寫。 \end{matrix}\right.$



##### Multiple base/limit regisers (不建議)



- 拆解 Process 的記憶體為「Code section」與「Data section」，分開連續配置的空間 ( Hole ) ，以**降低外部碎裂發生的機率(非解決外部碎裂為題)。**
- 因為每個 Process 需要兩套的 Base 與 Limit 暫存器，分別記錄「Code section」與「Data section」的起始位址與大小。



### Internal Fragmentation 情形



**配置的記憶體空間超過 Process 大小，而該差值的記憶體空間無法給該 Process 使用，亦其他 Processes 無法使用**，這種情況稱為「Internal Fragmentation」。




### Contiguous Memory Allocation - 連續性記憶體配置



也稱為 Dynamic Variable Parition Memory Managememt ( 動態變動分區記憶體配置 )。



- 作業系統配置 Process 一個**連續的可用記憶體空間 ( Free memory space )。**
- Dynamic Variable Parition Memory Managememt
  - 名詞解釋
    - **Partition ( 分區 )：Process 所占用的記憶體空間。**
    - **Partition 數量等價於「Process 數量」亦等價於「Multiprogramming Degree」。**
  - 不同時間點，系統內 Process 數量不固定，所以**Partition 數量不固定而稱為「Dynamic」。**
  - 由於各個 Process 大小不盡相同，所以**各個 Partition 大小也就不一定相同而稱為「Variable」。**
- 記憶體中會有一些可用的記憶空間 ( Free memory space or Free memory block ) 稱之為「Hole」，通常作業系統會使用「鏈結串列」的概念管理 Holes ，稱為可用空間串列 ( Available list；AV-list )。




![avlist](\willywangkaa\images\avlist.png)



#### 配置方法



- **First-fit：**從 AV-list 的開頭找，直到找到**第一個 Hole ，其大小大於等於 Process 的大小，**即可配置之；若找完串列中，所有 Hole 沒有一個適合的則終止。
- **Best-fit：**必須檢查 AV-list 中**所有 Holes** ，找出一個符合「其大小大於等於 Process 的大小」且**「其大小與 Process 大小的差值為最小」的 Hole 配置給 Process。**
- **Worst-fit：**必須檢查 AV-list 中**所有 Holes** ，找出一個符合「其大小大於等於 Process 的大小」且**「其大小與 Process 大小的差值為最大」的 Hole 配置給 Process。**



- Ex ( 依上圖 )
  - 有一 Process 為 90 K
    - 則使用「First-fit」，會配置 **（A）** Block 之 90 K 給Process，剩下 **210** K。
    - 則使用「Best-fit」，會配置 **（B）** Block 之 90 K 給Process，剩下 **10** K。
    - 則使用「Worst-fit」，會配置 **（C）** Block 之 90 K 給Process，剩下 **410** K。







- **比較表**



| 綜合比較 |           | 時間效率 | 空間利用度 |
| -------- | :-------: | -------- | ---------- |
| 佳       | First-fit | 最佳     | 佳         |
|          | Best-fit  | 差       | 最佳       |
|          | Worst-fit | 差       | 差         |



#### 問題與討論



- Ex
  - Page size：10 kB
  - Process 大小：32 kB


$$
\because 需要配置 4 個分頁給 \;Process \\
\therefore 內部碎裂 = 4\times10 - 32 = 8 \;kB
$$


### 分頁表的實現



#### 使用暫存器保存分頁表



使用暫存器保存分頁表中每個 Entry 的頁框編號。



- **Pros**
  - **存取分頁表時無須記憶體存取，速度最快。**

- **Cons**
  - 因為數量有限，**不適合用於大型的分頁表( 大型 Process )。**



#### 使用記憶體保存分頁表 ( 不太採用 )



用兩個個暫存器 ( PTBR；Page table base register 、PTLR；Page table length register ) 紀錄該分頁表位於記憶體中的位址與分頁表大小。



- **Pros**
  - **適用於大型分頁表。**
- **Cons**
  - **需額外多一次的記憶體存取，所以速度很慢。**



#### 使用 TLB ( Translation lookasdie buffer ) 加速



使用 **TLB ( Translation lookasdie buffer ) register ( or Associative register ) 保存分頁表中經常被存取之分頁編號與頁框編號**，而完整的分頁表還是置於記憶體之中。



- 圖示



![TLB](\willywangkaa\images\TLB.png)



- **使用 TLB 的「Effective memory acess time」 =  **$P \times ( TLB\; time + Memory \;acess \;time + (1-P) \times (TLB + 2 \times Memory \;acess \;time) )$
  - P 為 TLB 的 hit ratio。



#### 問題



##### 使用 TLB 之「Effective memory aceess time」



Ex

- Register acess time：0 ns ( ignored )。
- Memory access time：200ns。
- TLB time：100 ns
- TLB hit ratio：90%
- 求 Effective memory acess time 當分頁表儲存於：
  - 暫存器：200 ns
  - 記憶體：400 ns
  - 記憶體、TLB 加速：$0.9 \times (100 +200) + 0.1 \times (100 + 0.1\times200) = (100 + 200) + 0.1 \times 200$<br> = 320 ns。



##### Logical address 與 Physical address bit 數量計算



- Page size  = 1KB。

- Process 最大有 8 個 Pages。

- Physical memory 有 32 個 frames。

- 求取

  - Logical memory 需要多少 bits。
  - Physical memory 需要多少 bits。


- Logical address

 

因為一個分頁大小為 1 KB 也就是 $2^{32}$ bytes，所以 d 佔有 10 bits；

Process 最多 8 個分頁也就是 $2^3$ 個分頁，p 需要 3 個 bits 紀錄，所以需要 3 + 10 = 13 bits ( Logical )。



Physical memory 有 32 個頁框，也就是 $2^5$ 個頁框， f 需要 5 個 bits 紀錄，所以總共需要 5 + 10 = 15 bits ( Physical )。



##### Page table 大小計算



Ex1

- Page size = 8 KB。
- Process 大小 = 2MB。
- Page table entry 佔有 4 bytes。
- 求此 Process 的 Page table size。



一個 Process 中最多有 $\frac{2 \; MB}{8 \;KB} = 2^8$ 個分頁，也就是說每個 Process 的分頁表都有 $2^8$ 個 Entry ，所以分頁表大小為 $2^8 \times 4 \; B = 1 \; KB$。



Ex2

- Logical address = 32 bit。
- Page size = 16 KB。
- Page table entry 佔有 4 B。
- 求 Max page table size。



一個 Process 中最多有 $\frac{4 \; GB}{16 \;KB} = 2 ^{18}$ 個分頁，也就是說每個 Process 的分頁表都有 $2^{18} $ 個 Entry ，所以分頁表大小為 $2^{18} \times 4 \;B = 1 \; MB$。



- 承上，若 Logical address 改為 48 bit 。



一個 Process 中最多有 $\frac{ 2^{48}\;B}{16 \;KB} = 2 ^{34}$ 個分頁，也就是說每個 Process 的分頁表都有 $2^{34} $ 個 Entry ，所以分頁表大小為 $2^{34} \times 4 \;B = 64 \; GB$。



**＜Note＞：Page table 太大。**



Ex3



- Page size = 16 KB
- Page table entry 佔有 4B
- Max page table size 恰為 one page。
- 求 Logical address length。



因為一個分頁表的大小為一個分頁的大小 ( 16 KB ) ，而一個 Page entry 的大小為 4 B ，所以總共會有 $\frac{16\;KB}{4 \; B} = 4K$ entries ($2^{12}$)，所以 p 為 12 bits，而每個分頁的大小為 16 KB ($2^{14}$)所以 d 為 14 個 bits，最後羅技位址的長度為 12 + 14 = 26 bits。



##### 解決 Page table size 過大的問題



下一節細述。



### ☆處理過大分頁表



#### ☆☆Multilevel paging ( Hierarchical paging; Paging the page table; Forward mapping )



並不是將分頁表縮小，而是將不需要的分頁表先存在磁碟之中，縮小分頁表在 RAM 佔有的空間，**所以提出多層次的分頁，藉由此作法，不須將整個分頁表全部載入 RAM ，而是載入部分所需的內容，。**




![twolevelpaging](\willywangkaa\images\twolevelpaging.png)



- Ex ( Two-level paging ) 下圖
  - Level-1 page table：**有** $2^x$ **個 Entries，每個 Enteries 記錄某個 level-2 page table 的指標 (位址)。**
  - Level-2 page table：**有** $2^y$ **個 Entries，每個 Enteries 紀錄著頁框的編號。**
  - 可以將原本**分頁表為** $2^{x+y}$ 大小，拆成同樣可以記錄一樣多頁框編碼的 Multilevel page table (outer：$2^x$, inner：$2^y$、total：$2^x\times2^y$)
  - 雖然要記載頁框編號的**分頁表容量(存於硬碟)變多**，但是**每次需載至 RAM 的容量大幅減少** (Process 在執行時，只需要一個 Level1-page table 與某一個 Level2-page table 在 RAM 中即可)。




![twolevelpaging_2](\willywangkaa\images\twolevelpaging_2.png)



- Ex( Three-level paging ) 




![threelevelpaging](\willywangkaa\images\threelevelpaging.png)



- Cons
  - **Effect memory access time 更久**：需要更多次的存取在記憶體的分頁表。
    - Ex two-level paging 需要 3 次的記憶體存取，three-level paging 需要 4 次的記憶體存取。



##### 相關計算題



- Ex1
  - TLB time：100 ns
  - TLB hit ratio：80 %
  - Memory access time：200 ns
  - 採用 Two-level paging
  - 求出 Effect memory access time



則 $ ( 100 \;ns+ 200 \; ns ) + 20％ \times (100 \; ns + 3 \times 200 \; ns) = 420 \;ns$ 



- Ex2
  - Logical address = 32 bits
  - Page size = 4 kB
  - Page table entry 佔有 4B
  - 求 Max page table size
    - Single-level paging
    - 在 level-1 page table 與 level-2 page table 大小相等之下的 Two-level paging



> Single-level paging

因為 Page size 為 $4 KB = 2^{12} B$ 所以會有 $\frac{2^{32} B}{2^{12} B} = 1M \; entries$ ，而每個 Entry 佔有 4B，所以分頁表總量為 $1 \;M \times 4 \;B = 4 \;MB$。

> Two-level paging

每次要抓入 RAM 的 Page table 大小為 $(1 \;K + 1 \; K) \times 4B = 8KB$。

> 因為 Page size 為 $4 KB = 2^{12} B$ 所以會有 $\frac{2^{32} B}{2^{12} B} = 1M \; entries$ ，解著，將分頁表分成兩層，也就是 level 1 應有 $2^{10}$ 個 entries ，level 2 也有 $2^{10}$ 個 entries，但是總共有 $2^{10}$ 個 level 2 page table，而每個 Entry 佔有 4B，所以分頁表總量為 $(1 \;M + 1K) \times 4 \;B = 4.004 \;MB$。



- **☆Ex 3**
  - Logical address = 64 bits
  - page size = 16 KB
  - Page table entry 佔有 4 B
  - **任一 level paging 之 Max page table size 頂多為一個分頁**，則至少分幾層 (？-level paging)。



總共有 $\frac{2^{64} \;B}{2^{14} \;B} = 2^{50} \;entries$ ，然而任一 level paging 之 Max page table size 頂多為一個分頁(16 K)，**所以任一 level 之分頁表最多有** $\frac{16 \; KB}{4 \;B} = 2^{12} entries$，最後 $\lceil \frac{50}{12} \rceil = 5 \;levels$ 所以最少可以分 2 層。



#### Hashing page table




![hashpagetable](\willywangkaa\images\hashpagetable.png)



利用雜湊的技巧將分頁表視為雜湊表，具有**相同雜湊碼(位址)**的分頁碼與它的頁框碼資訊會置於同一個 Entry ( bucket ) 中，且以「Link list」(chain) 串接。



- 一開始先將雜湊表(指標陣列) 載入到 RAM ，算完雜湊函數之後，將該雜湊串列載入至 RAM 中，最後，當 RAM 位址不夠時，可將先前載入 RAM 的串列移除。
- **Cons**
  - **在「Link list」中使用線性搜尋找尋符合的分頁編號是很耗時的工作。**



##### 相關計算題



- Ex
  - $H(x) = x \;％\; 53$
  - Page table entry 佔有 4 B
  - 求 Hashing Page table size？



總共應有 53 個 Entries ，所以 $53 \times 4 B= 212 B$。



#### ☆Inverted page table ( 反轉分頁表 )




![invertpagetable](\willywangkaa\images\invertpagetable.png)



**以實體記憶體為記錄對象**，並非以 Process ( 虛擬記憶體 ) 為對象，**即若有 n 個頁框，則此表就有 n 個 entry，每個 entry 紀錄（Process id, Page no.）之配對資訊，代表這個頁框存放哪個 Process 之 Page相對於該分頁標號。**



- 整個作業系統只需要**一份表格( 不同的 Process 之間) **即可。

- Cons
  - 必須使用（Process id, Page no.）資訊一一比對查詢分頁表**相當耗時**。
  - **☆因為還需多比對「Process id」才能找到該頁框，所以當同時有兩個 Process 共享同一塊程式碼時，是必須要多使用一個 entry 來放置該對應的頁框碼，然而一個 entry 即是一個頁框，所以對應到同一塊位址的區塊會存於相異的頁框，無法實現「Memory sharing 」的功能。( 下圖 )**




![invertpagetable2](\willywangkaa\images\invertpagetable2.png)



##### 相關計算題



- Ex
  - Page size = 8 KB
  - Physical memory = 16 KB
  - Page table entry 佔有 4 B
  - 求 Inverted page table size？



現在有 $\frac{16\;GB}{8\;KB} = 2\;M \;entries$，也就是有 2 M 個頁框，所以 Inverted page table 大小為 $2 \;M \times 4 \;B = 8 MB$。



### Segement memory management




![segementation](\willywangkaa\images\segementation.png)



- 將**實體記憶體**視為一個夠大的連續可用空間。
- 將**邏輯記憶體 ( Process )** 視為一組 **Segement 的集合，且各 Segement 大小不一定相同。**
  - **CPU 以「Logical address」看待該 Process 的所有 Segement。**( User 相對於 RAM )
    - **Code segement、Data segement、Stack segement ...**
- 配置原則
  - **Segement 之間可以是非連續性記憶體配置。**
  - **每一個 Segement 必須占用連續的記憶體空間。**

- **作業系統**會替每個 Process 建立**分段表 (Segement table)**，紀錄每個 Segement 的**大小( limit )與起始位址( Base )**。
- **邏輯位址轉換實體位址。**
  - 邏輯位址有兩個**變量 ( 變量為獨立不相關，因為不同分段之間大小不等 )**。
    - s：分段編號
    - d：該分段的偏移量 ( offset )
  - **依照 s 在分段表查找，取得該分段的大小與起始位址。**
  - **必須檢查偏移量是否小於該分段大小。**
    - 若成立則將該實體位址返回給正在執行的 Process。( Physical address = base + d )
    - 若不成立則**「MMU」**會產生一個**非法存取的 exception 給作業系統**，將該 Process 終止。



- **Pros**
  - **沒有內部碎裂。**
  - 可以支持「Memory sharing」與「Memory protection」。
  - 可以支持「Dynamic loading」、「Dynamic linking」與「Virtual memory」的實現。
  - **因為 Segement 以邏輯位址查找，比起「Paging」更容易實現「Memory sharing」與「Memory protection」。**

> 以 Protection 為例：
>
> - 分段
>
>
![segementprotection](\willywangkaa\images\segementprotection.png)
>
> - 分頁 ( Page size = 100 K )
>
>
![pageprotection](\willywangkaa\images\pageprotection.png)




![segementhw](\willywangkaa\images\segementhw.png)



- **Cons**
  - **有外部碎裂。**
  - 需要有額外硬體的支持。
    - 分段表的保存。
    - 邏輯位址轉換需要「MMU」 的協助。
  - Effective memory access time **更久**。
    - 需檢查分段偏移量是否小於該分段大小。



#### 相關計算題



|      | Limit | Base |
| ---- | ----- | ---- |
| 0    | 100   | 4200 |
| 1    | 500   | 80   |
| 2    | 830   | 7300 |
| 3    | 940   | 1000 |



- Ex
  - 給一分段表如上圖，求出下列相對於該邏輯位址的實體位址為多少？
    - （1）(0, 90)
    - （2）(1, 380)
    - （3）(2, 900)
    - **（4）(3, 940)**
  - Ans：
    - （1）4290
    - （2）460
    - （3）非法存取
    - （4）非法存取



### 分頁分段比較表



|                        Page                         |                       Segement                        |
| :-------------------------------------------------: | :---------------------------------------------------: |
|               **各個 Page size 均同**               |              **各個分段大小不一定相同**               |
|          以「Physical address」的觀點處理           |            以「Logical address」的觀點處理            |
|               無外部碎裂，有內部碎裂                |                有外部碎裂，無內部碎裂                 |
| 「Memory protection」與「Memory sharing」較難以實施 | 「Memory protection」與「Memory sharing」較容易以實施 |
|                 無須檢查分頁偏移量                  |                  需要檢查分段偏移量                   |
|               邏輯位址為「單一變量」                |      邏輯位址為「雙變量」(分段編號、分段偏移量)       |
|               分頁表紀錄「頁框編號」                |        分段表紀錄分段的「起始位址」與「大小」         |



### ☆Page segement memory management (分頁式分段)



$$
Process \rightarrow Segement \rightarrow Page
$$



![segementpaging](\willywangkaa\images\segementpaging.png)

- 希望保有分段的優點「Logical viewpoint」，並解決外部碎裂，**所以先將 Process 先分段再分頁。**



# 結論



|              | Continue allocation | Page   | Segement | Page segement |
| ------------ | ------------------- | ------ | -------- | ------------- |
| **外部碎裂** | 有                  | **無** | 有       | 無            |
| **內部碎裂** | 無                  | **有** | 無       | 有            |

