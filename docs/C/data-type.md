---
sidebar_position: 1
---

# 資料型別與內存空間

```c
int sum;
int m = 1;
int n = 2;

sum = m + n;
```

定義了整數變數`m`, `n`, `sum`並運算，而機器內存發生了什麼?

C++ 每個資料型態占用的內存空間大小不同，上面範例在宣告時動態尋找可用空間並把`m`, `n`, `sum`存入，

運算時查找變數所在地址並取值出來運算，然後尋找可用空間將運算結果寫上，最終返回這個內存空間的值。

**以下列出 64 位元機器情況下的位元資料**

| 變數型別    | bytes 數 | 描述                                           | 範圍                                                                          |
| ----------- | -------- | ---------------------------------------------- | ----------------------------------------------------------------------------- |
| char        | 1        | 字元                                           | signed: -2<sup>7</sup> ~ 2<sup>7</sup>-1 <br/> unsigned: 0~2<sup>8</sup>-1    |
| short       | 2        | 短整數                                         | signed: -2<sup>15</sup> ~ 2<sup>15</sup>-1 <br/> unsigned: 0~2<sup>15</sup>-1 |
| long        | 4        | 長整數                                         | signed: -2<sup>31</sup> ~ 2<sup>31</sup>-1 <br/> unsigned: 0~2<sup>32</sup>-1 |
| int         | 4        | 整數                                           | signed: -2<sup>63</sup> ~ 2<sup>63</sup>-1 <br/> unsigned: 0~2<sup>32</sup>-1 |
| float       | 4        | 單精確度浮點數，最高位元是正負號，1 為負       | 3.4e +/- 38 (7 digits)                                                        |
| double      | 8        | 雙精確度浮點                                   | 1.7e +/- 308(15 digits)                                                       |
| long double | 8        | 長雙精確度浮點                                 | 1.7e +/- 308(15 digits)                                                       |
| bool        | 1        | 布林                                           | true 或 false                                                                 |
| wchar_t     | 2        | 寬(wide)字元，為儲存 2bytes 長的國際字元而設計 | 1 個寬字元                                                                    |

:::info
我在找 `int`和`long`有什麼差異時意外的找到很有趣的說法:

`long`不一定會比`int`長，`short`也不一定會比`int`短，

不同編譯器將`int`編譯成不同長度，例如在 32 位元編譯器時，`int`和`long`的長度皆為 4 bytes
:::

最後用個各變數型別的宣告來結尾

```c
char a[10] = "a";
short int s = 97;
int m = 97;
long int n = 97;
float f = 97.0f;
double d = 97.0;
long double k = 97.0;
bool b = true;
wchar_t w[10] = L"a";
```
