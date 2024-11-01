---
sidebar_position: 2
title: useCsInputTextItem
---

`useCsInputTextItem` は、`AxInputText`に対応する Item を初期化するためのフックです。

## シグネチャ

<h3>`useCsInputTextItem(label, state, rule, readonly?): CsInputTextItem`</h3>

## 使用例

```tsx
useCsView({
  // highlight-start
  inputTextItem: useCsInputTextItem(
    "名前",
    useInit(""),
    stringRule(true, 1, 10),
    RW.Editable
  ),
  // highlight-end
});
```

## 引数

| 引数名   | 必須 | 型                           | 説明                                                                                                                               |
| -------- | ---- | ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| label    | 〇   | `string`                     | 入力項目のラベルを指定します。 　                                                                                                  |
| state    | 〇   | `StateResult<string>`        | 入力項目の状態変数を指定します。通常、[useInit](../default-value-preference/useInit.md) を使用して初期化した状態変数を指定します。 |
| rule     | 〇   | `StringValidationRule`       | 入力項目のバリデーションルールを指定します。 通常、`stringRule` を使用して初期化したルールを指定します。                           |
| readonly |      | `RW.Editable \| RW.Readonly` | 入力項目が読み取り専用かどうかを指定します。デフォルトは `RW.Editable` です。 　                                                   |

## 返り値

入力項目に関する情報が集約された `CsInputTextItem` クラスのインスタンスを返します。
