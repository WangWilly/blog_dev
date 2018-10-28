title: Operating System - Disk Management
author: Willy Wang (willywangkaa)
tags:
  - Disk Management
categories:
  - Operating System
date: 2018-09-10 14:24:00
---
# 	Disk management



## Disk system




![disksystem](\willywangkaa\images\disksystem.png)


- 硬碟系統由多片「Disk」( 磁片 ) 組成
  - 每片磁片通常雙面都可存取資料。
  - 每一面劃分為多個同心圓軌道，稱為「Track」( 磁軌 )。
  - 每條「Track」由多個「Sector」( 磁區 ) 組成。
  - 不同面之相同「Track no.」組成之集合叫做「Cyclinder」( 磁柱 )。
- Ex
  - Disk system 有 10 片磁片
  - 每片皆雙面存取
  - 每面有 2048 條磁軌
  - 每條磁軌有 4096 個磁區。
  - 每個磁區可以存 16 kB 資料
  - 求 Disk system 可存放大小？


$$
( 10 \times 2 ) \times 2048 \times 4096 \times 16 kB = 2.5 \;TB
$$


## Disk access time



- **Seek time**：**將磁頭移至愈存取的磁軌上方所花的時間**。
- **Latency time ( or Rotation time )：將欲存取之磁區轉到磁頭下方所花的時間。**
- **Transfer time：資料在磁碟與記憶體之間傳輸的時間**，與傳輸量呈正比關係。



- Ex
  - 磁碟轉速 7200 RPM
  - 求平均 Latency ( rotation ) time？

$$
Avg. Latency \;( rotation ) \;time = \frac12 \times \frac{1}{7200} \;MPR = \frac{60}{14400} \;SPM = \frac{1}{240} \;second
$$



- Ex
  - Disk system 有 3 片磁片
  - 雙面可存
  - 每面有 1024 條磁軌
  - 每條磁軌有 4096 個磁區
  - 每個磁區可存 32 kB
  - 轉速 6000 RPM
  - 求 Transfer rate ( 每秒可傳輸多少量的資料 )
  - **＜Note＞ 每轉一圈可傳輸一個磁柱的容量**

$$
\frac{6000}{60} \;RPS \times ( ( 3 \times 2 )\;磁軌 \times (4096 \times 32 \;kB) ) = 600 \times 128 \;MB/sec
$$



- Ex
  - Disk system 有 3 片磁片
  - 雙面可存
  - 每面有 1024 條磁軌
  - 每條磁軌有 4096 個磁區
  - 每個磁區可存 32 kB
  - 轉速 6000 RPM
  - **磁碟的平均「Seek time」 = 10 ms**
  - 讀取一個大小為 2 MB 的檔案，要花多少 I/O time？

$$
10 \;ms + \frac{1}{2} \times \frac{60}{6000} \;SPR + \frac{2 \;MB}{600\times128 \;MB/sec} \\
= 10 ms + 5ms + \frac{10}{3\times128} ms
$$



## Disk free space management



- **Block**
  - **磁碟配置空間及存取的最基本單位。**



### Bit vector ( Bitmap )



每一個 Block 皆用一個bit 表示空閒與否 $\left\{\begin{matrix}0 &：& 代表空閒\\1 &：& 代表已被配置 \end{matrix}\right.$ 。若磁碟有 n 個 Blocks，則 Bit vector 大小為 **n bits**。



- Pros
  - 簡單容易實施。
  - **容易找到連續的閒置空間 ( 利用演算法找到連續足夠的 0 )。**
- Cons
  - **適用於小型磁碟；不適用於大型磁碟 ( Blocks 數量龐大造成 Bit vector size 太大，占用記憶體空間 )。**



- Ex




![bitvector](\willywangkaa\images\bitvector.png)



### Link list


![linklistfreespace](\willywangkaa\images\linklistfreespace.png)

作業系統直接在磁碟上，將這些「Free block」以鍊結的方式串接管理。

- **Pros**
  - **適用於大型磁碟**
  - **插入/刪除「Free block」簡單**
- **Cons**
  - 找尋大量的可用「Block」不夠迅速( **因為在磁碟上進行 I/O 讀取「鍊結資訊」非常耗時** )<br>**＜Note＞ 使用「Grouping」改善**
  - **不容易找到「連續的Free block」**<br>**＜Note＞ 使用 「Counting」改善**



#### Grouping



**在「Free block」內除了紀錄「鍊結資訊」，額外紀錄其他「Free block」之編號。 ( Address )**

- **Pros**
  - **可快速找到大量的「Free block」**

- Ex ( 令一個 Block 可以記錄 5 個欄位 )


![Linklist_grouping](\willywangkaa\images\Linklist_grouping.png)



#### Counting



利用連續性配置以及歸還的特性，改變鍊結串列紀錄的方式；Free block 內除了紀錄 鍊結資訊以外，**另外紀錄在此 Free block 之後的連續「Free block」的個數。**

- Pros
  - **適用於連續性配置，方便找到「連續的 Free block」，若連續的 Free block 很多「Link list」 長度也可大幅縮短。**

- Ex


![Linklist_counting](\willywangkaa\images\Linklist_counting.png)



## ☆File Allocation method



### Contiguous allocation




![contiguousallocation](\willywangkaa\images\contiguousallocation.png)



若檔案的大小為 n Block，則作業系統必須在磁碟中找到 n 個「連續的 Free block」，才能配置給它，此外作業系統在「Physical directory」會記錄下列資訊。

1. File name

2. Start block number

3. Size ( 區塊數量 )



- **Pros**
  - **因為連續的 Block 大多落在同一條磁軌或鄰近的磁軌上，所以平均的 Seek time 較小。**
  - **可以支持 Random direct access** ( 任意存取該檔案 i-th block )與 Squential access。<br>$i－th \;block \;no.  = Start \;no. + (i-1)$
  - **與「Linked allocation」相比，可靠度較高。**
  - **與「Linked allocation」相比，循序存取速度較快。**
- **Cons**
  - **會有外部碎裂的問題**<br>磁碟使用磁碟重組 ( Repack )方式解決，**類似記憶體中的「Compaction」**<br>**＜Note＞ 因為所有的檔案是以 Block 為單位在配置，所以所有配置方法都有「內部碎裂」問題**
    - Ex
      - Block size = 10 kB
      - 檔案大小 = 44 kB
      - 所以配置 5 block
      - 內部碎裂 = (5 × 16) - 44 = 6 kB
  - **檔案不易動態擴充**
  - **建檔之前必須事先宣告大小**





- Ex (如上圖)
  - 檔案{ Count } 的大小為 2 block ( Block1 , Block2 )
  - 作業系統在「Physical directory」紀錄資訊如上圖右側。



### Linked allocation




![Linkallocation](\willywangkaa\images\Linkallocation.png)



若檔案大小為 n Block，則作業系統只需要在磁碟中找到 n 個 Free blocks **(不需連續)** 即可配置，且「Allocates block」 之間以鍊結方式串聯，另外作業系統在「Physical directory」紀錄：

1. File name
2. Start block no.
3. End block no.

- **Pros**
  - **無外部碎裂的問題**
  - **檔案容易動態擴充**
  - **建檔之前必不須事先宣告大小**
- **Cons**
  - **因為不連續的 Block 可能散落在不同條磁軌上，所以平均的 Seek time 較大。**
  - **不支援 Random direct access** 使用者的觀點還仍是 Random access 但實際上是先經由 Sequential access 讀至記憶體。
  - **與「Contiguous allocation」相比，若鍊結一旦斷裂資料毀損，可靠度較低。**
  - **與「Contiguous allocation」相比，要在磁碟上讀取鍊結的資訊，才知道下一個 Block 為何，所以循序存取速度較慢。**




#### File Allocation Table (FAT) method - Microsoft windows 採用




![fileallocationtable](\willywangkaa\images\fileallocationtable.png)



**「Allocates block」之間的鍊結資訊存在於「作業系統記憶體區塊」中的一個表格稱為 FAT**，並非存於磁碟中。



- **Pros**
  - 讓「Link allocation」在作「Random access」時能加速
    - 因為可使用存在於記憶體中的「FAT」快速 ( 不用 I/O ) 找到第 i 個 Block 的編號，接著再到磁碟存取該 Block 即可。( **不用在磁碟中追蹤該鍊結的資訊** )



### Index allocation



![indexallocation](\willywangkaa\images\indexallocation.png)



若檔案大小為 n 個 Block，則作業系統配置 n 個 Block ( **無須連續** ) 存放資料之外，**另外需要額外配置「Index block」，儲存所有 Data block 的編號( Address )，**且作業系統在「Physical directory」紀錄：

1. File name
2. **Index block no.**



- Pros
  - **不會有外部碎裂問題**
  - **支援「有效率的 Random access」**與 Sequential access
  - 檔案大小容易動態擴充
  - 建檔之前無須事先宣告大小
- Cons
  - **Index block 會占用額外的空間**
  - **Link space 浪費 ( Overhead ) 比「Linked allocation」大很多**
  - **若檔案很大，則單一個 Index block 可能無法容 ( 保存 ) 納所有 data block no.**




> **＜解決單一個 Index block 不夠存放所有 data block 的問題＞**
>
>
>
> - Linking scheme - 使用多個 Index block，**且彼此以鍊結串接。**
>   - **Cons**
>     - 要對 i-th block 作 Random access 之平均 I/O 次數大幅增加
>   - Ex ( 令一個 Index block 可存放 5 個 no. )
>
>
![linksheme](\willywangkaa\images\linksheme.png)
>
> - Multilevel scheme - 使用階層式的 Index 架構
>   - Pros
>     - 要對 i-th block 作 Random access 之平均 I/O 次數一致
>   - Cons
>     - 因為 index block 太佔空間，甚至多於 data block 數量，**所以極不適合小型檔案**。
>   - Ex ( Two-level index structure )
>
>
![mulitilevelscheme](\willywangkaa\images\mulitilevelscheme.png) 
>
> - Combined scheme - UNIX i-node
>   - 見下面細述


- Ex (如上圖)
  - 若檔案 ｛jeep｝ 大小為 5 block
  - 作業系統已配置 ｛9 , 16, 1, 10 ,25｝ 號 block 給它存放資料，另外配置 19 號 block 作為「Index block」
  - 「Physical directory」( 上圖右上側 )







#### ☆ UNIX i-node

![unix_inode](\willywangkaa\images\unix_inode.png)



- Ex ( i-node with 15 entry，一個 block 可存放 n 個指標 )
  - **1st ~ 12th entry：直接紀錄 data block no.** ( 目前可記錄的 data block：12 )
  - **13th entry：為一指標指向「Single-level index」** ( 目前可記錄的 data block：12 + n )
  - **14th entry：為一指標指向「Two-level index」**( 目前可記錄的 data block：$12 + n + n^2$ )
  - **15th entry：為一指標指向「Three-level index」**( 目前可記錄的 data block：$12 + n + n^2+n^3$ )



- Ex ( 正常的 i-node 定義沿用 )
  - Block size：16 kB
  - Block no. 占用 4 B
  - 求 max file size 大小？

一個 index block 可存 $\frac{16 \;kB}{4 \;B} = 2^{12}$ data block no，所以


$$
max \;file \;size = ( 12 + 2^{12} + 2^{12^2} + 2^{12^3} ) block \\ 
= ( 12 + 2^{12} + 2^{12^2} + 2^{12^3} ) \times 16 \;kB \approx 2^{36} \times 16 \;kB = 2^{50} B = 1 \;PB
$$



- Ex ( 正常的 i-node 定義沿用 )
  - 檔案大小為 8000 block
  - 假設 i-node 已經在記憶體之中，則要存取此檔案的第 6000 data block 需要幾次 I/O？




$$
6000 - 12 = 5988 \\
6000 - 12 - 4096 = 1892 \\
$$



**在「Two level index」的 1892th block，所以需要三次 I/O 存取。**



- Ex ( 正常的 i-node 定義沿用 )
  - 檔案大小為 8000 block
  - 循序存取前 6000 個 data block，需要幾次 I/O？


$$
6000 + (1 \;single－level+ 2 \;two－level) = 6003
$$




## Disk scheduling algorithm



> **＜Note＞：Disk schduling 既無「最好」也無「最差」。**



### FCFS (First come first service)




![FCFSdiskscduale](\willywangkaa\images\FCFSdiskscduale.png)



**最早到達的磁軌請求**優先服務。



- **Cons**
  - **排班效果不佳、磁軌移動量大，「Seek time」較長**
- Pros
  - **公平；No starvation**



- Ex
  - 磁碟有 200 軌，編號：0~199，**磁頭目前停在第 53 軌**，方才服務完第 60 軌，現在「Disk queue」中有上圖磁軌請求。
  - 求磁軌移動總數

$$
|183-53|+|37-183|+|122-37|+\\
|14-122|+|124-14|+|65-124|+|67-65| = 640 \;軌
$$



### SSTF ( Shortest seek time track first )




![SSTFdiskscheduale](\willywangkaa\images\SSTFdiskscheduale.png)



**距離磁頭目前為址最近的磁軌要求**，最優先服務。

- **Pros**
  - **排班效果不錯，需移動之磁軌數較少、「Seek time」小，【但並非為 Optimal scheduling】**。<br>依照目前的題目使用 Look 法則會比較好。
- **Cons**
  - **不公平，有可能會 Starvation**



- Ex
  - 磁碟有 200 軌，編號：0~199，**磁頭目前停在第 53 軌**，方才服務完第 60 軌，現在「Disk queue」中有上圖磁軌請求。
  - 求磁軌移動總數


$$
|67 - 53| + |14 - 67| + |183 - 14| = 236 \; 軌
$$


### Scan




![SCANdiskschedule](\willywangkaa\images\SCANdiskschedule.png)



磁頭來回雙向移動掃描，遇到有「磁軌請求」即執行服務，當磁頭遇到磁軌的開端或是盡頭時，才「折返」提供服務。


![SCANdemo](\willywangkaa\images\SCANdemo.png)

- **Pros**
  - **適用於「大量負載的情況」**，排班效能尚可接受。<br>由於磁軌請求有比較均勻的等待時間。
- **Cons**
  - **在某些時候，對某些「磁軌請求」不盡公平。**(下圖一)<br>＜Note＞：用「C-Scan 方法」解決。
  - 磁頭需要遇到「磁軌開端或盡頭」才折返**會耗費不必要的 Seek time**。(下圖二)<br>＜Note＞：用「Look 方法」解決。

![SCANdemo2](\willywangkaa\images\SCANdemo2.png)


![SCANdemo3](\willywangkaa\images\SCANdemo3.png)

- Ex
  - 磁碟有 200 軌，編號：0~199，**磁頭目前停在第 53 軌**，**方才服務完第 60 軌**，現在「Disk queue」中有上圖磁軌請求。
  - 求磁軌移動總數

(往小的方向)
$$
|53 - 0| + |0 - 183| = 236
$$


#### C-Scan ( Circular-scan )




![CSCANschedule](\willywangkaa\images\CSCANschedule.png)



**只提供「單向的服務」**，折返回程不提供服務。



> **＜爭議＞**
>
> **是否需要將磁軌回程的移動量**$\left\{\begin{matrix}列入\\ 不列入 \end{matrix}\right.$ **計算。** ( 通常不列入 )



- Ex
- - 磁碟有 200 軌，編號：0~199，**磁頭目前停在第 53 軌**，現在「Disk queue」中有上圖磁軌請求。
  - 求磁軌移動總數


$$
|199-53|+|37-0|\\
OR \\
|199-53|+|37-0| + |199 - 0|
$$


#### Look



磁頭服務完該方向的最後一個「磁軌請求」後，**即可折返提供回程服務。**



- Ex
  - 磁碟有 200 軌，編號：0~199，**磁頭目前停在第 53 軌**，**方才服務完第 60 軌**，現在「Disk queue」中有上圖磁軌請求。
  - 求磁軌移動總數

(往小的方向)
$$
|14 - 53| + |183 - 14|
$$


##### C-Look




![CLOOKschedule](\willywangkaa\images\CLOOKschedule.png)



**只提供單向的服務。**



> **＜無爭議＞**
>
> **需要將磁軌回程的移動量列入計算。**



- Ex
  - 磁碟有 200 軌，編號：0~199，**磁頭目前停在第 53 軌**，現在「Disk queue」中有上圖磁軌請求。
  - 求磁軌移動總數

$$
|183 - 53| + |14-183| + |37 - 14|
$$



### 補充



- **不同版本比對**



| 恐龍教科書 |      Modern、其他版本      |
| :--------: | :------------------------: |
|    Scan    |             X              |
|   C-Scan   |             X              |
|    Look    | **Scan ( Elevator 法則 )** |
|   C-Look   |         **C-Scan**         |



## 其它名詞



### Formatting ( 格式化 )


![lowlevelformat](\willywangkaa\images\lowlevelformat.png)

- **Physical format** ( Low-level format )
  - 工廠生產「Disk system」時執行
  - 劃分出「Disk controller」可以存取的「磁區」(如上圖)
  - 偵測有無「Bad sector」( 壞磁區 )
- **Logical format**：在使用者使用磁碟之前必須執行
  - Partition：切割分區，即為「Logical drive」( E.g. C、D、E 磁碟機 )
  - **Logical format：作業系統製作(寫入) 「File management system」所需的資料結構。**
    - 空閒空間管理 ( E.g. bitvector )
    - FAT、i-node
    - 空的「Physical directory」



### Row - I/O



將磁碟視為一個大型的陣列使用，一個「磁區」就視為陣列的一個 entry。



- 沒有「File system」的支援。

- Pros
  - **存取速度快**
- Cons
  - 使用者不易使用，**通常用在資料庫系統的底層。**



### ☆Bootstrap loader



![windowsMBR](\willywangkaa\images\windowsMBR.png)



**開機時讓電腦可以從磁碟載入作業系統的「Object code」到記憶體的特殊「Loader」。**



#### 早期


![boot1](\willywangkaa\images\boot1.png)

- 流程
  1. Power-on
  2. 執行存在 ROM 裡的「Bootstrap loader」
  3. 「Bootstrap loader」將存在於磁碟中的「作業系統目的碼」載入到記憶體之中
  4. 作業系統執行「System configuration」
  5. 開機完成，等待使用者下命令
- **Cons**
  - 「Bootstrap loader」無法任意變更
  - ROM 大小有限，「Bootstrap loader」無法做大



#### 現今


![boot2](\willywangkaa\images\boot2.png)

> 完整的 Bootstrap Loader 位於磁碟的固定「Block」位置，稱為「Boot block」；
>
> 擁有「Boot block」的磁碟稱為「Boot disk」或「System disk」。

- 流程
  1. Power-on
  2. 執行存在 ROM 裡的「Simple bootstrap loader」( 固定 5 - 10 條指令 )
  3. 「Simple bootstrap loader」將存在於磁碟中的「Complete bootstrap loader」載入到記憶體之中
  4. 執行「Complete bootstrap loader」
  5. 「Complete bootstrap loader」將 「OS object code」載入記憶體之中。
  6. 作業系統執行「System configuration」
  7. 開機完成，等待使用者下命令



### 處理 Bad sector



- 磁區毀壞原因
  - 工廠生產時已經毀壞
  - 正常使用後一段時間正常毀損
- 處裡方法
  - **Mark bad sector**
    - 標註完後之後看到這個標記就不使用之
    - Ex：**IDE disk controller** 採用
  - **Spare(備料) sector** ( 下圖一 )
    - **作業系統無法看到及使用「Spare sector area」，只有「Disk controller」可使用。**
    - **工廠生產時，就已經在「Low-level formatting」中預留。**
    - 一旦有「Bad sector」**則「Disk controller」會從「Spare sector」選擇一個 Spare sector 來替代 Bad sector**，將來作業系統在存取該「Bad sector」 時，**SCSI controller 會將它導向至替換後的 sector** ( 作業系統不知情 )。
    - **Cons**
      - **「充新導向」的動作，可能會破壞作業系統「Disk scheduling」的效益。( 下圖二 )**<br>改善：將「Spare sector」分散到每條磁軌( 或磁柱 )上，不要集中存放；若磁區發生毀壞，**則使用相同或鄰近的磁軌上的「Spare sector」來作替代。**
    - Ex：**SCSI disk controller 採用**
  - **Sector slipping ( 下圖三 )**
    - Cons
      - 讀寫次數過大，業界不常使用
- 圖一

![sparesector](\willywangkaa\images\sparesector.png)

- 圖二


![SCSIsparesector](\willywangkaa\images\SCSIsparesector.png)

- 圖三


![sectorslipping](\willywangkaa\images\sectorslipping.png)


### Swap space management



在「Vitrual memory」裡，「Medium-term scheduler」會將磁碟作為「Swap out」的分頁或「Process image」之暫存處。

決定「Swap space」大小時，**最好超估( Overestimate )，比較安全。**

![swapspace](\willywangkaa\images\swapspace.png)

#### 方法



用甚麼方式保存 Swap out page/process image？



##### 使用 File system



仍用檔案的形式保存。



- **Pros**
  - **實現簡單**
- Cons
  - 因為通常使用連續性配置 ( Seek time 小；I/O 時間小 )，**所以會有外部碎裂**
  - **效能比較差**



##### 使用獨立的「Partition」來保存



- Pros
  - 因為採用「Raw-I/O」，無須「File system」支持，**所以效能佳**
- Cons
  - 內部碎裂
  - **若「Partition」不夠大，則需要「Re-partition」**



## 提升「Disk data access」效能



- 「Data striping」( Interleaving )
  - 將多部「Physical disk」組成一個單一的「Logical disk」，**運用「平行存取技巧」來提升效能。**
    1. **Bit-level striping**
    2. **Block-level striping** ( 下圖 )




![datastriping](\willywangkaa\images\datastriping.png)



## 提升「Disk reliability」( 可靠度 )



> 當「Block」毀損，發生「Data lost」時要如何作「Data recovery」？



### Mirror ( shadow ) 技術



每一部正常的磁碟均配備有對應的「Mirror disk」，資料須同時存入正常磁碟與該「Mirror disk」；將來若正常的磁碟發生毀損，則使用「Mirror disk」來取代。



- **Pros**
  - **可靠度高**
  - **「Data recovery」最快**。
- **Cons**
  - **價格盎貴**



### Parity-check 技術



多準備一部磁碟用來儲存**「Parity-check block」之用**，資料寫入時，需額外算出「Parity-check block」內容；**將來，若某一「Block」發生毀損，只要用其它「Block」與「Parity block」作偶同位，即可「Recovery data」。**



- Pros
  - 成本比起 Mirror 技術便宜許多
- Cons
  - 可靠度低於 Mirror 技術<br>若多個 Block 同時毀壞，則無法恢復資料
  - 因為需要偶同位的計算，「Data recovery」的速度比 Mirror 技術的慢
  - **因為資料的寫入也需要偶同位的計算，所以寫入的速度比 Mirror 技術的慢**



## Redundant Array of Implement Disk ( RAID )



![RAID](\willywangkaa\images\RAID.png)



### RAID 0



只提供「Block-level striping ( interleaving )」，未提供任何**可靠度 ( Reliability ) 技術**。



- 用在**存取效能高，但可靠度不重要的場合。**
  - E.g. VOD server



### RAID 1



只提供「Mirror」，未提供任何**提升效能技術**。



### RAID 2



採用記憶體的「Error correcting code」技術來改善可靠度，希望降低「Mirror」成本，**但是成本降低有限 ( 有比 Mirror 少一部磁碟 )。**



- **Cons**
  - **與「RAID 3」相比，可靠度相同，但成本卻比較高。**



**RAID 2 並無實際產品**



### RAID 3



**採用「Bit-level striping」與「Parity check」技術。**



### RAID 4



**採用「Block-level striping」與「Parity check」技術。**



### RAID 5



**採用「Block-level striping」與「Parity check」技術。**



- 將「Parity block」**分散存於不同的磁碟之中，並非集中在一部磁碟。**
  - 避免對單一磁碟過度使用。



### RAID 6



不用「Parity check」技術，改用類似「Reed-Solomon」技術，可以做到兩部「Disk block」同時出錯還是能恢復資料。



- **Cons**
  - **比起「RAID 2」的製作成本太高**



**RAID 6 並無實際產品**



### RAID 01




![RAID01](\willywangkaa\images\RAID01.png)

先 Striping 再整體 Mirror。



- 雖然成本相當昂貴，但是通常用在高效能與高可靠度的場合。

- **Cons**
  - **一部磁碟毀壞，須整組替換**



### RAID 10 ( 優秀 )




![RAID10](\willywangkaa\images\RAID10.png)



先個別 Mirror 再整體Striping。



- 雖然成本相當昂貴，但是通常用在高效能與高可靠度的場合。



### 結論



p.9-31

- RAID 0 用在「高性能( 高存取效能 ) 」且「資料的損失並不重要」的應用場合。
- RAID 1 用在「快速復建」且「高可靠度」的應用場合。
- RAID 01、RAID 10 用在「高性能」與「高可靠度」的應用場合。
- RAID 5 通常適合「儲存大量資料」的應用場合。
- Mirror 高可靠度，但卻非常昂貴。
- Striping 提供高資料傳輸速率，但卻**不能**增進可靠度。
- RAID 3 比起 RAID 1，「增加可靠度但卻使用較少的磁碟完成」、「Bit-level striping 存取效能高」。
  - 但是 RAID 3 的磁碟使用度 ( Utilization ) 比較低，一次只能一個執行一個 I/O 操作。
- ~~**RAID 4 大量讀取傳輸速率高，因為磁碟可以平行的讀取/寫入。**~~
  - ~~**小規模獨立寫入必須存取所有硬碟的資料( 包括 Parity disk 資料 )，算出 Parity 值後再寫入。**~~

- ~~**RAID 5 **藉由散布同未位元到所有磁碟機之中，避免對單一台位元磁碟機過度使用( RAID4 、RAID3 )。~~



＜Note＞ 解釋不完整，稍後更正。

