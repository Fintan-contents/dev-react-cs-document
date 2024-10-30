---
sidebar_position: 5
title: 5. APIイベントを定義する
---

省力化コンポーネントで API 通信を行う際は「イベント」を定義する必要があります。ここでいう「イベント」とは、特定の操作（ボタンのクリックなど）をトリガーとして実行される API 処理（リクエストの送信、レスポンスの処理など）をまとめたものです。イベントに必要な情報を定義しボタンに渡すことで、ボタン押下時に行いたい API 処理を実装することができます。

更新系の API（POST, PUT, DELETE) を呼び出すのか、参照系の API (GET, 検索系）を呼び出すのかで使用する型、フック、ボタンが異なります。下記にそれぞれの対応表について載せています。
| | 型*¹ | フック*¹ | ボタン |
| ------ | ----------------------------- | -------------------------------- | ---------------- |
| GET | `CsXxxQueryLoadEvent` | `useCsXxxQueryLoadEvent` | なし |
| POST, PUT, DELETE | `CsXxxMutateButtonClickEvent` | `useCsXxxMutateButtonClickEvent` | `AxMutateButton` |
| 検索系 | `CsXxxQueryButtonClickEvent` | `useCsXxxQueryButtonClickEvent` | `AxQueryButton` |

＊1：Xxx に入る文字は API 生成方式で選択した項目によって異なります。[API 呼び出し方式の選択](../../introduction-guide/introduction-tool.md#api-呼び出し方式の選択)で「Orval(シンプル版)」もしくは「TanStack Query」 を選択された方は「Rq」、「Orval(拡張版)」 を選択された方は 「RqA」 を使用します。

:::info
本節では登録 API を題材にした API 呼び出し方法の説明をします。CRUD 機能全般について知りたい方は[Todo アプリを作る](../crud-function-implementation.md)を参照してください。
:::

## ハンズオンの事前準備

-

## イベントの型を定義する

:::info
本ドキュメントでは 「Orval（シンプル版）」 を推奨しているため、以降では 「Rq」 を使用した実装例を提供します。
:::
イベントの型定義はイベントの変数名と Event のクラス型のセットで記述します。

```tsx
イベントの変数名：CsXxxMutateButtonClickEvent<リクエストデータ型, レスポンスデータ型, エラー型*¹, コンテキスト型*¹>

＊1：エラー型、コンテキスト型にはデフォルトで`unknown`型が指定されているため省略が可能です。
```

以下の例では [View の型を定義する](./define-screen.md#view-の型を定義する)で定義した View の型定義 にイベントの型定義を追加しています。
登録 API ではイベントに`CsRqMutateButtonClickEvent`を指定します。

```tsx title="Viewにイベントの型を追加する"
// Orvalで自動生成されたUserRegistrationの型定義をimport

type RegisterUserView = CsView & {
  userName: CsInputTextItem;
  password: CsInputPasswordItem;
  gender: CsRadioBoxItem;
  birthDay: CsInputDateItem;
  terminalNum: CsInputNumberItem;
  // イベントの型定義を追加
  createButton: CsRqMutateButtonClickEvent<
    {
      data: UserRegistration; // APIのリクエストデータ型を定義
    },
    void // APIのレスポンスデータ型を定義
  >;
};
```

## イベントを View に定義する

イベントの初期化はイベントの変数名と Event のフックのセットで記述します。Event の引数には API を呼び出す API フックを指定する必要があります。

```tsx
イベントの変数名: Eventのフック（APIフック）;
```

以下の例では、[View の初期化を定義する](./define-screen.md#view-の初期化を定義する)で定義した View にイベントの初期化処理を追加しています。登録 API では Event のフックに`useCsRqMutateButtonClickEvent()`、API フックに`usePostUserInfo()`を指定します。

```tsx title="Viewを初期化する"
// Orvalで自動生成されたAPIフック（usePostUserInfo）をimport

const useRegisterUserView = (): RegisterUserView => {
  return useCsView({
    userName: useCsInputTextItem(
      "ユーザー名",
      useInit(""),
      stringRule(true, 3, 30, "nameRule"),
    ),
    password: useCsInputPasswordItem(
      "パスワード",
      useInit(""),
      stringRule(true, 8, 16, "passwordRule"),
    ),
    gender: useCsRadioBoxItem(
      "性別",
      useInit(""),
      stringRule(true),
      selectOptionStrings(["男性", "女性", "回答しない"]),
    ),
    birthDay: useCsInputDateItem(
      "生年月日",
      useInit("2000-01-01"),
      stringRule(true),
    ),
    terminalNum: useCsInputNumberItem(
      "利用端末数",
      useInit(),
      numberRule(false, 1, 10),
    ),
    createButton: useCsRqMutateButtonClickEvent(usePostUserInfo()), // イベントの初期化処理の追加
  });
};
```

:::info
API フックには、実施したい API に対応した API フックを使用してください。
本節では Orval で自動生成された登録 API のフックを使用しています。
:::
