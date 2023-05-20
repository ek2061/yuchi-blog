---
slug: can-not-upload-same-file
title: 上傳選擇相同檔案竟然不會觸發onChange?!
authors: [yuchi]
tags: [react, html, typescript]
---

:::info 前情概要
最近剛好有做到上傳檔案並預覽圖片的功能，

上傳檔案本身並不難，但有時候就是會有一些疑難雜症，這邊記錄一下
:::

## 原始程式碼

上傳圖片檔案的程式這裡就不贅述了

```tsx
const [img, setImg] = useState<File | null>(null); // selected image file

return (
  <div>
    <input
      type="file"
      accept="image/jpeg, image/png, image/gif"
      className="hidden"
      id="file"
      onChange={(e) => setImg(e.target.files?.[0] || null)}
    />
    <label htmlFor="file" className="flex items-center">
      <PhotoIcon className="h-6 cursor-pointer" />
    </label>
  </div>
);
```

## 過程

照理來說 `<input />` 都會有 `value` 這個屬性

沒錯! 它的 `value` 屬性就是檔案路徑，比方說這樣

`C:\fakepath\你現在在開車欸.jpg`

那可能你會想，不然我就去控制清空 `<input>` 的 `value`

但偏偏 `type="file"` 又不給你控制，設置它的 `value` 屬性還會報錯說什麼你不能把不受控元件改成受控元件([官方 reference](https://react.dev/reference/react-dom/components/input#controlling-an-input-with-a-state-variable))

我去看了 Stack Overflow 還滿多人講多這個的 XD

看到有人說把 `<input>` 銷毀再建一個，但重新渲染相同元件明顯不是好做法

## 解法

總之我想到 `onClick` 會在 `onChange` 之前觸發，透過 `onClick` 把 `value` 清除

```tsx
<input
  type="file"
  accept="image/jpeg, image/png, image/gif"
  className="hidden"
  id="file"
  onChange={(e) => setImg(e.target.files?.[0] || null)}
  // highlight-next-line
  onClick={(e) => ((e.target as HTMLInputElement).value = "")}
/>
```

這裡要把 `e.target` 指定成 `HTMLInputElement`

因為傳入的 event 是 `EventTarget`，底下可沒有 `value` ([mdn `EventTarget`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget))
