---
sidebar_position: 2
title: useCsInputTextItem
---

`useCsInputTextItem` は、`AxInputText`に対応する Item を初期化するためのフックです。

## シグネチャ

<h3>`useCsInputTextItem(label, state, rule, readonly?, placeholder): CsInputTextItem`</h3>

## 引数

| 引数名      | 必須 | 型                             | 説明                                                                                                                                                              |
| ----------- | ---- | ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| label       | 〇   | `string`                       | 入力項目のラベルを指定します。 　                                                                                                                                 |
| state       | 〇   | `StateResult<string>*¹`        | 入力項目の状態変数を指定します。通常、[useInit](../helper-function/useInit.md) を使用して初期化した状態変数を指定します。                                         |
| rule        | 〇   | `StringValidationRule*²`       | 入力項目のバリデーションルールを指定します。 通常、[stringRule](../helper-function/stringRule.md)を使用して初期化したルールを指定します。                         |
| readonly    |      | `RW.Editable \| RW.Readonly*³` | 入力項目が読み取り専用かどうかを指定します。`RW.Editable` は読み取り・書き込み可能、`RW.Readonly`は読み取り専用を表す値です。デフォルトは `RW.Editable` です。 　 |
| placeholder |      | `string`                       | 入力フィールドが未入力の場合に表示されるテキストを指定できます。                                                                                                  |

\*1：`StateResult`は 状態変数の情報を保持するための型定義です。

\*2：`StringValidationRule`は文字列型のバリデーション定義情報を保持する型定義です。最小・最大文字数、正規表現などの情報を保持しています。

## 使用例

³

```tsx
useCsView({
  // highlight-start
  inputTextItem: useCsInputTextItem(
    "名前",
    useInit(""),
    stringRule(true, 1, 10),
    RW.Editable,
    "入力してください"
  ),
  // highlight-end
});
```

## 返り値

引数で定義した初期値やバリデーションルールなど、入力項目に関する情報が集約された `CsInputTextItem` クラスのインスタンスを返します。
