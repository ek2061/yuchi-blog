---
sidebar_position: 2
---

# STL

## STL 是什麼

STL 是一個 C++函式庫，英文全名為 `Standard Template Library`，提供了一系列通用的資料結構和演算法，使得 C++程式可以像 Java 更容易編寫。

## STL 的好處

STL 提供許多通用的資料結構和演算法，例如向量(vector)、串列(list)、堆疊(stack)、佇列(queue)、二元樹(binary tree)等，也被納入 C++的標準規範。

我舉個例子好了，假設今天要操作陣列(Array)，以 C++定義一個陣列:

```cpp
int a[n];
```

必須要先定義好陣列長度(n 為常數)，又或者動態申請記憶體空間像是:

```cpp
int *p = new int[n];
```

這樣可根據變數 n 動態申請記憶體空間，不會出現浪費的問題。

但...如果程式執行過程中出現空間不足，則需要加大儲存空間，這時就必須進行以下順序的操作:

1. 再申請一個更大的空間 `int * temp = new int[m];`
2. 複製原記憶體空間的資料到新位置 `memecpy(temp, p, sizeof(int)*n);`
3. 釋放原記憶體空間 `delete [] p; p = temp;`

而相同操作如果使用 STL 則會簡單很多，因為大多細節將不需要工程師關心，以下舉例:

```cpp
// 宣告長度為0的陣列 a ，但這個陣列可以根據儲存的資料自動增長
vector <int> a;

// 塞 10 個數字到陣列 a
for (int i = 0; i < 10 ; i++) {
  a.push_back(i)
}

// 手動調整陣列大小與修改值
a.resize(100);
a[90] = 100;

// 清空陣列 a
a.clear();

// 調整陣列 a 的大小為 20，並全部塞入 -1
a.resize(20, -1)
```

STL 也把`vector`實作的與 C++原本的陣列效能相近。
