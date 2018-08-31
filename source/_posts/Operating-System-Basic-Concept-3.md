title: Operating System - Basic Concept 3
author: Willy Wang
date: 2018-07-10 21:44:53
tags:
---
# System Call

---



It's a programming interface to the  services provides by the OS.

- 定義：幫助執行中的 user process 與 kernel 之間溝通，當 user peocess 需要OS提供某種服務時，會先以 Trap 通知 OS (由 User mode 轉為 Kernel mode) 並傳入System call ID (no.) 及所需參數，接著 OS 執行對應的 System call，完成後將服務結果回傳至 User process。




![1526605131439](\willywangkaa\images\1526605131439.png)



- 種類 (Ref P.3-8 ~ 3-9)
  - **Process Control**：建立、終止、暫停、恢復執行 process，設定/讀取 process attribute。
  - **File Management**：建立、讀、寫、開啟、關閉、刪除檔案。
  - **Device Management**：讀、寫 Device。
  - **Information of Maintenance：**取得系統日期、時間，取得process屬性。
  - **Communications：**process之間的通訊，且只針對**Massage Passing**方式提供服務。
  - Protection：硬體資源的保護，檔案讀取控制。
- System call 的參數(Parameter) 傳遞方式
  - **暫存器( Register )**：保存參數於暫存器之中傳遞給作業系統。<br>Pros：簡單、存取速度快( 無記憶體存取 )。<br>Cons：不適用於存取大量參數的情況。
  - **記憶體( Memory )**：以一個區塊( Block, Table )儲存這些參數並將此區塊的起始位址存於一個暫存器之中傳遞給作業系統。<br>Pros：適用於大量參數。<br>Cons：存取速度較慢，且操作較為麻煩。
  - **系統堆疊( Stack )**：將參數 Push 堆疊之中，作業系統再從堆疊 Pop 取得參數。<br>Pros：也適用大量參數的傳遞，操作也很簡單。<br>Cons：目前無( 可用暫存器或是記憶體實現堆疊 )。
- $Ex. 20$ (Ref P.3-36)



# 作業系統的架構( Structure )

---



- 分類
  - Simple
  - More complex than simple
  - **Layered approach**
  - **Microkernel**
  - **Module**
  - Hybrid



## Simple



- 無雙重模式( Duel-mode )
- 單工
- 無模組化( Module )的設計
- 例如：MS-DOS



## More complex than simple



- Limited by hard functionality.( 因為當時硬體技術不夠成熟，作業系統受到硬體很大的限制 )
- The original UNIX had limited structuring.
- 例如：UNIX
- UNIX 包含兩個分開的部分
  - System Programs
  - Kernel
- Beyond simple but not fully layered.




![1526608163203](\willywangkaa\images\1526608163203.png)



## Layered approach




![1526609224214](\willywangkaa\images\1526609224214.png)



- 定義：
  - 採取 Top-down 的方式**切割系統功能/元件**以降低複雜度。
  - 元件/模組之間呼叫關係*分層*，即：**上層可以呼叫下層的功能，但下層不得呼叫上層的功能。**
  - 使用由 Button-up 的測試、除錯( 防止下層可以呼叫上層 )。
  - 層次( Layer )的劃分沒有明確的規定。
- Pros
  - 降低複雜度。
  - 有助於分工。
  - 測試、除錯、維護容易。
- Cons
  - 很難精準的劃分層次。例如：$A \rightarrow_{call} B \rightarrow_{call} C$ 但又因為某些原因必須要設計 $C \rightarrow_{call} A$，最後變成  $A, B, C$ 都在同一層次；解決：將 $C$ 再拆開分成不同層次(難)。
  - **若層次太多，可能會導致作業系統的效能變差。**



## ＊Microkernel - 微核心



- 由卡內基-美隆大學( CMU )率先提出。
  - 代表產品：Mach OS
- 定義：將 Kernel 中一些**不必要的服務移至 User mode 提供服務，以 System program 方式存在**，如次一來可以得到一個比較小的 Kernel 。
- 一般而言，微核心提供下列三個最必要的服務：
  - Process control
  - Memory Management (不包含Virtual memory)
  - Process communications (只提供 massage passing 的服務)




![1526610608669](\willywangkaa\images\1526610608669.png)



- Pros
  - Easier to extend a microkernal. ( 因為服務是在User mode，服務的新增或刪除可以不會改到 Kernel 的架構，相對來說更簡單 )
  - Easer to port the OS to new archutectures.( 作業系統移植到新的服務平台-cpu不用改太多 )
  - More reliable. ( 若有一個服務失效時，對 Kernel 的傷害較小 )
  - More secure.
- Cons
  - 效能差，因為存在大量的 User mode 與 Kernel mode 的訊息傳遞。



**＜Note＞：**相對於微核心的相反$\rightarrow$**Monolitic kernel**。<br>定義：所有的系統服務皆須在 Kernel 執行。( 商用如：Windows $\Leftrightarrow$ 使用者無法輕易的更改功能 )<br>Pros and Cons 皆與微核心相反。



## Module - 模組化



- Many OS implement loadable kernel modules. ()
- Use **object-oriented approach**.
- Each core component is **separate**.
- Each talks to the others over known interface.
- **Each is loadable as needed within the kernel.** ( 需要該服務的時候再載入到記憶體執行，不需要時就從記憶體移除 )
- **Similar to "Layers" but with more flexible.**( 效能更好 )
- 例如：Linux, Solaris ...
- Pros
  - 因為與 Kernel 傳輸的距離不長，效能比較好。


![1526611810996](\willywangkaa\images\1526611810996.png)



## Hybrid - 混和型



- 現在的 OS 很難純粹關屬於某一型。
- $Ex.$ Linux and Solaris 是 **Monolitic** 且也是**Moduler for dynamic loading.**
- $Ex.$ **Windows**大致上是 **Monolitic**，有時真針對不同客戶的需求才會對一些子系統再加入*Microkernel*。
- $Ex.$ **Apple Mac OS Kernel**
  - **Mach microkernal**
  - Some of BSD UNIX
  - I/O kit
  - **Dynamic loadable module( kernel extension )**


![1526613108853](\willywangkaa\images\1526613108853.png)


Aqua：負責 GUI 的顯示。



# Vritual Machine

---




![1530162737868](\willywangkaa\images\1530162737868.png)



- 定義：利用**軟體技術**模擬出一份與底層硬體一模一樣的功能介面之**抽象化機器( abstract machine )。**


![1526613748648](\willywangkaa\images\1526613748648.png)

- 名詞解釋
  - Host：underlying hardware system, OS. ( 原生的硬體與 VMM 都可稱之為 Host )
  - **VMM ( Virtual Macihine Management or Hypervisor )**：*Creates* and *managing/runs* Virtual machine. ( **可以用硬體或是軟體實現** )
  - Guest：Process provided with virtual copy of the host.
- Abstract hardware of a single computer into several different execution environments.
- Similar to layered approach, but layer creates virtual machine(VM).
- Pros
  - 為測試**開發中的作業系統之良好的負載平台**，具有：$\;^{[1]}$其他 user, user process 工作時仍可持續運作不須暫停；$\;^{[2]}$萬一測試中的作業系統不穩定，就算當機了也不會影響 Host hardware, OS, user procrss 的工作，因為只是相當於一個 user process 失效而已，**不會對 system 有重大危害。**
  - 同一部 Host hardware 上可以在虛擬機中執行多個作業系統，**可節省成本**。
  - Consolidation( 資源的彙整、調度 )：在雲端運算環境時，通常會用有限的機器建立為數眾多的虛擬機，可以依虛擬機上的應用程式( Applications )之執行的負擔輕重**調動 Host Machine 資源作為因應的支援。**
  - 較為安全，若被病毒入侵不至於擴散。( 虛擬機之間是獨立的個體 )
  - 可以 Freeze, Suspend, Running 及 Clone 虛擬機。


![1526615255622](\willywangkaa\images\1526615255622.png)


![1526615230230](\willywangkaa\images\1526615230230.png)



### VMM的實現



#### 主流



主要以模擬與底層一模一樣的環境(如 cpu)給作業系統使用，有分成下列三種模擬方式。

- **Type 0 Hypervisor** ( Hardware )
  - **Hardware** - based solutions via firmware. Ex. IBM LPARS and Oracle LDOMs.
- **Type 1 Hypervisor** ( Kernel mode )
  - **OS-like software.** Ex. VMware ESX, Joyent SmartOS, **Citrix XenServer**.
  - General purpose OS that provide VMM functions( Serveices ). Ex. MicroSoft Window Server with **HyperV**, Redhat Linux with **KVM**.

- **Type 2 Hypervisor** ( User mode )
  - **Applications level provides VMM functionally.** Ex. Parallel Desktop, Oracle VirtualBox.



#### 非主流



所實現出來的 VM 不等同於 host 的硬體環境。

- **Paravirtualization**
  - The **guest OS** need **modify** to work in cooperator with VMM to optimize performance 
  - **Preseents guest with similars but not identical to host hardware.** <br>(指模擬出常用的機器指令，將少用的指令移除以達到 VM最佳化)
  - **Guest must be modified** to run on paravirtualization hardware.
- **Application containment** ( 創造一個執行環境 )
  - **Oralcle Soaris Zones, BSD Jails IBM AIX WPARs.**




![1526804733939](\willywangkaa\images\1526804733939.png)



- Programming - environment virtualization
  - **VMMs do not virtualize real hardware** but instand create an optimized virtual system.
  - Ex. **Java ** Virtual Machine (JVM) Microsoft.Net
  - **＜Note＞：**JVM is a specification (規格) , not a implementation. 其中規範有：$^{[1]}$Class Loader, $^{[2]}$ Class Verifier, $^{[3]}$ Java Interpreter.
- Emulator - 模擬器 ( 不同架構的cpu使用模擬器做轉換 )
  - Allow application writtem for one hardware to run on a very different hardware such as **different type of cpu**.



# Policy and Mechanism

---



- Policy
  - 定義：**"What" to be provided"**
  - Policy 會經常改變。
- Mechanism
  - 定義：**"How" to do that**
  - 「The underlying mechanism」甚少改變或不變。
- **設計原則**
  - Policy and mechanism 最好分開設計，以增進系統的彈性( Flexibility )。
- $Ex1. $
  - Mechanism：運用 Timer 幫助 cpu protection。
  - Policy：Max time Quantum 的大小制定。
- $Ex2. $
  - Mechanism：cpu 排班採**優先權高低**。
  - Policy：**優先權高低的定義**。