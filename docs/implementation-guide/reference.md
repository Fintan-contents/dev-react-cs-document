---
sidebar_position: 99
title: APIリファレンス
---

## `useCsView(definitions, options?, validationEventHook?)`

useCsView メソッドは、ビュー定義とオプションを受け取り、カスタムバリデーションルールを適用したビューインスタンスを生成します。

<h3>引数</h3>

| 引数名              | 必須 | 型                                                                                                               | 説明                                                                                 |
| ------------------- | ---- | ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| definitions         | 〇   | D                                                                                                                | ビュー定義を含むオブジェクト。                                                       |
| options             |      | `{ readonly?: boolean, customValidationRules?: AppValidationRules, validationTrigger?: "onSubmit" \| "onBlur" }` | ビューのオプション設定。                                                             |
| validationEventHook |      | `(instance: CsView & D, customRules?: CustomValidationRules) => CsValidationEvent`                               | カスタムバリデーションイベントフック。デフォルトは `getCsDefaultValidationEvent()`。 |

<h3>戻り値</h3>
CsView & D オブジェクトを返します。このオブジェクトは、指定されたビュー定義とバリデーションルールを持っています。

<h3>使用例</h3>

```tsx
const view = useCsView(
  {
    title: useCsInputTextItem(
      "タイトル",
      useInit(""),
      stringRule(true, 1, 10),
      RW.Editable
    ),
    description: useCsTextAreaItem(
      "説明",
      useInit(""),
      stringRule(true, 1, 100),
      RW.Editable
    ),
    createButton: useCsRqMutateButtonClickEvent(usePostTodo()),
  },
  {
    readonly: false,
    validationTrigger: "onBlur",
  }
);
```

## `useCsInputTextItem(label, state, rule, readonly?)`

useCsInputTextItem メソッドは、テキスト入力項目のバリデーションルールを生成します。この関数は、ラベル、状態、バリデーションルール、および読み取り専用設定を受け取ります。

<h3>引数</h3>

| 引数名   | 必須 | 型                    | 説明                                                                                   |
| -------- | ---- | --------------------- | -------------------------------------------------------------------------------------- |
| label    | 〇   | string                | テキスト入力項目のラベルを指定します。 　                                              |
| state    | 〇   | `StateResult<string>` | テキスト入力項目の状態を指定します。                                                   |
| rule     | 〇   | StringValidationRule  | テキスト入力項目のバリデーションルールを指定します。                                   |
| readonly |      | RW = RW.Editable      | テキスト入力項目が読み取り専用かどうかを指定します。デフォルトは RW.Editable です。 　 |

<h3>戻り値</h3>

CsInputTextItem オブジェクトを返します。

<h3>使用例</h3>

```tsx
const inputTextItem = useCsInputTextItem(
  "名前",
  useInit(""),
  stringRule(true, 1, 10),
  RW.Editable
);
```

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
