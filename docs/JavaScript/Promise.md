---
sidebar_position: 4
---

# Promise

## 前言

Promise 是滿基本的東西但花樣很多，所以我寫了這個

在 JavaScript 中，Promise 是一種處理異步操作的技術，它可以更優雅地處理回調函數的問題。

## 為什麼需要 Promise

JavaScript 是一種單線程(thread)的語言，這代表它一次只能執行一個任務，

假設今天有一個耗時的操作(例如發送 AJAX 請求或讀取文件)需要進行時，你該怎麼辦呢? 寫個 function 每秒檢查完成了嗎?

這個做法是很不切實際的，雖說 JavaScript 是一種單線程的語言，

但負責執行的瀏覽器可以創建多個線程來迴避 JavaScript 單線程會阻塞的問題，這裡就不細說阻塞了

## Promise 是什麼？

Promise 是一個表示異步操作最終完成或失敗的 object。

Promise 如同英文名字的意思可以被認為是一個承諾，有三種不同的狀態：

- 未定（pending）：初始狀態，表示異步操作正在進行中。
- 已完成（fulfilled）：表示異步操作已成功完成。
- 已拒絕（rejected）：表示異步操作失敗。

## 如何使用

```javascript
const myPromise = new Promise((resolve, reject) => {
  let name = "yuchi";
  if (name === "yuchi") {
    resolve(1);
  } else {
    reject(2);
  }
});

myPromise
  .then(
    (fulfilledValue) => {
      console.log(fulfilledValue);
    },
    (rejectedValue) => {
      console.log(rejectedValue);
    }
  )
  .catch((error) => {
    console.log(error);
  });
```

宣告一個 Promise 的 object，裡面設定完成(resolve)和拒絕(reject)的回傳值分別是 1 和 2

透過`then`獲取回傳成功或失敗的結果，第一個參數是成功時的 callback，第二個則是失敗時的 callback，

也可以用 catch 捕獲錯誤時的處理

## Promise.resolve 與 Promise.reject

可以用`Promise.resolve`直接設定新的 Promise object 的狀態為 fulfilled(已實現)

```javascript
Promise.resolve("Success").then(
  (fulfilledValue) => {
    console.log(fulfilledValue); // "Success"
  },
  (rejectedValue) => {
    // 不會被呼叫
  }
);
```

而`Promise.reject`則相反，是直接設定為 rejected(已失敗)狀態

```javascript
Promise.reject(new Error("fail")).then(
  (fulfilledValue) => {
    // 不會被呼叫
  },
  (rejectedValue) => {
    console.log(rejectedValue); // Stacktrace
  }
);
```

## Promise.all 與 Promise.race

Promise.all 是"並行運算"使用的靜態方法，當所有 Promise 物件都要解決(resolve)完了才進行下一步，

而 Promise.race 則是"任何一個"陣列傳入參數的 Promise 物件有解決，就會到下一步去。

```javascript
const promiseArray = [1, 2, 3, 4, 5].map((item) => {
  return new Promise((resolve) => {
    resolve(item);
  });
});

Promise.all(promiseArray).then((res) => {
  console.log(res);
});
// [1,2,3,4,5]

Promise.race(promiseArray).then((res) => {
  console.log(res);
});
// 1
```
