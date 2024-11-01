---
sidebar_position: 2
title: stringRule
---

`stringRule` は、文字列に対するバリデーションルールを生成するための関数です。必須項目の設定や文字列の最小・最大長、カスタムルール名の設定が可能です。[useCsInputTextItem](../screen-define/useCsInputTextItem.md)の`rule`で使用できます。

## シグネチャ

<h3>`stringRule(required: boolean, min?: number, max?: number, customRuleName?: string): StringValidationRule`</h3>

## 引数

| 引数名         | 必須 | 型      | 説明                                                                                     |
| -------------- | ---- | ------- | ---------------------------------------------------------------------------------------- |
| required       | 〇   | `boolean`| 入力項目が必須かどうかを指定します。                                                      |
| min            |      | `number` | 入力文字列の最小文字数を指定します。                                                         |
| max            |      | `number` | 入力文字列の最大文字数を指定します。                                                     |
| customRuleName |      | `string` | カスタムバリデーションルールの名前を指定します。                                          |

## 使用例

```tsx
const rule = stringRule(true, 1, 10, "customRuleName");
```

## 返り値

バリデーションの条件を保持した`StringValidationRule`クラスのインスタンスを返します。