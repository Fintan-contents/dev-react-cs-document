---
sidebar_position: 6
title: APIを呼び出す
---

## モックサーバーを起動する

:::warning
[Orval の設定](../../introduction-guide/working-after-introduction.md#orval-の設定)を実施されていない方は、まずはそちらの設定を行ってください。
:::

モックサーバーの起動方法について説明をします。

## ボタンを配置する

### イベントの型を定義する

省力化コンポーネントで API 通信を行う際にはイベントを定義する必要があります。
更新系の API 処理を行いたい場合は、イベントに`Cs〇〇MutateButtonClickEvent`、参照系 API 処理を行いたい場合は`Cs〇〇QueryButtonClickEvent`を指定します。

:::info
Cs 〇〇については API 生成方式で選択した項目によって異なります。Orval を選択された方は 「Ov」、React Query を選択された型は 「Rq」 が入ります。本ドキュメントでは Orval を推奨しているため、以降では 「Ov」 を使用した実装例を提供します。
:::

下の例では View にイベントの型を指定しています。本節では登録 API を実装したいため、更新系である`CsOvMutateButtonClickEvent`を指定します。

```tsx title="Viewにイベントの型を追加する"
type RegisterUserView = CsView & {
  userName: CsInputTextItem;
  password: CsInputPasswordItem;
  gender: CsRadioBoxItem;
  birthDay: CsInputDateItem;
  terminalNum: CsInputNumberItem;
  createButton: CsOvMutateButtonClickEvent; // イベントの型定義を追加
};
```

イベントの型定義はイベントの変数名と Event のクラス型のセットで記述します。

```tsx
イベントの変数名：Eventのクラス型；
```

:::info
イベントの Event クラスの型は以下の～点あります。
詳しい内容はリファレンスを参照
:::

### イベントを View に定義する

イベントを View に定義する方法について説明をします。
イベントの初期化は、フックを使用することで定義することができます。
更新系の API 処理を行いたい場合はイベントに`useCs〇〇MutateButtonClickEvent`、参照系 API 処理を行いたい場合は`useCs〇〇QueryButtonClickEvent`を定義します。

:::info
〇〇についてはイベントの型を定義するの INFO で述べた内容と同様です。
型定義とフックの対応については[リファレンス](../reference/_category_.json)を参照してください。
:::

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
    createButton: useCsOvMutateButtonClickEvent(),
  });
};
```

```tsx
イベントの変数名: Eventのフック（イベントに対応したAPI）;
```

### ボタンを配置する

省力化コンポーネントでは API 送信に対応したボタンを提供しています。更新系の API では`AxMutateButton`、参照系の API では`AxQueryButton`を使用します。
登録 API に対応しているコンポーネントは`AxMutateButton`ボタンになります。
下の例で

:::info
更新系と参照系によって使用する型、フック、ボタンに違いが出てきます。それぞれの対応表については[リファレンス](../reference/_category_.json)を参照してください。
:::

## 動作確認
これまでの設定によって必要な処理を定義することができました。
本節では設定した内容に基づいて動作確認を行っていきます。

### バリデーションの実施
「バリデーションの定義」で設定した内容について確認をします。

### API 送信の確認

API 送信は開発者ツールで確認する場合とモックサーバーで確認する方法があります。

### 開発者ツール

### モックサーバー

##

###

```

```