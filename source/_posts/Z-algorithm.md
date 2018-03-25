title: Z - algorithm
author: Willy Wang
tags:
  - String
  - Substring
  - Z Algorithm
categories:
  - Algorithm
date: 2018-03-19 13:27:00
---
這個演算法可以線性時間在一段**文本(text)** 裡面找到所有我們欲求的**段落(pattern)**。
今天，當我們的文本(text)的長度為 $n$ 且欲求的段落(pattern)為 $m$時 ，搜尋只需要線性長度的時間 $O(m+n)$ 即可，雖然這個演算法需要的空間(space complexity)與時間複雜度(time complexity)都與**KMP algorithm**一致，但是這個演算法比起KMP algoritjm還要*容易了解*。

KMP algorithm：每個前綴與其後綴的次長共同前綴（最長的後綴）<br>
Z algorithm：每個後綴與母字串的最長共同前綴（單純的長度）

首先，我們需要一個 $Z$陣列($Z$ array)

# $Z$陣列

當我們將欲檢索的文本存為一個字串 $str\[0\ldots n-1\]$ 時，同時也建立一個與字串一樣長的$Z$陣列。
在$Z$陣列中，第 $i$ 元素紀錄「**最長共同前總和 (Longest Common Prefix)**的長度」，而 *LCP 的長度* 是由「從 $i$ 開始的後總和 (Postfix)」與「該文本」共同決定。
( **注意： ** $Z[0]$ **毫無意義可言，因為從第0個開始的後總和(Postfix) 必與原本的文本字串相同。** )

大致上我們可以看成如下的函式：

![Z演算法的表示法](\blog\images\Z演算法的表示法.png)

$Ex.$
```
Index            0   1   2   3   4   5   6   7   8   9  10  11 
Text             a   a   b   c   a   a   b   x   a   a   a   z
Z values             1   0   0   3   1   0   0   2   2   1   0 
```
$More$ $ex.$
```
str  = "aaaaaa"
Z[]  = {x, 5, 4, 3, 2, 1}

str = "aabaacd"
Z[] = {x, 1, 0, 2, 1, 0, 0}

str = "abababab"
Z[] = {x, 0, 6, 0, 4, 0, 2, 0}
```

## $Z$ 陣列如何幫助演算法加速?
這個演算法的想法是將段落(pattern)與文本字串(text string)連接起來，若視段落(pattern)為「P」，視文本字串(text string)為「T」，並加上一個從未在段落與文本中出現過的*字元*`「\$」再產生出如「P$T」的字串。

最後，我們再產生一個屬於「P$T」的 Z陣列，在 Z陣列之中，若該 Z值等於段落(pattern)的長度，段落出現在該處。

```
Example:
Pattern P = "aab",  Text T = "baabaa"

The concatenated string is
"a, a, b, $, b, a, a ,b ,a, a".
Z array for above concatenated string is 
{x, 1, 0, 0, 0, 3, 1, 0, 2, 1}.
                ^
Since length of pattern is 3, the value 3 in Z array 
indicates presence of pattern.
```

## 如何建立 $Z$陣列

最簡單的就是使用兩個迴圈，外層迴圈將整個「P\$T」跑過一遍，內層迴圈則是看看到底 i 位置的**後總和**與「P$T」的LCP長度為何。
$Time$ $complexity:$
$$O(n^2)$$

我們當然可以使用另一種方法讓建立陣列的時間複雜度降低。
此演算法的關鍵在於要維護一個區間$[L \ldots R]$，$R$ 的位置代表由 $L$ 處之後可以和整個字串最長的**前總和**重疊到的最後一個位置( 換句話說：$[L \ldots R]$是整個字串的**前綴子字串** )，若完全不重疊，則 $L$ 與 $R$相等。
```
Index            0   1   2   3   4   5   6   7   8   9  10  11 
Text             a   a   b   $   a   a   b   x   a   a   a   z
Z values             1   0   0   3   1   0   0   2   2   1   0
                 =========       ^L      ^R 
```
- 步驟 ($i$ 為當前位置)
1. 若 $i > R$ ，就代表當前 $i$ 沒有經過任何**「P\$S」的前綴子字串**，所以重置 $L$ 與 $R$ 的位置($L = i, R = i$)，經由比對**「P\$S」的前綴**與 **$i$ 之後的前綴**，並找出最長的子字串($R$ 的位置)，計算新的 $L$ 與 $R$ 的位置，也一併將 $Z[i]$值算出來($= R - L + 1$)。
2. 若 $i \leq R$ ，令 $K = i - L$ ，再來我們知道 $Z[i] \geq  min(Z[K], R-i+1)$ 因為$String[i \ldots]$與$String[K\ldots]$共同前$R-i+1$個字元必然為[P\$T]的**前綴子字串**。現在有兩種情形會發生：
 - case1：
   若$Z[K] < R-i+1$ ，代表沒有任何**「P\$S」的前綴子字串** 從 $i$ 位置開始(否則 $Z[K]$ 的值會更大)，所以也意味著$Z[i] = Z[K]$，還有區間$[L\ldots R]$不變。
 - case2： 
   若$Z[K] \geq R-i+1$，代表$String[i \ldots]$可以和$String[0\ldots]$ 繼續比對相同的字元，也就意味有可能拓展$[L \ldots R]$ 區間，因此，我們會設 $L = i$ ，接著從 $R$ 之後開始繼續比對**「P\$S」的前綴子字串**，最後我們會得到新的$R$，並更新$[L \ldots R]$ 區間與計算 $Z[i]$ $( = R - L + 1)$。

想要了解上述的演算法可以經由這個連結觀看[動畫](http://www.utdallas.edu/~besp/demo/John2010/z-algorithm.htm)。

---
**小視窗**


![Z演算法的子問題](\blog\images\Z演算法的子問題.png)

如果一個位置 $i$ 位於之前比過的那段 $[L, R]$ 當中，他是否跟 $Z[i − L]$ 相同呢？我們可以分成三種情形：
1. 要比的後綴根本不在以前比過的範圍$[L, R]$內     → 就去比吧！
2. 要比的後綴在以前比過的範圍$[L, R]$但長度未知 → 還是去比吧！
3. 要比的後綴在以前比過的範圍$[L, R]$但長度已知 → 直接記錄囉！

---

# 程式碼實作

---
## [台大資工PPT by nkng](https://www.csie.ntu.edu.tw/~sprout/algo2016/ppt_pdf/Z_value.pdf)
```cpp
void z_build(const char *S, int *Z) {
    Z[0] = 0;
    int bst = 0;
    for(int i = 1; S[i]; i++) {
        if(Z[bst] + bst < i) Z[i] = 0;
        else Z[i] = min(Z[bst]+bst-i, Z[i-bst]);
        while(S[Z[i]] == S[i+Z[i]]) Z[i]++;
        if(Z[i] + i > Z[bst] + bst) bst = i;
    }
}
```

## [Z algorithm - GeeksforGeeks](https://www.geeksforgeeks.org/z-algorithm-linear-time-pattern-searching-algorithm/)
```cpp
// A C++ program that implements Z algorithm for pattern searching
#include<iostream>
using namespace std;
 
void getZarr(string str, int Z[]);
 
//  prints all occurrences of pattern in text using Z algo
void search(string text, string pattern)
{
    // Create concatenated string "P$T"
    string concat = pattern + "$" + text;
    int l = concat.length();
 
    // Construct Z array
    int Z[l];
    getZarr(concat, Z);
 
    //  now looping through Z array for matching condition
    for (int i = 0; i < l; ++i)
    {
        // if Z[i] (matched region) is equal to pattern
        // length  we got the pattern
        if (Z[i] == pattern.length())
            cout << "Pattern found at index "
                 <<  i - pattern.length() -1 << endl;
    }
}
 
//  Fills Z array for given string str[]
void getZarr(string str, int Z[])
{
    int n = str.length();
    int L, R, k;
 
    // [L,R] make a window which matches with prefix of s
    L = R = 0;
    for (int i = 1; i < n; ++i)
    {
        // if i>R nothing matches so we will calculate.
        // Z[i] using naive way.
        if (i > R)
        {
            L = R = i;
 
            // R-L = 0 in starting, so it will start
            // checking from 0'th index. For example,
            // for "ababab" and i = 1, the value of R
            // remains 0 and Z[i] becomes 0. For string
            // "aaaaaa" and i = 1, Z[i] and R become 5
            while (R<n && str[R-L] == str[R])
                R++;
            Z[i] = R-L;
            R--;
        }
        else
        {
            // k = i-L so k corresponds to number which
            // matches in [L,R] interval.
            k = i-L;
 
            // if Z[k] is less than remaining interval
            // then Z[i] will be equal to Z[k].
            // For example, str = "ababab", i = 3, R = 5
            // and L = 2
            if (Z[k] < R-i+1)
                 Z[i] = Z[k];
 
            // For example str = "aaaaaa" and i = 2, R is 5,
            // L is 0
            else
            {
                //  else start from R  and check manually
                L = i;
                while (R<n && str[R-L] == str[R])
                    R++;
                Z[i] = R-L;
                R--;
            }
        }
    }
}
 
// Driver program
int main()
{
    string text = "GEEKS FOR GEEKS";
    string pattern = "GEEK";
    search(text, pattern);
    return 0;
}
```

## [建國中學 2012 年資訊能力競賽培訓講義 - 08](http://pisces.ck.tp.edu.tw/~peng/index.php?action=showfile&file=fab7c1879e544bcefffb4b8717f2747436e1c425c)
```cpp
void Z_maker( int z[], char s[], int n ){
    z[0] = n;
    int L = 0, R = 0, i, x;
    for( i = 1 ; i < n ; i++ ){
        if( R < i || z[i-L] >= R-i+1 ){
            R < i ? x = i : x = R+1;
            while( x < n && s[x] == s[x-i] ) x++;
            z[i] = x-i; if( i < x ){ L = i; R = x-1; }
        }
        else z[i] = z[i-L];
    }
}
```

## [Z algorithm - codeforces](http://codeforces.com/blog/entry/3107)

```cpp
int L = 0, R = 0;
for (int i = 1; i < n; i++) {
  if (i > R) {
    L = R = i;
    while (R < n && s[R-L] == s[R]) R++;
    z[i] = R-L; R--;
  } else {
    int k = i-L;
    if (z[k] < R-i+1) z[i] = z[k];
    else {
      L = i;
      while (R < n && s[R-L] == s[R]) R++;
      z[i] = R-L; R--;
    }
  }
}
```

## [Z algorithm1 - 日月卦長的模板庫](http://sunmoon-template.blogspot.tw/2015/05/z-algorithm-linear-time-pattern.html)
```cpp
inline void z_alg1(char *s,int len,int *z){
	int l=0,r=0;
	z[0]=len;
	for(int i=1;i<len;++i){
		z[i]=r>i?min(r-i+1,z[z[l]-(r-i+1)]):0;
		while(i+z[i]<len&&s[z[i]]==s[i+z[i]])++z[i];
		if(i+z[i]-1>r)r=i+z[i]-1,l=i;
	}
}
```

## [Z algorithm2 - 日月卦長的模板庫](http://sunmoon-template.blogspot.tw/2015/05/z-algorithm-linear-time-pattern.html)

```cpp
inline void z_alg2(char *s,int len,int *z){
	int l=0,r=0;
	z[0]=len;
	for(int i=1;i<len;++i){
		z[i]=i>r?0:(i-l+z[i-l]<z[l]?z[i-l]:r-i+1);
		while(i+z[i]<len&&s[i+z[i]]==s[z[i]])++z[i];
		if(i+z[i]-1>r)r=i+z[i]-1,l=i;
	}
}
```

## [培訓-4 字串- tioj](https://tioj.infor.org/uploads/attachment/11/43/4.pdf)
```cpp
void z_build(const char* S,int *z){
    z[0]=0;
    int bst=0;
    for(int i=1;S[i];i++){
        if(z[bst]+bst<i) z[i]=0;
        else z[i]=std::min(z[bst]+bst−i,z[i−bst]);
        while(S[z[i]]==S[i+z[i]]) z[i]++;
        if(z[i]+i>z[bst]+bst) bst=i;
    }
}
```

# 例題

---
[TIOJ 1725_Z algorithm_Massacre at Camp Happy](http://codingbeans.blogspot.tw/2016/03/tioj-1725z-algorithm-massacre-at-camp.html)

# 參考

---
[Z algorithm - GeeksforGeeks](https://www.geeksforgeeks.org/z-algorithm-linear-time-pattern-searching-algorithm/)

[建國中學 2012 年資訊能力競賽培訓講義 - 08](http://pisces.ck.tp.edu.tw/~peng/index.php?action=showfile&file=fab7c1879e544bcefffb4b8717f2747436e1c425c)

[培訓-4 字串- tioj](https://tioj.infor.org/uploads/attachment/11/43/4.pdf)

[台大資工講義 by nkng](https://www.csie.ntu.edu.tw/~sprout/algo2016/ppt_pdf/Z_value.pdf)

[Z algorithm - codeforces](http://codeforces.com/blog/entry/3107)

[Gusfield algorithm - momo funny codes](http://momo-funnycodes.blogspot.tw/2012/07/gusfield-algorithm.html)

[Z algorithm - 日月卦長的模板庫](http://sunmoon-template.blogspot.tw/2015/05/z-algorithm-linear-time-pattern.html)

# 待補充

---

## KMP 字串比對演算法
http://mropengate.blogspot.tw/2016/01/leetcode-kmpimplement-strstr.html