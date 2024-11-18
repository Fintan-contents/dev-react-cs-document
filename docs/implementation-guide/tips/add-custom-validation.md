---
sidebar_position: 5
title: カスタムバリデーションの追加方法
---

○○ のような固有のバリデーションルールを追加したい場合は本節が役立ちます。

## 正規表現（シンプル）

```tsx
nameRule: stringCustomValidationRule(
  createRegExpValidator(/^[A-Za-z ]*$/),
  (label) => `${label}は、アルファベットと空白のみ使用可能です。`,
);
```

## 正規表現（複雑）

```tsx
passwordRule: stringCustomValidationRule(
    (newValue, item) => {
        if (!newValue) {
        return true;
        }
        let count = 0;
        if (/[A-Z]/.test(newValue)) {
        count++;
        }
        if (/[a-z]/.test(newValue)) {
        count++;
        }
        if (/[0-9]/.test(newValue)) {
        count++;
        }
        if (/[!-)+-/:-@[-`{-~]/.test(newValue)) {
        count++;
        }
        return count >= 4;
    },
    (label, newValue, item) => {
        let requireds = ["大文字", "小文字", "数字", "記号"];
        if (/[A-Z]/.test(newValue)) {
        requireds = requireds.filter((e) => "大文字" !== e);
        }
        if (/[a-z]/.test(newValue)) {
        requireds = requireds.filter((e) => "小文字" !== e);
        }
        if (/[0-9]/.test(newValue)) {
        requireds = requireds.filter((e) => "数字" !== e);
        }
        if (/[!-)+-/:-@[-`{-~]/.test(newValue)) {
        requireds = requireds.filter((e) => "記号" !== e);
        }
        return `${label}は、${requireds.join("、")}を含めてください`;
    },
),
```

## 項目間精査

```tsx

```
