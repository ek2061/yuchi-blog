# 刪除 set 資料

## erase

```cpp
#include <iostream>
#include <set>
#include <string>
using namespace std;

int main(void) {
  // 建立 set
	std::set<int>myset{ 1,2,3,4,5 };
	cout << "myset size = " << myset.size() << endl;  // myset size = 5

  // (1)
	int num = myset.erase(2);
	cout << "1、myset size = " << myset.size() << endl;  // 1、myset size = 4
	cout << "num = " << num << endl;  // num = 1

  // (2)
	set<int>::iterator iter = myset.erase(myset.begin());
	cout << "2、myset size = " << myset.size() << endl;  // 2、myset size = 3
	cout << "iter->" << *iter << endl;  // iter->3

  // (3)
	set<int>::iterator iter2 = myset.erase(myset.begin(), --myset.end());
	cout << "3、myset size = " << myset.size() << endl;  // 3、myset size = 1
	cout << "iter2->" << *iter2 << endl;  // iter2->5

	system("pause");
	return 0;
}
```

1. 第 1 種方法指定刪除 set 中值為`2`的元素，並返回刪除的數量，因此會得到刪除後 set 長度為 4，以及刪除 1 個元素
2. 第 2 種方法指定刪除 set 的第一個元素，並返回指向下一個元素的迭代器，因此會得到刪除後 set 長度為 3，以及指向 3 的迭代器
3. 第 3 種方法指定保留 set 的最後一個元素，並返回指向下一個元素的迭代器，因此會得到刪除後 set 長度為 1，以及指向 5 的迭代器

## clear

如果要清空整個 set 可以直接使用 set 底下的`clear`函數，函數的語法格式如下

```cpp
void clear();
```

該函數不需要傳入任何參數也沒有返回值

```cpp
#include <iostream>
#include <set>
#include <string>
using namespace std;
int main(void) {
    std::set<int>myset{1,2,3,4,5};
    myset.clear();

    system("pause");
    return 0;
}
```
