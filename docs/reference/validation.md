---
sidebar_position: 2
title: バリデーション
---

## `stringRule(required, min?, max?, customRuleName?)`

stringRule メソッドは、文字列のバリデーションルールを生成します。この関数は、必須項目の指定、文字列の最小および最大長の設定、カスタムルール名の指定を行うことができます。

<h3>引数</h3>

| 引数名         | 必須 | 型      | 説明                           |
| -------------- | ---- | ------- | ------------------------------ |
| required       | 〇   | boolean | 必須かどうかを指定します。     |
| min            |      | number  | 最小文字数を指定します。       |
| max            |      | number  | 最大文字数を指定します。       |
| customRuleName |      | string  | カスタムルール名を指定します。 |

<h3>戻り値</h3>

StringValidationRule オブジェクトを返します。このオブジェクトは、指定されたバリデーションルールを持っています。

<h3>使用例</h3>

```tsx
stringRule(true, 1, 10, "nameRule");
```

## `customValidationRule<T>(validator, message)`

customValidationRule メソッドは、カスタムバリデーションルールを生成します。この関数は、バリデータ関数とバリデーションメッセージを受け取ります。

<h3>引数</h3>

| 引数名    | 必須 | 型                         | 説明                                   |
| --------- | ---- | -------------------------- | -------------------------------------- |
| validator | 〇   | `CustomValidator<T>`       | バリデータ関数を指定します。           |
| message   | 〇   | `CustomValidateMessage<T>` | バリデーションメッセージを指定します。 |

<h3>戻り値</h3>

CustomValidationRule オブジェクトを返します。このオブジェクトは、指定されたバリデーションルールを持っています。

<h3>使用例</h3>

```tsx
const customRule = customValidationRule<string>(
  (value) => value.length > 5,
  "文字列は5文字以上でなければなりません。"
);
```
