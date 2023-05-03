---
sidebar_position: 5
slug: checkbox
---

# Checkbox

## props

| 參數     | 說明       | 類型或值  | 預設值 |
| -------- | ---------- | --------- | ------ |
| children | 多選框標籤 | ReactNode | 無     |

其他參數繼承自 `html` 原生的 `<input>`

## 本文

我本來覺得 Checkbox 不就是 `<input type="checkbox">`，

但實際在寫時發現無法改變原生的 checkbox 顏色和尺寸，所以後面就改用`svg`去顯示是否勾選，

總體而言跟 checkbox 沒太大區別，差別在於我必須取 `checked` 與 `onChange` 給 `svg` 顯示是否打勾，不能把預設值全部塞給他。

我取 `_checked` 的順位依序是: 使用者給的 `checked` -> 使用者給的 `defaultChecked` -> 預設 `defaultChecked` 為 false

(由於撞名，所以我的 `checked` 前面多了底線)

```typescript
const [_checked, _setChecked] = useState(checked ?? defaultChecked);

const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  _setChecked(e.target.checked);
  if (onChange) {
    onChange(e);
  }
};
```

而 `svg` 部分則是依我傳入的 `_checked`決定要顯示哪個`svg`

```typescript
const CheckSVG: React.FC<{ checked?: boolean }> = ({ checked = true }) => {
  return (
    <StyledSvg
      xmlns="http://www.w3.org/2000/svg"
      fill="currentcolor"
      viewBox="0 0 24 24"
    >
      {checked ? (
        <path
          fill="currentColor"
          d="M19 3H5c-1.11 0-2 .9-2 2v14c0 1.1.89 2 2 2h14c1.11 0 2-.9 2-2V5c0-1.1-.89-2-2-2zm-9 14l-5-5 1.41-1.41L10 14.17l7.59-7.59L19 8l-9 9z"
        ></path>
      ) : (
        <path
          fill="currentColor"
          d="M19 5v14H5V5h14m0-2H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2z"
        ></path>
      )}
    </StyledSvg>
  );
};
```

## 原始碼連結

[github](https://github.com/ek2061/yuchi-ui/tree/main/src/components/Checkbox)
