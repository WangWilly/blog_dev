title: Operating System - Process Management and Thread Management
author: Willy Wang
tags:
  - Process
  - Thread
  - Management
categories:
  - Operating System
date: 2018-07-10 22:14:00
---
# Process

---



- 定義：A program in execution.

  - Process 建立後，主要組成有：$^{[1]}$Process NO.(ID), $^{[2]}$Process state, $^{[3]}$Process menory space：Code section, Data section。
  - Programming Counter (PC)：下一條指令的位址。
  - Stack
  - Cpu registers value
  - **是 OS 分配資源(CPU, I/O - Devce, Memory)的基本單位。**

- 與Program (程式) 的差異

  

  | Process          | Program              |
  | ---------------- | -------------------- |
  | **執行中的程式** | 只是一個在硬碟的檔案 |
  | "執行中"的單位   | "被動的"單位         |




## Procress Control Block



- 定義：作業系統為了管裡所有的 process ，會在 kernel memory 中，替每個 process 各自準備一個 block 紀錄 process 之所有相關的資訊。

- 內容：
  - Process No. (ID)：是唯一( unique )的。
  - Process state：有 「ready」、「running」、「wait」...。
  - Programming Counter ( PC )
  - CPU registers：有accmulatorm, psw, stack top...
  - CPU schedualing infomation：優先權值、process arrival time (FIFO)。
  - Memory management infomation：隨作業系統記憶體管理方式不同記錄不同的資訊。如：Base/limit register, page table 或 segememt table。
  - Accounting infomation：Pocess 已使用了多少 CPU time ，那些資源還剩多少資源、還剩 CPU time 可用，**系統管理員依據此資訊用以調校效能**。
  - I/O status infomation：Process 已發出多少 I/O-request，完成狀態如何，目前占用哪些 **I/O 資源**。

- $Ex29.$ (Ref P.4-75)：以下哪些項目是**不包含**在正常的 PCB 當中？

  - Process number

- - CPU register
  - **I/O device queue**：為作業系統管理，不屬於每個 process。

- $Ex34.$ (Ref P.4-78)：以下哪些項目是**不包含**在正常的 PCB 當中？

  - Process state
  - The **bitmap of this process**：用於磁碟可用空間的管理方法。

  - Register



## State trasition diagram (S.T.D.)



用於描述 Process 的 Lifecycle。



### 五個狀態



- 狀態
  - New (Create)：Process 被建立，已分得 PCB 空間，**尚未載入 memory( 尚未取得 memory 資源 )**。
  - Ready：Process 已在 memory 中，**且在 ready queue 內，具有資格爭奪 CPU**。
  - Running：Process 取得 CPU 並執行中。
  - Wait( Blocked )：Process 在 waitting queue 中，等待 I/O completed 或 事件發生。( **不會爭奪CPU** )
  - Exit( Terminate, Zombie, *abort* )：Process 完成工作正常結束或異常終止，**此時可能其 PCB 尚未回收，因為要等其父親 (paraent-process)得到孩子(child-process)的成果之後**，才會回收 PCB space。( 其他資源如memory, cpu, I/O-Device 等已回收 )
- 轉換
  - Admited：當記憶體空間足夠時，可由 **long-term scheduler**依優先權決定此工作載入與否。( in Batch system, not in Time sharing, not in Interactive system )
  - Dispatch：由 **short-term scheduler** 決定，讓高優先權的process 取得 CPU。
  - Interrupt (Time-out)：**執行中的 process 會因為某些事件發生而被迫放棄CPU，回到 ready queue。**<br>例如：**Time-out, interrupt 發生**，高優先權的 process 到達而被插隊。
  - Exit：Process 完成工作或異常終止。




![1526880431737](\blog\images\1526880431737.png)



### *七個狀態



- 狀態

  - Block/Suspend：Process 被 swapout 到磁碟中暫存。(Blocked/Sleep in Disk)
  - Ready/Suspend：事件完成或 I/O-completed。(Ready in Disk)

- 轉換

  - Admit (實線)：記憶體足夠時。
  - Admit (虛線)：記憶體不足時。
  - Block-Suspend ( Swap-out )：當 memory 空間不足且有其他高優先權 process 需要更多 memory space 時，會由 **medium-term scheduler 決定將 blocked process swap-out 至磁碟以空出 memory space**。
  - Block-Ready ( Swap-in )：*這是一個不好的設計，但仍可以支持*主要是因為若所有的「Blocked-suspend state」process 優先權皆高於「Ready-suspend state」process，且作業系統相信後者會比較早進入**等待狀態**時。
  - Ready-Suspend( Swap-out )：支持此行為( trainsition )的理由：<br>$^{[1]}$ 所有 blocked process 皆 swap-out 後 memory 仍不足時。<br>$^{[2]}$ **所有 blocked state process 之優先權皆高於 ready state process 時。**
  - Ready-Active ( Swap-in )：當 memory 有空時，**medium-term scheduler 可將process swap-in memory 之中(Ready)準備被執行。**
  - Running-Suspend( Swap-out )：*這是一個不好的設計，但仍可以支持*主要是因為若有一個高優先權process 從 「Blocked/suspend」變為「Ready/suspend」時，則作業系統可以強迫**低優先權的process放掉 CPU 及 memory，供高優先權的 process 使用。**

  


![1526883114200](\blog\images\1526883114200.png)



### UNIX STD




![1526885420905](\blog\images\1526885420905.png)



# Scheduler

---



## I/O-bound & CPU-bound




![1526887552589](\blog\images\1526887552589.png)

- I/O-bound Job
  - 定義：此類型的工作大都是需要*大量的 I/O operation( Resourse )但對於 CPU time( Computation )需求很少*，**所以其工作效能會受限於 I/O-device 之速度。**
  - 例如：資料庫管理、財報列印...。
- CPU-bound Job
  - 定義：此類型的工作大都是需要*大量的 CPU time( Computation ) 但對於  I/O operation( Resourse )需求很少*，**所以其工作效能會受限於 CPU 之速度。**
  - 氣象預估的大氣模型、科學模擬...。



## Long-term schduler



- 定義：又稱為 Job schduler，目的是為了從 Job-queue 中挑選一些工作載入到記憶體中。
- **特點：**
  - 執行頻率最低。
  - 可以調控 Multiprogramming degree。
  - 可以調控 I/O-bound Job 與 CPU-bound Job 之混和比例。
  - 由 Batch system 採用，但是 Real-time system 及 Time-sharing system 皆不會採用。



## Short-term schduler



- 定義：又叫做**CPU schduler 或 process schduler，**目的是從「Ready queue」中**挑出一個高優先權的 process，分派 CPU 給予執行。**
- **特點：**
  - 執行頻率**最高**。
  - **無法**調控 Multiprogramming degree。
  - **無法**調控 I/O-bound Job 與 CPU-bound Job 之混和比例。
  - 所有的 system 皆採用。



## *Midium-term scheduler



- **定義：為 Time-sharing system** 所採用。( 不為 Real-time system 所採用，因為該系統不支援 Swap-out - 虛擬記憶體 )
- 目的：當記憶體空間不足時，且又有其他高優先權的 process 需要記憶體時，此 scheduler **會挑選一些process ( 例：Blocked process、低優先權 procss ) 將其 swap-out 到磁碟中保存，以空出記憶體空間，翁其他 process 使用，將來等到有足夠的記憶體空間被釋放時，此 sechduler可再將 Swap-outed process Swap-in 至記憶體等待執行( Ready for execution )。**
- **特點：**
  - 執行頻率**居中**。
  - **可以**調控 Multiprogramming degree。
  - **可以**調控 I/O-bound Job 與 CPU-bound Job 之混和比例。
  - 為 Time sharing system 採用，但 real-time system 與 batch system 不採用。



# Context switching



當 CPU 要從 running process 切給另一個 process 使用之前，kernel 執行 **context-switching** 包含：

- **保存** running process 之目前狀態的資訊( PCB )。如：PC、stack、CPU register...。
- 要載入( Restore )另一個 Process 之狀態資訊( PCB )。

**Context switching 是一額外負擔**，其時間長短大都取決於硬體的因素。如：暫存器之數量、記憶體存取速度。



### 如何降低 Context switching 的負擔？



- 方法一：**若暫存器很多，則可以**
  - 讓每個 process 皆有私有的( private )「Register set」，則作業系統只要切換指標指向另一個 process 的「Register set」即可完成 context swiching，而不用記憶體的存取( store and restore )。
  - 此種方法速度最快。
- 方法二：**使用「Multithreading」機制。**
- 方法三：讓 **system process** 以及**user process**各自有自己的 register set，如此一來兩者之間的切換只要改變指向register set 的 pointer 即可。



# Dispatcher分派器<br>Dispatch Latency 分派延遲

---



- 定義：Dispatcher 為一*模組*用來**將 CPU 控制權授予「經由 CPU scheduler」所選出之 process (user process)。**
- 主要工作
  - **Context switching** ( 費時最久 )
  - **Change mode from kernel mode to user mode**
  - **Jump to the execution entry of that selected process**
- 在做上述這些工作的時間的總和就稱為「Dispatch Latency」。
- Dispatch Latency 越小越好。



# Process control operations

---

也就是 process 的建立、終止、暫停、回復執行、設定、修改、讀取 process atteributes ...，**而這些都應是作業系統提供的服務(system call)。**

- Process 可以建立自己的 child process，**目的是要 child process 工作。**
- **Child process 的工作可分為兩類：**
  - **與 parent 相同的工作。**(word編輯器)
  - **特定工作 。**(與 parent 不同) 
- **Parent 與 child 之互動關係**
  - Concurrent execution
  - Parent waits for child until child terminated
- Child process 的資源取得
  - 方法一：**作業系統供應**
  - 方法二：**Parente供應**
- *Parent 若終止*，則 child process 會：
  - 方法一 - Cascading termination：**一併終止。**
  - 方法二：存活資源向$^{[1]}$作業系統取得，或向$^{[2]}$祖先 process 取得。



## UNIX system call




![1527077415190](\blog\images\1527077415190.png)



- **fork()：**用以建立 child  process ，而 fork() 之回傳結果如下：
  - 失敗，因為資源不足無法建立 process ，**回傳 -1 給作業系統，在傳給 parent process。**
  - *成功*，作業系統會回傳 <br>$^{[1]}$ **0，給 child process。 **<br>$^{[2]}$ **大於 0 的正整數，給 parent process，且此值為 child process ID (PID)。**
- **wait()：**用以暫停 process 的執行直到某個事件發生。例如：parent 等到 child 直到 child 終止。
- **exit()：**用以終止 process 的執行且回收期資源 ( 不包含PCB )，通常 <br>$^{[1]}$ exit(0) 表示正常終止。 <br>$^{[2]}$ exit(-1) 表示異常終止。
- **execlp() 或 exec() 或 execve()：**用以載入特定的 Binary code( 可執行檔 ) 執行。 <br>**例：execlp("目錄名稱", "檔名", 個數)**
- getpid()：用以取得 process 的 ID。




![1527077382823](\blog\images\1527077382823.png)



**＜Note＞：**

1. 呼叫 fork() 後作業系統會配置 child process  memory space，此空間與 parent 是佔用不同的記憶體空間，且 child 的資料區塊及程式碼區塊內容均來自 parent 的一份副本。
2. 若 child 所做的工作與 parent 相同( 平行處理 )，則只要使用 fork() 便可達到此目的。
3. **若 child 要做特定的工作(與 parent 不同)，則 child 必須執行 execlp() 以載入特定工作的執行檔。**




![1527077442901](\blog\images\1527077442901.png)



## 程式實際操作

---



### $Ex1.$



建立 child process 執行 ls 命定檔且 parent 等到 child 完成之後 parent 才輸出「child complete」。

Creating a separate process using the UNIX fork() system call.

```cpp
#include<stdio.h>
#include<unistd.h>
#include<sys/types.h>
#include<sys/wait.h>

int main(void) {
	
    pid_t pid;
    pid = fork();                        //fork a child process
    if (pid < 0) {                       // error occurred
        fprintf(stderr, "Fork Failed\n");
        return 1;                        //exit(-1);
    }
    else if (pid == 0) {                 // child process
    	execlp("/bin/ls","ls",NULL);     // 程式區塊指標轉換，下方不做，見上圖。
    }
    else {                               // parent process
        wait(NULL);                      // parent will wait for the child to complete
        printf("Child Complete\n");
    }
    return 0;
}
```





### $Ex2.$ (Ref p. 4-52)



Identify the values of pid at lines A, B, C, and D. ( Assume that the actual pids of the **parent and child are 2600 and 2603**, respectively. ) 

```cpp
#include<stdio.h>
#include<unistd.h>
#include<sys/types.h>
#include<sys/wait.h>

int main(void) {
    pid_t pid, pid1;
    pid = fork();                          // fork a child process
	
    if (pid < 0) {                         // error occurred
        fprintf(stderr, "Fork Failed\n");
        return 1;
    }
    else if (pid == 0) {                   // child process
        pid1 = getpid();
        printf("A: Child: pid = %d\n",pid);     // A -> 0
        printf("B: Child: pid1 = %d\n",pid1);   // B -> 2603
    }
    else {                                 // parent process
        pid1 = getpid();                   // child process ID
        printf("C: Parent: pid = %d\n",pid);    // C -> 2603
        printf("D: Parent: pid1 = %d\n",pid1);  // D -> 2600
        wait(NULL);
    }
    return 0;
}
```



### $Ex3.$ (Ref p. 4-53)



```cpp
#include<stdio.h>
#include<unistd.h>
#include<sys/types.h>
#include<sys/wait.h>

int value = 5;                                // global varible
int main(void) {
    pid_t pid;
    pid = fork();
    
    if (pid == 0) {                           // child process
        value += 15;
        return 0;
    }
    else if (pid > 0) {                       // parent process
        wait(NULL);
        printf("PARENT: value = %d\n",value); // LINE A value -> 5
        return 0;
    }
	return 0;
}
```





### *$Ex4.$ 程序的並行





1. 求輸出結果？ ( Tip: **父與子是並行的。** )

```cpp
#include<stdio.h>
#include<unistd.h>
#include<sys/types.h>
#include<sys/wait.h>

int main(void) {
    pid_t pid;
    pid = fork();
    
    if (pid == 0) {                         // child process
		printf("A\n");
    }
    else if (pid > 0) {                     // parent process
        printf("B\n");
    }
	return 0;
}
```



Ans：

```
A
B
```

OR

```
B
A
```

都有可能。



2. 求輸出結果？ ( Tip: **父與子是並行的。** )

考慮：


![1527080836257](\blog\images\1527080836257.png)

```cpp
#include<stdio.h>
#include<unistd.h>
#include<sys/types.h>
#include<sys/wait.h>

int main(void) {
    pid_t pid;
    pid = fork();
    
    if (pid == 0) {                         // child process
		printf("A\n");
    }
    else if (pid > 0) {                     // parent process
        printf("B\n");
        wait(NULL);
    }
    printf("C\n");
	return 0;
}
```

Ans：

```
A
C
B
C
```

OR

```
A
B
C
C
```

OR

```
B
A
C
C
```



### *$Ex5.$ 共享變數



1. **假設 Count 是 parent 與 child process 的共享變數，且初值為 5。**

```cpp
#include <sys/types.h>
#include <stdio.h>
#include <unistd.h>

int main() {
    pid_t pid;
    pid = fork();
    
    if (pid == 0) {                         // child process
		count++;
		printf("%d\n", count);
    }
    else if (pid > 0) {                     // parent process
		wait();
		count--;
		printf("%d\n", count);
    }
}
```

Ans：

```
6
5
```

> **＜Note＞：**共享變數可用以下方式達成。
>
> 1. 檔案共享。
> 2. UNIX 的 pipe。
> 3. 共享記憶體區間。( Windows / Linux )



2. **假設 Count 是 parent 與 child process 的共享變數，且初值為 5。**

考慮：


![1527081997770](\blog\images\1527081997770.png)

```cpp
#include <iostream>
#include <cstring>
#include <cerrno>
#include <unistd.h>
#include <fcntl.h>
#include <sys/mman.h> 
using namespace std;

int main(int argc, char **argv) {
    int* count;
	count = (int*) mmap(NULL, sizeof(int), PROT_READ | PROT_WRITE, MAP_SHARED | MAP_ANON, 0, 0);
	if (count == MAP_FAILED) {
		cout << "mmap failed..." << strerror(errno) << endl;
		return -1;
	}
	*count = 5;
	
    pid_t pid;
    pid = fork();
    if (pid == 0) {	
        (*count)++;
    } else if (pid > 0) {
		(*count)--;
	}
 	cout << (*count) << endl;
    return 0;
}
```

Ans：

```
5
5
```

OR

```
4
5
```

OR

```
6
5
```

OR ( **當指令不是 atomic 時** )

```
6
6
```

OR ( **當指令不是 atomic 時** )

```
4
4
```

*OR ( **當指令不是 atomic 時** )

```
4
6
```

*OR ( **當指令不是 atomic 時** )

```
6
4
```



### $Ex6.$ 有幾個 process 被建立了？ including main() (Ref p. 4-54, Text book p. 150)



1. 

```cpp
#include <stdio.h>
#include <unistd.h>
int main() {
    /* fork a child process */
    fork();
    /* fork another child process */
    fork();
    /* and fork another */
    fork();
    return 0;
}
```

Ans：8 個。

2. 

```cpp
#include <stdio.h>
#include <unistd.h>
int main() {
    if(fork() == 0) { // child process
        fork();
        fork();
    }
    fork();
    return 0;
}
```

Ans：10 個。




![1531135436305](\blog\images\1531135436305.png)



3. *

```cpp
#include <stdio.h>
#include <unistd.h>
int main() {
    fork();
    if(fork()>0) {       // parent process
        fork();
    }
    else if(fork()==0) { // child process
        fork();
        fork();
    }
    return 0;
}
```
Ans： 14 個。(~~30個~~)




![1531136821130](\blog\images\1531136821130.png)




![1531137337724](\blog\images\1531137337724.png)



4. **

```cpp
#include <stdio.h>
#include <unistd.h>
int main() {
    for(int i = 0; i < 3; i++) {
        if(fork()==0) {
            fork();
            fork();
            fork();
        }
    }
}
```

Ans：729 個。




![1531138419381](\blog\images\1531138419381.png)




![1531139238689](\blog\images\1531139238689.png)



$i == 2$, 會再多新生 $8 \times 81 = 648$ 加上原本的 81 個等於 729 個 Processes。



5. 

```cpp
#include <stdio.h>
#include <unistd.h>
int main() {
    for(int i = 0; i < 3; i++) {
        if(fork()==0) {
            fork();
        }
        else if (fork()>0) {
            if(fork()==0) {
                fork();
            }
        }
    }
}
```

Ans：216 個。(~~1728 個~~)




![1531139971022](\blog\images\1531139971022.png)



$i == 1$，上面六個 Processes 會再各自生成 5 個行程，所以會再多生產 $5 \times 6 = 30$ 個 Processes ，加上原本的 Processes 等於 36 個。<br>$i == 2$，36 個 Processes 會再各自生成 5 個行程，所以會再多生產 $5 \times 36 = 180$ 個 Processes ，加上原本的 Processes 等於 216 個。



6. 
   1. `printf("%d\n;", a);`共做幾次？
   2. 印出`0`幾次？
   3. 印出`1`幾次？
   4. 印出`2`幾次？

```cpp
#include <stdio.h>
#include <unistd.h>
int main() {
	int a = 2;
    fork();
    a--;
    printf("%d\n", a);
    if(fork()==0) {
        a--;
        printf("%d\n", a);
        fork();
    }
    else {
        a++;
        fork();
        printf("%d\n", a);
    }
}
```

Ans：

```
1
1
0
0
2
2
2
2
```




![1531141578065](\blog\images\1531141578065.png)



```
\\ 錯誤答案
1
1
0
0
2
2
2
2
2
2
2
2
2
2
2
2
2
2
2
2
2
```



# CPU 排程

---



## 評估 CPU scheduking 效能之五個準則(criteria) 

- CPU utilization - CPU 利用度(率)
  - $\frac{CPU \; Process \; execution \; time}{CPU \; total \; time}$
  - $CPU \; total \; time = process \; execution \; time + comtext \; switch \; time + idle \; time \dots$
  - $Ex. $ Process 平均花 5 ms 在 execution 上，花 1 ms 在 comtext switching，則 $CPU \; utilization = \frac{5}{5+1}$
- Throughput - 產能
  - 單位時間完成的工作數目。




![1527426695265](\blog\images\1527426695265.png)



- *** Waiting time - 等待時間**
  - Process 在 Ready Queue 中等待獲得 CPU 之等待**時間的加總**。
  - $Ex.$上圖的等待時間為 $(8-2)+(19-15) = 10$。
- *** Turnaruond time - 完成時間**
  - 自 process **進入(到達)** 到工作完成的這段時間差值。 
  - $Ex. $上圖的等待時間為 $26-2 = 24$。
- *Response time - 回應時間*
  - 自使用者 (user process) 輸入命令(或資料)給系統後，到系統產生第一個回應的時間差。(**Time-shaeing system, user-interactive application 特別重視。**)

由上述可知，CPU utilization、throughput 愈高愈好；waiting time、turnaround time、response time 愈低愈好。



## 排班法則



### Starvation



- Process 因為**長期無法取得完工所需之各項資源，導致遲遲無法完工，形成「Indefinite blocking - 未知期停滯」現象。**
- 容易發生在*不公平對待的環境之中*，若又有「Preemptive - 可搶奪機制」，**則更容易發生。**
- 解決方案：使用「Aging」技術。隨著 process 待在 system 內的時間逐漸增加，**我們也逐步調高此process 的優先權**，經過一段有限的時間後，此 process 會有最高優先權，故可取得需要的資源(resourses)完工。
  - **＜Note＞：soft real-time system 不採用 「Aging」。**



### Non-preemptive 與 premptive 法則



#### 觀點一



- Non-premptive 法則
  - 除非執行中的 process **自願放掉 CPU**，其他的 process 才有機會取得 CPU，否則就只能 wait ，不可逕自搶奪 CPU。
  - $Ex.$ 自願放棄使用 CPU 的情況：**完成工作、wait for I/O-completed after issue I/O-request ...。**
  - Pros
    1. Comtext switching 次數**較少** ( 時間節省 )。
    2. **Process 之完工時間點較可以預期 ( preditable )。**
    3. 比較不會有「Race condition problem - 資源競爭問題」。
  - Cons
    1. **排班效能較差，因為可能會有 「Convoy effect」**。
    2. **不適合用在 Time-sharing system 與 Real-time system。**
- Preemptive 法則
  - 執中的 Provess 有可能**被迫放棄 CPU ，**回到 Ready queue ，再將 CPU 指派給別的 Process 使用。
  - $Ex.$ **Time-out ( 用在分時系統 )、interrupt ...。**
  - Pros
    1. 排班效益較佳，平均 waiting / turnaround time 較小。
    2. 適用於 Real time system 與 Time sharing system。
  - Cons
    1. 完工時間較不可預期。
    2. Context switching 次數較多，負擔重。
    3. **須注意 Race condition 的發生。**



#### *觀點二



**從 CPU 排班決策(啟動)的時機點區分。**



- Running $\rightarrow$ Block
  - Ex. Wait for I/O-completed (**自願**)
- Running $\rightarrow$ Ready
  - Ex. Time out (**被迫**)
- Wait $\rightarrow$  Ready
  - Ex. I/O-completed (**高優先權的 Process 開始需要 CPU，作業系統啟動排班器**，低優先權的 process 被迫放棄 CPU )
- Running $\rightarrow$ Exit (terminate)
  - Ex. Task completed (**自願**)

**所以若排班決策之啟動點只包含** Running $\rightarrow$ Block 與 Running $\rightarrow$ Exit (terminate) **未包含** Running $\rightarrow$ Ready 或 Wait $\rightarrow$  Ready **則為 Non-preemptive，否則為 Preempt。**

**＜Note＞：凡是 **$\ldots \rightarrow$ **Ready 皆列入 Preemptive 元素，所以 Ready/suspend ** $\rightarrow_{Swap\; in}$ **Ready、New** $\rightarrow$ **Ready ... 也列入。** 



###預估 Process Next CPU Burst Time



- 公式

$$
\tau_{n+1} = \alpha \cdot t_n + (1-\alpha) \cdot \tau_n
$$

- $\tau_{n+1}$：下次預估值。
- $t_n$：本次實際值。
- $\tau_n$：本次預估值。
- $\alpha$：加權值。($0 \leq \alpha \leq 1$)
- $Ex.$ 當$\alpha = 0.5$


![1527475744485](\blog\images\1527475744485.png)



**＜Note＞：**$\alpha$ 的值用於條整與**歷史紀錄的相依性高低。**



### FIFO



**到達時間最小的 process 取得 CPU，也就是說先來先做。**




![1527427440948](\blog\images\1527427440948.png)

- 到達時間皆為 0。
- 到達順序為：P1、P2、P3。
- Gantt chart。




![FIFO](\blog\images\FIFO.png)



- Avg. waitting time


$$
\frac{(0-0)+(24-1)+(27-0)}{3}=17
$$




- Avg. turnaround time


$$
\frac{(24-0)+(27-0)+(30-0)}{3}=27
$$

- **分析**
  - 易於製作。
  - 排班效能最差，**即 Avg. waiting time & Avg. turnaround time 最長** (*其他準則不看*)。
  - 可能有「Convoy effect - 護衛效應」，許多 processes 均等待一個需要很長 CPU time 之 process 完成工作，才能取得 CPU，**導致 Avg. waiting time 太長**。
  - 非常公平。
  - 沒有 starvation 現象。
  - **Non-premptive**，不可搶奪、插隊。



### SJF ( Shortest Job First )



**具有最小 CPU time 的 process，**優先取得 CPU。




![1527472805224](\blog\images\1527472805224.png)

- 到達時間皆為 0。
- Gantt chart。




![1527472898945](\blog\images\1527472898945.png)

- Avg. waiting time


$$
\frac{(0-0+(3-0)+(9-0)+(16-0)}{4}=7
$$

- **分析**

  - **排班效益最佳( Optimal ) 即 Avg. waiting/turnaround time 最小。**<br>Proof：

  

  
![1527473571408](\blog\images\1527473571408.png)

  - 由上圖可知<br>Waiting time for long job：$0 \rightarrow CPU \; execution \; time_{short \; job}$<br>Waiting time for short job：$CPU \; execution \; time_{long \; job} \rightarrow 0$<br>Avg. waiting ime：$\frac{(CPU \; execution \; time_{short \; job}-0)+(0-0)}{2} < \frac{(0-0)+(CPU \; execution \; time_{long \; job}-0)}{2}$
  - 以這種方式 **Short job 所減少的等待時間必定大於等於** Long job 所增加的等待時間，所以**會使平均等待時間變小，最後可歸納到必為最佳的排班法則。**
  - **不公平，偏好 short job。**
  - **可能會 Starvation (for long job)。**
  - 又可以分為：
    1. Non-preemptive **( SJF )**
    2. Preemptive **( SRTF or SRJF )**
  - **較不適合用在 shortest-trem scheduler，**因為 short-term schduler 執行頻率太高，所以**很難在極短時間內預估出精確每個process 的 CPU burst time 又要挑出最小值，**不易真正呈現出 SJF 之行為；**但比較適合 long-term schduler。**



### SRTF ( Shorest Remaining-time Job First )



又稱為 SRJF 或 SRTN ( Shorest Remaining-time Job Next )，**即為「Preemptive - SJF」**，將剩餘 CPU burst time 最小的 Process 取得 CPU，若「新到達的 process」 的 CPU burst time 目前執行中 process 剩下的 CPU time ，則新到達的 Process 可以**插隊( Preemption )目前執行中的 Process。**




![SRTF](\blog\images\SRTF.png)

- Avg. waiting time for

  - ***SRTF**

  

  
![1527477828916](\blog\images\1527477828916.png)

  
  $$
  \frac{ ((0-0)+(10-1)) + (1-1) + (17-2) + (5-3) }{ 4 } = 6.5
  $$
  

  - **SJF**

  


![SJF](\blog\images\SJF_correct.png)  


  
  $$
  \frac{ (0-0)+(8-1)+(12-3)+(17-2) }{4} = 7.75
  $$
  

  - FIFO



![FIFO](\blog\images\SJF.png)


$$
\frac{ (0-0) + (8-1) + (12-2) + (21 -3) }{4} = 8.75
$$

- **分析**
  - **與 SJF 相比 SRTF 的平均 waiting / turnaround time 會比較小，但是付出較大的 Context switching 負擔。**
  - 不公平，偏好 Short remaining time job。
  - 可能會有 Starvation。
  - 屬於 Preemptive。



### Priority Method



**具有最高優先權的 Process** 取得 CPU ，若多個 Process 權值相同，則以 **FIFO** 為準。




![1527479225080](\blog\images\1527479225080.png)

- 到達時間皆為 0。
- Non-preemptive priority method 且 **Priority number 愈小優先權愈大**。
- Avg. waiting time




![1527479387651](\blog\images\1527479387651.png)


$$
\frac{ (6-0)+(1-0)+(16-0)+(18-0)+(1-0) }{5}=8.2
$$

- $Ex2.$



| Process | Arrvial time | Priority | Burst time |
| :-----: | :----------: | :------: | :--------: |
|   P1    |      0       |    5     |     5      |
|   P2    |      2       |    2     |     3      |
|   P3    |      5       |    4     |     8      |
|   P4    |      10      |    3     |     4      |
|   P5    |      13      |    1     |     6      |



- Preemptive priority method。
- **Priority number 愈小優先權愈大**。
- Avg. waiting time




![1527486332570](\blog\images\1527486332570.png)


$$
\frac{ ((0-0)+(23-2)) + (2-2) + ((5-5)+(20-10)) + ((10-10)+(19-13)) + (13-13) }{5} = \frac{37}{5}
$$


- 分析
  - 不公平。
  - 可能會有 Starvation ，但可以用「Aging」解決。
  - **分為 Non-preemptive 與 Preemptive。**
  - 是一個具有參數化的方法，**給予高低不同的優先權值**，可展現出不同的排班行為。

 

| Priority 的定義                 | 行為 |
| ------------------------------- | ---- |
| Arrival time 愈小，優先權愈大。 | FIFO |
| CPU time 愈小，優先權愈大。     | SJF  |
| 剩餘時間愈小，優先權愈大。      | SRTF |



### *Round Robin



**為 Time sharing system 所採用**，作業系統會規定一個 CPU time Quantum (Slice) ，當 Process 取得 CPU 執行後，若未能在此 Quantum 完成工作，則「Timer」會發出一個「Time-out interrupt」通知作業系統強迫回收 CPU 並將此 Process 送回「Ready queue」中等待下一輪再取得 CPU 執行，**每一輪之中，是採以 FIFO 的排班法則規劃**。



| Process | CPU time |
| :-----: | :------: |
|   P1    |    8     |
|   P2    |    4     |
|   P3    |    9     |
|   P4    |    5     |



- 到達時間為 0。
- 順序為：P1～P4。
- Quamtum = 4。
- Avg. waiting time。




![1527829801602](\blog\images\1527829801602.png)


$$
\frac{((0-0)+(16-4))+((4-0))+((8-0)+(20-12)+(25-24))+((12-0)+(24-16))}{4} = \frac{53}{4}
$$


| Process | *Arrival time | CPU time |
| :-----: | :-----------: | :------: |
|   P1    |       0       |    20    |
|   P2    |       2       |    5     |
|   P3    |       7       |    3     |
|   P4    |      13       |    8     |

- Quamtum = 4。
- Avg. waiting time。




![1527831109693](\blog\images\1527831109693.png)




$$
\frac{((0-0)+(8-4)+(16-12))+((4-2)+(15-8))+((12-7))+((18-13))}{4} = \frac{27}{4}
$$




| Processs | *Arrival time |       **行程行為       |
| :------: | :-----------: | :--------------------: |
|    P1    |       0       | 5 CPU + 6 I/O + 4 CPU  |
|    P2    |       3       |         15 CPU         |
|    P3    |       8       | 3 CPU + 10 I/O + 9 CPU |
|    P4    |      14       |         8 CPU          |

- Quantum = 5 **(Ref p.4-111)**。




![1528097302440](\blog\images\1528097302440.png)



- Turnaround time。
  - P1 = 22 - 0。
  - P2 = 32 - 3。
  - P3 = 44 - 8。
  - P4 = 40 - 14。
- Avg. waiting time。


$$
\frac{((0-0)+(18-11))+((5-3)+(13-10)+(27-18))+((10-8)+(32-23)+(40-37))+((22-14)+(37-27))}{4} = \frac{113}{4}
$$




**＜Note＞：有爭議的題目。**



| Process | Arrival time | CPu time |
| :-----: | :----------: | :------: |
|   P1    |      0       |    10    |
|   P2    |      4       |    9     |
|   P3    |      8       |    6     |

- Quantum = 4。




![1527833536050](\blog\images\1527833536050.png)

- 不知道為 P2 還是 P1 先進入 Waiting queue。
- **分析**
  - **Time sharing system 採用此方法。**
  - 也為一種可**參數化 (ex. Quantum)**的法則。
  - **公平。**
  - 沒有 starvation。
  - ***Preemptive。**
  - Round robin 排班效益取決於 Time quantum 大小決定。<br> $Ex1. \; Quantum = \infin$ 則 RR 會變成 **FIFO**，也可以說排班的效能很差。<br>$Ex2. \; Quantum \rightarrow 0$ 則 Context switching 太頻繁，**CPU utilization 會變得非常差** ($\approx 0$)。<br>$Ex3. $ 根據經驗法則，若 Quantum 能讓 80% 的工作量在該時間完成，效能最佳。



**＜Note＞：** RR 雖然是公平，但**可支持差異化 ( 優先權差異 ) 的實現，**請問該如何達成？<br>Ans：

- **方法一**
  - **針對高優先權的 Process 在 Ready queue 中置入多個 PCB pointer 指向此 Process ，使得每一輪當中可以多次取的 CPU 的機會。**
- **方法二**
  - **針對高優先權 Process 給予較大的 Quantum。**



### Multilevel Queues



- 將原本單一一條 Ready queue 變成多條 Ready queue ，且高低優先權不同。
- Queue 之間也有以排班的方式管理，通常採取**「Preemptive priority」管理**。
- 每個 Queue 可以有自己的排班法則。
- Process 一旦被置入於某個 Queue 中，**就不可(不允許)在不同 Ready queue 之間移動。**




![1527835326994](\blog\images\1527835326994.png)



- $Ex. $ I/O - Bound 與 CPU - Bound Job 各自要置入哪種等級的 Queue 比較好？
  - Ans：I/O - Bound Job $\Rightarrow$ 高優先 Queue ( 使用 CPU 不多 )；CPU - Bound Job $\Rightarrow$ 低優先 Queue ( 會使用大量 CPU )
- 分析
  - **可參數化 ( Queue 數目、Queue之間的排班法則、每個 Queue自己的排班法則、Process被放入哪個 Queue 之準則---Critera ) 的項目眾多，**有助於排班設計及效能調校之彈性 ( flexibility )。
  - 不公平。
  - ***有 Starcation。**
  - Preemptive。



### Multilevel Feedback Queues



與 Multilevel queue 相似，**差別在於「允許」Process 在不同 Queue 之間移動。( 可以採取類似「Aging」的技術或是可以搭配「降級」的方式來避免「Starvation」 )**



- 分析
  - **可參數化 ( Queue 數目、Queue之間的排班法則、每個 Queue自己的排班法則、Process被放入哪個 Queue 之準則---Critera ) 的項目眾多，**有助於排班設計及效能調校之彈性 ( flexibility )。
  - 不公平。
  - **可解決 Starcation。**
  - Preemptive。



### 結論



- 哪些是 Non-preemptive。
  - FIFO
  - SJF
  - **Non-preemptive priority**
- 哪些是 Preemptive。
  - SRJF
  - RR
  - Preemptive priority
  - Multilevel (Feedback) queue
- 哪些沒有 Starvation。
  - FIFO
  - RR
  - Multilevel feedback queue
- **那些包含於( $\subset$ )關係是錯的？**
  - FIFO $\subset$ Priority
  - SJF $\subset$ Priority
  - FIFO $\subset$ RR
  - **SJF **$\subset$ **RR**
  - *RR* $\subset$ *MFQs*



### (補充) CPU utilization 計算

- Modern 版

  - 假設採 RR 排班，令 Time quantum 為 Q、Context switching time 為 S，<br>Process 平均執行每隔 T 時間會發出 I/O-request，求下列狀況的 CPU utilization。

- 0 < S < T << Q

    
![1527838126019](\blog\images\1527838126019.png)

所以 $\frac{T}{T+S}$

- 0 < S < Q << T

    
![1527838656554](\blog\images\1527838656554.png)

所以 $\frac{Q}{Q+S}$

- 0 < S = Q << T

    由上圖可知 $\frac{Q}{Q+S} = \frac{Q}{Q+Q} = 50 \%$

    4. Q 趨近於 0。

    $\frac{Q}{Q+S} \approx \frac{0}{0+S} = 0$，**CPU utilization 趨近於 0。**

- 恐龍版 ( Ref p. 4-86 Ex.50 )

  - 10 個 I/O-Bound tasks、1 個 CPU-Bound task，I/O-Bound task 執行每隔 1ms 發出 I/O-request ，*每個 I/O 運作花 10 ms* ( 此例子有 CPU-Bound task 所以不會因此 Idle )。Context switching time: 0.1 ms，所有process 永遠不會結束，求 CPU utilization ，採 RR 法則。 

    1. Quantum = 1ms。<br>針對 I/O-Bound task，在 Time-out 的同時也發出了 I/O-request，接著花 0.1 ms 在 Context switching，所以一個 I/O-Bound task 共花了 $1 + 0.1 = 1.1$ (ms)。<br>針對 CPU-Bound task，會將所有 CPU time 用完後 Time-out ，接著花 0.1 ms 在 Context switching，所以一個 CPU-Bound task 共花了 $1 + 0.1 = 1.1$ (ms)。<br>$CPU utilization = \frac{CPU \; time_{execution}}{CPU \; time_{total}} = \frac{10 \times 1 + 1 \times 1 }{10 \times 1.1 + 1 \times 1.1} = \frac{1}{1.1} \approx 91\%$

    2. Quantum = 10 ms。<br>針對 I/O-Bound task，CPU time 用不完，隔 1 ms 後直接發出 I/O-request ，並也花 0.1 ms 在 Comtext switching 上，所以一個 I/O-Bound task 共花了 $1 + 0.1 = 1.1$ (ms)。<br>針對 CPU-Bound task ， 會將所有的 CPU-time 用完後 Time-out 接著花 0.1 ms 在 Context switching ，所以一個 CPU-Bound task 共花了 $10 + 0.1 = 10.1$ (ms)。<br>$CPU utilization = \frac{CPU \; time_{execution}}{CPU \; time_{total}} = \frac{10 \times 1 + 1 \times 10 }{10 \times 1.1 + 1 \times 10.1} = \frac{20}{21.1} \approx 94\%$



## 特殊系統之排班設計考量



### Multiprocessor system



- ASMP (Master-Slave 架構)：因為都是以 Master-processor 來排班，類似於過去單顆 CPU，所以沒有特殊的排班設計。
- **SMP**：主要有兩個排班的機制。
  - **方法一**： 每個CPU 共享同一個 Ready queue ，當一個 CPU 完程某 Process 後，**就去存取 Ready queue**。<br>設計重點：
  - 1. 必須提供上述 Ready queue 的**互斥存取之機制**，若未提供，則可能發生 **Process 重複執行，或有 Process 被忽略的錯誤**。<br>例：CPU 去取 Process 之工作如下：<br>第一步，取得(read) Queue Front 端 Process 之 PCB pointer；第二步，從 Queue 中刪除此 Process pointer 。
    2. 不須考慮附載平衡 (load balance)，因為每個 CPU 在工作都做完時會再繼續從 Ready queue 中挑選工作，不會讓自己閒置(idle)。
  - **方法二**：每個 CPU 都有自己的 Ready queue ，每個 CPU 只會檢查自己的 Ready queue ，不會去檢查其他 CPU 的 Ready queue ，**有工作就執行，無工作就閒置 (idle)**。<br>**設計重點：**<br>
    1. **不須有互斥存取的考量。**
    2. **需考慮附載平衡 ( Load balanceing )，避免 CPU 之勞務不均 (有人忙、有人閒)。**<br>通常使用 2 種機制來調整  CPU 的附載 ( loading )：
       1. Push migration ( 移轉 ) --- 像是領班、工頭
       2. Pull migration --- 好同事



### Process affinity



- 在 multiprocesors system 中，當 Process 已決定某 CPU 上執行，則在他執行過程之中，盡量**不要將之移轉到其他 CPUs 上執行，**除非有其必要。( 如：Processor bad、Load balancing... )**避免 CPU 之 Cache、暫存器的內容要複製且又要刪除該工作，而影響效能。**
- 有兩種 Affinity：
  - Hard-affinity：該 Process 不可移轉。
  - Soft-affinity：盡可能不移轉。( 若有需要，仍可移轉。)



## Real-time system 排班設計考量



### Hard read-time system




![1528098291707](\blog\images\1528098291707.png)



- **排班設計考量**
  - **確認這些工作是否可排程 ( schedulable )？也就是 CPU 可否負荷？**<br>判斷公式：若 $\sum_{i = 1}^n \frac{C_i}{P_i} \leq 1$ **則為可排程，反之為不可排程。**<br>其中：$n$ 表示 Real-time event (Process)之數目、 $C_i$1表示 $Event_i$ (Process)之所需 CPU time、P_i 表示 $Event_i$ (Process)之發生週期( Period time )。<br>例：有下列四個 Real-time event ，其 CPU burst time 分別是：20 ms、50 ms、30 ms、X ms。其 period time 分別是 80ms、100ms、300ms、1ms。則在可排程的要求情況下，X 不可超過多少？<br>Ans：$\frac{20}{80}+\frac{50}{100}+\frac{30}{300}+\frac{X}{1000} \leq 1 \Rightarrow \frac{X}{1000} \leq 0.15 \Rightarrow X \leq 150$ (ms)。
  - **再考慮是否可以滿足各工作的 Dead line。**有兩個排班則：
    1. **Rate-monotonic scheduling**
    2. **EDF ( Eaeliest Deadline First )**
  - 如何排程，以滿足各工作的 deadline？



#### Rate-monotonic

- 採取靜態的優先權值且可插隊( Preemptive )。
- **Period time 愈小，優先權值愈高**。
- $Ex1. $

| Process | Period time | CPU time |
| :-----: | :---------: | :------: |
|   P1    |     50      |    20    |
|   P2    |     100     |    35    |



- 是否可排程化？
  - $\frac{20}{50}+\frac{35}{100}=0.4+0.35=0.75 \leq 1 \Rightarrow$ OK
- **若規定 P2 優先權高**，且為 preemptive，是否滿足 deadline？
  - 甘特圖




![Ratemonotonic](\blog\images\Ratemonotonic.png)

P1 未能滿足 deadline，P2 滿足 deadline。



- 採用 Rate-monotinic 是否滿足 deadline？
  - **Period time 愈小，優先權愈高**，所以 P1 的優先權高。
  - 甘特圖




![1528099381692](\blog\images\1528099381692.png)

P1 滿足 deadline，P2 滿足 deadline。



- $Ex2. $


| Process | Period time | CPU time |
| :-----: | :---------: | :------: |
|   P1    |     50      |    25    |
|   P2    |     80      |    35    |

- 採用 Rate-monotinic 是否滿足 deadline？
  - **Period time 愈小，優先權愈高**，所以 P1 的優先權高。
  - 甘特圖




![1528099949133](\blog\images\1528099949133.png)



- 分析：
  - 並不保證可以滿足 deadline。
  - **在靜態的優先權值要求下，是最佳的狀況( optimal )。(若該手法無法滿足 deadline，其他針對靜態優先權值的排班也無法滿足。)**



#### Earliest deadline First (EDF)



- 採用**動態優先權值**，且為**可插隊**。
- **規定 deadline 愈早，優先權愈高**。
- $Ex 1.$



| Process | Period time | CPU time |
| ------- | ----------- | -------- |
| P1      | 50          | 25       |
| P2      | 80          | 35       |

 

- 以 Rate-monotinic 是否滿足 Deadline？
  - P1 的 Period time：50 < P2 的 Period time：80，P1的優先權大於 P2 的優先權。




![1528701363468](\blog\images\1528701363468.png)



- 以 EDF 是否滿足 Deadline？
  - (1) P1 的 Deadline：50 < P2 的 Deadline：80，P1的優先權大於 P2 的優先權。
  - (2) P1 的 Deadline：100 > P2 的 Deadline：80，P2的優先權大於 P1 的優先權。
  - (3) P1 的 Deadline：100 < P2 的 Deadline：160，P1的優先權大於 P2 的優先權。
  - (4) P1 的 Deadline：150 < P2 的 Deadline：160，P1的優先權大於 P2 的優先權。
  - (5) P1 的 Deadline：200 < P2 的 Deadline：240，P1的優先權大於 P2 的優先權。
  - (6) P1 的 Deadline：250 < P2 的 Deadline：240，P2的優先權大於 P1 的優先權。
  - (7) P1 的 Deadline：300 < P2 的 Deadline：320，P1的優先權大於 P2 的優先權。
  - (8) P1 的 Deadline：350 < P2 的 Deadline：400，P1的優先權大於 P2 的優先權。
  - **(8) P1 的 Deadline：400 = P2 的 Deadline：400，P1的優先權等於 P2 的優先權。**




![1528704009904](\blog\images\1528704009904.png)



- $Ex 2.$

| Process | Period time | CPU time |
| ------- | ----------- | -------- |
| P1      | 50          | 25       |
| P2      | 75          | 30       |



- 以 Rate-monotinic 是否滿足 Deadline？
  - P1 的 Period time：50 < P2 的 Period time：75，P1的優先權大於 P2 的優先權。




![1528704266407](\blog\images\1528704266407.png)



- 以 EDF 是否滿足 Deadline？
  - (1) P1 的 Deadline：50 < P2 的 Deadline：75，P1的優先權大於 P2 的優先權。
  - (2) P1 的 Deadline：100 > P2 的 Deadline：75，P2的優先權大於 P1 的優先權。
  - (3) P1 的 Deadline：100 < P2 的 Deadline：150，P1的優先權大於 P2 的優先權。
  - (4) **P1 的 Deadline：150 = P2 的 Deadline：150，P1的優先權等於 P2 的優先權。**




![1528705571205](\blog\images\1528705571205.png)



- **分析**
  - **在可排程的情況之下必 EDF 保證最佳 (optimal)。(任何工作皆不違反 deadline)**
  - 理論上，CPU utilization 可達 100%，**但實際上不可能達 100% ，因為還要再加上 Context switching、interrupt handling 等額外付擔。**



### Soft real-time system

- **就 CPU scheduling design必須要具備：**

  - 支援 preemptive-priority 法則。
  - 不支援「Aging」技術。
  - **盡可能降低 kernel dispatch latency time，可得 read-time process 可以即早工作。**

- 降低 lermel latency 的困難處：

  - **大部分的作業桶接不允許 kernel 整在執行 system call 或其他 system processes 時被 user process 任意的插隊 (preemption)，目的是要確保 kernel data structures 的正確性(就是避免有 race condition)，但是這種做法對於 soft real-time system 極為不利。**

  - 假設目前 kernel 正在執行一個「long-time」system call ( I/O-operation ) 而此時有一個 soft real-time process 到達(或是 fork())，但是他必須到 kernel 完成此 long-time system call 後才能取得 CPU。(**Dispatch latency 太長**)。**要解決此問題原則是：必須插隊 kernal 且要保障 kernel data structure之正確性。**

    - **方法一 - Preemptive point**

    

    在 system call code 中加入一些「preemptive point」( **在此時點將 kernel 插隊是安全的** )，將來system call 執行時若遇到 preemptive point，system call 會先暫停 kernel 會檢查此時是否有 real-time process 到達。**若有，方才的 kernel system call 會暫停執行， CPU 分派給 real-time process 使用**；若無，方才的 kernel system call 繼續執行直到遇見下一個 preemptive point。

    

    Cons：system call 中可以加入的 preemption point 數目不夠多(插入點有限)，Dispatch latency 仍然很長。

    - **方法二 - kernel 可隨時被 real-time process 插隊**

    

    **需要具備有對於 kernel 的共享 data structure/resource 提供嚴謹的「互斥存取」( synchronization機制 )，以確保資料之正確性。**

    

    Cons：使用互斥存取可能造成**「優先權反轉 ( Priority inversion )」問題。**



#### Priority inversion - 優先權反轉

- **高優先權的 process 所需要的共享 data/resourses 恰巧被一些低優先全 process 所把持**，無法存取 (因為互斥存取控制)，**造成高優先權等待低優先權 process 之情況，再加上低優先權 process 往往無法很快的取得 CPU ，已完成對共享 data/resources 之使用進而釋放資源，所以高優先權 process 被迫要等一段很久的時間。**
- 解決方法：**讓低優先權 process 暫時繼承高優先權的權值**，使得低優先權 process 可以很快的 取得 CPU 完成共享 data/resource 之使用並釋放資源，**同時也立刻恢復其原本的低權值**。



### Real-time system 之 dipatch latency 的架構

由兩個 phases 組成：

- **Conflict phase**
  - **Preempts kernel**
  - **低優先權釋放高優先權之 data/resource**
- Disoatch phase
  - Context switching
  - Change mode to user mode
  - Jump




![1528637800281](\blog\images\1528637800281.png)



# Thread management

---



- Thread：又稱之為「Lightweight proces」，為作業系統分配 CPU time 之基本單位 (It's a basic unit of CPU utilization)。( Process 是分配資源如：I/O, memory，的最基本單位 )
- Thread 建立後，其私有的內容 ( 紀錄於TCB - thread control block 之中 ) 組成有：
  - **Programming counter**
  - **CPU registers value**
  - **Stack**
  - Thread ID, state ...
- **此外，同一個 process 內之不同 threads 彼此共享此 process 的：**
  - **Code section**
  - **Data section**
  - **Other OS resources** (Open files, I/O resources, siginal, ...)
  - **Code section 與 Data section 合稱為 Memory address space。**
- Tradition process (Single-thread model)


![1528638942439](\blog\images\1528638942439.png)



- Multithreading mode




![1528639073333](\blog\images\1528639073333.png)



- Pros
  - Responsiveness：當 process 內執行中的 thread 被 blocked，則 CPU 可以交給此 process 內其他可執行的 threads 執行，**故整個 process 不會被 blocked，**仍持續執行，所以若將 multithreading 用在 user-interactive application，可增加對使用者之回應程度。
  - Resource sharing：因為 process 內之多條 threads 共享此 process code section，所以在同一個 memory space 上可有多個工作同時執行。
  - Economy：**因為同一個 process 內之不同 threads 彼此共享此 process 的 memory 及其他作業系統的資源，所以 thread 之私有成份量少，故 thread 之 creation、context switching 更快、Thread 的管理成本更少。**
  - Scalability (Utilization of multiprocessors Architecture)：**可以做到同一個 Process 內之不同 threads 可在不同 CPUs 上平行執行，所以可以增加對 multiprocessors system 之效益(平行程度)提升。**



## Process VS. Thread



|                            Thread                            |                           Process                            |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                     Light weight Process                     |                     Heavy weight process                     |
|                     Multithreading model                     |                    Single-threaded model                     |
|       是作業系統分配**資源( Resource )**的最基本單位。       |          是作業系統分配 **CPU time** 的最基本單位。          |
| 不同的 Processes 不會有共享的 Memory 以及其他資源，除非在 shared memory 的情形下。 | **同一個 process 內之 threads 彼此共享此 process 之 memory 與其他資源。** |
| 若 process 內的 single thread 被 blocked，則整個 process 也被 blocked。 |  **只要 process 內尚有可執行的 threads 就不會被 blocked。**  |
|  process 之 creation、context switching 慢，管理成本*高*。   | **process 之 creation、context switching 快，管理成本低。**  |
|         對於 multiprocessors 架構之效益發揮*較差*。          |        **對於 multiprocessors 架構之效益發揮較加。**         |
| Process 較無 race condition 問題。( 除非是採用「Shared memory」溝通 ) | **因為同一個 Process 內之 Thread 彼此共享此 Process data section，所以必須對共享的資料提供互斥存取的機制，防止 Race condition。** |



- 適合使用 Multithread 開發的程式。
  - **一個時間點有多個工作要同時執行**。如：Client-server model。

- 不適合使用 Multithread 開發的程式。( 以傳統 process 開發即可 )
  - **一個時間點最多只有一個工作可執行**。如：命令解譯器 ( UNIX 的 Shell )。



## User thread and Kernel thread

Thread management 的工作 (如：thread creation、thread destory、thread suspend、thread wakeup、thread scheduling、thread context switching) 由誰負責。



### User-level Thread



Thread management 是由在 User mode 之 thread library 提供的 APIs 以讓 user process 呼叫使用、管理。**Kernel 完全不知道( be unknowed with ) user level threads 的存在。(只知道有 process 的 single thread)**所以 thread management 不須 kernel 介入干預。



- Pros
  - Thread creation、context switching ...等管理成本低，速度快。
- Cons
  - **當 Process 內某條執行中的 user-thread 是被 blocked 的，會導致整個 porcess 亦被 blocked。( 即使 process 內還是有其他可執行的 thread。)**
  - **因為無法做到 process 內之多條 user-threads 的平行執行，導致 Multprocessors 的效能發揮較差。**
- $Ex. $
  - **舉凡 Thread liberary 皆是 user threads。**
  - 如：POSIX 的 pthread library ( **是只在UNIX、Linux系統上的規格** )、Mach 的 c-thread library、Solaris 2以上的 UI thread library、Green thread library。



### Kernel-level thread



**Thread managemet 完全由 Kernel 負責，kernel 知道每一條 thread 之存在並進行為護理。**



- ***不需要 Kernel 的任何協助。( With no support from kernel. )**
- Pros
  - **當 Process 內某條執行中的 kernel-thread 是被 blocked 的，不會導致整個 porcess 亦被 blocked。( 若 process 內有其他可執行的 thread 時。)**
  - **可以管理 process 內之多條 kernel-threads 的平行執行，導致 Multprocessors 的效能發揮較差。**
- Cons
  - Thread creation、context switching ...等管理成本**較高**，速度**較慢**。
- $Ex .$
  - 大部分作業系統都支援。
  - Windows (2000、NT)
  - UNIX、Linux ...
  - Solaris
- [ Modern例題 ]：CPU time 依照分配對象數平均分配 thread，$P_a$ 有三條 threads ，$P_b$ 有兩條 threads，則 $P_a, P_b$ 各分到多少趴的 CPU time。
  - 若全部的執行序皆為 User thread。<br>**Kernel 只知道有兩個 Process ，所以分配 CPU time 給**$P_a, P_b$ **各 50%。**
  - 若全部的執行序皆為 Kernel thread。<br>**Kernel 知道有 5 條執行序要來分配 CPU time，所以** $P_a$ **分到 60 %**，$P_b$ **分到 40 %。**

### Multithreading model



- User thread to Kernel thread



#### Many to one ( User thread )




![1528718261665](\blog\images\1528718261665.png)




![1528718722383](\blog\images\1528718722383.png)



This model maps **many** user threads to one kernel thread. Thread management is done in **User space.**



- 與 User thread 一致，不同解釋法。

- Pros
  - Thread creation、context switching ...等管理成本低，速度快。
- Cons
  - **當 Process 內某條執行中的 user-thread 是被 blocked 的，會導致整個 porcess 亦被 blocked。( 即使 process 內還是有其他可執行的 thread。)**
  - **因為無法做到 process 內之多條 user-threads 的平行執行，導致 Multprocessors 的效能發揮較差。**
- $Ex. $
  - **舉凡 Thread liberary 皆是 user threads。**
  - 如：POSIX 的 pthread library ( **是只在UNIX、Linux系統上的規格** )、Mach 的 c-thread library、Solaris 2以上的 UI thread library、Green thread library。





#### One to one ( Kernel thread )




![1528718998029](\blog\images\1528718998029.png)




![1528719859725](\blog\images\1528719859725.png)



This model maps **each** user threads to **a** kernel thread.



- 與 kernal thread **不盡相同。**
- Pros
  - **當 Process 內某條執行中的 kernel-thread 是被 blocked 的，不會導致整個 porcess 亦被 blocked。( 若 process 內有其他可執行的 thread 時。)**
  - **可以管理 process 內之多條 kernel-threads 的平行執行，導致 Multprocessors 的效能發揮較差。**
- Cons
  - Thread creation、context switching ...等管理成本**較高**，速度**較慢**。
  - ***Process 每建立一條 User-thread，作業系統就必須配合產生一條 Kernel-thread 與 User-thread 搭配，所以若 User-thread 數產生眾多，則會讓作業系統負擔太大，耗資源大。**
- $Ex .$
  - 大部分作業系統都支援。
  - Windows (2000、NT)
  - UNIX、Linux ...
  - Solaris
  - OS/2




#### Many to many ( Kernel thread )




![1528719979394](\blog\images\1528719979394.png)




![1528720444327](\blog\images\1528720444327.png)




![1530181565681](\blog\images\1530181565681.png)



This model maps **many** user threads to **a small or equal number of** kernel thread.



- Pros
  - **當 Process 內某條執行中的 kernel-thread 是被 blocked 的，不會導致整個 porcess 亦被 blocked。( 若 process 內有其他可執行的 thread 時。)**
  - **可以管理 process 內之多條 kernel-threads 的平行執行，導致 Multprocessors 的效能發揮較差。**
  - **負擔不和 one-to-one model 大。**
- Cons
  - Thread creation、context switching ...等管理成本**較高**，速度**較慢**。
  - **製作、設計較為複雜。**

- $Ex .$
  - **Solaris 2 以上 ( Two-level modeling )**




![1530181715559](\blog\images\1530181715559.png)




![1530182120849](\blog\images\1530182120849.png)



## Multithreading issues



### fork() issue




![1530178012484](\blog\images\1530178012484.png)

- **Child1：適用在「子程序」工作與「父程序」相同時。**
- **Child2：適用於「子程序」與「父程序」不同時，也就是說「子程序」出生後立即呼叫「execl」system call。**

### Signal delivery issue




![1530178614262](\blog\images\1530178614262.png)



#### Signal




![1530178832898](\blog\images\1530178832898.png)



**It's used in UNIX to notify the process that a particular event has occurred.**

- 當 process 收到 siginal 通知後，他必須處理 ( **可交 process 自行處理或交付給 default signal handler  kernel 處理** )
- **Synchronous signal**
  - Divide-by-zero
  - Illegal memory access
- **Asynchronous signal**
  - 「ctrl - c」 by administrator
  - Time-out by timer



#### Signal Delivery Issue



- **Case 1 ( Ex. Synchronous signal )**




![Signalisssue](\blog\images\Signalisssue.png)



- **Case 2 ( Ex.「Ctrl-break」)**




![1530179630741](\blog\images\1530179630741.png)



- **Case 3**




![1530179857612](\blog\images\1530179857612.png)



- **Case 4 ( Set a default signal handler. Ex. Solaris )**




![1530180057880](\blog\images\1530180057880.png)



### Thread pool



在 client-server model 中，當 server 收到 client 的 request 後，server **才建立 thread 以服務此一請求，然而 thread creation 仍需耗用一些時間，所以對 client 的回應不能非常即時，以「Thread pool」解決。**Process ( server ) 事先建立一些 threads 置於「thread pool」中，當收到 client 的 request 後，就從「thread pool 」中指派一條可使用的 thread 以服務此請求，**不須重新建立 thread ，回應較為即時，**當此 thread 完成工作之後，再回到 thread pool 中待命，如果 thread pool 中沒有可用的 thread 則 client 的 request 需要等待。



- **Cons**
  - 萬一 process 事先在 thread pool 中產出過多 threads，對作業系統負擔較大，所以作業系統通常會限制 thread pool 的大小。



## Thread 程式追蹤



- pthread library



```cpp
#include <pthread.h>
#include <stdio.h>

int sum; /* this data is shared by the thread(s) */
void *runner(void *param); /* threads call this function */

int main(int argc, char *argv[]) {
    pthread_t tid; /* the thread identifier */
    pthread_attr_t attr; /* set of thread attributes */
    
    if (argc != 2) {
        fprintf(stderr,"usage: a.out <integer value>\n");
        return -1;
    }
    if (atoi(argv[1]) < 0) {
        fprintf(stderr,"%d must be >= 0\n",atoi(argv[1]));
        return -1;
    }
    
/* get the default attributes */
    pthread_attr_init(&attr);
/* create the thread. 根據 attr 屬性值建立一條 thread，ID 記錄在 tid 中，執行 runner() 副程式*/
    pthread_create(&tid, &attr, runner, argv[1]);
/* wait for the thread to exit */
    pthread_join(tid, NULL);
    
    printf("sum = %d\n",sum); // 輸出應為 15
}

/* The thread will begin control in this function */
void *runner(void *param) {
    //若為 static 變數就為全域共享變數。
    int i, upper = atoi(param);
    sum = 0;
    for (i = 1; i <= upper; i++)
    	sum += i;
    /*Thread 終止*/
    pthread_exit(0);
}
```



```cpp
#include <pthread.h>
#include <stdio.h>
#include <types.h>
int value = 0;
void *runner(void *param); /* the thread */
int main(int argc, char *argv[]) {
    pid_t pid;
    pthread_t tid;
    pthread_attr_t attr;
    pid = fork();
    if (pid == 0) { /* child process */
        pthread_attr_init(&attr);
        pthread_create(&tid,&attr,runner,NULL);
        pthread_join(tid,NULL);
        printf("CHILD: value = %d",value); /* LINE C -> 5 */
    }
    else if (pid > 0) { /* parent process */
    	wait(NULL);
    	printf("PARENT: value = %d",value); /* LINE P -> 0 */
    }
}
void *runner(void *param) {
    value = 5;
    pthread exit(0);
}
```



# 問題

---



- Which following is **shared by thread ?**
  - Static local variable **共享**
  - Program text/executable binary (code section) **共享**
  - Registers value of CPU **私有**
  - Heap memory (code + data scetion memory space) **共享**
  - Programming counter **私有**
  - Stack memory **私有**
  - Open files **共享**
  - I/O-resourses **共享**
  - Local variables **私有**
  - Global variables **共享**



# 參考

---



### [Operating Systems: Internals and Design Principles](http://dinus.ac.id/repository/docs/ajar/Operating_System.pdf)