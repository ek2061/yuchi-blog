---
sidebar_position: 4
---

# STL map 容器迭代器

C++ STL 函式庫為`map`配置雙向迭代器(`bidirectional iterator`)，這代表 `map` 只能進行`++p`、`p++`、`--p`、`p--`、`*p `、`==`與`!=`。

| 方法                    | 功能                                                                                                                                              |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| `begin()` / `end()`     | 指向已排序容器第一個/最後一個元素的雙向迭代器                                                                                                     |
| `rbegin()` / `rend()`   | 指向已排序容器第一個/最後一個元素的反向雙向迭代器                                                                                                 |
| `cbegin()` / `cend()`   | 與`begin()` / `end()`相同但加了`const`，不能修改容器內儲存的鍵和值                                                                                |
| `crbegin()` / `crend()` | 與`rbegin()` / `rend()`相同但加了`const`，不能修改容器內儲存的的鍵和值                                                                            |
| `find(key)`             | 尋找鍵為`key`的雙向迭代器，如果找不到則返回和`end()`                                                                                              |
| `lower_bound(key)`      | 返回一個指向當前容器中第一個大於或等於 `key` 的鍵和值雙向迭代器，如果容器是`const`限定，則返回`const`類型的                                       |
| `upper_bound(key)`      | 返回一個指向當前容器中第一個大於 `key` 的鍵和值雙向迭代器，如果容器是`const`限定，則返回`const`類型的                                             |
| `equal_range(key)`      | 返回一個包含 2 個雙向迭代器的`pair`物件，其中 `pair.first` 和 `lower_bound()` 方法的返回值等價，`pair.second` 和 `upper_bound()` 方法的返回值等價 |

![表1方法](../img/C/STL_map_iterator/map_methods.gif)
圖中 E<sub>i</sub>是`pair`物件(鍵和值)，以`map`容器來說，每個鍵和值都必須保證唯一

## 使用 `begin()/end()`範例

```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;

int main() {
	std::map<std::string, std::string>myMap{ { "name","Yuchi" },{ "age","26" } };

	for (auto iter = myMap.begin(); iter != myMap.end(); ++iter) {
		cout << iter->first << " " << iter->second << endl;
	}
	system("pause");
	return 0;
}

```

排序過後順序是`age`、`name`

```
age 26
name Yuchi
```

:::info
`auto` 是 C++11 引入的，可以自動推導迭代器或是變數的類型，就不用在寫上
for 迴圈的型別原先要寫上`std::map<std::string, std::string>::iterator`

```cpp
for (std::map<std::string, std::string>::iterator iter = myMap.begin(); iter != myMap.end(); ++iter) {
```

:::

如果改成使用`rbegin()/rend()`則打印順序會反過來

如果使用`cbegin()/cend()`這種`const`的迭代器要把類型從`::iterator`改為`::const_iterator`

## 使用`lower_bound()/upper_bound()` 範例

```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;
int main() {
	std::map<std::string, std::string>myMap{
	{ "name","Yuchi" },
	{ "age","26" },
	{ "height","175" } };

	// 尋找第一個鍵的值不小於 "height"
	auto iter = myMap.lower_bound("height");
	cout << "lower：" << iter->first << " " << iter->second << endl;

	// 尋找第一個鍵的值大於 "height"
	iter = myMap.upper_bound("height");
	cout << "upper：" << iter->first << " " << iter->second << endl;
	system("pause");
	return 0;
}
```

排序過後順序是`age`、`height`、`name`

不小於`height`是自己(`height`)

大於`height`是`name`

```
lower：height 175
upper：name Yuchi
```

```cpp
#include <iostream>
#include <utility>
#include <map>
#include <string>
using namespace std;
int main() {
	std::map<string, string>myMap{
		{ "name","Yuchi" },
		{ "age","26" },
		{ "height","175" } };

	pair <std::map<string, string>::iterator, std::map<string, string>::iterator> myPair = myMap.equal_range("name");
	for (auto iter = myPair.first; iter != myPair.second; ++iter) {
		cout << iter->first << " " << iter->second << endl;
	}
	system("pause");
	return 0;
}
```

尋找所有`key`是`name`的元素並返回一個`pair`，其中的第一個元素是指向範圍的起始迭代器，第二個元素是指向範圍的結束迭代器的後一個位置。

```
name Yuchi
```
