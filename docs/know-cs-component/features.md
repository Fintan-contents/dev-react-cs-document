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

画面には 2 種類の入力項目があります。省力化コンポーネントを用いるとこれらの項目をそれぞれ 1-2 行で定義することができます。
userName や password が画面の各項目と対応しています。

```typescript
// View 定義
export const ConceptApplyedPane: React.FC = () => {
  const view: RegisterUserView = useCsView({
    userName: useCsInputTextItem("ユーザー名", useInit(""), stringRule(true, 3, 30)),
    password: useCsInputPasswordItem("パスワード", useInit(""), stringRule(true, 8, 16)),
  })
}
```

仮に入力項目を増やしたい場合でも、1 項目につき 1-2 行コードを増やすだけで実装できます。
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
}
```

![省力化コンポーネントの例](/img/image.png)

## バリデーション定義が簡潔に書ける

省力化コンポーネントを使用することでバリデーション定義が簡潔に書けます。これにより、開発者は複雑なバリデーションロジックを手動で記述する必要がなく、コードの保守性が向上します。
具体的には、Item定義の第3引数に実施したいバリデーションルールを記載する形でバリデーションを設定します。例えば、userNameという入力項目に対しては、以下のようなバリデーションを定義することが可能です。

文字列型で必須
最小文字数3文字
最大文字数30文字

このように、標準的なバリデーションルールを簡潔に指定できるため、入力項目のバリデーション設定が効率的に行えます。

さらに、カスタムバリデーションを用いると「半角数字」や「全角カナ」など、より詳細なバリデーションを定義することも可能です。これにより、アプリケーション固有の要件に合わせた柔軟なバリデーションが実現できます。

例として、userNameのバリデーションで「半角数字のみ許可」や「全角カナのみ許可」といったカスタムバリデーションを追加することができ、入力データの品質を高めることができます。

以上のように、画面項目定義を適切に行うことで、開発効率の向上と入力データの品質向上が期待できます。

```typescript
// View 定義
export const ConceptApplyedPane: React.FC = () => {
  const view: RegisterUserView = useCsView({
    userName: useCsInputTextItem("ユーザー名", useInit(""), stringRule(true, 3, 30)),
    password: useCsInputPasswordItem("パスワード", useInit(""), stringRule(true, 8, 16)),
  })
}
```

より詳しい実装方法については[バリデーション](http://localhost:3000/dev-react-cs-document/docs/implementation-guide/validation)を参考にしてください。

## 高機能なボタン

高機能なボタンは、アプリケーションのユーザビリティと開発効率を大幅に向上させるための重要なコンポーネントです。以下に、その特徴を詳述します。
- API呼び出し

CRUD（Create, Read, Update, Delete）機能に対応したAPI呼び出しがシンプルに実装できます。これにより、データの操作が直感的に行えるため、開発の手間が大幅に削減されます。
- バリデーションの実行

ボタンをクリックする前に必要なバリデーションを自動的に実行することができるため、ユーザーからの入力データの整合性を確保することができます。
- スピナー（Spinner）の表示

API呼び出しの結果やバリデーションエラーなどの情報をユーザーにわかりやすく伝えることができます。これにより、ユーザーは次に何をすべきかを直感的に理解できます。
- メッセージの表示機能

API呼び出しの結果やバリデーションエラーなどの情報をユーザーにわかりやすく伝えることができます。これにより、ユーザーは次に何をすべきかを直感的に理解できます。



以上のように、高機能なボタンは、API呼び出しの簡便化、バリデーションの実行、スピナーの表示、メッセージの表示といった多機能を備えており、開発効率を上げることができます。

高機能なボタンを利用することで API 呼び出しやバリデーション実行など多様な機能を使用することができます。
View 定義に Item とボタンを定義し、ボタンコンポーネントに必要なパラメータを渡すだけで API 呼び出しやバリデーションを実装できます。

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

より詳しい実装方法については[ボタンとイベント](http://localhost:3000/dev-react-cs-document/docs/implementation-guide/button-and-event)を参考にしてください。
