---
sidebar_position: 2
title: 画面定義で使用するフック
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
