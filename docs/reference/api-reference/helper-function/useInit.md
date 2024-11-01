---
sidebar_position: 2
title: useInit
---

`useInit` は、入力項目の初期値を設定するためのフックです。[入力項目定義のフック](../../../category/入力項目定義のフック)にある`useCsXxxItem`で使用できます。

## シグネチャ

<h3>`useInit<T>(value?: T): any`</h3>

## 引数

| 引数名 | 必須 | 型  | 説明                           |
| ------ | ---- | --- | ------------------------------ |
| value  |      | `T` | 入力項目の初期値を設定します。 |

## 使用例

```tsx
useCsView({
  inputTextItem: useCsInputTextItem(
    "名前",
    // highlight-start
    useInit("Sample"),
    // highlight-end
    stringRule(true, 1, 10),
    RW.Editable
  ),
});
```

## 返り値

入力項目の初期値が格納された`UseInitResult<T>`のインスタンスを返します。
