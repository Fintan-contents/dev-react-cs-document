---
sidebar_position: 3
title: 特徴
---

# 特徴

省力化コンポーネントには以下の 3 つの特徴があります。

- 画面項目定義が 1-2 行で書ける
- バリデーション処理の簡略化
- API の呼び出しを簡略化

以上の 3 点についてそれぞれ説明をします。

## 画面項目定義が１行で書ける

まずは Item が 2 つ並んでいる画面を考えて見ます。

![省力化コンポーネントの例](/img/image.png)
この画面に必要なコードを以下に示します。

View 定義

```typescript
userName: useCsInputTextItem("ユーザー名", useInit(""), stringRule(true, 3, 30)),
password: useCsInputPasswordItem("パスワード", useInit(""), stringRule(true, 8, 16)),
```

呼び出し元

```typescript
<AxTableLayout
  view={view}
  colSize={propColSize}
  labelPlacement={labelPlacement}
  labelWidth={labelWidth}
  hideLabel={hideLabel}
/>
```

このように画面項目 1 つに対して画面項目定義で 1 行、呼び出し元画面で 1 行を書くことで入力フォームが簡単に出来上がります。

## バリデーション定義が簡潔に書ける

次にバリデーション定義が簡潔に書けるについて説明をします。省力化コンポーネントでバリデーションを実施するためには以下のように書きます。

View 定義

```typescript
title: useCsInputTextItem("タイトル", useInit(""), stringRule(true, 1, 100), RW.Editable, "タイトル"),
description: useCsInputTextItem("説明", useInit(""), stringRule(true, 1, 100), RW.Editable, "説明"),
```

このように Item の第 3 引数に行いたいバリデーションルールを記載することで簡単にバリデーションを定義することができます。

## 高機能なボタン

最後に、高機能なボタンについて説明をします。省力化コンポーネントでいう高機能なボタンとは、API 呼び出しやバリデーション実行などの機能が搭載されたボタンです。
高機能なボタンを使用することで API やバリデーションの実行が簡単にできます。
以下に、コードを示します。

View 定義

```typescript
title: useCsInputTextItem("タイトル", useInit(""), stringRule(true, 1, 100), RW.Editable, "タイトル"),
description: useCsInputTextItem("説明", useInit(""), stringRule(true, 1, 100), RW.Editable, "説明"),
createButton: useCsRqMutateButtonClickEvent(usePostTodo()),
```

呼び出し元

```typescript
<AxMutateButton
  type="primary"
  event={updateView.updateButton}
  validationViews={[updateView]}
>
  決定
</AxMutateButton>
```

このように View 定義に Item とボタンを定義し、呼び出し元画面で必要なパラメータを渡すだけで簡単に API やバリデーションを実行できます。
