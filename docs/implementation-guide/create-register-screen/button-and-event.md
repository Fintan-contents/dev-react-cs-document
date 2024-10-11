---
sidebar_position: 7
title: APIを呼び出す
---

ボタン押下時に API を呼び出す方法について説明をします。
省力化コンポーネントで API 通信を行う際には「イベント」といった概念を定義する必要があります。イベントに必要な情報を定義しボタンに渡すことで、ボタン押下時に行いたい API 処理を実装することができます。

更新系の API を呼び出すのか、参照系の API を呼び出すのかで使用する型、フック、ボタンが異なります。下記にそれぞれの対応表について載せています。
| | 更新系＊2 | 参照系＊2 |
| ------ | --------------------------------- | -------------------------------- |
| 型＊1 | `Cs〇〇MutateButtonClickEvent` | `Cs〇〇QueryButtonClickEvent`, `Cs〇〇QueryLoadEvent` |
| フック＊1 | `useCs〇〇MutateButtonClickEvent` | `useCs〇〇QueryButtonClickEvent`, `useCs〇〇QueryLoadEvent` |
| ボタン | `AxMutateButton` | `AxQueryButton`＊3 |

＊1：〇〇に入る文字は API 生成方式で選択した項目によって異なります。Orval を選択された方は 「Ov」、React Query を選択された型は 「Rq」 を使用します。

＊2：更新系では「Mutate」、参照系では「Query」に対応しています。

＊3：`useCs〇〇QueryLoadEvent`を使用する場合はボタンを使用しません。

:::info
本節では登録 API を題材にした API 呼び出し方法の説明をします。CRUD 機能全般について知りたい方は[Todo アプリを作る](../crud-function-implementation.md)を参照してください。
:::

## イベントの型を定義する

更新系の API 処理を行いたい場合は、イベントに`Cs〇〇MutateButtonClickEvent`を指定します。

:::info
本ドキュメントでは Orval を推奨しているため、以降では 「Ov」 を使用した実装例を提供します。
:::

以下の例では [View の型を定義する](./define-screen.md#view-の型を定義する)で定義した View にイベントの型定義を追加しています。

```tsx title="Viewにイベントの型を追加する"
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

イベントの型定義はイベントの変数名と Event のクラス型のセットで記述します。

```tsx
// useCs〇〇MutateButtonClickEvent
イベントの変数名：useCs〇〇MutateButtonClickEvent<リクエストデータ型, レスポンスデータ型, エラー型, コンテキスト型>
```

- API リクエスト時に渡したいリクエストデータ型を指定します。
- API リクエスト成功時に返されるレスポンスデータ型を指定します。
- API リクエスト失敗時に返されるエラー型を指定します。
- API リクエスト時のコンテキスト型を指定します。

:::info
エラー型、コンテキスト型にはデフォルトで`unKnown`型が指定されているため、省略が可能です。
:::

## イベントを View に定義する

イベントはフックを使用することで定義することができます。
更新系の API 処理を行いたい場合は、`useCs〇〇MutateButtonClickEvent`を指定します。

以下の例では、[View の型を定義する](./define-screen.md#view-の型を定義する)で定義した View にイベントの初期化を追加しています。

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
    createButton: useCsOvMutateButtonClickEvent(usePostTodo()), // イベントの初期化処理の追加
  });
};
```

イベントの定義はイベントの変数名と Event のフックのセットで記述します。

```tsx
イベントの変数名: Eventのフック（APIフック）;
```

:::info
API フックには、実施したい API に対応した API フックを使用してください。
本節では Orval で自動生成された API フックを使用しています。
:::

## API リクエストの設定

`Cs〇〇MutateButtonClickEvent`は`setRequest`といったメソッドを使用することができます。`setRequest`を使用することで API 送信時に渡したいリクエストデータを指定することができます。
以下の例では、登録 API 送信時に入力された項目の値をリクエストデータに設定する方法を記載しています。

```tsx
const view = useRegisterUserView();
view.createButton.setRequest({
  data: {
    userName: view.userName.value ?? "", // ユーザー名に入力された値をuserNameにセット
    password: view.password.value ?? "", // パスワードに入力された値をpasswordにセット
    gender: view.gender.value ?? "", // 性別に入力された値をgenderにセット
    birthDay: view.birthDay.value ?? "", // 生年月日に入力された値をbirthDayにセット
    terminalNum: view.terminalNum.value ?? "", // 利用端末数に入力された値をterminalNumにセット
  },
});
```

## ボタンを配置する

省力化コンポーネントでは API 送信に対応したボタンを提供しています。更新系の API では`AxMutateButton`を使用します。

以下の例では [入力フォームを配置する](./arrange-items.md)で配置した Item にボタンを追加しています。

```tsx
// 自動で配置している場合
const view = useRegisterUserView();
return (
  <>
    <AxTableLayout view={view} colSize={2} />;
    <AxMutateButton event={view.createButton} validationViews={[view]} /> // ボタンを追加
  </>
);
```

```tsx
// 手動で配置している場合
const view = useRegisterUserView();
return (
  <>
    <AxInputText item={view.userName} />
    <AxInputPassword item={view.password} />
    <AxRadioBox item={view.gender} />
    <AxInputDate item={view.birthDay} />
    <AxInputNumber item={view.terminalNum} />
    <AxMutateButton event={view.createButton} validationViews={[view]} /> // ボタンを追加
  </>
);
```

「ボタンが追加された画面」

- `AxMutateButton`の引数には View 定義で指定したイベントを渡す必要があります。
- バリデーションを実施したい場合は`validationViews`にバリデーションを実施したい`View`を渡す必要があります。