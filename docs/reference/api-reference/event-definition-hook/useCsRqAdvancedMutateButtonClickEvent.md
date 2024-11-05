---
sidebar_position: 2
title: useCsRqAdvancedMutateButtonClickEvent
---

:::warning
執筆途中
:::

`useCsRqAdvancedMutateButtonClickEvent` は、Axios（シンプル版）もしくは React Query に対応する更新系APIイベントを初期化するためのフックです。

## シグネチャ

<h3>`useCsRqAdvancedMutateButtonClickEvent<TApiRequest, TApiResponse, TApiError, TContext = unknown>(mutationResult: RqAdvancedMutationResult<TApiResponse, TApiError, TApiRequest, TContext>): CsRqAdvancedMutateButtonClickEvent<TApiRequest, TApiResponse, TApiError, TContext>`</h3>

## 引数

| 引数名         | 必須 | 型                                                                         | 説明                               |
| -------------- | ---- | -------------------------------------------------------------------------- | ---------------------------------- |
| mutationResult | 〇   | `RqAdvancedMutationResult<TApiResponse, TApiError, TApiRequest, TContext>` | 更新系APIを呼び出す API フックを指定します。OpenAPIで自動生成されたAPIフックを指定できます。 |

\*1：`TApiResponse`は APIレスポンスデータを保持する型定義です。

\*2：`TApiError`は エラー時のAPIデータを保持する型定義です。

\*3：`TApiRequest`は APIリクエストデータを保持する型定義です。

\*4：`TContext`はAPIに関するコンテキスト情報を保持する型定義です。

\*1：`RqAdvancedMutationResult`はAPIのリクエスト、レスポンス、エラー、コンテキストに関する情報をまとめて扱うことができる型定義です。

## 使用例

```tsx
export const useTodoPostView = (): TodoPostView => {
  return useCsView(
    {
      title: useCsInputTextItem(
        "タイトル",
        useInit(""),
        stringRule(false, 1, 10),
        RW.Editable,
      ),
      description: useCsTextAreaItem(
        "説明",
        useInit(""),
        stringRule(false),
        RW.Editable,
      ),
      // highlight-start
      createButton: useCsRqMutateButtonClickEvent(usePostTodo()), // Orvalで自動生成されたusePostTodoを定義済み
      // highlight-end
    },
    {
      validationTrigger: "onSubmit",
    },
  );
};
```

## 返り値

APIのリクエストやレスポンス、成功・失敗のステータスなどの情報が含まれるデータが格納される`CsRqMutateButtonClickEvent`クラスのインスタンスが返却されます。
