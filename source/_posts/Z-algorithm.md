title: Z - algorithm
author: Willy Wang
date: 2018-03-19 13:27:10
tags:
---
這個演算法可以線性時間在一段**文本(text)** 裡面找到所有我們欲求的**段落(pattern)**。
今天，當我們的文本(text)的長度為 $n$ 且欲求的段落(pattern)為 $m$時 ，搜尋只需要線性長度的時間 $O(m+n)$ 即可，雖然這個演算法需要的空間(space complexity)與時間複雜度(time complexity)都與**KMP algorithm**一致，但是這個演算法比起KMP algoritjm還要*容易了解*。

首先，我們需要一個 $Z$陣列($Z$ array)

# $Z$陣列

---
當我們將欲檢索的文本存為一個字串 $str\[0\ldots n-1\]$ 時，同時也建立一個與字串一樣長的$Z$陣列。
在$Z$陣列中，每一個元素存入

# 參考
[Z algorithm - GeeksforGeeks](https://www.geeksforgeeks.org/z-algorithm-linear-time-pattern-searching-algorithm/)

# 待補充

---
http://codeforces.com/blog/entry/3107
http://sunmoon-template.blogspot.tw/2015/05/z-algorithm-linear-time-pattern.html
http://momo-funnycodes.blogspot.tw/2012/07/gusfield-algorithm.html
https://www.geeksforgeeks.org/z-algorithm-linear-time-pattern-searching-algorithm/

## KMP 字串比對演算法
http://mropengate.blogspot.tw/2016/01/leetcode-kmpimplement-strstr.html

## manacher’s-algorithm
http://xpower2888.pixnet.net/blog/post/221920327-%E5%AD%97%E7%AC%A6%E4%B8%B2%E7%9A%84%E6%9C%80%E9%95%B7%E5%9B%9E%E6%96%87%E4%B8%B2%EF%BC%9Amanacher%E2%80%99s-algorithm
http://www.csie.ntnu.edu.tw/~u91029/Palindrome.html#3
