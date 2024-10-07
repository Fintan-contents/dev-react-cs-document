---
sidebar_position: 2
title: バリデーションを定義する
---

## 標準ルールを使ってバリデーションを定義する

```tsx
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
});
```

| ルール名          | 説明                                                                                                             |
| ----------------- | ---------------------------------------------------------------------------------------------------------------- |
| `stringRule`      | 文字列項目のバリデーションルールを定義するためのメソッド                                                         |
| `numberRule`      | 数値項目のバリデーションルールを定義するためのメソッド                                                           |
| `stringArrayRule` | 値を文字列で管理する、複数選択可能なチェックボックスやラジオボタンのバリデーションルールを定義するためのメソッド |
| `numberArrayRule` | 値を数値で管理する、複数選択可能なチェックボックスやラジオボタンのバリデーションルールを定義するためのメソッド   |
| `booleanRule`     | 単一のチェックボックスのバリデーションルールを定義するためのメソッド                                             |

## カスタムのバリデーションルールを使う

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

## バリデーションを実行する

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

### フォーカスアウトでバリデーションを実施する

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
