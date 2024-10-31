---
sidebar_position: 2
title: stringCustomValidationRule
---

`stringCustomValidationRule` は、指定されたバリデータとエラーメッセージを使用して、文字列に対するカスタムバリデーションルールを生成します。[useCsView](../screen-define/useCsView.md)の`options`で使用できます。

### 使用例

```tsx
const customValidationRules: CustomValidationRules = {
  半角英字: stringCustomValidationRule(
    createRegExpValidator(/^[a-zA-Z]*$/),
    (label: string) => `${label}は半角英字で入力してください`
  ),
  全角文字: stringCustomValidationRule(
    createRegExpValidator(/^[^ -~｡-ﾟ]*$/),
    (label: string) => `${label}は全角文字で入力してください`
  ),
};
```

### シグネチャ

<h3>`stringCustomValidationRule(validator, message): CustomValidationRule<string>`</h3>

### 引数

| 引数名    | 必須 | 型                              | 説明                                                                                                                                    |
| --------- | ---- | ------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| validator | 〇   | `CustomValidator<string>`       | 入力値を検証するためのバリデータを指定します。`createRegExpValidator`関数を利用し、引数に特定の文字列を検出する正規表現を指定します。この正規表現に該当しない文字列の場合はバリデーションエラーになります。　 |
| message   | 〇   | `CustomValidateMessage<string>` | バリデーション時のエラーメッセージを指定します。入力項目の情報をパラメータとして受け取り、メッセージ内容に含めることができます。                                                             |

### 返り値

`CustomValidationRule<string>` クラスのインスタンスを返します。このインスタンスは、指定されたバリデータとエラーメッセージを使用して文字列のバリデーションを行います。
