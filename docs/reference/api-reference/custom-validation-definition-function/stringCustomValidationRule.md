---
sidebar_position: 2
title: stringCustomValidationRule
---

`stringCustomValidationRule` は、指定されたバリデータとエラーメッセージを使用して、文字列に対するカスタムバリデーションルールを生成します。[useCsView](../screen-define/useCsView.md)の`options`で使用できます。

## シグネチャ

<h3>`stringCustomValidationRule(validator, message): CustomValidationRule<string>`</h3>

## 引数

| 引数名    | 必須 | 型                                | 説明                                                                                                                                                  |
| --------- | ---- | --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| validator | 〇   | `CustomValidator<string>*¹`       | 入力値を検証するためのバリデータを指定します。正規表現を用いてバリデーションを実施したい場合は、`createRegExpValidator`を使用することで指定できます。 |
| message   | 〇   | `CustomValidateMessage<string>*²` | バリデーションエラー時のエラーメッセージを指定します。入力項目の情報をパラメータとして受け取り、メッセージに含めることができます。                    |

\*1：`CustomValidator`は バリデーションエラーが発生したかどうかを検証する型定義です。

\*2：`CustomValidateMessage`はバリデーションエラーが発生した際のエラーメッセージを出力する型定義です。

## 返り値

バリデーションの条件やメッセージを保持した`CustomValidationRule<string>` クラスのインスタンスを返します。

## 使用例

```tsx
const customValidationRules: CustomValidationRules = {
  // highlight-start
  半角英字: stringCustomValidationRule(
    createRegExpValidator(/^[a-zA-Z]*$/),
    (label: string) => `${label}は半角英字で入力してください`
  ),
  全角文字: stringCustomValidationRule(
    createRegExpValidator(/^[^ -~｡-ﾟ]*$/),
    (label: string) => `${label}は全角文字で入力してください`
  ),
  // highlight-end
};
```
