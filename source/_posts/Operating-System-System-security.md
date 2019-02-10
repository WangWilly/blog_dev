title: Operating System - System security
author: Willy Wang (willywangkaa)
tags:
  - Security
categories:
  - Operating System
date: 2019-02-02 18:21:00
---
# System security

- 提供「安全功能」與「安全服務」
- 針對各種常用作業系統，進行相關配置，以正確對付、防禦各種入侵
- 保證網路作業系統提供的網路服務得到安全配置



## 病毒（Virus）

藉由用存取控制（Access control）的機制來保護資訊的安全，防止非法使用者的入侵與蓄意破壞

- 特徵
  - A fragment of code embedded in a legitimate program
  - 依附在別的程式上
  - 會自行複製、感染、傳播、發作
  - 「UNIX」與「Multiuser」作業系統不易受到病毒影響，因為「可執行檔」的寫入被系統保護



1. 啟動磁區病毒

   - 早期的病毒類型，感染開機磁片中「Boot」程式或硬碟分割表以取得控制權限

2. 檔案型病毒

   - 寄生於執行檔後，會再自主感染其他執行檔
     - 「常駐型」：會常駐在記憶體中，當其他程式執行時會進一步執行感染動作
     - 「非常駐型」：感染磁碟機中找出尚未被感染的程式加以感染

3. 巨集病毒

   - 裡用軟體本身提供的巨集功能設計出的病毒
     - Microsoft word 的 VBA 巨集

4. 電腦蠕蟲

   - A process that use spawn mechanism（孕育機制） to duplicate itself

   - 不須依附在別的程式內
   - 可能不用使用者操作也會自我複製執行
   - 未必破壞被感染的系統
   - 執行垃圾程式碼以發動「分散式阻斷服務攻擊」（DDos）
     - 採用垃圾郵件、漏洞傳播，如：**莫里斯蠕蟲**

5. 特洛伊木馬

   - A code segment that misuses its environment

   - 病毒依附在一般程式碼
   - 滿足特定條件後便將使用者資料傳出，未達條件不進行任何動作
   - 「Spyware」：常伴隨著商業軟體，施行網頁綁架與廣告騷擾

6. Autorun病毒

   - 利用 USB 當作媒介，會自動執行程式侵入電腦

7. 千面人病毒

   - 每繁殖一次會以不同病毒碼傳染到別處，每一個中毒的檔案所含的病毒程式碼皆不同



## 攻擊手段



1. Dumpster diving
   - 攻擊者翻找目標系統文件、檔案、垃圾資料與殘存資訊，試圖找到密碼與其他有用的資訊
2. Trap door
   - The designer of a program or system might leave a hole in the software that only is capable of use
   - 隱藏在程式中的秘密功能
3. Logic bomb
   - A program that initiates a security incident only under certain circumstances
   - 難以查出問題所在
   - 正常情況不發生，當發生特定條件時會發生錯誤
4. Stack overflow / Buffer overflow
   - Attacker sends more data than the program expected
   - 溢位（Overflow）後將原本「Stack」中的「Return address」改成惡意程式的位置
   - 避免方法
     - Use static code analysis to look for memory writes that go beyond array boundaries
     - Randomize the base address of program libraries/stack in the memory
     - 使用安全的函數（strcpy()→strncpy()）
     - 使用高階程式語言
       - Java、Python
5. Port scanning
   - Is not a attacker but rather a means for a cracker to detect a system's vulnerabilities to attacker
   - 首先搜尋**目標網路**上所有可以接觸到的主機，然後查出主機上開啟了哪些通訊埠，並且進一步偵測出目標主機上所執行的作業系統類型及版本（OS fingerprinting），最後刺探出目標主機上正在執行或待命的網路服務與目標主機的 IP 位址之間的關係
   - Port scanning （通訊埠端口掃描）
     - 駭客**會傳送一連串的特殊訊息去測試此主機上有執行哪些網路服務**，如 HTTP 使用 Port 80，透過「Port Scanning」**得知哪些通訊埠是開啟的**，讓駭客知道主機上目前所執行的網路服務類型
   - Network Scanning（網路架構掃描）
     - 若能得知目標網路內有哪些主機是可以接觸到、啟動（active）的，**能夠縮小攻擊範圍並知道哪些主機可以成為被攻擊目標**
   - Vulnerability Scanning（弱點掃描）
     - 偵測及發現目標網路內被掃描的**主機上所存在的弱點與漏洞**，以便未來進行入侵攻擊
6. Denial of service（Dos）
   - Dos attacks are aimed not at gaining information or stealing resources but rather at disrupting legitimate use of  a system or facility
   - DDos
     - 利用多台殭屍系統（Zombies）**同時對目標伺服器發出大量請求以癱瘓服務**



# Symmetric encryption and asymmetric encryption



## Symmetric encryption

Same key is used to encrypt and to decrypt



### Data encrypt standard（DES）



- **64 bit block cypher**
  - 每次加密操作只處理 64-bit 資料，稱為「Block」
- **56 bit key length**



**Triple DES**

- C：Cypher；密文
- M：Plaintext；明文
- 使用「金鑰包」包含三個 DES 金鑰 $K_1、K_2、K_3$ 均為 56-bits
  - 加密：使用 $K_1$ 的金鑰進行 DES 加密，在用 $K_2$ 的金鑰**進行 DES「解密」**，最後使用 $K_3$ 進行 DES 加密，『$C = E_{K_3}(D_{K_2}(E_{K_1}(M)))$』
  -  解密：使用 $K_3$ 的金鑰進行 DES 解密，在用 $K_2$ 的金鑰**進行 DES「加密」**，最後使用 $K_1$ 進行 DES 解密，『$M = D_{K_1}(E_{K_2}(D_{K_1}(C)))$』

- 金鑰選項（$K_1、K_2、K_3$）
  - 金鑰選項1：三個金鑰獨立
    - 強度最高，擁有 3×56 = 168 個獨立金鑰位元（Bit）
  - **金鑰選項2**：$K_1、K_2$ 獨立、$K_3 = K_1$
    - 安全性稍低，**擁有 2×56 = 112 個獨立金鑰位元（Bit），但可以防禦「[中途相遇攻擊](https://zh.wikipedia.org/wiki/%E4%B8%AD%E9%80%94%E7%9B%B8%E9%81%87%E6%94%BB%E6%93%8A)」**
  - 金鑰選項3：$K_1 = K_2 = K_3$
    - 等同於普通 DES（因為會互相抵銷），擁有 56 個獨立金鑰位元（Bit）



### Advanced encryption standard（AES）

在[密碼學](https://zh.wikipedia.org/wiki/%E5%AF%86%E7%A0%81%E5%AD%A6)中又稱「Rijndael加密法」，是[美國聯邦政府](https://zh.wikipedia.org/wiki/%E7%BE%8E%E5%9B%BD%E8%81%94%E9%82%A6%E6%94%BF%E5%BA%9C)採用的一種[區段加密](https://zh.wikipedia.org/wiki/%E5%8D%80%E5%A1%8A%E5%8A%A0%E5%AF%86)標準，用來替代原先的[DES](https://zh.wikipedia.org/wiki/DES)

> 嚴格地說，AES 和 Rijndael 加密法並不完全一樣（雖然在實際應用中兩者可以互換），因為Rijndael加密法可以支援更大範圍的區段和金鑰長度
>
> - **AES 的區段（Block cypher）長度固定為128-bit**
> - **金鑰長度（Key length）則可以是 128、192、256-bit**
>
> 而Rijndael使用的金鑰和區段長度均可以是128、192、256-bit
>
> 加密過程中使用的金鑰是由「[Rijndael金鑰生成法](https://zh.wikipedia.org/wiki/Rijndael%E5%AF%86%E9%92%A5%E7%94%9F%E6%88%90%E6%96%B9%E6%A1%88)」產生

**RSA 比 DES 和其它對稱演算法要慢得多**；實際上「傳送方」會使用**一種對稱演算法來加密他的資訊**，然後用 RSA 來加密他的比較短的對稱密碼，然後將用 RSA 加密的對稱密碼和用對稱演算法加密的訊息送給「接收方」

- C：Cypher；密文
- M：Plaintext；明文

- $K_{as}$：AES 金鑰
- $K_{bs}、K_{bp}$：RSA 私鑰與公鑰（**接收方**）
- 加密
  - 利用「AES 金鑰」對明文（M）加密
  - 再使用「RSA 公鑰」對「AES 金鑰」加密
  - 將上述兩者一併傳給接收方
  - $\Rightarrow E_{K_{as}}(M)｜E_{K_{bp}}(K_{as})$
- 解密
  - 用「RSA 私鑰」解密出「AES 金鑰」
  - 再使用「AES 金鑰」解密出明文
  - 爾後雙方傳遞資料只需使用「AES 金鑰」加密明文即可
  - $\Rightarrow D_{K_{bs}}(E_{K_{bp}}(K_{as})) = K_{as} \\ D_{K_{as}}(E_{K_{as}}(M)) = M$



### Rivest cypher 4（RC4）

是密鑰長度可變的一種「[串流流加密](https://zh.wikipedia.org/wiki/%E6%B5%81%E5%8A%A0%E5%AF%86)算法」，也屬於[對稱加密算法](https://zh.wikipedia.org/wiki/%E5%AF%B9%E7%A7%B0%E5%8A%A0%E5%AF%86)，RC4 是[有線等效加密](https://zh.wikipedia.org/wiki/%E6%9C%89%E7%B7%9A%E7%AD%89%E6%95%88%E5%8A%A0%E5%AF%86)（WEP）中採用的加密算法，也曾經是「[Transport layer security（TLS）](https://zh.wikipedia.org/wiki/%E4%BC%A0%E8%BE%93%E5%B1%82%E5%AE%89%E5%85%A8%E5%8D%8F%E8%AE%AE)」可採用的算法之一

- RC4 由[偽隨機數](https://zh.wikipedia.org/wiki/%E4%BC%AA%E9%9A%8F%E6%9C%BA%E6%95%B0)生成器（Pseudo-random bit generator）和異或運算（XOR）組成

- RC4的密鑰長度可變
  - 40 ~ 2048 bit
- RC4一個字節一個字節（Byte）地加解密
- 給定一個密鑰，偽隨機數生成器接受密鑰並產生一個「[S盒](https://zh.wikipedia.org/wiki/S%E7%9B%92)」
  - S盒用來加密數據，而且在加密過程中S盒會變化
- 快速但是安全性低



## Asymmetric encryption



### Rivest–Shamir–Adleman（RSA）



1. 「接收方」生成一對公鑰、私鑰
   1. 找出兩個極大質數 p、q
      - **計算 N = p×q**
      - 同時算得 $r = \phi (N) = (p-1)\times(q-1)$
   2. **求出整數 e**其滿足 `gcd(e,r) = 1` 且 e < r
   3. **求出整數 d**其滿足 `e﹡d ≡ 1 (mod r) `
      - $e\cdot d-1 = k\cdot r, k \in Z$
   4. 最後得到公鑰：(N, e)、私鑰：(N, d)
2. 「接收方」公開其公鑰給欲傳遞訊息的「傳遞方」
3. 「傳遞方」使用公鑰將明文加密
   1. 將明文（M）轉換成一整數（m）
      - f：M→m
   2. $C \equiv m^e (\mod N)$，C 為密文
4. 「傳遞方」將密文傳送給「接收方」使用**私鑰將其密文解密**
   1. $m \equiv C^d (\mod N)$
   2. 將整數（m）轉換明文（M）
      - f'：m→M

> - 由尤拉公式可以得知
>   - 模反元素必存在
>     - $a\times?\equiv1(\mod n)\\ \Rightarrow a\times a^{\phi(n)-1} = a^{\phi(n)} \equiv 1(\mod n)$
> - 私鑰解密證明（$C^d \equiv M(\mod n)$）
>   - 根據加密原則 $M^e \equiv C (\mod n)$
>     - 則 $C = M^e - kn, k \in Z $
>   - 將 C 帶入解密規則
>     -  $(M^e - kn)^d \equiv M(\mod n), k \in Z \\ \Rightarrow (M^e)^d \equiv M(\mod n)$
>     - 因為 $e \cdot d \equiv 1(\mod \phi(n))$ ，則 $e\cdot d = h\phi(n)+1, h\in Z$
>     - 則 $M^{h\phi(n)+1} \equiv M (\mod n)$



> **Authentication**
>
> Constraining the set of potential sender of a message and is also useful for proving that a message has not been modified
>
> **Message authentication code（MAC）**
>
> 特定演算法後產生的一小段資訊附加在原始訊息後傳送，以確認「某段訊息的完整性」與「傳送者的身分驗證」
>
> ![MAC](\willywangkaa\images\MAC.png)
>
>
>
> - 資訊鑑別碼（MAC）不能提供對資訊的保密，若要同時實現保密認證，同時需要對資訊進行加密
>
>
>
> **以 RSA 為一個訊息署名**
>
> 1. 「傳送者」為**原始訊息計算一個雜湊值（Message digest）**
>    - 用**「傳送者」的私鑰「加密」這個雜湊值即為一個「署名文」**
>    - 將原始訊息與「署名文」一併傳給「接收方」
>    - 這個訊息只有用「傳送者」的公鑰才能解密
> 2. 「接收者」獲得「署名文」後使用「傳送者」的公鑰「解密」得到雜湊值
>    - **將雜湊值與「接受者」自己為這個訊息計算的雜湊值比較**
>      - 假如兩者相符，則此訊息在傳播路徑上沒有被篡改過



#### 第三方攻擊

和其它加密過程一樣，對RSA來說分配公鑰的過程是非常重要的，分配公鑰的過程必須能夠抵擋中間人攻擊

1. 「竊聽者」交給 A 一個公鑰使 A 相信這是 B 的公鑰，則「竊聽者」可以截下 A 傳遞給 B 的資訊
2. 「竊聽者」亦將自己的公鑰傳給 B 使 B 以為這是 A 的公鑰，則「竊聽者」可以將所有 B 遞移給 A 的訊息截下來，將這個訊息用自己的私鑰解密，然後將這個訊息再用 A 的公鑰加密後傳給 A

> 理論上 A 和 B 都不會發現訊息被竊聽，所以今天一般用[可靠的第三方機構簽發憑證](https://zh.wikipedia.org/wiki/%E5%85%AC%E9%96%8B%E9%87%91%E9%91%B0%E5%9F%BA%E7%A4%8E%E5%BB%BA%E8%A8%AD)來防止這樣的攻擊