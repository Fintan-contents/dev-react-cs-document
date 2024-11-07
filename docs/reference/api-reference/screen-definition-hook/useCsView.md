---
sidebar_position: 2
title: useCsView
---

`useCsView` は、画面項目を 1 つの View（画面）として初期化するためのフックです。カスタムバリデーションの設定やバリデーションのタイミングなど、追加のオプションを設定することもできます。

## シグネチャ

<h3>`useCsView(definitions, options?, validationEventHook?): CsView & D`</h3>

## 引数

<table>
  <thead>
    <tr>
      <th>引数名</th>
      <th>必須</th>
      <th>型</th>
      <th>説明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>definitions</td>
      <td>〇</td>
      <td><code>D extends CsViewDefinition*¹</code></td>
      <td>フックで定義した画面項目を指定します。画面項目には<a href="../../../category/入力項目定義のフック">入力項目定義のフック</a>や<a href="../../../category/イベント定義のフック">イベント定義のフック</a>のみを指定できます。</td>
    </tr>
    <tr>
      <td>options</td>
      <td></td>
      <td><code> readonly?: boolean, customValidationRules?: AppValidationRules extends CustomValidationRules,*² validationTrigger?: "onSubmit" | "onBlur"</code></td>
      <td><p><code>readonly</code>には読み取り専用にするかどうかを指定します。 デフォルトは<code>false</code>が指定されます。</p>
        <p><code>customValidationRules</code>には適用したいカスタムバリデーションルールを指定します。バリデーションルールには<a href="../../../category/カスタムバリデーション定義の関数">カスタムバリデーション定義の関数</a>で初期化したルールを指定します。</p>
        <code>validationTrigger</code>にはバリデーションの実行タイミングを指定します。<code>onSubmit</code>はボタン押下時、<code>onBlur</code>は入力項目からフォーカスが外れたタイミングでバリデーションを実施します。デフォルトは<code>onSubmit</code>です。</td>
    </tr>
    <tr>
      <td>validationEventHook</td>
      <td>あああ</td>
      <td><code>(instance: CsView & D, customRules?: CustomValidationRules*²) => CsValidationEvent*³</code></td>
      <td>使用するバリデーションイベントを指定します。デフォルトは <code>getCsDefaultValidationEvent()</code>が指定されており、初期設定でインストールしたバリデーションライブラリに対応したフックが使用されます。</td>
    </tr>
  </tbody>
</table>

\*1: `CsViewDefinition`は、キー値に`string`、値に`CsItemBase`または`CsEvent`のみを許容する`Record`型です。

\*2: `CustomValidationRules`はバリデーションの実行条件やメッセージの表示などの情報を保持する型定義です。

\*3: `CsValidationEvent`はバリデーションエラーが発生したかどうか、エラー時の表示するメッセージなどバリデーションに関する情報を保持する型定義です。

## 返り値

画面項目が 1 つに集約された`CsView`の拡張クラスのインスタンスを返します。

## 使用例

```tsx
const view = useCsView(
  {
    title: useCsInputTextItem("タイトル", useInit(""), stringRule(true, 1, 10)),
    description: useCsTextAreaItem(
      "説明",
      useInit(""),
      stringRule(true, 1, 100)
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
