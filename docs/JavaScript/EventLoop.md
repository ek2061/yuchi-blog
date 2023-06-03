---
sidebar_position: 5
---

# 事件循環 / 微任務 / 宏任務

## 前言

剛畢業面試時其實滿容易被問到事件循環，但沒聽過有人提微任務(micro-task)和宏任務(macro-task)，

微任務和宏任務是我在去年(22 年)看網路文章看到的，這裡我就記錄下我所了解的

## 為什麼 JavaScript 是單線程執行

JavaScript 是運行於瀏覽器的腳本語言，因為經常用來操作 DOM，所以當初被設計為單線程。

如果可以多線程執行，兩個不同的線程改了 DOM，那不能保證執行結果是相當可怕的。

## 為什麼瀏覽器感覺不像單線程執行

比起 JavaScript， 瀏覽器要做更多的事，例如發送與接收請求，渲染繪出網頁內容等等，

如果瀏覽器只能單線程執行，那網頁的載入將會非常慢，

因為瀏覽器本身是一個複雜的系統，他要做的事很多，例如請求頁面資源、解析 HTML、JavaScript 與 CSS 的程式碼、運行、渲染頁面、回應使用者等等。

在實現層面，瀏覽器內部會有不同功能的模組去各自負責不同的任務。

## 瀏覽器的事件循環

瀏覽器執行 JavaScript 程式碼如果遇到耗時的工作，瀏覽器會將這些操作依據不同種類放入微任務或宏任務佇列中。

### 宏任務(macro-task)

是指需要在 JavaScript 主線程上執行的任務，可能會涉及到一系列的操作，例如 I/O 操作、計時器等。

常見的宏任務有 setTimeout、setInterval、I/O 操作(比方說讀檔或發送網路請求)、DOM 事件處理等。

在事件循環中，宏任務會被加到宏任務佇列中，而 JavaScript 引擎則會按照佇列的順序依次執行。

### 微任務(micro-task)

微任務通常與 Promise 有關，其執行時間較短，通常只涉及一些簡單的回調函数或處理邏輯。

常見的微任務有 Promise 的回調函數（.then()、.catch()、.finally()）、MutationObserver 的回調函數等。

在事件循環中，當宏任務執行完成以後，在下一个宏任務開始之前，JavaScript 引擎會去檢查並執行所有在微任務佇列中排隊的微任務。

## 看個範例吧

### 範例 1

```javascript
console.log("start");

setTimeout(() => {
  console.log("time");
});

Promise.resolve().then(() => {
  console.log("resolve");
});

console.log("end");
```

執行結果會是什麼? 我這裡分析一下執行過程:

1. 首先整段 JavaScript 腳本會先作為一個宏任務執行，對於同步程式(console.log)則直接執行，所以看到

```
"start"
"end"
```

2. 而`setTimeout`被加入宏任務的佇列，`Promise`被加入微任務佇列，

3. 前面提到在下一个宏任務開始之前，JavaScript 引擎會去檢查微任務佇列，找到了`Promise`並執行，所以看到

```
"resolve"
```

4. 接著進入下一輪宏任務，檢查宏任務佇列執行`setTimeout`，所以看到

```
"time"
```

### 範例 2

再看一個稍微複雜的吧

```javascript
const promise = new Promise((resolve, reject) => {
  console.log(1);
  setTimeout(() => {
    console.log("timerStart");
    resolve("success");
    console.log("timerEnd");
  }, 0);
  console.log(2);
});

promise.then((res) => {
  console.log(res);
});

console.log(4);
```

1. 從上而下先遇到`new Promise`並先執行了建構函數中的`console.log(1)`並執行(JavaScript 的建構函數是立即執行)

2. 遇到了`setTimeout`並加入宏任務佇列

3. 遇到 `console.log(2)`並執行

4. 離開建構函數以後遇到`promise.then`，但因為還沒拿到結果(`resolve` 或 `reject`)所以先維持等待中(`pending`)

5. 遇到 `console.log(4)`並執行

6. 下一輪宏任務開始前檢查微任務佇列但目前為空，所以進入下一輪宏任務，發現佇列中有`setTimeout`

7. 執行`console.log("timerStart");`

8. 遇到`resolve`將 promise 的狀態從原本的`pending`改為`resolved`，保存結果`success`並將之前遇到的`promise.then`丟入微任務佇列

9. 執行`console.log("timerEnd");`

10. 本輪宏任務做完了執行微任務佇列中的`promise.then`，印出剛剛拿到的結果`success`

本範例的執行結果為

```
1
2
4
"timerStart"
"timerEnd"
"success"
```
