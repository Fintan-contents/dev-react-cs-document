---
sidebar_position: 1
title: 基本的な画面を作る
---

本節では、省力化コンポーネントを使用して基本的な画面を作る方法について解説します。

## 本節のゴール

本節を通して、以下に示すような入力項目が複数並んだ画面を作ることができるようになります。

![入力項目の定義5個](/img/screen-item-6.png)

## 画面を定義する

省力コンポーネントでは、1 つの画面とそれに紐づいた入力項目を View と Item という概念で定義します。

### View の型を定義する

まず初めに View の型を定義します。 View とは、複数の入力項目を含んだ 1 つの画面に相当する概念です。
// 画面内の Item(入力項目)を View と呼ばれるクラスに集約することで、バリデーションスキーマやレイアウトを自動生成できるようにします。

下の例では、画面内容に応じた名前で `CsView` を拡張した Type を定義しています。
このように、画面ごとに View の型を定義することを推奨しています。

```tsx title="Viewの型を定義する"
type RegisterUserView = CsView & {
  userName: CsInputTextItem;
  password: CsInputPasswordItem;
  mailAddress: CsInputTextItem;
  gender: CsRadioBoxItem;
  birthDay: CsInputDateItem;
};
```

View の定義では、画面に配置する入力項目数分、変数名と Item クラスの型のセットを記述します。

```tsx
type View = CsView & {
  項目の変数名: Itemクラスの型;
  ...
}
```

Item とは 1 つの入力項目に対応する概念であり、ラベルや React の State 変数、バリデーションルールなどが Item の中に集約されます。
省力化コンポーネントでは、テキストフォームやセレクトボックス、チェックボックスなどの入力形態に対応する Item クラスが用意されています。使用可能な Item クラス一覧については、リファレンスを参照してください。

### View を初期化する

View を初期化するためには、 `useCsView` という省力化コンポーネントのフックを使用します。  
`useCsView` の引数には、`useCsXxxItem` というフックによって必要なパラメータを設定した Item を渡します。

下の例では、`useCsView` の結果をカスタムフックとして定義しています。このように、画面に対応した名前のカスタムフックを定義することを推奨しています。

```tsx title="Viewを初期化するフックを作成する"
const useRegisterUserView = (): RegisterUserView => {
  return useCsView({
    userName: useCsInputTextItem("ユーザー名", useInit(""), stringRule(true)),
    password: useCsInputPasswordItem(
      "パスワード",
      useInit(""),
      stringRule(true, 8, 16)
    ),
    mailAddress: useCsInputTextItem(
      "メールアドレス",
      useInit(""),
      stringRule(false)
    ),
    gender: useCsRadioBoxItem(
      "性別",
      useInit(""),
      stringRule(false),
      selectOptionStrings(["男性", "女性", "回答しない"])
    ),
    birthDay: useCsInputDateItem(
      "生年月日",
      useInit("2000-01-01"),
      stringRule(false)
    ),
  });
};
```

Item の初期化は、入力項目に関する情報をパラメータとしてメソッドに渡すことで実現できます。

```tsx
項目の変数名: useCsXxxItem(ラベル名, 初期値, バリデーションルール, 選択肢(※1));

※1 選択肢を持つ入力項目のみ指定します。
```

:::info
Item の初期化にはヘルパ関数を使用します。
ヘルパ関数とは`useInit`や`selectOptionStrings`などです。詳細については、リファレンスを参照してください。
:::

:::info
バリデーションルールの詳細については、[バリデーションを定義する](./validation.md)で説明します。
:::

## 入力項目を配置する

画面の定義が完了したら、画面コンポーネントを使用して入力項目を配置していきます。
入力項目の配置は、`AxTableLayout`というコンポーネントを使用して全ての入力項目を自動で配置する方法と、一つ一つの入力項目に対応する画面コンポーネントを手動で配置する方法の 2 つがあります。

### 自動で配置をする

自動で配置をする場合は、`AxTableLayout` というコンポーネントを使用します。 `view` という Props に、表示したい画面の View の変数を指定します。 `colSize` という Props に、画面内に並べる項目の列数を指定します。

```tsx
const view = useRegisterUserView();
return <AxTableLayout view={view} colSize={2} />;
```

上記のように記述した場合、次のような画面が表示されます。

// ここに画像を挿入する //

### 手動で配置をする

手動で配置をする場合は、各入力項目の Item の型に応じた画面コンポーネントを使用します。 `item` という Props に、対応する入力項目の item の変数 を指定します。
:::note
省力化コンポーネントでは 、UI コンポーネントライブラリごとに専用の画面コンポーネントを提供しています。
それぞれの画面コンポーネントは、以下に示すプレフィックスによって分類されます。

- Ax : Ant Design
- Mx : Material UI
- Bsx : React Bootstrap

実装ガイドでは、Ant Design (Ax から始まる部品) を例として記述しています。

:::

```tsx
const view = useRegisterUserView();
return (
  <>
    <AxInputText item={view.userName} />
    <AxInputPassword item={view.password} />
    <AxInputText item={view.mailAddress} />
    <AxRadioBox item={view.gender} />
    <AxInputDate item={view.birthDay} />
  </>
);
```

上記のように記述した場合、次のような画面が表示されます。

// ここに画像を挿入する //

:::info
自動配置のメリット

- 項目数が多くても、コードの記述量が多くならない
- `colSize` を変更することでレイアウトが簡単に変えられる

手動配置のメリット

- 複雑なレイアウトにも対応できる
- 画面コンポーネントに item 以外の Props を渡すことができる
