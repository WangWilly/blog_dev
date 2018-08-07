title: Operating System - Process Synchronization 2
author: Willy Wang
tags:
  - Process
  - Synchronization
categories:
  - Operating System
  - ''
date: 2018-08-04 11:09:00
---
# Process Synchronization ( Process Communication, Inter Process Communication )

---



## Race Condition Problem in Memory Communication



![thesolutionofracecondition](\blog\images\thesolutionofracecondition.png)



### Monitor




![thestructofmonitor](\blog\images\thestructofmonitor.png)



monitor 是一個用來解決同步問題的高階「資料結構」，是一種 **ADT ( Abstract data type )**。



- 組成架構
  - **共享變數宣告區**
  - **一組 local funtions or procedures**
  - **初始區 ( Initialization area )**
- 語法架構



```cpp
type Monitor-Name = Monitor
var 共享變數宣告
procedure entry fun1-Name
	begin
		BODY;
	end
	
...
procedure entry fun2-Name
	begin
		BODY;
	end
	
begin
	初始區
end
```



- Pros
  - **Monitor 本身已保證互斥( mutual exclusive )，即「任和時間點，最多只允許 1 個 Process 在 Monitor 內進行活動 ( active )」**：在任何時間點，最多只允許 1 個 Process 呼叫 ( calling ) Monitor 的某一個 function or procedure 執行，不可有多個 processes 同時呼叫 Monitor 的 funciton 執行。
  - 上述的互斥性質，是將**「共享變數區的共享變數」設成只能以 Monitor 的 local function 存取**，而不能直接從 Process 直接存取，**而 Monitor 保障互斥，所以也會不會造成「Race condition」，完全無需再在意額外的演算法以防止 Race condition**，只需專心解決同步問題即可( 在這一點比起 semaphore 好用很多，因為 semaphore 並不會保證 mutex 存取變數還須要演算法以輔助之 )。
- **Ex. 當我們在解決同步問題時，Semaphore 比起 Monitor 容易使用？**
  - **False。**



#### Condition ( 資料型別 )



Condition 型別是用在 Monitor 中，提供給開發者解決同步問題之用。



- **Operatior**：令 $x$ 為一 Condition type 變數。
  - **wait** ( x.wait )：**因為某些同步條件而被卡住時，執行此運作的該 Process 會 Blocked**，並且置入 Monitor 內該資料變數的所屬之等待佇列( Waiting Queue：預設為 FIFO )
  - **signal** ( x.signal )：**如果先前有 Processes 卡在該變數 ( x ) 的「Waiting queue」中，則此運作會自此「Waiting queue」移走一個 Process 並且恢復 ( Resume ) 其執行，否則無任何作用。**



#### 解決 Dining-philosophers Problem



1. 先定義所需的 Monitor ADT。<br>type Dining-ph = Monitor<br>var state：[0...4] of ｛ think, hungry, eating ｝。
2. self：[0...4] of condition



```cpp
procedure entry pickup(i:0...4)
    state[i] = hungry
    test(i);
	if(state[i] != eating) then
    	self[i].wait              // 繼續餓著，將該 Process blocked
```



```cpp
procedure test(k:0...4)
    if(state[(k+4)%5]!= eating &&     // 左邊哲學家沒吃飯
           state[k] == hungry &&      // 由於 putdown 會要讓左右邊的哲學家是否要吃飯，所以必須存在 
           state[(k+1)%5] != eating   // 右邊哲學家沒吃飯
           ) then
    	state[k] = eating            // 改為正在吃飯
    	self[k].signal               // *將自己喚醒
```



**＜Note＞：** `state[...]!= eating` **為何是測試「是否正在吃飯而不是測試是否飢餓」？**<br>**因為能呼叫到目前的函式代表該 Process 是正在進行的 ( Active )**。



```cpp
procedure entry putdown(i:0...4)
	state[i] = thinking
	test((i+4)%5)               // 給左右邊哲學家機會使用
	test((i+1)%5)
```



```cpp
init()
    for(i = 0; i < 4; i++)
        state[i] = thinking
```



##### 使用方式



- 共享變數宣告：
  - dp：Dining-ph



```cpp
// P_i
    repeat
        // hungry
        dp.pickup(i);  // active in monitor
        // eating
        dp.putdowm(i); // active in monitor
        // thinking
    until false
```



### Conditional Monitor



Condition 變數所屬的 Waiting queue 一般皆是 FIFO queue ( 甚至 Monitor 的 entry queue 也是 FIFO )，**可是有時候我們會需要「Priority queue」優先移除高優先權的 Process ，恢復執行或讓他進入 Monitor 內進行活動，此時我們就會需要 Conditional monitor。**



- **Operatior**：令 $x$ 為一 Condition type 變數。
  - **wait** ( **x.wait(c)** )：c 為此 Process 的 Priority number。



#### 解決互斥問題之應用



##### 互斥資源配置問題



- Process ID 越小者優先權越高可以優先取得資源。



- 定義 Monitor<br>type ResourceAllocator = Monitor<br>var busy：boolean                                   // 代表資源已經配置與否<br>var x：condition                                       // 用以將 Process blocked
- 想法
  - $\left\{\begin{matrix} 非優先權需求 \Rightarrow 寫入\; Monitor \; 的函式定義中處理。 \\ 優先權需求 \Rightarrow 只須告知閱卷人你的\; Monitor \; 是以 \;Priority \; queue\; 實現的。 \end{matrix}\right.$
  - $此題之分析\left\{\begin{matrix} 非優先權需求 \Rightarrow 互斥資源配置。\\ 優先權需求 \Rightarrow Process \;ID \;之優先權。 \end{matrix}\right.$



```cpp
procedure entry Apply(pid:int)
    if(Busy) then
    	x.wait(pid)
    else
    	Busy = true
```



```cpp
procedure entry Release()
	Busy = false
	x.signal
```



```cpp
init()
	Busy = false
```



###### 使用方式



- 共享變數
  - RA：ResourceAllocator
  - Pi ( i 代表 ProcessID )



```cpp
// P_i
    ...
    RA.Apply(i)
        // 使用資源
    RA.Release()
    ...
```



- **此 Monitor 的** $x$ **condition 變數之 waiting queue 及 monitor 的 entry queue 是 priority queue，且 process ID 越小優先權越高，也優先移出。**



##### 共享檔案問題



有一個「檔案」可以被多個 Processes 所使用，每一個 Process 有唯一的優先權值( Unique priority number )，存取檔案時必須要： **1. 所有正在存取此檔案的 Process 的優先權值加總必須** $< n$。 **2. 優先權值小的優先權越高。**

- 定義 Monitor<br>type FileAccess = Monitor<br>var sum：int<br>$x$：condition



```cpp
procedure entry Access(i:priorityNum)
	while((sum+i) >= h) do x.wait(i)  // !!
    sum += i
```



```cpp
procedure entry Leave(i:priorityNum)
    sum -= i
    x.signal()
```



```cpp
init()
	sum = 0
```



###### 使用方式



- 共享變數
  - FA：FileAccess
  - Pi ( i：process priority number ) 程式



```cpp
// P_i
    ...
    FA.Access(i)
    // 使用檔案
    FA.Leave(i)
    ...
```



- **此 Monitor 的** $x$ **condition 變數之 waiting queue 及 monitor 的 entry queue 是 priority queue，且 process ID 越小優先權越高，也優先移出。**



##### 印表機使用權問題



(P.6-72) 有三部 Printer 被 processes 使用且規定 process ID 越小優先權越高。



- 定義 Monitor<br>type Allocator = Monitor<br>var p：[0...2] of boolean<br>x：condition



```cpp
int Acquire(i:processID)
	if(p[0] && p[1] && p[2])
		x.wait(i)
	if(!p[0])
		tmp = 1
    else if(!p[1])
		tmp = 1
    else
		tmp = 2

    p[tmp] = true

    return tmp
```



```cpp
void Release(y:printerNum)
    p[y] = false
    x.signal()
```



```cpp
init()
    for(i = 0; i < 2; i++)
        p[i] = false
```



###### 使用方式



- 共享變數
  - RA：Allocator
  - Pi (i = Process ID)



```cpp
// P_i
    ...
    pno: printer number
    pno = PA.Acquire(i)
    // 使用編號 pno 的印表機
    PA.Release(pno)
    ...
```



- **此 Monitor 的** $x$ **condition 變數之 waiting queue 及 monitor 的 entry queue 是 priority queue，且 process ID 越小優先權越高，也優先移出。**



##### 使用 Monitor 定義 Semaphore？



- 定義 Monitor
	- type semaphore = Monitor<br>var value：int                // 號誌值<br>$x$：condition



```cpp
procedure entry wait()
	value--
    if(value < 0) then x.wait()
```



```cpp
procedure entry signal()
	value++
	x.signal()
```



```cpp
init()
    value = 1
```



### Monitor 的種類 ( 3種 )



- **探討**
  - 假設 Process Q 目前因為先前有執行了 x.wait ，所以卡在 x condition 變數之 Waiting queue 中，**接著，目前的 Process P 在 Monitor 中進行活動，當 P 執行了 x.signal 後，P 會將 Q 從 Waiting queue 中恢復 ( Resume ) 執行，此時代表 P 與 Q 同時在 Monitor 中進行活動( Active )，但是如此一來會違反 Mutual exclusive，**因此我們要*抉擇* 讓 P 或 Q 其中一個能夠在 Monitor 中活動。



![thetypeofmonitor](\blog\images\thetypeofmonitor.png)



#### Type 1 - Hoare Monitor ( 效果最好 )



**P waits Q until Q completed function or Q is blocked again.**



- Pros
  - 保證 Q 一定可以被恢復執行 ( Resume execution )。
  - 在不會退出 Monitor 的情況下可以恢復比較多的 Processes ，但是是效果其實與 Type3 的相去不遠。



#### Type 2 ( 效果最差 )



Q waits P until P completed function or P is blocked.



- Cons
  - **不保證 Q 一定可以被恢復執行**，因為允許 P 繼續往下執行的過程中，P 有可能改變可能讓 Q 可以恢復行為的同步條件值，使得 Q 仍被卡住。



#### Type 3



**P leaves monitor ( 將 P 重新插在 Monitor entry queue 的第一個 ) and let Q resume execution 直到 Q finished or Q is blocked again,** then P reenter Monitor.



- Consurrent C / Consurrent PASCAL 所採用。
- **Pros**
  - **保證 Q 一定可以被立刻恢復執行( Resume execution )。**
- Cons
  - 效果不若 **Type1 Monitor，因為在 P 一進一出的期間，頂多只能恢復一個 Q，然而 Type1 可以一次恢復多個 Q。**



### 目前的分類 ( 2種 )



#### Signal-and-wait



- Type1 Hoare Monitor
- Type3 Monitor



#### Signal-and-continue



- Type2 Monitor



### 實作 Monitor



- 使用 Semaphore 。
- 架構
  - **保證「互斥」**：最多只能有一個 Process 在 Monitor 中進行活動。
  - **「Hoare」的設計手法。**
  - **Condition 變數** $x$ **：x.wait()、x.signal。**
- 共享變數
  - mutex：semaphore = 1 ( 用以保障函式使用的「互斥控制」)
  - next：semaphore = 0 ( 若 P 執行 x.signal，則用來 block P )
  - next_count：int = 0 ( 統計 P 的個數 )
  - x_sem：semaphore = 0 ( 當 Q 執行 x.wait() ，則用以 block Q )
  - x_count：int = 0 ( 統計 Q 的個數 )
- **控制碼 - 確保互斥**：在 Monitor 的每個**函式**的「body」之**前**及**後**加入一些額外的控制碼。

```cpp
procedure entry _monitorfunction()
	wait(mutex)
    
	// body
    
	if(next_count > 0)      // 若 P 存在，必先讓 P 恢復執行
		signal(next)
	else                    // 若沒有 P 在等待，才讓下一個 Q 進入執行
		signal(mutex)
```

- **控制碼 - 「**$x$**.wait()」**：

```cpp
// x.wait
	x_count++                    // Q 的個數加一
	if(next_count > 0)           // 將要 blocked 前的準備動作(與上述程式碼相同)
		signal(next)
	else
		signal(mutex)
	wait(x_sem)                  // Q 將自己 block
	x_count--                    // 當 Q resume 後，將 x_count 減一
```

- **控制碼 - 「**$x$**.signal()」**：

```cpp
// x.signal
	if(x_count > 0)     // 先前若有 Q blocked 才有作用
		next_count++
		signal(x_sem)  // resume Q
		wait(next)
		next_count--
```



#### 問題與討論



- 證明 *Monitor* 與 *Semaphore* 解決同步問題的能力是**一樣的( idemtical; equivalent )**。
  - **由於 Monitor 與 semaphore 可以「相互實作」**，所以兩者解決「同步問題」的能力是*相同的*。
- 當開發者在解決同步問題時，**Monitor 的使用會比起 Semaphore 容易**，因為 Monitor 本身已經保證「互斥」，所以 condition variable 不會有「race condition」的問題，所以只要專心解決「同步問題」即可，**但是當開發者要使用 Semaphore 解決同步問題時還需要關心「Race condition」與「同步條件」的滿足。**



## Message Passing




|              Direct ( symmtric ) communication               |                    Indirect communication                    |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|     **收、送雙方須相互指名對方 ID**，才能建立通訊鏈結。      |       **雙方透過「Shared mailbox」**才能建立通訊鏈結。       |
| **通訊鏈結是專屬於溝通的雙方，不得與其他 Processes 共享**。下圖(一) |           **可以多 Processes 一起共享。**下圖(二)            |
| 溝通雙方**最多只能一條通訊鏈結(禁止多條通訊鏈結)。**下圖(三) | **溝通雙方可以同時存在多條通訊鏈結，每一條皆須有「Shared mailbox」。**下圖(四) |




![thetypeofmessagepassing](\blog\images\thetypeofmessagepassing.png)



### 直接通訊 ( Direct Communication )



#### Symmetric



雙方須「互相指名」對方的 Process ID ，才能建立通訊鏈結 ( Communication link )，**OS 提供 send()、receive() 的 System call。**

```cpp
// P_1
    ...
    sent(P_2_ID, message)
    ...
```

```cpp
// P_2
    ...
    receive(P_1_ID, message)
    ...
```



#### Asummetric



**只有「送出的 Process」需要指名「收受的 Process 之 ID」**，但收受方無須指名送方，即任何 Process 皆可以收下這則訊息。

```cpp
// P_1
    ...
    send(P_2_ID, message)
    ...
```

```cpp
// P_2
    ...
    receive(id, message)
    ...
```

當 P_2 收到訊息後，**會將「送出訊息的 Process」之 ID 存在**`id`**變數之中。**



### 間接通訊 ( Indirect Communication )



收、送雙方是透過「Shared mailbox」，才能建立通訊鏈結。

```cpp
// P_1
    ...
    send(A, message)
    ...
```

```cpp
// P_2
    ...
    receive(A, message)
    ...
```




### 用 Message Passing 解決 Producer-comsumer Problem



- Producer

```cpp
// Producer
    while(condition != false) {
        produce an item in next_tp;
        send(Consumer, next_tp);
        ...
    }
```



- Comsumer

```cpp
// Consumer
    while(condition != false) {
        receive(Producer, next_tp);
        Consumer the item in next_tp
        ...
    }
```



### 「同步」意義之呈現



#### Link capacity



假設「收受的 Process」是以：若未收到訊息則**暫停執行，直到收到訊息後才往下執行**；**而對於「傳送的 Process」看待 Link capacity 時，其實針對每條傳輸鏈結裡的「Message queue」**( 用以保存「除了正在傳輸中的訊息以外之其他 *送方* 訊息」)**之容量**。




![linkcapacity](\blog\images\linkcapacity.png)



- **★ Zero capacity**
  - 送方直到**收方收到訊息後才能繼續往下執行**，此時雙方的同步模式也稱為**「Rendezvous」**( 法文：約會一次；偶遇 )

```cpp
// P_1
    ...
    send(P_2_ID, message)
    receive(P_2_ID, "Acknowledge")
    ...
```

```cpp
// P_2
    ...
    receiver(P_1_ID, message)
    sned(P_1_ID, "Acknowledge")
    ...
```



- Bounded capacity
  - 當 Queue 滿了之後，送方會被迫暫停執行。
- Unbounded capacity
  - 送方無須被迫暫停執行。



#### 4 條指令之組合來表現不同之「同步模式」



- **Blocking send**
  - 送出訊息後直到收方收到訊息後才繼續往下執行。(Like zero capacity)
- **Nonblocking send**
  - 送出訊息後無須收方確認收到訊息即可繼續往下執行。(Like unbounded capacity)
- **Blocking receive**
  - 一定要收到訊息才能繼續往下執行。
- **Nonblocking receive**
  - 不一定要確認**訊息的正確性**亦能繼續向下執行。



##### 問題與討論



- Ex1.「Rendezvous」模式

```cpp
// P_1
    ...
    Blocking_send(P_2_ID, message)
    ...
```

```cpp
// P_2
    ...
    Blocking_receive(P_1_ID, message)
    ...
```



- Ex2. 假設收方的程式如下，則代表著甚麼含意：

```cpp
	...
    Nonblocking_receive(A, message)
    if(message == null) {
        Blocking_receive(B, message)
        Blocking_receive(A, message)
    }
    else {
		Blocking_receive(B, message)
    }
	...
```

選項：

1. ~~從 A 或 B 收到訊息後，即可往下。~~
2. 一定要從 A  與 B 收到訊息後且要先 A 後 B，才可以繼續往下執行。
3. 同上，但 A、B 順序無所謂。

**Ans： (3)**



#### Exception handling



1. 在「Rendezvous」模式之下，若收或送任一方的 Process 以死亡( terminate )，但另一方不知情時，**則另一方會永久停滯 ( ~~Starvation、Deadlock~~ )**：作業系統會以「Exception」的方式刪除另一個 Process。

```cpp
// P_1
    ...
    Blocking_send(P_2_ID, message)
    ...
```

```cpp
// P_2
    ...
    Blocking_receive(P_1_ID, message)
    ...
```

2. **在直接通訊 ( Direct symmtric communication )下**，若「未通報其他 Process 自己的 ID」且 「New Process 也不知道他人 Process 的 ID」，則*無法溝通*。
3. 訊息的傳輸過程之中，有可能會丟失訊息( Lost message ) ，作業系統負責偵測訊息是否已丟失，**若丟失訊息時，作業系統會通知「送方」重送訊息，**偵測丟失訊息的方法類似於「網路概論」中談到的**封包丟失偵測有關 ( Time-out 法則 )**。