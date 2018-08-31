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



- Life cycle of I/O request (via Tnterrupted I/O) (P.3-65) (原文書P.612)




![1526806092878](\willywangkaa\images\1526806092878.png)



## Blocking I/O  and Nonblocking I/O

- Blocking - I/O：Process **suspended** until I/O completed.
  - Pros：Easy to use and understand.(撰寫維護簡單，明確的知道什麼user program 需求。)
  - Cons：Insufficient for some needs.(當要載入一個大的影片檔，要等到所有的資料載完才feedback給使用者，會導致使用者體驗不好)




![Synchronous-I/O 也就是 Blocking I/O](\willywangkaa\images\1526806717525.png)


- Nonblocking I/O：I/O calls return as much as available.(控制權在交付 I/O 的需求後馬上還給user process)
  - Eg：user interface, data copy (Buffer I/O).
  - **Implemented via multithreading.**
  - Returned quickly with count of bytes read or written.(回傳目前一定的量給 process)
  - 有多少的 I/O 的資料完成就傳回多少資料。
- Asynchronous - I/O (屬於Nonblocking I/O 的一步分)：Process runs while I/O executes.
  - Difficult to use.
  - I/O subsystem signal process when I/O - completed.
  - 要完整的將整個 I/O 完成才將資料回傳。




![1526807435082](\willywangkaa\images\1526807435082.png)



## Intruupt 機制與種類

- 當 interrupt 發生，OS之處理如下：
  - OS 收到中斷後，若中斷是必須要被立即處理的，則OS會暫停目前的 Process  之執行，且保存其Status and register comtents。
  - OS會依據 interupt ID ( No. ) 查詢**Interrupt Vector**確認何種中斷發生，且找出其 ISR 的位置。
  - Jump to ISR位置，執行ISR。
  - ISR完成後，控制權返回kernel。
  - OS 會恢復( resume ) 中斷之前 process 的進行。

  


![1526808564478](\willywangkaa\images\1526808564478.png)



- interrupt 種類

  - 分類一 ( 分三種 )

  1. External interrupt：cpu以外的周邊設備、控制卡...等所引起。<br>例如：I/O completed, I/O - error, machine-check...。
  2. Internel interrupt：CPU 在執行程式過程中，遭遇重大錯誤而引發。(通常優先權最高)。<br>例如：Divide-by-zero, 執行非法的特權指令 ... 。
  3. Software interrupt：User process在執行之中，若需要OS提供服務時，必須發出此類中斷，目的是**通知OS**執行對應的服務請求。<br>例如：I/O-request ...。

  - 分類二

  1. Interrupt：**Hardware-generated** change control flow.<br>例如：設備發出 I/O-completed, I/O-error, machine-check...以及Timer(cpu排班、cpu保護的**硬體**)發出的time-out
  2. Trap：**Software-generated** interrupt.<br>Catch the arithematic error。利如：divide-by-zerp, 執行非法指令, 非法記憶體存取...<br>user process 需要OS提供服務時，也會發trap通知OS。例如：I/O-request。

  - 分類三：以中斷的優先權高低分類。

  1. Maskable (可遮罩) interrupt：此類中斷發生後，可被忽略或延後處理，也就是說不一定要馬上處理。<br>例如 Software interrupt (優先權低)
  2. Non-maskable (不可遮罩) interrput：此類中斷必須立刻處理。<br>例如：internal interrupt (重大錯誤), I/O-error, ...。 



## Hardware Resource Protection



### 建設基礎的保護




![1526808604540](\willywangkaa\images\1526808604540.png)



#### Dual mode operation - 雙重模式

- 定義：作業系統的運作模式只少要可被區分為兩種模式。
  - Kernel mode (又稱為 supervisor mode, system mode , privileged mode, monitor mode)：<br>代表此刻是kernel取得系統控制權(i.e. 取得 cpu 執行)，且**允許使用**特權( privileged )指令
  - User mode：<br>代表User process取得 cpu 執行，且不允許執行指令。
- **＜Note＞：**Duel-mode必須要有hardware的支持才可以實現。<br>例如：cpu內會有mode bit用以區分現在的mode為何。



#### Privileged intrustions - 特權指令



- 定義：任何可能會造成系統重大危害的指令，可設為特權指令。**只可以**在kernel mode下執行，不得在user mode 下執行，一旦在user mode執行會發出**trap 通知OS**，OS通常會終止(terminate)該process。
- 包含：
  - I/O instruction (for I/Oprotection)
  - Clear memory
  - 關於memory management(保護)所用之Register修改指令(base、limit register)
  - Timer設定指令：set, change (for cpu protection)
  - Enable, Disable Interrupt指令
  - Halt指令
  - change user mode to kernel mode指令
- $Ex.33$ (Ref P.3-42) <br>以下哪些是特權指令？ (1)(3)(4)(5)<br>**(1) Set value of Timer**<br>(2) Read the clock<br>**(3) Clear Memory**<br>**(4) Turn-off interrupt**<br>**(5) Switch from user to kernel mode**<br>
- $Ex.34$<br>以下哪些是必要的特權指令，且讓使用者程式能最方便且兼顧安全？ (2)**(3)**(4)**(5)**(7)<br>(1) Change to user mode ＊原本就是user mode，所以使用也沒差。<br>**(2) Change to monitor mode**<br>※(3) Read from monitor memory<br>**(4) Write to monitor memory**<br>※(5) Fetch an instruction from monitor memory<br>(6) Turn on timer interrupt ＊本來就開啟，所以再開啟也沒關係。<br>**(7) Turn off timer interrupt**



### I/O Protection



- 目的：由於I/O運作較為繁瑣複雜，為了**降低 user process 操控 I/O之複雜度**及**避免 user process 對 I/O-Device 之不當操作**。
- 作法：把所有的I/O指令都設為特權指令，再藉由Duel-mode，一律讓user process委託kernel 執行I/O運作。



### Memory Protection

- 目的：防止user process存取其他 user process 之記憶體空間以及kernel的記憶體空間。(**防止非法的記憶體的存取**)
- 作法：(以 contiguous memory allocation 為例)針對每個process , kernel會提供一套Base registers 與 Limit registers。其中Base registers 紀錄 process 在記憶體中的**起始位址**，Limit registers 紀錄 process 在記憶體中的大小。




![1526809058649](\willywangkaa\images\1526809058649.png)



**＜Note＞：**其中檢查是否逾越記憶體位置是交由硬體檢查(**MMU**, Memory management unit)。(因為記憶體讀取是很頻繁的，若交由軟體檢查是否逾越則會導致中斷太多而讓效能低落)



**＜Note＞：**Base / Limit register 的設定修改必為**特權指令**。



### CPU Protection



- 目的：防止user process 無限期/長期佔用 CPU 而不釋放。
- 作法：利用硬體 **Timer** 實施保護，同時，OS會規定process使用 cpu time 之最大配額(max time quantum)，當process取得cpu之後time初值即設為max time quantum 值，隨著 process 執行時間增加，timer 值會逐步遞減值到timer 為 0 時會一同發出「Time-out interrupt」以通知 OS 強迫該process 放掉cpu。



**＜Note＞：**Timer 的設定與修改必為**特權指令**。