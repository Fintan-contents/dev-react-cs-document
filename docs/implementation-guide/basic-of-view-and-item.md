---
sidebar_position: 1
title: 基本的な画面を作る
---

本節では、省力化コンポーネントを使用し、以下のような入力項目が 5 つ並んだ画面を作る方法について解説します。

![入力項目の定義6個](/img/screen-item-6.png)

## 入力項目を定義する

・View の型を定義する
View は複数の入力項目を含んだ 1 画面に対応。View の役割についてはコンセプトを参照。

```tsx
type RegisterUserView = CsView & {
  userName: CsInputTextItem;
  password: CsInputPasswordItem;
  mailAddress: CsInputTextItem;
  gender: CsRadioBoxItem;
  birthDay: CsInputDateItem;
};
```

表示したい項目の入力形態を定義する

```tsx
userName: CsInputTextItem;
```

Item は入力項目が持つ情報を集約したオブジェクト。情報の集約に関してはコンセプトを参照。
入力部品の一覧に関しては、一覧表を参照。詳細は Storybook に記載。

・View を初期化するフックスを作る (View, Item, useCs○○Item)

```tsx
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

表示したい入力項目の情報を指定する。

```tsx
userName: useCsInputTextItem(
      "ユーザー名",
      useInit(""),
      stringRule(true, 3, 30, "nameRule")
),
```

```tsx
useCsXxxItem(ラベル名, 初期値, バリデーションルール, 選択肢);
```

バリデーションルールについては# バリデーションの節で詳細に説明します。

＝＝＝
View と Item の関係性を表す図を入れたい！
＝＝＝

## 入力項目を配置する

・手動で配置をする

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

・自動レイアウトを使って配置をする

```tsx
const view = useRegisterUserView();
return <AxTableLayout view={view} colSize={1} />;
```

## Reference

### useCsView

### useCsXxxItem

### 入力部品クラス

### 入力部品コンポーネント
