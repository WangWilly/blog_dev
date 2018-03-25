title: Longest Palindromic Substring (Manacher’s Algorithm)
author: Willy Wang
tags:
  - String
  - Palindromic
  - Z Algorithm
  - Substring
  - Manacher's Algorithm
categories:
  - Algorithm
date: 2018-03-25 19:41:00
---
Manacher's Algorithm 使用了[Gusfield's Algorithm(Z algorithm)](https://wangwilly.github.io/blog/2018/03/19/Z-algorithm/#more)的概念，使得時間複雜度為 O(N) 。<br>

# $Z$ 陣列

---
 $Z[i]$ 是指以 $String[i]$ 為中心的最長迴文，從中心到外端的長度，也就是說 $$String[i \ldots i-Z(i)+1] = String[i \ldots i+Z(i)-1]$$ 呈鏡面對稱。

# 步驟

---
## Step1.

 而這種方式無法記錄偶數長度的迴文。解決辦法是：在開頭和結尾與每個字元中間，同時插入一個沒出現過的字元( # )，如此就只剩下奇數長度的迴文了，而且也能記錄原先偶數長度的迴文。

```
  |                    10  12  14  16
  | 0 1 2 3 4 5 6 7 8 9  11  13  15
--+-----------------------------------
s | a b a a b a a b
s'| # a # b # a # a # b # a # a # b #
Z | 1 2 1 4 1 2 7 2 1 8 1 2 5 2 1 2 1 (Step2之後會講解如何計算)
```

## Step 2
宣告一個陣列 $Z[n]$ ： 其中 $n$為 $String$ 的長度。$Z[i]$ 表示以 $i$ 為中心的最長回文串長度。續上例：

```
Z(0)=1：.，由中心可以左右延伸長度1。
Z(1)=2：.a.，由中心可以左右延伸長度2。
Z(2)=1：.，由中心可以左右延伸長度1。
Z(3)=4：.a.b.a.，由中心可以左右延伸長度4。
```
宣告變量 $Cur$ ：表示當前回文串最長的中心的下標（current position），初值為0。<br>
宣告變量 $Right$ ： 表示以 $Cur$ 為中心的回文串最右一個字符所處位置。如下：


![Palindromic Substring_1](\blog\images\Palindromic Substring_1.png)


## Step 3

開始計算 $Z[i]$ ，是運用*已經算好*的 $Z[j]$ ， $j < i$ 。也就是指某一段已經算好的 $$String[j  \ldots  j-z(j)+1] = String[j  \ldots  j+z(j)-1]$$ 。首先找出有覆蓋到 $String[i]$ 的 $String[j  \ldots  j+Z[j-1]]$ 是哪一段，而且 $j+Z[j]-1$ 越右邊越好。<br>
所以我們使用剛剛所宣告的 $Cur$ 與 $Right$ 紀錄目前能跨越最右邊的中心點在哪。


$String ：$![Palindromic Substring_2](\blog\images\Palindromic Substring_2.png)

再來會分成兩種情況：

### Case1
如果 $Right$ 不能覆蓋 $String[i]$ ，表示已經算好的部份都派不上用場。從 $String[i+1]$ 與 $String[i-1]$ 開始比對，逐字比下去。

$String ：$
![upload successful](\blog\images\Palindromic Substring_3.png)

### Case2
如果 $Right$ 能覆蓋 $String[i]$，表示 $String[i]$ 也會出現在 $String[j  \ldots  j-Z[j]+1]$ 之中，把 $i$ 鏡射到對應的位置 $i'$ (出現在 $String[j  \ldots  j-Z[j]+1]$的位置)。接著運用 $Z[i']$ ，也就是指 $String[i'  \ldots i'-Z[i']+1] = String[i'  \ldots  i'+Z(i')-1]$ ，再看看是否要繼續比對字串，會分成三種子情況：

#### Subcase1

若 $String[i  \ldots  i+z(i')-1]$ 短少於 $Right$，那就可以直接賦予 $Z[i]$ 與 $Z(i')$ 一樣的值。


![Palindromic Substring_4](\blog\images\Palindromic Substring_4.png)


#### Subcase2

若 $String[i  \ldots  i+Z[i'] -1]$ 剛好貼齊於 $Right$ ，那就必須檢查**未確定**的部分，直接從 $String[i+Z[i']]$ 與 $String[i-Z[i']]$ 繼續比對，逐字比下去。


![upload successful](\blog\images\Palindromic Substring_5.png)


#### Subcase3

若 $String[i  \ldots  i+Z[i']-1]$ 突出了 $Right$，根據 $Z[j]$ 可知 $String[j-Z[j]]$ 與 $String[j+Z[j]]$ 一定是不同字元，根據 $Z[i']$ 可知 $String[j-Z[j]]$ 與其鏡射位置是相同字元。對於 $i$ 來說， $String[j+Z[j]]$ 與其鏡射位置就會是不同字元，不可能形成更長的迴文，因此可以直接算出 $Z[i]$ 的值，就是 $j+z[j]-i$ 。


![Palindromic Substring_6](\blog\images\Palindromic Substring_6.png)<br>

![Palindromic Substring_7](\blog\images\Palindromic Substring_7.png)<br>

![Palindromic Substring_8](\blog\images\Palindromic Substring_8.png)<br>

# 範例程式

## [Palindrome - 演算法筆記_1](http://www.csie.ntnu.edu.tw/~u91029/Palindrome.html#3)

```cpp
char t[1001];           // 原字串
char s[1001 * 2];       // 穿插特殊字元之後的t
int z[1001 * 2], L, R;  // 源自Gusfield's Algorithm
 
// 由a往左、由b往右，對稱地作字元比對。
int extend(int a, int b)
{
    int i = 0;
    while (a-i>=0 && b+i<N && s[a-i] == s[b+i]) i++;
    return i;
}
 
void longest_palindromic_substring()
{
    int N = strlen(t);
 
    // t穿插特殊字元，存放到s。
    // （實際上不會這麼做，都是細算索引值。）
    memset(s, '.', N*2+1);
    for (int i=0; i<N; ++i) s[i*2+1] = t[i];
 
    N = N*2+1;
//  s[N] = '\0';    // 可做可不做
 
    // Manacher's Algorithm
    z[0] = 1;
    L = R = 0;
    for (int i=1; i<N; ++i)
    {
        int ii = L - (i - L);   // i的映射位置
        int n = R + 1 - i;
 
        if (i > R)
        {
            z[i] = extend(i, i);
            L = i;
            R = i + z[i] - 1;
        }
        else if (z[ii] == n)
        {
            z[i] = n + extend(i-n, i+n);
            L = i;
            R = i + z[i] - 1;
        }
        else
        {
            z[i] = min(z[ii], n);
        }
    }
 
    // 尋找最長迴文子字串的長度。
    int n = 0, p = 0;
    for (int i=0; i<N; ++i)
        if (z[i] > n)
            n = z[p = i];
 
    // 記得去掉特殊字元。
    cout << "最長迴文子字串的長度是" << (n-1) / 2;
 
    // 印出最長迴文子字串，記得別印特殊字元。
    for (int i=p-z[p]+1; i<=p+z[p]-1; ++i)
        if (i & 1)
            cout << s[i];
}
```

## [Palindrome - 演算法筆記_2](http://www.csie.ntnu.edu.tw/~u91029/Palindrome.html#3)

```cpp
char t[1001];
char s[1001 * 2];
int z[1001 * 2];
 
void longest_palindromic_substring()
{
    // t穿插特殊字元，存放到s。
    int n = strlen(t);
    int N = n * 2 + 1;
    memset(s, '.', N);
    for (int i=0; i<n; ++i) s[i*2+1] = t[i];
//  s[N] = '\0';
 
//  z[0] = 1;   // 無須使用，無須計算。
 
    int L = 0, R = 0;
    for (int i=1; i<N; ++i) // 從z[1]開始
    {
        z[i] = (R > i) ? min(z[2*L-i], R-i) : 1;
        while (i-z[i] >= 0 && i+z[i] < N &&
               s[i-z[i]] == s[i+z[i]]) z[i]++;
        if (i+z[i] > R) L = i, R = i+z[i];
    }
 
    // 尋找最長迴文子字串的長度
    int n = 0, p = 0;
    for (int i=1; i<N; ++i) // 從z[1]開始
        if (z[i] > n)
            n = z[p = i];
 
    cout << "最長迴文子字串的長度是" << (n-1) / 2;
}
```

# 相關題目

---
[Timus-1297](http://acm.timus.ru/problem.aspx?space=1&num=1297)<br>
[LeetCode](https://leetcode.com/problems/longest-palindromic-substring/description/)

# 參考

---

[Manacher’s Algorithm - Simon的網路人工智慧實驗室](http://xpower2888.pixnet.net/blog/post/221920327-%E5%AD%97%E7%AC%A6%E4%B8%B2%E7%9A%84%E6%9C%80%E9%95%B7%E5%9B%9E%E6%96%87%E4%B8%B2%EF%BC%9Amanacher%E2%80%99s-algorithm)

[Palindrome - 演算法筆記](http://www.csie.ntnu.edu.tw/~u91029/Palindrome.html#3)