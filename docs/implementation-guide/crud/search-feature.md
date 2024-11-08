---
sidebar_position: 6
title: 検索機能
---

本節では以下に示すような検索機能の実装方法について説明します。

あとでとるわ

## イベントの型を定義する

検索機能のイベントの型定義には`CsQueryButtonClickEvent`を指定します。検索用の View（`TodoSearchView`）のプロパティにイベントの型を定義します。型パラメータには検索 API のレスポンスの型を指定します。

```ts title="src/app/todo/page.view.ts"
// Orvalで自動生成されたListTodoResponseの型定義をimnport

/**
 * 担当者検索用のViewの型定義
 */
export type TodoSearchView = CsView & {
  searchTodo: CsQueryButtonClickEvent<ListTodoResponse>;
};
```

## イベントを初期化する

検索用の View（`TodoSearchView`）にイベントの初期化処理を追加します。検索 API で Event のフックに`useCsRqAdvancedQueryButtonClickEvent`、引数には Orval で自動生成された API フック`useListTodo()`を指定します。

```ts title="src/app/todo/page.view.ts"
// Orvalで自動生成されたAPIフック（useListTodo）をimport

/**
 * 担当者検索用のViewの初期化
 *
 * @param assignee 検索対象の担当者
 * @returns TodoSearchView 担当者検索用のView
 */
export const useTodoSearchView = (assignee: string): TodoSearchView => {
  return useCsView({
    // highlight-start
    searchTodo: useCsRqAdvancedQueryButtonClickEvent(
      useListTodo(
        { assignee_eq: assignee },
        {
          query: {
            enabled: true,
            refetchOnWindowFocus: false,
          },
        }
      )
    ),
  });
};
```

## View 定義を呼び出す

[イベントを初期化する](./search-feature.md#イベントを初期化する)で定義した 検索用の View 定義を呼び出します。

```tsx title="src/app/todo/page.tsx"
// 検索条件入力用のView
const todoSearchInputView = useTodoSearchInputView();

// highlight-start
const todoSearchView = useTodoSearchView(todoSearchInputView.assignee.value ?? ""); // 検索用のViewの呼び出し
// highlight-end
```

## ボタンを配置する

検索ボタンを配置する際は、画面コンポーネントとして `AxQueryButton` を使用します。（型定義で用いた `CsQueryButtonClickEvent` に対応した画面コンポーネントを使用します。）`event`という Props に、対応するイベントの変数を指定します。また、`validationViews` に View の変数を指定することで、バリデーションが実行できます。

```tsx title="src/app/todo/page.tsx"
<AxQueryButton
  type="primary"
  event={todoSearchView.searchTodo}
  validationViews={[todoSearchView]}
  onBeforeApiCall={() => {
    todoSearchView.validationEvent?.resetError();
    setAssignee(todoSearchView.assignee.value ?? "");
  }}
  onAfterApiCallSuccess={() => {
    setIsFilter(true);
  }}
  addClassNames={["vertical-center"]}
>
  検索
</AxQueryButton>
```

## 値を取得する

イベントの`response`メソッドを使用して、取得した値を変数（`searchResult`）に格納します。取得した値を画面表示する際などは、こちらの変数を(`searchResult`)を使用します。

```tsx title="src/app/page.tsx"
const searchResult = todoSearchView.searchTodo.response ?? [];
```
