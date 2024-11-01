---
sidebar_position: 3
title: createRegExpValidator
---

`createRegExpValidator` は、引数で渡された正規表現パターンに基づいて入力値を検証する関数です。[カスタムバリデーション定義の関数](../../../category/カスタムバリデーション定義の関数)で使用できます。

## シグネチャ

<h3>`createRegExpValidator(pattern: RegExp): CustomValidator<string>`</h3>

## 引数

| 引数名  | 必須 | 型       | 説明                                                                                                                     |
| ------- | ---- | -------- | ------------------------------------------------------------------------------------------------------------------------ |
| pattern | 〇   | `RegExp` | 入力値を検証するための正規表現パターンを指定します。指定したパターンに一致しない場合にバリデーションエラーが発生します。 |

## 使用例

```tsx
const customValidationRules: CustomValidationRules = {
  半角英字: stringCustomValidationRule(
    // highlight-start
    createRegExpValidator(/^[a-zA-Z]*$/),
    // highlight-end
    (label: string) => `${label}は半角英字で入力してください`
  ),
  全角文字: stringCustomValidationRule(
    // highlight-start
    createRegExpValidator(/^[^ -~｡-ﾟ]*$/),
    // highlight-end
    (label: string) => `${label}は全角文字で入力してください`
  ),
};
```

## 返り値

`CustomValidator<string>` 型の関数を返します。入力値が未定義もしくは入力値が正規表現に一致する場合は`true`、一致しない場合は`false`が返されます。
