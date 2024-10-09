---
sidebar_position: 3
title: バリデーションを定義する
---

# バリデーションを定義する

## 標準ルールを使ってバリデーションを定義する

Item を初期化する `useCsXxxItem` の第三引数にバリデーションルールを設定することができます。

```tsx
useCsXxxItem(ラベル名, 初期値, バリデーションルール, 選択肢);
```

省力化コンポーネントでは、入力項目の値の種類に応じて 5 つのバリデーションルールメソッドを提供しています。
各バリデーションルールメソッドのシグネチャについてはリファレンスを参照してください。

| ルールメソッド    | 説明                                                                                                             |
| ----------------- | ---------------------------------------------------------------------------------------------------------------- |
| `stringRule`      | 文字列項目のバリデーションルールを定義するためのメソッド                                                         |
| `numberRule`      | 数値項目のバリデーションルールを定義するためのメソッド                                                           |
| `stringArrayRule` | 値を文字列で管理する、複数選択可能なチェックボックスやラジオボタンのバリデーションルールを定義するためのメソッド |
| `numberArrayRule` | 値を数値で管理する、複数選択可能なチェックボックスやラジオボタンのバリデーションルールを定義するためのメソッド   |
| `booleanRule`     | 単一のチェックボックスのバリデーションルールを定義するためのメソッド                                             |

バリデーションルールメソッドの基本的なシグネチャは以下の通りです。

```tsx
xxxRule(必須指定、最小値(※1)、最大値(※1)、カスタムバリデーションルール名(※2))

※1 最小値と最大値は stringRule と numberRule のみ設定可能です。
※2 カスタムバリデーションルール名については次節で解説します。
```

前節で作成した画面項目に対して、バリデーションルールを設定すると以下のようになります。

```tsx title="各項目にバリデーションルールを設定する"
ronst useRegisterUserView = (): RegisterUserView => {
  return useCsView({
    userName: useCsInputTextItem(
      "ユーザー名",
      useInit(""),
      stringRule(true, 3, 30)
    ),
    password: useCsInputPasswordItem(
      "パスワード",
      useInit(""),
      stringRule(true, 8, 16)
    ),
    mailAddress: useCsInputTextItem(
      "メールアドレス",
      useInit(""),
      stringRule(true, 8, 20)
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
  });
};
```

上の例では、「ユーザ名」に対して `stringRule(true, 3, 30)` が設定されているため、必須入力でかつ文字数は 3 文字以上 30 文字以下でなければならない、というバリデーションルールが適用されます。

## カスタムのバリデーションルールを使う

省力化コンポーネントでは、標準のバリデーションルールに加えて、独自のカスタムバリデーションルールを定義して使用することができます。これにより、特定のビジネスロジックや要件に応じた柔軟なバリデーションを実現できます。

この節では、カスタムバリデーションルールの定義方法と、それを入力項目に適用する方法について詳しく解説します。具体的な例を通して、カスタムバリデーションルールの作成と使用方法を理解できます。

```tsx
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
});
```

```tsx
// カスタムバリデーションルールの定義
const customValidationRules: CustomValidationRules = {
  // シンプルなルール
  nameRule: customValidationRule<string>(
    createRegExpValidator(/^[A-Za-z ]*$/),
    (label) => `${label}は、アルファベットと空白のみ使用可能です。`
  ),
  // 複雑なルール
  passwordRule: customValidationRule<string>(
    (newValue, item) => {
      if (!newValue) {
        return true;
      }
      let count = 0;
      if (/[A-Z]/.test(newValue)) {
        count++;
      }
      if (/[a-z]/.test(newValue)) {
        count++;
      }
      if (/[0-9]/.test(newValue)) {
        count++;
      }
      if (/[!-)+-/:-@[-`{-~]/.test(newValue)) {
        count++;
      }
      return count >= 4;
    },
    (label, newValue, item) => {
      let requireds = ["大文字", "小文字", "数字", "記号"];
      if (/[A-Z]/.test(newValue)) {
        requireds = requireds.filter((e) => "大文字" !== e);
      }
      if (/[a-z]/.test(newValue)) {
        requireds = requireds.filter((e) => "小文字" !== e);
      }
      if (/[0-9]/.test(newValue)) {
        requireds = requireds.filter((e) => "数字" !== e);
      }
      if (/[!-)+-/:-@[-`{-~]/.test(newValue)) {
        requireds = requireds.filter((e) => "記号" !== e);
      }
      return `${label}は、${requireds.join("、")}を含めてください`;
    }
  ),
};
```

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
