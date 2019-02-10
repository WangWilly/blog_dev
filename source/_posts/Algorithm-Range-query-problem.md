title: Algorithm - Range query problem
author: Willy Wang (willywangkaa)
tags:
  - Sequence
  - Sparse table
categories:
  - Algorithm
date: 2018-10-03 20:42:00
---
# Segement tree



用來存放紀錄在特定區間內 ( segement, interval ) 的資訊。

- Pros
  - 可以動態的更新欲求數組之元素。
  - 數組區間的查詢
    - 區間求和值
    - 區間最大值
    - 區間最小值
    - 區間**異或值 ( Exclusive or；XOR )**
- Cons
  - 無法刪除數值



> 線段樹無法新增節點，只能更新節點的數值並保持區間的最大最小值仍保持正確。



## 實現



- 特性
  - 完全二元樹
  - 節點保存**特定區間的訊息**
  - 採取 Buttom-up 的方式建構，從每一個葉節點建構

> 
![STree_1](\willywangkaa\images\STree_1.png)
>
>
>
> - 上面的分段樹根節點保存 0 ~ 7 的資訊，2 號節點保存 0 ~ 3 的資訊，以此類推。



### 初始化



在建構的過程中，內部節點之建構會使用兩個子節點的資訊，而建構的方式以處理的問題而作法不同 ( 最大、最小、和、XOR )。



- Segement create ( 求區間最小值 )
  - ST：線段樹
  - A：欲判斷之數組資料
  - 線段樹因為為**完全二元樹**所以通常以陣列製作
    - 陣列大小需求：$2 \times 2^{\lfloor\lg N\rfloor + 1}$，N 為數組大小 (見下圖)


![STree_2](\willywangkaa\images\STree_2.png)

- 程式碼 ( c++ )


```cpp
typedef vector<int> vi;

// ST:segement tree; A: target number array
void st_create(vi &ST, const vi &A) {
	int len = 2<<(sizeof(unsigned int) - __buildin_clz((unsigned int)A.size()));
	ST.assign(len, 0); // 線段樹歸零
	st_build(ST, A, 1, 0, (int) A.size()-1);
}
                    
void st_build(vi &ST, const vi &A, int vetex, int L, int R) {     
	if (L==R)
        ST[vertex] = L;
	else {
		int nL =  vertex << 1;
		int nR = (vertex << 1) + 1; 

		st_build(ST, A, nL, L           , ((L+R)>>1));
		st_build(ST, A, nR, ((L+R)>>1)+1, R         ); 
			
		int lContent=ST[nL], rContent=ST[nR];
		int lValue=A[lContent], rValue=A[rContent];
		ST[vertex]=(lValue <= rValue)? lContent : rContent;
	}
}
```



- 時間複雜度
  - $\Theta(\log n)$



### 更新



與建立線段樹的方法相同，將位於數組 `i` 的數字更新後，即從此葉節點向上執行更新至根節點。



```cpp
// 以數值 v 更新 A[p]
// x: 數根
// L: 數組左端
// R: 數組右端
int update (vi &ST, vi &A, int x, int L, int R, int p, int v) {  
	int mid=L+(R-L)/2;

	if (L == R)
		A[x] = v;
	else {     
		if (p <= mid)
			update(ST, A, x*2  , L    , mid, p, v); // 更新左子樹
		else
			update(ST, A, x*2+1, mid+1, R  , p, v); // 更新右子樹
	}

	ST[x] = (A[ST[x*2]]<=A[ST[x*2+1]])? ST[x*2]:ST[x*2+1]; // 以左右子樹的資訊更新母節點
}
```



### 查詢



分為三種情形討論，若**當前節點所代表的區間**

- **完全位於**欲求取之區間之**外**
- **完全位於**欲求取之區間之**內**
- **部分位於**欲求取之區間



```cpp
int query (vi &ST, const vi &A, int x, int L, int R, int ql, int qr) {  
	
	int mid = (L+R)/2, ans = -1;
	
	// 當前節點完全位於欲求取之區間之內
	if (L>=ql && R<=qr)
		// 取出該區間值最小的位址
		return ST[x];
	
    // 當前節點部分或無位於欲求取之區間之內
	if (ql<=mid) {
		// 取出左區間
		ans = query(ST, A, x*2  , L    , mid, ql, qr);
	}
	if (qr>mid) {
		// 取出右區間以及比較
		int tmp = 
		      query(ST, A, x*2+1, mid+1, R  , ql, qr);
		
		if (ans == -1)
			ans = tmp;
		else
			ans = (A[ans] < A[tmp])? ans:tmp;
	}
	return ans;
}  
```



## 其它實現



[Efficient Segment Tree Tutorial](https://www.youtube.com/watch?v=Oq2E2yGadnU)



```cpp
#include <vector>
#include <algorithm>
#include <limits>
#include <iostream>

class SegmentTree {
public:
	SegmentTree(int count) {
		n = count;
		data = std::vector<int>(2 * n, 0);
	}

	SegmentTree(std::vector<int> const &values) {
		n = values.size();
		data = std::vector<int>(2 * n);
		std::copy(values.begin(), values.end(), &data[0] + n);
		for (int idx = n - 1; idx > 0; idx--)
			data[idx] = std::min(data[idx * 2], data[idx * 2 + 1]);
	}

	void update(int idx, int value) {
		idx += n;
		data[idx] = value;

		while (idx > 1) {
			idx /= 2;
			data[idx] = std::min(data[2 * idx], data[2 * idx + 1]);
		}
	}

	int minimum(int left, int right) { // interval [left, right)
		int ret = std::numeric_limits<int>::max();
		left += n;
		right += n;

		while (left < right) {
			if (left & 1) ret = std::min(ret, data[left++]);
			if (right & 1) ret = std::min(ret, data[--right]);
			left >>= 1;
			right >>= 1;
		}
		return ret;
	}

private:
	int n;
	std::vector<int> data;
};

int main() {
	SegmentTree st(5);
	st.update(0, 5);
	st.update(1, 2);
	st.update(2, 3);
	st.update(3, 1);
	st.update(4, 4);
	for (int i = 0; i < 5; i++) {
		std::cout << i << ": " << st.minimum(i, i+1) << std::endl;
	}

	std::cout << st.minimum(1, 4) << std::endl;
	st.update(3, 10);
	std::cout << st.minimum(1, 4) << std::endl;
	std::cout << st.minimum(0, 5) << std::endl;
	st.update(4, 0);
	std::cout << st.minimum(1, 4) << std::endl;
	std::cout << st.minimum(0, 5) << std::endl;

	SegmentTree st2({5, 2, 3, 1, 4});
}
```





# Sparse table



> 「Sparse table」為古代之稱，如今詞不達意



- Pros
  - 數組區間的查詢
    - 區間最大值
    - 區間最小值

- Cons
  - 不能更新、插入、刪除值
  - **浪費空間**
  - 無法數組區間的
    - 求和值
    - **異或值 ( Exclusive or；XOR )**



### 實現



#### 初始化



- Construct sparse table
  - N：資料量
  - value[N]：數組
  - cnt［logN］[N]：Sparse table



依序先求出寬度為 $2^0, 2^1, 2^2, \ldots, 2^{\lfloor\lg N\rfloor}$ 的區間**最小值**，區間的所有可能位置都要算一遍。兩個窄區間可以快速合成出一個寬區間。




![STable_1](\willywangkaa\images\STable_1.png)



將所有區間算完存入 Sparse table




![STable_2](\willywangkaa\images\STable_2.png)

> 實作時，通常表格中記錄的是索引值、指標，而不是直接記錄數值的最小值。(如下程式碼)



- 程式碼 (c++)

```cpp
const int N = 1000000; //No. of elements
const int logN = ceil(log(N));
//const int logN = sizeof(unsigned int) - __builtin_clz((unsigned int)dist) - 1;;

int value[N];
int cnt[logN][N]; //cnt[i][j]: RMQ index
 
void construct_ST() {
	//Initialization
	for (int i=0; i<N; ++i)
		cnt[0][i] = i;

	// 2^i - 1 < N <=> i < ceil(log(N))
	for (int i=1; (1<<i)-1 < N; i++)
        // j + (2^i-1) < N ; buttom-up 建立
		for (int j=0; j+(1<<i)-1 < N; j++) {
			int L = cnt[i-1][j];
			int R = cnt[i-1][j+(1<<(i-1))];
			cnt[i][j] = (value[L] <= value[R])? L : R;
		}
}
```



- 時間複雜度
  - $\Theta(N\log N)$ 

- 空間複雜度
  - $\Theta(N \log N)$



#### 查詢



從表格中找到寬度略短於（相等於）查詢區間的區間，以靠左、靠右的兩條等寬區間，求得查詢區間的最小值：




![STable_3](\willywangkaa\images\STable_3.png)



- 如何知道要查「Range」大小為何的表？
  - 令 $k$ 為我們所求 → $k = \lfloor \lg N \rfloor$



```cpp
int query(int a, int b) {
	int dist = abs(b - a) + 1;
	// 2^i < N <=> i < cel(logN) ; 區間計算
	int i = sizeof(unsigned int) - __builtin_clz((unsigned int)dist) - 1;

	int L = cnt[i][a];          // 左區間
	int R = cnt[i][b-(1<<i)+1]; // 右區間
	return value[L] <= value[R]? L : R;
}
```



- 時間複雜度
  - $\Theta(1)$




# 參考




- [线段树（segment tree)，看这一篇就够了](https://blog.csdn.net/Yaokai_AssultMaster/article/details/79599809)



- 輔大張信宏老師講義



- [演算法筆記](http://www.csie.ntnu.edu.tw/~u91029/Sequence2.html#5)