---
sidebar_position: 2
title: Item間やEventとItemの間に依存関係があるとき
---

## どのようなときに推奨されるか

View で定義した Item 間で値を参照する場合や`XxxMutateButton`のイベントでリクエストに View で定義した Item の値を渡すような Event と Item の間に依存関係があるときには注意する点があります。

## 実装方法

Item を一度変数として定義することで Event 側で同じ View に定義されている Item の値を参照することができます。

```tsx
export const useTodoSearchView = (): TodoSearchView => {
  // highlight-start
  // returnの外側に変数として定義する。
  const assignee = useCsInputTextItem(
    "担当者",
    useInit(""),
    stringRule(true, 1, 20),
    RW.Editable,
    "検索する担当者を入力してください",
  );
  // highlight-end
  return useCsView({
    assignee: assignee,
    searchTodo: useCsRqAdvancedQueryButtonClickEvent(
      useListTodo(
        // highlight-start
        { assignee_eq: assignee.value ?? "" },
        // highlight-end
        {
          query: {
            enabled: false,
            refetchOnWindowFocus: false,
          },
        },
      ),
    ),
  });
};
```
