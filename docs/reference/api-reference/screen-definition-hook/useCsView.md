---
sidebar_position: 2
title: useCsView
---

`useCsView` は、画面項目を 1 つの View（画面）として初期化するためのフックです。View に対してオプションを設定することもできます。

## シグネチャ

<h3>`useCsView(definitions, options?, validationEventHook?): CsView & D`</h3>

## 引数

| 引数名              | 必須 | 型                                                                                                                                             | 説明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ------------------- | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| definitions         | 〇   | `D extends CsViewDefinition`\*¹                                                                                                                | フックで定義した画面項目を指定します。画面項目には[入力項目定義のフック](../../../category/入力項目定義のフック)や[イベント定義のフック](../../../category/イベント定義のフック)のみを指定できま²す。                                                                                                                                                                                                                                                                                                                                |
| options             |      | `{ readonly?: boolean, customValidationRules?: AppValidationRules extends CustomValidationRules,*² validationTrigger?: "onSubmit" \| "onBlur" }` | ①`readonly`には読み取り専用にするかどうかを指定します。 デフォルトは`false`が指定されます。②`customValidationRules`には適用したいカスタムバリデーションルールを指定します。バリデーションルールには[カスタムバリデーション定義の関数](../../../category/カスタムバリデーション定義の関数)で初期化したルールを指定します。③`validationTrigger`にはバリデーションの実行タイミングを指定します。`onSubmit`はボタン押下時、`onBlur`は入力項目からフォーカスが外れたタイミングでバリデーションを実施します。デフォルトは`onSubmit`です。 |
| validationEventHook |      | `(instance: CsView & D, customRules?: CustomValidationRules*²) => CsValidationEvent`                                                             | 使用するバリデーションイベントを指定します。デフォルトは `getCsDefaultValidationEvent()`が指定されており、初期設定でインストールしたバリデーションライブラリに対応したフックが使用されます。                                                                                                                                                                                                                                                                                                                                        |

\*1: `CsViewDefinition`は、キーに`string`、値に`CsItemBase`または`CsEvent`のみを許容する`Record`型です。他の型が指定されるとエラーになります。

\*2: `CustomValidationRules`はバリデーションの実行条件やメッセージの表示などの情報を保持する型定義です。

\*3: `CsValidationEvent`は、

## 使用例

```tsx
const view = useCsView(
  {
    title: useCsInputTextItem(
      "タイトル",
      useInit(""),
      stringRule(true, 1, 10),
    ),
    description: useCsTextAreaItem(
      "説明",
      useInit(""),
      stringRule(true, 1, 100),
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

## 返り値

画面項目が 1 つに集約された`CsView`の拡張クラスのインスタンスを返します。
