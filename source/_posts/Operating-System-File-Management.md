title: Operating System - File Management
author: Willy Wang (willywangkaa)
tags:
  - File Management
categories:
  - Operating System
date: 2018-09-10 18:14:00
---
# File management



## File open、File close



作業系統對檔案進行任何運作之前，皆必須到磁碟的「Physical directory」找出檔案的配置資訊。



- 問題
  - 因為檔案數目太龐大，**所以搜尋的時間長**
  - **磁碟 I/O time(次數)很多，非常耗時**



### File open



當檔案第一次被使用時，作業系統必須到磁碟的「Physical directory」找出檔案的配置資訊，**接著將此資訊複製到作業系統的記憶體空間之中的一個表格稱為「Open file table」。**將來對這個檔案進行任何操作之前，**作業系統只需要到此表格搜尋取得檔案的配置資訊即可。**由於「Open file table」中的檔案很少 ( E.g., 20 個)，**搜尋的時間可以大幅降低，**此表格位於**記憶體中，所以可以節省可觀的 I/O time (次數)。**



![fileopen](\willywangkaa\images\fileopen.png)



- 因為檔案可以被多個 Process 共用，「Open file table 」分為：
  - **System-open file table**
    - 保存檔案共通配置的資訊
      - 檔案名稱
      - 配置區塊
      - 檔案大小
  - **Process-open file table**
    - **Process 存取檔案時，會有不同的資訊要保存**<br>**( E.g. 檔案目前讀取的指標位置、對檔案的存取權利 )**



![openfiletable](\willywangkaa\images\openfiletable.png)


### File close



當檔案不再使用時，**作業系統會將「Open file table」中此檔案的配置資訊更新回磁碟的「Physical directory」，且自表格中刪除此檔案的配置資訊。**



## Consistency semantic - 一致性語意



> 檔案可以被多個 Process / User 共享，而共享的模式 ( Model ) 有那幾種？



### UNIX semantic



- Ex
  - **訂票系統的「座次表」檔案**
    - **需要具備「互斥存取」**
    - **全部單位讀取檔案必須是「一致的」**
    - **某個 Process 對檔案作的任何改變其它 Process 會知道**



### Session semantic



- Ex
  - **「空白報名表」檔案供人下載填寫**
    - **不需具備「互斥存取」**：大家是在各自的「副本」上進行讀寫，讀寫不受限制。
    - **全部單位看到的內容「不一定一致」**



### Immutable (不可改變) semantic



- Ex
  - **「總經理公告文件」檔案**
    - **唯讀；不可更改**
    - **檔名不得重複**





## File protection



### Physical protection



防止因為「磁碟毀壞」，所造成的「資料遺失」。



- **Backup only**



### Logical protection



**防止非法使用者對檔案之不當存取**。



- Name protection
- Password protection
- Access list
- Access group



#### Access group


![accessgroup](\willywangkaa\images\accessgroup.png)

- **User**
  - **Owner**
  - **Group ( member )**
  - **Others ( universal )**
- **Access right**
  - **R：Read**
  - **W：Write**
  - **X：Execute**

