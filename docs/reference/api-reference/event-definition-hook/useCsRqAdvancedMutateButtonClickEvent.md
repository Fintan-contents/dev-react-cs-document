---
sidebar_position: 2
title: useCsRqAdvancedMutateButtonClickEvent
---

:::warning
執筆途中
:::

`useCsRqAdvancedMutateButtonClickEvent` は、Axios（シンプル版）もしくは React Query に対応する更新系 API イベントを初期化するためのフックです。

## シグネチャ

<h3>`useCsRqAdvancedMutateButtonClickEvent<TApiRequest, TApiResponse, TApiError, TContext = unknown>(mutationResult: RqAdvancedMutationResult<TApiResponse, TApiError, TApiRequest, TContext>): CsRqAdvancedMutateButtonClickEvent<TApiRequest, TApiResponse, TApiError, TContext>`</h3>

## 引数

| 引数名         | 必須 | 型                                                                         | 説明                                                                                              |
| -------------- | ---- | -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| mutationResult | 〇   | `RqAdvancedMutationResult<TApiResponse, TApiError, TApiRequest, TContext>*¹` | 更新系 API を呼び出す API フックを指定します。OpenAPI で自動生成された API フックを指定できます。 |

\*1：`RqAdvancedMutationResult`は API のリクエスト、レスポンス、エラー、コンテキストに関する情報をを保持する型定義です。

## 使用例

```tsx
export const useTodoPostView = (): TodoPostView => {
  return useCsView(
    {
      title: useCsInputTextItem(
        "タイトル",
        useInit(""),
        stringRule(false, 1, 10),
      ),
      description: useCsTextAreaItem(
        "説明",
        useInit(""),
        stringRule(false),
      ),
      // highlight-start
      createButton: useCsRqMutateButtonClickEvent(usePostTodo()), // Orvalで自動生成されたusePostTodoを定義済み
      // highlight-end
    },
    {
      validationTrigger: "onSubmit",
    }
  );
};
```

## 返り値

API のリクエストやレスポンス、成功・失敗のステータスなどの情報が含まれるデータが格納される`CsRqMutateButtonClickEvent`クラスのインスタンスが返却されます。
