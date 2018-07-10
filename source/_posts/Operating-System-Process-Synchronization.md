title: Operating System - Process Synchronization
author: Willy Wang
tags:
  - Process Synchronization
categories:
  - Operating System
date: 2018-07-10 23:05:00
---
# Process Synchronization ( Process Communication, Inter Process Communication )

---



- Synchronization
  - Process 因為「某件事情」的已發生或是未發生( 有多個 process 相互合作的時候 )，導致必須等待該事件完成或發生才得以繼續進行。



## Process Communication



### Shared memory




![1530175318032](\blog\images\1530175318032.png)



Process 透過對共享變數 ( Shared variables ) 之存 ( Write )、取 ( Read )，達到構通 ( Infomation exchange ) 的目的。



- **分析**
  1. 適用於**大量資料( Data, Message )**傳輸的狀況。
  2. 因為**不須 Kernel 的介入干涉**，因此傳輸速度**較快**。
  3. 不適用在分散式系統( distributed system )。
  4. Kernel 不須提供額外的支援( 最多就只供應共享的記憶體空間 Shared memory space )，**而所有的控制都交付給 Programmer 自行負擔，必須撰寫額外的控制碼防止 Race condition 發生。**





### Message passing



Process 雙方要溝通必須要遵循以下步驟

1. **建立 Communication link。**
2. **訊息可雙向傳輸。**
3. **傳輸完畢，釋放 Communication link。**



- **分析**
  1. 適用於**少量資料( Data, Message )**傳輸的狀況。
  2. 因為**須要 Kernel 的介入干涉**，因此傳輸速度**較慢**。
  3. **適用在分散式系統( Distributed system )。**
  4. Kernel 必須提供額外的支援( **如：System call of send/receive, Management of communication link, Detection of message lost, 例外狀況的處理** )，而所有的通訊控制都交付給作業系統。



## Race Condition Problem in Memory Communication



在利用共享記憶體作為通訊橋梁時，若未對共享變數存取提供**任何互斥存取控制之同步( Synchronization )機制，會因為 Processes 之間的交錯執行而導致前後順序不同，進而造成共享變數的最終結果有所不同，**這種**資料不一致( Data inconsistency )的情型**，稱為 Race condition。



- $Ex .$
  - C 為共享變數且初值為 5。
  - 兩個 Processes $P_i, P_j$，分別程式碼如下。



```cpp
//P_i
...
C = C + 1;
...
```



```cpp
//P_j
...
C = C - 1;
...
```


$P_i, P_j$ 個執行一次則 **C 的最終值可能是 5 或 4 或 6。**

- 正常結束( No race condition ) ：5



```cpp
// processing flow
c = c + 1 // P_i
c = c - 1 // P_j
```



```cpp
//processing flow
c = c - 1 // P_j
c = c + 1 // P_i
```



- Race condition：4



```cpp
// assembly processing flow
// P_i
load c; // c == 5
// P_j
load c; // c == 5
// P_i
process c = c + 1; // c == 6
// P_j
process c = c - 1; // c == 4
// P_i
restore to c; // c == 6
// P_j
restore to c; // c == 4
```



- Race condition：6



```cpp
// assembly processing flow
// P_i
load c; // c == 5
// P_j
load c; // c == 5
// P_i
process c = c + 1; // c == 6
// P_j
process c = c - 1; // c == 4
// P_j
restore to c; // c == 4
// P_i
restore to c; // c == 6
```



- $Ex 1.$  x、y 是共享變數，初值各別為 5, 7 ，有兩個 process $P_i, P_j$ 分別程式碼如下。



```cpp
//P_i
...
x = x + y;
...
```



```cpp
//P_j
...
y = x * y;
...
```



$P_i, P_j$ 各作一次，求 (x, y) 可能的值？



- (12, 84)



```cpp
// processing flow
x = x + y;
y = x * y;
```



- (40, 35)



```cpp
// processing flow
y = x * y;
x = x + y;
```



- *** (12, 35)**



```cpp
// assembly processing flow
// P_1
load x, y; // x = 5, y = 7
// P_2
load x, y; // x = 5, y = 7
// P_1
process x = x + y;
restore to x;
// P_2
process y = x * y;
restore to y;
```





- $Ex 2.$  x 是共享變數**初值等於零** ， i 為*區域變數*，另外有兩個 process $P_i, P_j$ 分別程式碼如下。



```cpp
//P_i
...
for( int i = 1; i <= 3; i++ )
    x = x + 1;
...
```



```cpp
//P_j
...
for( int i = 1; i <= 3; i++ )
    x = x + 1;
...
```



$P_i, P_j$ 各作一次，求 x 可能的值( **可能的最小值、可能的最大值** )？



- 先拆解 for loop



```cpp
// P_i
x = x + 1;
x = x + 1;
x = x + 1;
```



```cpp
// P_j
x = x + 1;
x = x + 1;
x = x + 1;
```



- 分析若為最小值的狀況：3。



``` cpp
// assembly processing flow
// P_i
load x; // x = 0
// P_j
load x; // x = 0
// P_i
x = x + 1;
restore x; // x = 1
// P_j
x = x + 1;
restore x; // x = 1

// P_i
load x; // x = 1
// P_j
load x; // x = 1
// P_i
x = x + 1;
restore x; // x = 2
// P_j
x = x + 1;
restore x; // x = 2

// P_i
load x; // x = 2
// P_j
load x; // x = 2
// P_i
x = x + 1;
restore x; // x = 3
// P_j
x = x + 1;
restore x; // x = 3
```



- 正常結束：6
- 所以 x 的值會介於在 3 ～ 6 之間，都是有可能會出現的狀況。



- $Ex 3.$  x 是共享變數**初值等於零** ， i 為*區域變數*，另外有兩個 process $P_i, P_j$ 分別程式碼如下。



```cpp
//P_i
...
for( int i = 1; i <= 3; i++ )
    x = x + 1;
...
```



```cpp
//P_j
...
for( int i = 1; i <= 3; i++ )
    x = x - 1;
...
```



$P_i, P_j$ 各作一次，求 x 可能的值( **可能的最小值、可能的最大值** )？



- 先拆解 for loop



```cpp
// P_i
x = x + 1;
x = x + 1;
x = x + 1;
```



```cpp
// P_j
x = x - 1;
x = x - 1;
x = x - 1;
```



- 分析若為最小值的狀況：-3。



```cpp
// assembly processing flow
// P_i
load x; // x = 0
// P_j
load x; // x = 0
// P_i
x = x + 1;
restore x; // x = 1
// P_j
x = x - 1;
restore x; // x = -1

// P_i
load x; // x = -1
// P_j
load x; // x = -1
// P_i
x = x + 1;
restore x; // x = 0
// P_j
x = x - 1;
restore x; // x = -2

// P_i
load x; // x = -2
// P_j
load x; // x = -2
// P_i
x = x + 1;
restore x; // x = -1
// P_j
x = x + 1;
restore x; // x = -3
```



- 最大值的狀況：3。



### 解決 Race Condition



#### Disable inrerrput ( 針對CPU )



Process 在對共享變數存取之前，**先 Disable interrupt**，等到完成共享變數的存取後，**才 Enable interrupt，**如此一來可以保證 Process 在存取共享變數的期間 **CPU 不會被搶走( Preempted )**，所以這種存取的方法稱為「**Atomically executed (不可分割之執行)**」，所以可以防止 Race condition。



- $Ex.$



```cpp
//P_i
...
disable interrupt
C = C + 1;
enable interrupt
...
```



```cpp
//P_j
...
disable interrupt
C = C - 1;
enable interrupt
...
```



-  Pros
  - Simple, easy to implementation
  - **適用於 Uniprocessor system。(單一CPU)**
- Cons
  - **不適合用在 Multiprocessors system 中。因為只 Disable 一顆 CPU 的 Interrupt 功能無法防止 race condition ( 其他的 CPUs  上執行之 Processes 仍可以存取該共享變數 )；但若 Disable 所有 CPU 的 Interrupt 功能，雖然可以防止 Race condition，但是會導致 Multiprocessor 的效能低落( low performance，因為無法「平行執行」 )變得和 Single-processor 的環境差不多。**
  - **風險很高：因為 Disable interrupt 指應為特權指令(風險高，可能阻擋 Kernel 的插斷)，**所以必須信任使用者程式在 Disable interrupt 後，在短時間內會再 Enable interrupt **，不然 CPU 從此再也不會回到 Kernel 管理的狀態 ( 高風險!! )。**
  - **＜Note＞通常「 Disable interrupt 解決方法」不會下放給 User process，通常只存在於作業系統 Kernel 的實踐之中。**( 只有作業系統開發者可以使用 )




![1530176275538](\blog\images\1530176275538.png)



#### Critical section design( 針對共享資料製作臨界區塊 )



針對「共享變數」的存取進行管制，當 P_i 取得共享變數存取管制，在該 Process 尚未完成之期間，任何其餘 Processes 即便已取得 CPU 的使用權之下，**仍無法存取共享變數**。



![1530176581925](\blog\images\1530176581925.png)



- **Critical section**
  - Process 中對於共享變數進行存取的敘述集合。
- **Remainder section**
  - Process 中除了 C. S. 之外的區間統稱為 Remainder section。



```
// Process
repeat
	entery section
		
		critical section
		
    exit section
    
    	remainder section
    	
until falseS
```



Critical section 的主要設計，是設計每個 Critical  section 的前後 Programmer 需增加的控制碼( **Entery section、Exit section** )。





```cpp
//P_i
...
// Enter section
...
C = C + 1;
...
// Exit section

// Remainder section
...
```



```cpp
//P_j
...
// Enter section
...
C = C - 1;
...
// Exit section

// Remainder section
...
```



#### Critical Section ( Spinlock, Busy-wiating ) VS. Disable interrupt



- Critical Section (Pros)
  - **適用於多處理器系統( Multiprocessor system )。**
- Critical Section (Cons)
  - **設計較為複雜。**
  - **較不適合用在單處理器系統( Uniprocessor system )。**



### Busy waiting ( Spinlock ) 技巧



透過使用迴圈相關敘述達到**讓 Process 暫時等待該共享變數。**



- Cons
  - **在 Spinlock 等待中的 Porcess 會與其他 Processes 競爭 CPU ，將得到的 CPU time 用在於空轉( spinlock )中**，因此若 Process 要等待很長的時間才能離開迴圈，則這種技巧非常浪費 CPU time。
- Pros
  - **若 Process 在迴圈等待的時間短( 小於「Context switching time」 )，則 Spinlock 非常有用。**




![1531041864596](\blog\images\1531041864596.png)



- ( 恐龍課本 )謬誤：因為 Critical Section 設計當中，「Entery section」中經常使用 Busy-waiting ( spinlock ) 技巧，而課本將 Busy-waiting ( Spinlock ) 與 Critical Section 視為相同，**進而與 Disable interrupt 比較( 應是 Busy waiting 與 Non-busy waiting 來比較 )**。



### Non-busy waiting 技巧



當 Process 因為同步事件( Synchronization event )被長時間卡住，**則可以使用 Block system call 將該 Process 送入 Blocked state，所以不會與其他 Process 競爭 CPU ，直到該事件觸動了，才會喚醒(wake up system call) 該 Process 移至 Ready state**。



- Pros
  - **等待中的 Process 不會與其他 Process 不會浪費 CPU time。**
- Cons
  - **需額外付出「Context switching time」。**



### Critical Section Design



- **關鍵性質**
  - **Mutual exclution：在任何時間點最多只允許一個 Process 進入它的 C.S. ，不可以有多個 Processes 分別進入各自的 C.S.。**
  - ***Progress：不想進入 C.S. 的 Process ( 在 R.S. 中活動的 Process )，不可阻礙( 不參與「 進入C.S.」的決策 )其他 Processes 進入 C.S.。**<br>「安排欲進入 C.S 的 Process」之決策，要在**有限的時間中完成 ( Non-deadlock：不可以無窮等待，使得全部之 Process 皆無法進入 C.S. )。**---全部 Process 都無法進入
  - **Bounded waiting：**當有 Process 提出「進入 C.S.」之申請，等待核准的時間是有限的( 防止該 Process 出現 Starvation 的情形 )。---單一 Process 一直無法進入<br>**若有 n 個 Process 欲進入 C.S. 則每個 Process 最多等待 n-1 次後即可進入 C.S.。**



|                              |                                                       | **著重於**          |                                     |
| ---------------------------- | ----------------------------------------------------- | ------------------- | ----------------------------------- |
| 高階( 位於函式庫中 )         | **Monitor**                                           | 同步問題之解決      |                                     |
| 中階( 通常於 System call )   | **Semaphore**                                         | 同步問題之解決      |                                     |
| 基礎建設( 作業系統核心製作 ) | **Software solutions、Hardware instrustions support** | C.S. 設計的正確與否 | **Disable interrupt ** 也在基礎建設 |



#### Software Solutions



##### 兩個 Processes 之 critical section design



有兩個 Process $P_i, P_j$。



###### Algorithm 1 ( Fall )：權力層面



- 共享變數：**Turn：int，值恰為 i 或是 j 值。**
  - **視為一種權力的象徵，當 turn 值為 i 時 $P_i$ 始得 進入 C.S. 的權力，且只能讓 $P_i$ 下一個進入 C.S. ，反之亦然。**



```cpp
// P_i
Repeat
	while ( turn != i ); // enter
	C.S.
    turn = j; // leave
	R.S.
Until False
```



```cpp
// P_j
Repeat
	while ( turn != j ); //enter
	C.S.
    turn = i; //leave
	R.S.
Until False
```



**＜分析＞**：

1. Mutual exclution ( OK ) ：**因為 turn 值不會同時為 i 且為 j ，只會為 i 或 j 之其中一個值，所以只有 **$P_i$ 或 $P_j$ **一個可進入 C.S. 而不會兩個同時進入。**
2. Progress ( FALL ) ：假設目前 $P_i$ **在 R.S. ( 且** $P_i$ **不想進入 C.S. )，而目前的 turn = i 且 **$P_j$ **欲進入 C.S，但是因為** $P_i$ **仍在 R.S. 無法交付 turn  ，所以被** $P_i$ **阻礙進入。**
3. Bounded waiting ( OK ) ：**證明 $P_i$ 無法連續兩次進入 C.S 之中。**假設目前 turn = i，且$P_i$ 已先早於 $P_j$ 進入 C.S. 使 $P_j$ 開始等待，若 $P_i$ 離開後又想立刻想再次進入，但因為 $P_i$ 離開時會將 turn = j，使得 $P_i$ 無法再度比 $P_j$ 早進入 C.S.，這一次 $P_j$ 必定先進入之 ，**所以 **$P_j$ **最多等一次後即可進入。**



###### Algorithm 2 ( Fall )：意願層面



- 共享變數：flag[ i ... j ] of Boolean。( 初值皆為 False )
  - $flag[i] = \left\{\begin{matrix}True \quad P_i \; 有意願進入 \; C.S.\\ False \quad P_i \; 無意願進入\; C.S. \end{matrix}\right.$



```cpp
// P_i
Repeat
	flag[i] = true; // 表達意願
	while (flag[j]); // enter
	C.S.
    flag[i] = false; // leave
Until false
```



```cpp
// P_j
Repeat
	flag[j] = true; // 表達意願
	while (flag[i]); // enter
	C.S.
    flag[i] = false; // leave
Until false
```



**＜Note＞**

1. Progress ( 造成死結  -> Fall )
2. Mutual exclusion ( OK )
3. Bounded waiting ( OK )



```cpp
flag[i] = true; // P_i 表達意願
flag[j] = true; // P_j表達意願
	while (flag[j]); // P_i can't enter
	while (flag[j]); // P_j can't enter
... // Progress fall ( Deadlock )
```



###### Algorithm 3 - Peterson's Algorithm ( OK )：權力、意願層面



- 共享變數：flag[ i ... j ] of Boolean。( 初值皆為 False )
  - $flag[i] = \left\{\begin{matrix}True \quad P_i \; 有意願進入 \; C.S.\\ False \quad P_i \; 無意願進入\; C.S. \end{matrix}\right.$
- 共享變數：**Turn：int，值恰為 i 或是 j 值。**



```cpp
// P_i
Repeat
	flag[i] = true;                 // 表達意願
	turn = j;                       // * enter ( 權力先給對方 )
	while ( flag[j] && turn == j ); //   enter
//  turn = i;                       //   enter
	C.S.
//  turn = j;                       // enter ( fall )
    flag[i] = false;                // leave
Until false
```



```cpp
// P_j
Repeat
	flag[j] = true;                 // 表達意願
	turn = i;                       // *enter
	while ( flag[i] && turn == i ); //  enter
//  turn = i;                       //  enter
	C.S.
//  turn = j;                       // enter ( fall )
    flag[j] = false;                // leave
Until false
```



- Mutual exclusion (OK)：**若 **$P_i, P_j$ **皆欲進入 C.S. 代表 **`flag[i]` **與** `flag[j]` **皆為 True，當雙方皆作到**`while`**測試的時候，代表雙方已分別執行過** `turn = i` **以及** `turn = j` **的設定，差異點在於可能會交錯執行，但是** `turn` **必為兩值之其中一者，所以只有P_i, P_j 一個可以進入 C.S。**
- Progress (OK)：
  - 假設 turn 值目前為 i ，且 P_i 不想進入 C.S.，**帶表 flag[i] = false** 若此時 P_j 欲進入 C.S. ，則 P_j 必可通過 while 的關卡進入 C.S. ，所以**P_i 不會阻礙 P_j 進入 C.S.。**
  - **若 P_i, P_j 皆欲進入 C.S 則在有限的時間內必可決定出** `turn` **值為** `i` **或為** `j` **使得 P_i, P_j 可以進入，兩者不會永遠互相等待造成死結。**
- **Bound waiting (OK)：假設 turn 為 i ， P_i 已早於 P_j 進入 C.S. 而 P_j 等待進入中，所以 flag[i] = flag[j] = true，若 P_i 離開 C.S. 後，又想再進入 C.S. ，則 P_i 必定會將** ~~flag[i]=false~~ **turn = j ，使得 P_i 無法再度先早於 P_j 進入 C.S. ，一定是 P_j 進入 C.S. ，所以 P_j 至多等待一次後即可進入 C.S.。**



今有一程式如下，請問該程式是否可以正確執行，或是違反 Cirtial section 中的關鍵性值？**可以正確執行，只是 flag 互相對調而已。**

```cpp
// P_i
Repeat
	flag[j] = true;                 // 表達意願
	turn = j;                       // * enter ( 權力先給對方 )
	while ( flag[i] && turn == j ); //   enter
	C.S.
    flag[j] = false;                // leave
Until false
```



```cpp
// P_j
Repeat
	flag[i] = true;                 // 表達意願
	turn = i;                       // *enter
	while ( flag[j] && turn == i ); //  enter
	C.S.
    flag[i] = false;                // leave
Until false
```



今有一程式如下，請問該程式是否可以正確執行，或是違反 Cirtial section 中的關鍵性值？**可以正確執行，只是 turn 互相對調而已。**

```cpp
// P_i
Repeat
	flag[i] = true;                 // 表達意願
	turn = i;                       // * enter ( 權力先給對方 )
	while ( flag[j] && turn == i ); //   enter
	C.S.
    flag[i] = false;                // leave
Until false
```



```cpp
// P_j
Repeat
	flag[j] = true;                 // 表達意願
	turn = j;                       // *enter
	while ( flag[i] && turn == j ); //  enter
	C.S.
    flag[j] = false;                // leave
Until false
```





今有一程式如下，請問該程式是否可以正確執行，或是違反 Cirtial section 中的關鍵性值？**可以正確執行，只是 turn 與 flag 皆互相對調而已。**

```cpp
// P_i
Repeat
	flag[j] = true;                 // 表達意願
	turn = i;                       // * enter ( 權力先給對方 )
	while ( flag[i] && turn == i ); //   enter
	C.S.
    flag[j] = false;                // leave
Until false
```



```cpp
// P_j
Repeat
	flag[i] = true;                 // 表達意願
	turn = j;                       // *enter
	while ( flag[j] && turn == j ); //  enter
	C.S.
    flag[i] = false;                // leave
Until false
```



##### N 個 Process 之 Critical Section design



###### Bakery's Algorithm ( 麵包店號碼牌演算法 )



- 核心觀念：
  1. 客人要先取得號碼牌才可以入店內。
  2. 店內一次只能有一個客人進入。
  3. **「號碼牌最小的客人」或「號碼牌相同最小之 ID 最小的客人」，得以優先入內。**
- 共享變數
  - **choosing: [0 ... n-1] fo boolean：初值皆為 false。**<br>$choose[i] = \left\{\begin{matrix}True \quad P_i \; 正在取得號碼牌中，但尚未取得\\ False \quad P_i \; 已取得號碼牌(init) \end{matrix}\right.$
  - **number: [0 ... n-1] of int：初值為 0。**<br>$number[i] = \left\{\begin{matrix}0 \quad 代表 \; P_i \;無意願進入\\ >0 \quad P_i \; 有意願進入 \end{matrix}\right.$
- MAX(i ... j)：從 i ... j 挑最大值。
- (a, b) < (c, d)：**代表**<br>1. a < c<br>or 2. a==c and b < d
- P_i 之程式碼



```cpp
Repeat
	choosing[i] = true;
	number[i] = Max(number[0], ... , number[n-1]) + 1 
	choosing[i] = false;
    for( j = 0; j < n; j++ ) {
        while (choosing[j]) do no-op;  // 等待P_j 取得號碼牌。
        //比較號碼牌與 Process ID。
        while(number[j]>0 && (numbeer[j], j) < (number[i], i)) do no-op; 
    }
    C.S.
    Number[i] = 0 // P_i 已無意願
    R.S.
util false
```



- Ex1. 為何號碼牌( number[i] )會有兩個以上相同的問題？ 
  - 因為在 `number[i] = Max(number[0], ... , number[n-1]) + 1 ` 的執行階段時，有可能會被其他 Process 搶斷。



```cpp
// number[max] = 0
// P_i
// Processiing " choosing[i] = true; ".
Load max number[max];         // number[max] = 0
Process max++;
// P_j preempting
// Processiing " choosing[j] = true; ".
Load max number[max];         // number[max] = 0
Process max++;
// P_i
Store to number[i];           // number[i] = 1
// P_j
Store to number[i];           // number[i] = 1
```





- Ex2. 證明 Critical section **關鍵性值**。
  - Mutual exclusion (OK) ：<br>Case1. **假設號碼牌值皆不同且大於零，則具有最小號碼牌值之 Process 可以優先進入 Critical section，因為最小值必唯一，所以除了該 Process 可以進入其餘等待。**<br>Case2. 有多個 Processes 有相同的最小號碼牌，以 Process ID 最小者優先進入 Critical section，因為 Process ID 必唯一最小值也必定唯一，所以除了該 Process 可以進入其餘等待。<br>唯一性確保，互斥確保。
  - Process (OK)：<br>1. ( 不想進入但不會控制其他人 )**假設 P_j 不想進入 C.S. 代表 number[j] 為 0 ，若此時 P_i 欲進入 C.S.** 則 `while(number[j]>0 && (numbeer[j], j) < (number[i], i)) do no-op; ` 不會被 P_j 阻擋住。<br>2. ( 不會造成 deadlock ) 若 P_0 ~ P_n-1 n 個 Processes 皆欲進入 C.S. 會在**有限時間內必決定有一個 Process (號碼牌最小加上 ProcessID 最小)可以順利結束 for loop 進入 C.S.**
  - Bounded waiting (OK)：**假設 P_0~P_n-1 processes 皆欲進入 C.S. 令 P_i 具有最大的號碼牌等於 K ( nuber[i] = K ) 因此，其他 n-1 Process：P_j ( j!=i )，必定先早於 P_i 進入 C.S.，**若 P_j 離開 C.S 後，又立刻欲進入 C.S，**則 P_j 的再取得號碼牌必定大於 K，所以 P_j 不會再早於 P_i 進入 C.S. 進而可以知道 P_i 最多等待 n-1 次即可進入 C.S.。**
- Ex3.

```cpp
Repeat
	choosing[i] = true;
	number[i] = Max(number[0], ... , number[n-1]) + 1 
	choosing[i] = false;
    for( j = 0; j < n; j++ ) {
        ~~while (choosing[j]) do no-op;~~  // 移除這行程式碼是否還正確？請解釋。
        while(number[j]>0 && (numbeer[j], j) < (number[i], i)) do no-op; 
    }
    C.S.
    Number[i] = 0;
    R.S.
util false
```



- 不正確，違反「Mutual exclusion」
- 令目前 number[0 ... n-1] 所有的值皆為 0 ，今有兩 Process $P_i, P_j \quad where \; i \neq j$  欲進入 C.S. ，並且假設 Process ID 為 $P_i < P_j$ 
- **小結論**
  - 此行程式碼存在的目的，是為了讓**所有可能會同一時間拿到號碼牌的 Processes 能公平的取得該號碼牌，不會有的早拿到或是晚拿到該「同一號碼」之號碼牌。**



#### Hardware (CPU) Intructions Support



**程式開發者可以使用硬體預先定義之指立於設計 Critical Section。**



##### 「test_and_set(bool *lock)」機器指令



- 功能：**回傳 Lock 參數值並將 Lock 參數設為 True，且 CPU 保證此指令是不可中斷地執行( Atomically executed )。**
- 



```cpp
bool test_and_set(bool *lock) {
    bool return_value = *lock;
    *lock = true;
    return return_value;
}
```



###### Algorithm 1 ( Fall )



- 共享變數：
  - **Lock：bool = False**( Initial value )
- $P_i$ 之程式碼：



```cpp
do {
    while (test_and_set(&Lock)); // do nothing
    /* critical section */
    Lock = false;
    /* remainder section */
} while (condition == true);
```



- 分析
  - Mutual exclusion：OK
  - Progress：OK
  - Bounded waiting：Fall<br>假設 $P_i$ 以早於 $P_j$ 進入 C.S. ，所以 $P_j$ 進行等待，**當 **$P_i$ **離開 C.S. 後又想立即進入 C.S.，而以上述寫法可能會導致 **$P_i$ **有可能可以再次早於** $P_j$ **執行 test_and_set 進入 C.S.**，所以 $P_j$ 有可能會面臨「Starvation」。 



###### **Algorithm 2 ( Pass ) - FIFO



- 共享變數
  - **Lock：bool = False** ( Initial value )
  - **Waiting：[0 ... n-1] of bool 初值皆為 False**<br>$waiting[i] = \left\{\begin{matrix}True \quad P_i \; 有意願進入 \; C.S. ，且正在等待\\ False \quad 初值。P_i \; 不用等待，可以直接進入\; C.S. \end{matrix}\right.$
- $P_i$ 之程式碼：



 ```cpp
區域變數：key:bool;
         j  :int;
do {
    waiting[i] = true;
    key = true;
    
    // 假設目前無人在 C.S. 所以第一個執行此指令的 Process 可以直接進入下一步。
    while (waiting[i] && key) key = test_and_set(&Lock); 
    
    // **表明 P_i 不用繼續等待，可以進入 C.S.。
    waiting[i] = false;
    
    /* critical section */
    
    // 找出下一個欲進入 C.S. 之 P_j Process
    j = (i + 1) % n;
    // j != i -> 尚未繞完一圈 && !waiting[j] -> P_j 不想進入 C.S.
    while ((j != i) && !waiting[j])
    	j = (j + 1) % n;
    
    if (j == i)
    	lock = false;         // 無 Process 欲進入 C.S.
    else
    	waiting[j] = false;   // 讓 P_j 進入 C.S.
    
    /* remainder section */
    
} while (condition == true);
 ```



- 證明 Critical section 關鍵性值存在：
  - Mutual exclusion：OK<br>$P_i$ **可進入 C.S. 之條件有兩種可能：**<br>Case1：**Key 為 False，代表** P_i 是第一個執行 test_and_set 的 Process ，如此才能將 Key 賦予 False ，**唯一性。**<br>Case2：**Waiting[i] = False，因為一開始會先執行** `waiting[i] = true;` **代表 P_i 在離開** ` while (waiting[i] && key) key = test_and_set(&Lock); ` 前不會將 Waiting[i] 賦予 False ，**只有在 C.S. 的 Process 欲離開 C.S. 後才能將其他 Processes 其中一個 Process 的 Waiting 賦予 False 值，**但是能在 C.S. 之中執行的 Process 唯一，所以也只會讓一個 Process 的 waiting 賦予 False 值，**唯一性( Mutex 成立 )**。
  - Progress：OK<br>(1) 若 P_i 不想進入 C.S. 其 Waiting[i] 為 False ，且 P_i 不會與其他 Process 競爭執行 `while (waiting[i] && key) key = test_and_set(&Lock);` ，另外，從 C.S. 離開之 Process 不會改變 Waiting[i]，**所以 P_i 不會參與進入 C.S 之決策。**<br>(2) 若 n 個 Process 皆想進入 C.S. ，則在有限的時間內( 無死結 )必定會決定出第一個執行 `while (waiting[i] && key) key = test_and_set(&Lock);` 者。之後，該選中 Process 從 C.S. 離開後，也會在有限的時間內，讓下一個欲進入 C.S. 的 Process 進入 C.S 。
  - **Bounded waiting：OK**<br>n 個 Process 皆欲進入 C.S. ，**表示 Waiting[0] ~ Waiting[n-1] 皆為 true。**<br>令 P_i 是第一個執行 `while (waiting[i] && key) key = test_and_set(&Lock);` **者，率先進入 C.S.，當 P_i 離開 C.S. 後會將** $P_{(i+1) \mod n}$ **的 Waiting 賦予 False 值，使** $P_{(i+1) \mod n}$ **進入 C.S.，依此類推 Process 會以** $＜P_i, P_{(i+1) \mod n}, \ldots, P_{(i-1) \mod n}＞$ **的序列 ( FIFO ) 進入 C.S.，所以沒有「Starvation」。**



##### 「compare_and_swap(int *value, int expected, int new_value) 」指令



- **功能：將 value 與 new_value 兩值互換，且 CPU 保證為不可分割之執行( Atomically executed )。**
- **實現作法 ( 硬體寫死，無法使用高階程式語言實現 )**：



```cpp
int compare_and_swap(int *value, int expected, int new_value) {
    int temp = *value;
    if (*value == expected)
    	*value = new_value;
    return temp;
}
```



###### Algorithm 1 (Fall)



- 共享變數
  - Lock：bool = False ( Initial value )
- $P_i$ 之程式碼：



```cpp
區域變數: key:bool;
do {
    while (compare_and_swap(&lock, 0, 1) != 0)
    ; /* do nothing */
    /* critical section */
    lock = 0;
    /* remainder section */
} while (condition == true);
```



- 分析
  - Mutual exclusion：OK
  - Progress：OK
  - Bounded waiting：Fall<br>假設 $P_i$ 以早於 $P_j$ 進入 C.S. ，所以 $P_j$ 進行等待，**當 **$P_i$ **離開 C.S. 後又想立即進入 C.S.，而以上述寫法可能會導致 **$P_i$ **有可能可以再次早於** $P_j$ **執行 compare_and_swap 進入 C.S.**，所以 $P_j$ 有可能會面臨「Starvation」。 



###### **Algorithm 2 ( Pass ) - FIFO



- 共享變數
  - **Lock：bool = False** ( Initial value )
  - **Waiting：[0 ... n-1] of bool 初值皆為 False**<br>$waiting[i] = \left\{\begin{matrix}True \quad P_i \; 有意願進入 \; C.S. ，且正在等待\\ False \quad 初值。P_i \; 不用等待，可以直接進入\; C.S. \end{matrix}\right.$
- $P_i$ 之程式碼：



 ```cpp
區域變數：key:bool;
         j  :int;
do {
    waiting[i] = true;
    key = true;
    
    // 假設目前無人在 C.S. 所以第一個執行此指令的 Process 可以直接進入下一步。
    while (waiting[i] && key) key = compare_and_swap(&Lock); 
    
    // **表明 P_i 不用繼續等待，可以進入 C.S.。
    waiting[i] = false;
    
    /* critical section */
    
    // 找出下一個欲進入 C.S. 之 P_j Process
    j = (i + 1) % n;
    // j != i -> 尚未繞完一圈 && !waiting[j] -> P_j 不想進入 C.S.
    while ((j != i) && !waiting[j])
    	j = (j + 1) % n;
    
    if (j == i)
    	lock = false;         // 無 Process 欲進入 C.S.
    else
    	waiting[j] = false;   // 讓 P_j 進入 C.S.
    
    /* remainder section */
    
} while (condition == true);
 ```


##### 問題探討



- Ex1

```cpp
do {
    waiting[i] = true;
    key = true;
    
    while (waiting[i] && key) key = test_and_set(&Lock);
    
    ~~waiting[i] = false;~~ // 若此行刪除將導致什麼後果？
    
    /* critical section */
    
    j = (i + 1) % n;
	while ((j != i) && !waiting[j])
    	j = (j + 1) % n;
    
    if (j == i)
    	lock = false;
    else
    	waiting[j] = false;
    
    /* remainder section */
    
} while (condition == true);
```



違反 Progress 性質 ( 造成死結 )。



### Semaphore ( 號誌 )



令 S 為 `Semaphore type` 變數，架構在 `Integer type`。針對 S ，提供兩個「Atomic 運算元」<br>**wait(S) ( 或 P(S) ) 與 signal(S) ( 或 V(S) )**。( 因為是「Atomic 運算元」，所以不會有 Race condition。 )



- **wait(S)**

```cpp
while(S <= 0) ;
S = S - 1;
```

- **signal(S)**

```cpp
S = S + 1
```



- 主要功能：「設計 C.S. 」與「解決同步問題」
- 使用範例
  - 共享變數<br>**mutex：semaphore = 1 (Initial value)**
  - 程式結構



```cpp
repeat
	wait(mutex);
	C.S.
    signal(mutex);
	R.S.
until False
```



**＜Note＞：semaphore 的初值，有某些意意。**$\left\{\begin{matrix}1 \Rightarrow 互斥控制之用途。\\ 0 \Rightarrow 強迫等待之用途。 \end{matrix}\right.$



#### 解決簡單的同步問題 ( synchronization problem )



Synchronization

- Process 因為「某件事情」的已發生或是未發生( 有多個 process 相互合作的時候 )，導致必須等待該事件完成或發生才得以繼續進行。



