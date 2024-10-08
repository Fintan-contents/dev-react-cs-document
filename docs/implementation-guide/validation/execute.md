---
sidebar_position: 3
title: バリデーションを実施する
---

# バリデーションを実施する

## ボタン押下でバリデーションを実施する

引数として vIEW を受け取るボタンコンポーネントを使用する
AntDesign の場合は AxButton, AxMutateButton, AxQueryButton の 3 種類がそれにあたる。
※AxMutateButton, AxQueryButton は API を呼び出すためのボタンであるため、後続の章で解説。
バリデーションを実施したいだけであれば AxButton を使用することで実現できる。

```tsx
<AxButton validationViews={[view]}>
  バリデーションを実行するためのボタン
</AxButton>
```

## フォーカスが外れたタイミングでバリデーションを実施する

View を作るときに使用した useCsView の第二引数に validationTrigger を設定する

```tsx
useCsView(
    {
      title: useCsInputTextItem(
        "タイトル",
        useInit(""),
        stringRule(true, 1, 10),
        RW.Editable
      ),
      ...
    },
    {
      validationTrigger: "onBlur",
    }
)

```
