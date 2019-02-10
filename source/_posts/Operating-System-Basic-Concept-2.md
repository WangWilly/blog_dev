title: Operating System - Basic Concept 2
author: Willy Wang
tags:
  - Basic Concept
categories:
  - Operating System
date: 2018-07-10 11:20:00
---
#  基礎觀念 - Basic Concept (貳)

---


## 機器指令 Stage (階段)




![1526805202151](\willywangkaa\images\1526805202151.png)



1. IF：Instruction Fetch
2. ID：Instruction Decode
3. FO：Fetch Operands
4. EXE：Execution
5. WM：Write Result to Memory





|      | CPU 會 Memory Access | DMA 要用 Memory |
| :--: | --------------- | ---- |
|  IF  | 必用 | Conflict |
|  ID  | 不使用 | OK |
|  FO  | 可能使用 | OK or conflict |
| EXE  | 不使用 | OK |
|  WM  | 可能使用 | OK or conflict |



(P.3 - 66) (原文書 I/O Subsystem P. 596)

- DMA 之六個步驟：




![1526805611478](\willywangkaa\images\1526805611478.png)

1. **驅動程序（CPU）**
   1. 作業系統中的「裝置驅動程序」**提出要求**，欲將存於硬碟的資料傳輸至 RAM，並指定地址位在 x 的「Buffer」
   2. 「裝置驅動程序」接著**命令「硬碟控制器」（Disk controller）**傳輸 C Byte 到指定的位置
2. **硬碟控制器（Disk）**
   1. **對「DMA 控制器」作初始化設定**
   2. 「硬碟控制器」使用「PCI bus」將每個位元組資料傳輸給「DMA 控制器」
      - **原先不使用「DMA 控制器」的情況下，要將資料交給 CPU 再傳輸到 RAM 中**
3. **DMA 控制器**
   1. 在接受自「硬碟控制器」傳來的資料後，將資料**以「CPU memory bus」傳送到指定 RAM 中的位置 x**；接著將 x 值遞增，C 值遞減直到成為 0
   2. 傳輸完成後，**「DMA 控制器」觸發「Interrupt」以通知 CPU 中的驅動程序**



- Life cycle of I/O request (via Interrupted I/O) (P.3-65) (原文書P.612)




![1526806092878](\willywangkaa\images\1526806092878.png)



> 提升 I/O 效率
>
> 1. **減少執行 I/O 時所觸發的「Context switch」**（$B^+—tree$）
> 2. 當程序（Process）要與裝置溝通時，減少資料對 RAM 存取的次數
> 3. 在大量資料轉移、智慧控制器的運作、「Polling」執行時，盡量使用「Busy waiting」的方式解決短小的「I/O 等待」，減少觸發「Interrupt」的次數
> 4. 增加 DMA 使用率，盡可能**避免使用 CPU 來完成簡單資料的複製工作**
> 5. 讓某些簡單程序可以藉由裝置裡的處理器來執行，使得這些**運算能與 CPU 並行**
> 6. 使 CPU、RAM控制子系統、I/O 裝置效能平衡，因為任何一個步驟發生錯誤很可能會**導致系統的「停滯」（Idle）**



## Blocking I/O  and Nonblocking I/O

- **Blocking I/O**：Process **suspended** until I/O completed.
  - Pros
    - Easy to use and understand.
      - 撰寫維護簡單，明確的知道什麼user program 需求
  - Cons
    - Insufficient for some needs.
      - 欲載入一個大型影片，如要等所有資料載完才反饋給使用者，將導致使用者體驗不好




![Synchronous-I/O 也就是 Blocking I/O](\willywangkaa\images\1526806717525.png)


- **Nonblocking I/O**：I/O return controll as much as available.（**得以在提出 I/O 的需求**後馬上交付控制權回來，所以**該 Process 不用轉換至「Waiting state」**）
  - 應用
    - User interface、Data copy（Buffered I/O）
  - 以**多線程系統來實現**技術
    - 見 [Thread-management](wangwilly.github.io/willywangkaa/2018/07/10/Operating-System-Process-Management-and-Thread-Management/#thread-management) ，若為「One to one」、「Many to many」系統，則某一個線程發出「I/O 請求」時，CPU 會移轉控制權給該程序的其他可執行的線程
  - 快速的**回傳些許目前 I/O 裝置保存於緩存器的資料**
- **Asynchronous I/O**：Process runs while I/O executes.

  - 屬於「Nonblocking I/O」的一種
  - 難以駕馭其功能
  - **I/O 裝置完成需求後才將資料完整回傳並以「I/O subsystem」通知CPU**




![1526807435082](\willywangkaa\images\1526807435082.png)



## Intruupt 機制與種類



- 「Interrupt」觸發時

  1. 作業系統收到後，**若該處理為必要時**，則作業系統會暫停目前 Process  執行，且保存其 「Status」、「Register」（Context switch）

  2. 作業系統會**依據「Interrupt ID」（Interrupt number）查詢「Interrupt Vector」以確認何種中斷被觸發**，且找出其「Interrupt service routine」（ISR）在記憶體的位置

  3. 在處理器中的指令會跳至該服務程序位置，執行「Interrupt service routine」

  4. 完成「Interrupt service routine」後，將控制權返回給 Kernel

  5. 作業系統恢復（Resume）中斷之前 Process 的進行。



![1526808564478](\willywangkaa\images\1526808564478.png)



- 不同分類的「Interrupt」

  - 分類一

    1. 「External interrupt」：CPU 以外的周邊設備、控制卡… 所觸發
       - **I/O completed**、**I/O error**、Machine-check…

    2. 「Internal interrupt」：CPU 在執行程式過程中，**遭遇重大錯誤而觸發並且通常優先權最高**
       - **Divide-by-zero**、**執行非法的特權指令（Privileged instruction）**… 

    3. 「Software interrupt」：User process 在執行之中，若**需要作業系統提供的服務**，必須觸發此中斷以**通知作業系統執行對應的服務**
       - **I/O request**…

  - 分類二

    1. 「Interrupt」：硬體裝置為了某些目的**，需轉換目前處裡器裡執行的 Process** 而產生的「通知信號」
       - 「I/O 設備」為了告知 CPU 目前的狀態所以會觸發「I/O-completed」、「I/O-error」與「Machine-check」…
       - **「Timer」為一種硬體**，在協助「Round robin」排程、**防止單一 Process 霸佔而保護 CPU **時，**會觸發「Time out」**

    2. Trap：程序在執行時為了某些目的**需要作業系統的協助**而產生的「通知信號」
       - Arithmetic error
         - Divide-by-zero、執行非法指令、非法記憶體存取…
       - User process 需要作業系統提供服務，產生「Trap」以通知作業系統
         - I/O-request

  - 分類三：以**中斷的優先權高低分類**
    1. Maskable interrupt（遮蔽式中斷）：觸發後**可被忽略或延後處理**
       - Software interrupt （優先權低）
    2. Non-maskable interrupt（無蔽式中斷）：觸發後必須及刻處理
       - **Internal interrupt（重大錯誤）、I/O error…**




## Hardware Resource Protection






![1526808604540](\willywangkaa\images\1526808604540.png)



### Dual mode operation - 雙重模式

- **作業系統的運作模式只少要可被區分為兩種模式**
  - **Kernel mode**
    - 又稱為「supervisor mode」、「System mode」、「Privileged mode」、「Monitor mode」
    - 以此模式取得系統控制權（CPU）後，**即可使用特權指令（Privileged instruction）**
  - **User mode**
    - 「User process」通常在此模式之下，**取得系統控制權且不允許執行特權指令**

> **「Duel mode」必須要有硬體的支持才可以實現**，CPU 內有「Mode bit」以區分現在狀態為何



#### Privileged instruction（特權指令）

任何可能會造成系統重大危害的指令，則通常為特權指令

唯獨在「Kernel mode」下才能執行，**一旦在「User mode」之下執行會觸發「Trap」以通知作業系統**，而應對處理通常會終止（Terminate）該 Process 的運行



- 包含
  - **I/O instruction**
    - 為了保護 I/O 不被 User Process 濫用
  - 記憶體清除指令
  - **暫存器狀態修改指令**
    - 有關記憶體管理、保護而用的暫存器
    - 如：「Base register」、「limit register」
  - **「Timer」設定指令**
    - 為了保護 CPU 不被濫用
    - 如：「Set」、「Change」
  - **「Enable interrupt」**、**「Disable interrupt」**
  - 「Halt」
    - 當系統無及時的工作需要完成時作業系統會利用此指令以切換至「Idle state」
  - **自「User mode」轉換到「Kernel mode」之指令**



Ex 33​ (Ref P.3-42)

- 以下哪些是特權指令？
  1. Set the value of Timer
  2. Read the clock
  3. Clear Memory
  4. Turn-off interrupt
  5. Switch from user to kernel mode

**(1)(3)(4)(5)**



Ex 34

- 以下哪些是必要的特權指令，且讓使用者程序（User process）能更方便與兼顧安全？ (2)**(3)**(4)**(5)**(7)

  1. Change to user mode
     - **若下放給「User mode」使用也無關緊要，不會使得「User process」得以進入「Kernel mode」**

  2. Change to monitor mode

  3. Read from monitor memory ☆

  4. Write to monitor memory

  5. Fetch an instruction from monitor memory ☆

  6. Turn on timer interrupt
     - **「Timer」本來就保持啟動的狀態，所以此指令並不重要**

  7. Turn off timer interrupt

(2)**(3)**(4)**(5)**(7)



### I/O Protection

**將所有「I/O指令」設為特權指令**，「User process」一律委託「Kernel」執行「I/O 運作」

1. I/O 運作繁瑣複雜，**降低「User process」操控 I/O之複雜度**

2. **避免「User process」對「I/O 裝置」之不當操作**



### Memory Protection

**防止「User process」存取其他「User process」之記憶體空間以及「Kernel」的記憶體空間**

- 以「Contiguous memory allocation」為例，針對每個 Process
  - 「Kernel」會提供一套「Base registers」與「Limit registers」
  - 「Base registers」紀錄 Process 在記憶體中的**起始位址**
  - 「Limit registers」紀錄 Process 在記憶體中的**大小**




![1526809058649](\willywangkaa\images\1526809058649.png)



> Memory management unit（**MMU**；硬體）檢查是否逾越記憶體位置 
>
> （因為記憶體讀取很頻繁，若**交由軟體檢查是否逾越則會導致中斷太多而讓效能低落**）



### CPU Protection

防止「User process」無限期/長期佔用 CPU 而不釋放

- 以「Timer」實施保護

- 作業系統規定 Process 使用 CPU 之最大時限（Max time quantum）

  1. 當 Process 取得 CPU 之後，一開始會設定「Timer」裡的計時器為「Max time quantum」

  2. 隨著 Process 執行時間增加，計時器會逐漸遞減
  3. 當計時器結束時會一同觸發「Time-out interrupt」以通知作業系統強迫結束（Preemptive）該 Process 對 CPU 的使用權
