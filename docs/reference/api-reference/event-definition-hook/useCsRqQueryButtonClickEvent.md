---
sidebar_position: 1
title: useCsRqQueryButtonClickEvent
---

`useCsRqQueryButtonClickEvent` は、API 呼び出し方式が TanStack Query および Orval（シンプル版）に対応する参照系 API ボタンイベントを初期化するためのフックです。

## シグネチャ

<h3>
  <code>useCsRqQueryButtonClickEvent<br/>&nbsp;&lt;TApiResponse><br/>(queryResult: UseQueryResult<br/>&nbsp;&lt;TApiResponse>):<br/>CsQueryButtonClickEvent<br/>&nbsp;&lt;TApiResponse></code>
</h3>

## 引数

| 引数名     | 必須 | 型                                                       | 説明                                                               |
| ---------- | ---- | -------------------------------------------------------- | ------------------------------------------------------------------ |
| queryResult| 〇   | `UseQueryResult<TApiResponse>*¹`                  | TanStack Query の `useQuery` を使ったカスタムフックを指定します。   |

\*1：`UseQueryResult`は APIのレスポンスに関する情報を保持する型定義です。

## 返り値

API のレスポンス、成功・失敗のステータスなどの情報が含まれる`CsQueryButtonClickEvent`クラスのインスタンスを返します。

## 使用例

```tsx
export const useTodoSearchView = (): TodoSearchView => {
  const keyword = useCsInputTextItem("検索キーワード", useInit(""), stringRule(true)),

  return useCsView(
    {
      keyword: keyword,
      // highlight-start
      searchButton: useCsRqQueryButtonClickEvent(useSearchTodo({keyword: keyword.value ?? ""})), // Orvalで自動生成されたuseSearchTodoを指定
      // highlight-end
    },
  );
};
```
