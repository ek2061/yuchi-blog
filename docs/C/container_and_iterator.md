---
sidebar_position: 3
---

# 容器與迭代器

## 容器

STL 提供 3 種標準容器，分別是序列容器、排序容器和哈希容器

| 容器種類 | 功能                                                                                                                                                                                                               |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 序列容器 | 容器是無序的，將元素插入容器時指定在哪，元素就放哪，屬於該類的有`vector`向量、`list`串列、`deque`雙向佇列                                                                                                          |
| 排序容器 | 容器默認是由小到大排序，及插入元素會被放到合適的位置，所以在查找資料有更好的效能，屬於該類的有`set `集合、 `multiset`多重集合、 `map`映射、`multimap`多重映射                                                      |
| 哈希容器 | C++ 11 加入 4 種關聯容器分別是 `unordered_set`哈希集合、 `unordered_multiset`哈希多重集合、`unordered_map`哈希映射以及`unordered_multimap`哈希多重映射，和排序容器不同的是哈希容器的元素是無序且位置由哈希函數決定 |

## 迭代器

STL 函式庫為每種容器定義一種迭代器類型，而不同容器的迭代器也有不同的功能

1. 前向迭代器(forward iterator): 前向迭代器支援`++p`, `p++`, `*p`操作，可以被賦值、也可以使用等號比較。
2. 雙向迭代器(bidirectional iterator): 與前向迭代器相似，但多了`--p`與`p--`(一次向前移動一個位置)
3. 隨機訪問迭代器(random access iterator): 比起前兩種迭代器，該迭代器又更強，可以使用

- `p+=i`(向後移動 i 個位置)
- `p-=i`(向前移動 i 個位置)
- `p+i`(返回 p 後面第 i 個位置)
- `p-i`(返回 p 前面第 i 個位置)
- `p[i]`(返回 p 後面第 i 個參考)

| 容器                               | 迭代器類型     |
| ---------------------------------- | -------------- |
| array                              | 隨機訪問迭代器 |
| vector                             | 隨機訪問迭代器 |
| deque                              | 隨機訪問迭代器 |
| list                               | 雙向迭代器     |
| set / multiset                     | 雙向迭代器     |
| map / multimap                     | 雙向迭代器     |
| forward_list                       | 前向迭代器     |
| unordered_map / unordered_multimap | 前向迭代器     |
| unordered_set / unordered_multiset | 前向迭代器     |
| stack                              | 不支援迭代器   |
| queue                              | 不支援迭代器   |

## 定義迭代器

| 迭代器         | 格式                     |
| -------------- | ------------------------ |
| 正向迭代器     | ::iterator               |
| 常數正向迭代器 | ::const_iterator         |
| 反向迭代器     | ::reverse_iterator       |
| 常數反向迭代器 | ::const_reverse_iterator |

定義方式為 <容器>::<格式> <名稱>，例如

```cpp
vector<int>::iterator i
```

定義一個 int vector 的正向迭代器，命名為`i`

以下是一個遍歷 vector 容器與使用迭代器的範例

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main() {
  vector<int> v{1,2,3,4,5,6,7,8,9,10};

  // 第1種方式: 用 for 迴圈
  for (int i = 0; i < v.size(); ++i)
		cout << v[i] << " ";

  // 第2種方式: 用迭代器
  vector<int>::iterator i;

  // (1) 用 != 比較迭代器
	for (i = v.begin(); i != v.end(); ++i)
		cout << *i << " ";

  // (2) 用 < 比較迭代器
	for (i = v.begin(); i < v.end(); ++i)
		cout << *i << " ";

  // (3) 用 += 迭代
  i = v.begin();
	while (i < v.end()) {
		cout << *i << " ";
		i += 1;
	}
```
