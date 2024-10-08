---
sidebar_position: 1
title: 基本的な画面を作る
---

本節では、省力化コンポーネントを使用して基本的な画面を作る方法について解説します。

## 本節のゴール

本節を通して、以下に示すような入力項目が複数並んだ画面を作ることができるようになります。

![入力項目の定義5個](/img/screen-item-6.png)

## 画面を定義する

### View の型を定義する

まず初めに View の型を定義します。 View は複数の入力項目を含んだ 1 つの画面に相当する概念です。View の役割については[コンセプト](../know-cs-component/basic-concepts.md)を参照してください。

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

Item は 1 つの入力項目に対応する概念で、入力項目に関する情報を Item の中に集約します。入力項目に関する情報の集約については[コンセプト](../know-cs-component/basic-concepts.md)を参照してください。
省力化コンポーネントでは、入力形態に対応する Item クラスが用意されています。使用可能な Item クラス一覧については、リファレンスを参照してください。

### View を初期化するフックを作成する

次に View を初期化するフックを作成します。View を初期化するためには、 `useCsView` というメソッドを使用します。  
`useCsView` の引数には、`useCsXxxItem` というメソッドによって必要なパラメータを設定した Item を渡します。

```tsx title="Viewを初期化するフックを作成する"
const useRegisterUserView = (): RegisterUserView => {
  return useCsView({
    userName: useCsInputTextItem("ユーザー名", useInit(""), stringRule(false)),
    password: useCsInputPasswordItem(
      "パスワード",
      useInit(""),
      stringRule(false)
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
項目の変数名: useCsXxxItem(ラベル名, 初期値, バリデーションルール, 選択肢);
```

:::info
バリデーションルールの詳細については、[バリデーションを定義する](./validation.md)で説明します。
:::

## 入力項目を配置する

画面の定義が終わったら、実際に画面上に入力項目を配置していきます。
入力項目の配置は、一つ一つの入力項目を手動で配置する方法と、`AxTableLayout`というコンポーネントを使用して全ての入力項目を自動で配置する方法の 2 つがあります。

### 手動で配置をする

手動で配置をする場合は、各入力項目の形態に応じた AxXxx コンポーネントを使用します。`item`という Props に、対応する item を指定することで項目を画面上に表示することができます。

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

:::info
手動配置のメリット

- 複雑なレイアウトにも対応できる
- 画面コンポーネントに item 以外の Props を渡すことができる

手動配置のデメリット

- 項目数に伴ってコードの記述量が多くなる
- レイアウトの変更に手間がかかる

  :::

### 自動で配置をする

自動で配置をする場合は、AxTableLayout というコンポーネントを使用します。`view`という Props に表示したい画面の view を、`colSize`という Props に列数を指定することで、画面上の表示が自動で行われます。

```tsx
const view = useRegisterUserView();
return <AxTableLayout view={view} colSize={1} />;
```

:::info
自動配置のメリット

- 項目数が多くても、コードの記述量が多くならない
- `colSize` を変更することでレイアウトが簡単に変えられる

自動配置のデメリット

- 画面コンポーネントに `item` 以外の Props を渡すことができない
- 適用できる場面がシンプルなレイアウトに限定される
  :::
