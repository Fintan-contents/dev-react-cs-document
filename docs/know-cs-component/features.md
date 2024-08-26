---
sidebar_position: 3
title: 特徴
---

# 特徴

省力化コンポーネントには以下の 3 つの特徴があります。

- 画面項目定義が 1-2 行で書ける
- バリデーション定義が簡潔に書ける
- 高機能なボタン

以上の 3 点についてそれぞれ説明をします。

## 画面項目定義が 1-2 行で書ける

省力化コンポーネントを使用することで画面項目定義を 1-2 行で書くことができます。

以下の画面を例に考えてみます。

![省力化コンポーネントの例](/img/無題.png)

画面には 2 種類の入力項目があります。省力化コンポーネントを用いるとこれらの項目をたった 1-2 行で定義することができます。
userName や password が画面の各項目と対応しています。

```typescript
// View 定義
export const ConceptApplyedPane: React.FC = () => {
  const view: RegisterUserView = useCsView({
    userName: useCsInputTextItem("ユーザー名", useInit(""), stringRule(true, 3, 30)),
    password: useCsInputPasswordItem("パスワード", useInit(""), stringRule(true, 8, 16)),
  })
  return (
    <>
      <AxTableLayout
        view={view}
        colSize={2}
      />
  )}
```

仮に入力項目を増やしたい場合でも、1 項目につき 1-2 行コードを増やすだけで簡単に実装できます。
より詳しい実装方法については[View と Item の基本](http://localhost:3000/dev-react-cs-document/docs/implementation-guide/basic-of-view-and-item)を参考にしてください。

```typescript
// View 定義
export const ConceptApplyedPane: React.FC = () => {
  const view: RegisterUserView = useCsView({
    userName: useCsInputTextItem("ユーザー名", useInit(""), stringRule(true, 3, 30)),
    password: useCsInputPasswordItem("パスワード", useInit(""), stringRule(true, 8, 16)),
    mailAddress: useCsInputTextItem("メールアドレス", useInit(""), stringRule(true, 8, 20)),
    age: useCsInputNumberItem("年齢", useInit(), numberRule(true, 1, 150)),
    sex: useCsRadioBoxItem("性別", useInit(""), stringRule(true),
      selectOptionStrings(["男性", "女性", "回答しない"])),
    birthDay: useCsInputDateItem("生年月日", useInit("2023-01-01"), stringRule(true)),
  })
  return (
    <>
      <AxTableLayout
        view={view}
        colSize={2}
      />
  )}
```

![省力化コンポーネントの例](/img/image.png)

## バリデーション定義が簡潔に書ける

省力化コンポーネントを使用することでバリデーション定義が簡潔に書けます。

Item 定義の第 3 引数に、実施したいバリデーションルール（必須、最小文字数、最大文字数）を記載することでバリデーションを定義することができます。

```typescript
// View 定義
export const ConceptApplyedPane: React.FC = () => {
  const view: RegisterUserView = useCsView({
    userName: useCsInputTextItem("ユーザー名", useInit(""), stringRule(true, 3, 30)),
    password: useCsInputPasswordItem("パスワード", useInit(""), stringRule(true, 8, 16)),
  })
  return (
    <>
      <AxTableLayout
        view={view}
        colSize={2}
      />
  )}
```

カスタムバリデーションを用いると「半角数字」、「全角カナ」などより詳細なバリデーションを定義することも可能です。
より詳しい実装方法については[バリデーション](http://localhost:3000/dev-react-cs-document/docs/implementation-guide/validation)を参考にしてください。

## 高機能なボタン

高機能なボタンを利用することで API 呼び出しやバリデーション実行など多様な機能を使用することができます。
View 定義に Item とボタンを定義し、ボタンコンポーネントに必要なパラメータを渡すだけで API 呼び出しやバリデーションを実装できます。
より詳しい実装方法については[ボタンとイベント](http://localhost:3000/dev-react-cs-document/docs/implementation-guide/button-and-event)を参考にしてください。

```typescript
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
    title: useCsInputTextItem(
      "タイトル",
      useInit(""),
      stringRule(true, 1, 100),
      RW.Editable,
      "タイトル"
    ),
    description: useCsInputTextItem(
      "説明",
      useInit(""),
      stringRule(true, 1, 100),
      RW.Editable,
      "説明"
    ),
    createButton: useCsRqMutateButtonClickEvent(usePostTodo()),
  });
};
```

```typescript
<AxMutateButton event={view.createButton} validationViews={[view]}>
  post
</AxMutateButton>
```
