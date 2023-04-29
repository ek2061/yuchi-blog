---
sidebar_position: 4
slug: button
---

# Button

## props

| 參數     | 說明     | 類型或值                                    | 預設值    |
| -------- | -------- | ------------------------------------------- | --------- |
| variant  | 按鈕種類 | "primary" \| "default" \| "ghost" \| "text" | "default" |
| loading  | 轉圈動畫 | boolean                                     | false     |
| children | 按鈕內容 | ReactNode                                   | 無        |

其他參數繼承自 `html` 原生的 `<button>`

## 本文

Button 還滿簡單的，我參考 MUI 和 antd 的設計，寫了 4 種按鈕種類，

並且跟 MUI 一樣定義了基本和 `hover` / `active` 下的顏色

```typescript
import styled, { css } from "styled-components";

const primaryStyle = css`
  background-color: ${theme.primary.common};
  color: #fff;

  &:hover {
    background-color: ${theme.primary.hover};
  }
  &:active {
    background-color: ${theme.primary.active};
  }

  &:disabled {
    cursor: not-allowed;
    opacity: 0.4;

    &:hover,
    &:active {
      background-color: ${theme.primary.common};
    }
  }
`;

// ...other variant styles
```

然後根據 variant 給予 css

```typescript
export const StyledButton = styled.button<ButtonProps>`
  // ...button base style

  ${(props) => props.variant === "primary" && primaryStyle}
  ${(props) => props.variant === "default" && defaultStyle}
  ${(props) => props.variant === "ghost" && ghostStyle}
  ${(props) => props.variant === "text" && textStyle}
`;
```

而最終輸出的 Button 中使用這些樣式也根據 `loading` 決定是否多放一個轉圈動畫

```typescript
const Button: React.FC<ButtonProps> = ({
  type = "button",
  variant = "default",
  loading = false,
  disabled = false,
  children,
  ...props
}) => {
  return (
    <S.StyledButton
      type={type}
      variant={variant}
      disabled={disabled || loading}
      {...props}
    >
      {loading && (
        <S.StyledDiv right={7}>
          <CircleSVG />
        </S.StyledDiv>
      )}
      {children}
    </S.StyledButton>
  );
};
```

## 程式碼

[github](https://github.com/ek2061/yuchi-ui/tree/main/src/components/Button)
