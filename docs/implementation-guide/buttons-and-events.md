---
sidebar_position: 3
---

# ボタンとイベント
ボタンは３種類あります。Tanstack-Query に対応した 2 種類のボタンと、その他 1 種です。 Tanstack-Query の useMutate に対応したボタンが AxMutateButton, useQuery に対応したボタンが AxQueryButton です。

## AxMutateButton / CsMutateButtonClickEvent
まずはAxMutateButtonとそのイベントから説明をします。

### CsMutateButtonClickEvent イベントの定義と初期化

イベントの定義は`View`のフィールドとして定義し、`useCsRqMutateButtonClickEvent`で初期化します。
このカスタムフックに渡すのは、Ovalで自動生成されたuseMutate関数の実行結果（ `usePostTodo()` や `useDeleteTodo()` ）です。
これは、`useCsRqQueryButtonClickEvent`でも同様です。

```typescript
// View の定義
type TodosPostView = CsView & {
  title: CsInputTextItem;
  description: CsInputTextItem;
  createButton: CsRqMutateButtonClickEvent<
    {
      data: TodoRegistration;
    },
    Todo
  >;
};

// View の初期化
export const useTodosPostView = (): TodosPostView => {
  return useCsView({
    title: useCsInputTextItem("タイトル", useInit(""), stringRule(true, 1, 100), RW.Editable, "タイトル"),
    description: useCsInputTextItem("説明", useInit(""), stringRule(true, 1, 100), RW.Editable, "説明"),
    createButton: useCsRqMutateButtonClickEvent(usePostTodo()),
  });
};
```

### メモ
- 上記のコード例をサンプルアプリのコード例に変更する
- 以下、同様の形で説明＋サンプルコードの記述がされる

### API リクエストの設定
  - 説明
  - サンプルコード
### AxMutateButton の配置
  - 説明
  - サンプルコード

## AxQueryButton / CsQueryButtonClickEvent
次にAxMutateButtonとそのイベントについて説明をします。

### CsQueryButtonClickEvent イベントの定義と初期化
  - 説明
  - サンプルコード
### AxQueryButton の配置
  - 説明
  - サンプルコード

## AxButton
最後にAxMutateButtonとそのイベントについて説明をします。

onClick がカスタム可能な AxMutateButton/AxQueryButton と同様のボタンです。API を呼び出さない場合に使います。 バリデーションの実行等も可能です。

- サンプルコード


