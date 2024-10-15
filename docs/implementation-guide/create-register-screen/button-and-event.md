---
sidebar_position: 6
title: APIを呼び出す
---

## API リクエストの設定

`useCsXxxMutateButtonClickEvent`は`setRequest`メソッドを使用することができます。`setRequest`を使用することで、 API 送信時に渡したいリクエストデータを指定することができます。
以下の例では、入力された項目の値をリクエストデータに設定しています。

```tsx
const view = useRegisterUserView();
// リクエストデータに値をセット
view.createButton.setRequest({
  data: {
    userName: view.userName.value ?? "",
    password: view.password.value ?? "",
    gender: view.gender.value ?? "",
    birthDay: view.birthDay.value ?? "",
    terminalNum: view.terminalNum.value ?? "",
  },
});
```

## ボタンを配置する

省力化コンポーネントでは [特徴](../../know-cs-component/features.md#高機能なボタンが使える)で述べたような高機能なボタンを提供しています。高機能なボタンは API 送信に対応した機能を持っています。

以下の例では [自動で入力項目を配置する](./arrange-items.md#自動で入力項目を配置する)で作成した画面にボタンを追加しています。登録 API では`AxMutateButton`を使用します。

```tsx
// 自動で配置している場合
const view = useRegisterUserView();
return (
  <>
    <AxTableLayout view={view} colSize={2} />
    {/* ボタンを追加 */}
    <AxMutateButton event={view.createButton} validationViews={[view]} />
  </>
);
```

`AxMutateButton`には以下の Props を指定しています。

- `event`：`view`で定義したイベント
- `validationViews`：バリデーション対象の`view`

「自動配置でボタンが追加された画面」

## API 送信の確認

API 送信を行う場合はサーバーを起動する必要があります。本節ではモックサーバーの起動による API 送信の確認を行います。

### 1.モックサーバーの起動

:::warning
[モックサーバーの起動](../../introduction-guide/working-after-introduction.md#orval-の設定)を実施されていない方は、まずはそちらの設定を行ってください。
:::

モックサーバーの起動コマンドを実行してください。

```tsx title="モックサーバーの起動"
npm run mockoon
```

### 2. モックサーバーのログを確認

以下のような、入力項目にバリデーションが実施されないような値を入力してください。

「入力項目に適切な値が入力された画像」

入力が確認できたら「登録」ボタンを押下してください。モックサーバーのログを確認し、API が適切に送信されているかの確認をしてください。

:::info
開発者ツールのネットワークタブでも API 送信履歴を確認することができます。
:::

:::info
省力化コンポーネントが提供するボタンには API 送信時にスピナーが表示されます。これにより API の通信中であることを視覚的に理解することができます。

![ボタンデモ](/img/button_demo.gif)
:::
