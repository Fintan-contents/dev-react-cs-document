---
sidebar_position: 2
title: useCsView
---

`useCsView` は、画面項目を 1 つの View（画面）として初期化するためのフックです。View に対してオプションを設定することもできます。

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
    readonly: true,
    customRules: customRules, // customRuleには`xxCustomValidationRule`で初期化したルールを定義
    validationTrigger: "onBlur",
  }
);
```

### シグネチャ

<h3>`useCsView(definitions, options?, validationEventHook?): CsView & D`</h3>

<h3>引数</h3>

| 引数名              | 必須 | 型                                                                                                                                             | 説明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ------------------- | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| definitions         | 〇   | `D extends CsViewDefinition`                                                                                                                   | フックで定義した画面項目を指定します。画面項目には[画面定義](../../../category/画面定義)や[イベント定義](../../../category/イベント定義)を指定できます。                                                                                                                                                                                                                                                                                                                                                                         |
| options             |      | `{ readonly?: boolean, customValidationRules?: AppValidationRules extends CustomValidationRules, validationTrigger?: "onSubmit" \| "onBlur" }` | ①`readonly`には読み取り専用にするかどうかを指定します。デフォルトは`false`が指定されます。②`customValidationRules`には適用したいカスタムバリデーションルールを指定します。バリデーションルールには[バリデーション定義](../../../category/バリデーション定義)の`xxCustomValidationRule`で初期化したルールを指定します。③`validationTrigger`にはバリデーションの実行タイミングを指定します。`onSubmit`はボタン押下時、`onBlur`は入力項目からフォーカスが外れたタイミングでバリデーションを実施します。デフォルトは`onSubmit`です。 |
| validationEventHook |      | `(instance: CsView & D, customRules?: CustomValidationRules) => CsValidationEvent`                                                             | 使用するバリデーションイベントを指定します。デフォルトは `getCsDefaultValidationEvent()`が指定されており、初期設定でインストールしたバリデーションライブラリに対応したフックが使用されます。                                                                                                                                                                                                                                                                                                                                     |

<h3>戻り値</h3>
画面項目が1つに集約された`CsView`の拡張クラスのインスタンスを返します。
