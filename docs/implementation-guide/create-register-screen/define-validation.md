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

前節で定義した画面項目に対して設定されたバリデーションルールは以下の表の通りです。

```tsx title="前節で定義した画面項目"
const useRegisterUserView = (): RegisterUserView => {
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

| 項目名     | バリデーションルール                    |
| ---------- | --------------------------------------- |
| ユーザー名 | 入力必須、3 文字以上 30 文字以下        |
| パスワード | 入力必須、8 文字以上 16 文字以下        |
| 性別       | 入力必須                                |
| 生年月日   | 入力必須                                |
| 利用端末数 | 入力任意、入力する場合は 1 以上 10 以下 |

## カスタムルールを使ってバリデーションを定義する

省力化コンポーネントでは、標準のバリデーションルールに加えて、独自のカスタムバリデーションルールを定義して使用することができます。これにより、特定のビジネスロジックや要件に応じた柔軟なバリデーションを実現できます。

:::info
新しくカスタムバリデーションルールを作る方法については、拡張ガイドを参照してください。  
ここでは既に作成済みのカスタムバリデーションルールを使用する方法についてのみ解説します。
:::

省力化コンポーネントでは、使用頻度の高いカスタムバリデーションルールを`buildInCustomValidationRules`として提供しています。`buildInCustomValidationRules` が提供するカスタムバリデーションルールの一覧はリファレンスを参照してください。

カスタムバリデーションルールを使用するためには、`useCsView` の第二引数にカスタムバリデーションルールオブジェクトを指定する必要があります。

```tsx
import { buildInCustomValidationRules } from "//FIXME";

return useCsView(
  {
    userName: useCsInputTextItem(
      "ユーザー名",
      useInit(""),
      stringRule(true, 3, 30)
    ),
    // (...他の画面項目定義...)
  },
  {
    customValidationRules: buildInCustomValidationRules,
  }
);
```

カスタムバリデーションルールは、以下のようにキー(ルール名)とバリュー(ルール)のセットで定義されています。

```tsx
export const buildInCustomValidationRules = {
  半角数字: stringRule(
    createRegExpValidator(/^\d*$/),
    (label: string, _: string) => "半角数字で入力してください。"
  ),
  半角英字: stringRule(
    createRegExpValidator(/^[a-zA-Z]*$/),
    (label: string, _: string) => "半角英字で入力してください。"
  ),
  半角英数字: stringRule(
    createRegExpValidator(/^[a-zA-Z0-9]*$/),
    (label: string, _: string) => "半角英数字で入力してください。"
  ),
  // (...省略...)
};
```

カスタムバリデーションルールを使用する場合は、使用したいルールのキーを、`useCsXxxItem` の中で使用している標準バリデーションルールメソッドの第四引数に指定してください。

```tsx
return useCsView(
  {
    userName: useCsInputTextItem(
      "ユーザー名",
      useInit(""),
      stringRule(true, 3, 30),
      "半角英数字" // 第四引数にカスタムバリデーションルールのキーを指定
    ),
    // (...他の画面項目定義...)
  },
  {
    customValidationRules: buildInCustomValidationRules,
  }
);
```

## バリデーションの実施方法

### ボタン押下でバリデーションを実施する

引数として vIEW を受け取るボタンコンポーネントを使用する
AntDesign の場合は AxButton, AxMutateButton, AxQueryButton の 3 種類がそれにあたる。
※AxMutateButton, AxQueryButton は API を呼び出すためのボタンであるため、後続の章で解説。
バリデーションを実施したいだけであれば AxButton を使用することで実現できる。

```tsx
<AxButton validationViews={[view]}>
  バリデーションを実行するためのボタン
</AxButton>
```

### フォーカスが外れたタイミングでバリデーションを実施する

`useCsView` の第二引数に `validationTrigger: "onBlur"` を指定することで、各入力フォームのフォーカスが外れたタイミングでバリデーションを実施することができるようになります。

```tsx
return useCsView(
  {
    userName: useCsInputTextItem(
      "ユーザー名",
      useInit(""),
      stringRule(true, 3, 30)
    ),
    // (...他の画面項目定義...)
  },
  {
    validationTrigger: "onBlur",
  }
);

// GIFを入れる //
```
