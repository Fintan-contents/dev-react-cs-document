---
sidebar_position: 2
title: useInit
---

`useInit` は、入力項目の初期値を設定するためのフックです。[画面定義](../../../category/画面定義)にある`useCsXxxItem`で使用できます。

### 使用例

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

### シグネチャ

<h3>`useInit<T>(value?: T): UseInitResult<T>`</h3>

### 引数

| 引数名 | 必須 | 型  | 説明                                                                  |
| ------ | ---- | --- | --------------------------------------------------------------------- |
| value  |      | `T` | `useCsXxxItem `で定義された入力項目の初期値を設定します。|

### 返り値

入力項目の初期値を返します。