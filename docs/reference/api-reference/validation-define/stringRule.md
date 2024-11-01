---
sidebar_position: 2
title: stringRule
---

`stringRule` は、文字列入力項目に対するバリデーションルールを生成するための関数です。必須項目、最小・最大文字数、カスタムルールを設定できます。

### 使用例

```tsx
const view = useCsView(
  {
    title: useCsInputTextItem(
      "タイトル",
      useInit(""),
      stringRule(true, 1, 10, "全角文字") //
    ),
  },
  {
    customRules: customValidationRules, // 全角文字のバリデーションルールを定義済み
  }
);
```

\*1: 必須

### シグネチャ

<h3>`stringRule(required: boolean, min?: number, max?: number, customRuleName?: string): StringValidationRule`</h3>

| 引数名         | 必須 | 型        | 説明                                                                                                                                                                                      |
| -------------- | ---- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| required       | 〇   | `boolean` | 入力項目が必須かどうかを指定します。                                                                                                                                                      |
| min            |      | `number`  | 入力項目の最小文字数を指定します。                                                                                                                                                        |
| max            |      | `number`  | 入力項目の最大文字数を指定します。省                                                                                                                                                      |
| customRuleName |      | `string`  | カスタムバリデーションルールの名前を指定します。[useCsView](../screen-define/useCsView.md)の`customRules`で指定されたキー値を指定します。要件に応じた固有のバリデーションを実施できます。 |

### 返り値

StringValidationRule クラスのインスタンスを返します。このインスタンスは、指定されたバリデーション条件を保持し、入力項目のバリデーションを行います。
