---
sidebar_position: 2
title: 画面を定義する
---

省力化コンポーネントでは、1 つの画面とその画面内に含まれる入力項目を View と Item という概念で定義します。

### View の型を定義する

まず初めに View の型を定義します。 View とは、複数の入力項目を含んだ 1 つの画面に対応する概念です。
画面内の情報を View に集約することで、バリデーションスキーマの自動生成や画面項目の自動配置が可能となります。

下の例では、画面内容に応じた名前 `RegisterUserView` で `CsView` を拡張した Type を定義しています。
このように、画面ごとに View の型を定義することを推奨しています。

```tsx title="Viewの型を定義する"
type RegisterUserView = CsView & {
  userName: CsInputTextItem;
  password: CsInputPasswordItem;
  gender: CsRadioBoxItem;
  birthDay: CsInputDateItem;
  terminalNum: CsInputNumberItem;
};
```

View の型定義では、画面に配置する入力項目数分、変数名と Item クラスの型のセットを記述します。

```tsx
type XxxView = CsView & {
  項目の変数名: Itemクラスの型;
  ...
}
```

ここで、Item とは、 1 つの入力項目に対応する概念です。ラベルや React の State 変数、バリデーションルールなどの入力項目に関する情報が Item の中に集約されます。  
省力化コンポーネントでは、テキストフォームやラジオボタン、チェックボックスなどの様々な入力形態に対応する Item クラスが用意されています。使用可能な Item クラス一覧については、[リファレンス](../reference/_category_.json)を参照してください。

### View を初期化する

View を初期化するためには、 `useCsView` という省力化コンポーネントのフックを使用します。 `useCsView` の引数には、`useCsXxxItem` というフックによって必要なパラメータを設定した Item を渡します。

下の例では、`useCsView` の結果を返すカスタムフック `useRegisterUserView` を定義しています。このように、画面に対応した名前のカスタムフックを定義することを推奨しています。

```tsx title="Viewを初期化するフックを作成する"
const useRegisterUserView = (): RegisterUserView => {
  return useCsView({
    userName: useCsInputTextItem(
      "ユーザー名",
      useInit(""),
      stringRule(true, 3, 30, "nameRule")
    ),
    password: useCsInputPasswordItem(
      "パスワード",
      useInit(""),
      stringRule(true, 8, 16, "passwordRule")
    ),
    gender: useCsRadioBoxItem(
      "性別",
      useInit(""),
      stringRule(true),
      selectOptionStrings(["男性", "女性", "回答しない"])
    ),
    birthDay: useCsInputDateItem(
      "生年月日",
      useInit("2000-01-01"),
      stringRule(true)
    ),
    terminalNum: useCsInputNumberItem(
      "利用端末数",
      useInit(),
      numberRule(false, 1, 10)
    ),
  });
};
```

Item の初期化は、入力項目に関する情報をパラメータとして `useCsXxxItem` に渡すことで実現できます。

```tsx
項目の変数名: useCsXxxItem(ラベル名, 初期値, バリデーションルール, 選択肢(※1));

※1 選択肢を持つ入力項目のみ指定します。
```

:::info
`useCsXxxItem` および、その中で使用されているヘルパ関数( `useInit` や `selectOptionStrings` )のシグネチャについては[リファレンス](../reference/_category_.json)を参照してください。
:::

:::info
バリデーションルールの詳細については、[バリデーション](./validation.md)で説明します。
:::
