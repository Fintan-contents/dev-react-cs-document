---
sidebar_position: 6
title: APIを呼び出す
---

## モックサーバーの起動

:::warning
[Orval の設定](../../introduction-guide/working-after-introduction.md#orval-の設定)を実施されていない方は、まずはそちらの設定を行ってください。
:::

API を呼び出す際にはモックサーバーを起動する必要があります。[Orval の設定](../../introduction-guide/working-after-introduction.md#orval-の設定)
で設定したモックサーバーの起動コマンドを実行します。

```tsx
npm run mockoon
```

## イベントについて

省力化コンポーネントで API 通信を行う際には「イベント」を定義する必要があります。イベントに必要な情報を定義しボタンに渡すことで、ボタン押下時に行いたい API 処理を実装することができます。

更新系の API を呼び出すのか、参照系の API を呼び出すのかで使用する型、フック、ボタンが異なります。下記にそれぞれの対応表について載せています。
| | 更新系＊2 | 参照系＊2 |
| ------ | --------------------------------- | -------------------------------- |
| 型＊1 | `CsXxMutateButtonClickEvent` | `CsXxQueryButtonClickEvent`, `CsXxQueryLoadEvent` |
| フック＊1 | `useCsXxMutateButtonClickEvent` | `useCsXxQueryButtonClickEvent`, `useCsXxQueryLoadEvent` |
| ボタン | `AxMutateButton` | `AxQueryButton`＊3 |

＊1：Xx に入る文字は API 生成方式で選択した項目によって異なります。Orval を選択された方は 「Ov」、React Query を選択された型は 「Rq」 を使用します。

＊2：更新系では「Mutate」、参照系では「Query」に対応しています。

＊3：`useCsXxQueryLoadEvent`を使用する場合はボタンを使用しません。

:::info
本節では登録 API を題材にした API 呼び出し方法の説明をします。CRUD 機能全般について知りたい方は[Todo アプリを作る](../crud-function-implementation.md)を参照してください。
:::

## イベントの型を定義する

:::info
本ドキュメントでは Orval を推奨しているため、以降では 「Ov」 を使用した実装例を提供します。
:::

以下の例では [View の型を定義する](./define-screen.md#view-の型を定義する)で定義した View の型定義 にイベントの型定義を追加しています。
登録 API では、イベントに`CsOvMutateButtonClickEvent`を指定します。

```tsx title="Viewにイベントの型を追加する"
// UserRegistration型を定義
type UserRegistration = {
  // UserRegistration型の詳細を記述
};

type RegisterUserView = CsView & {
  userName: CsInputTextItem;
  password: CsInputPasswordItem;
  gender: CsRadioBoxItem;
  birthDay: CsInputDateItem;
  terminalNum: CsInputNumberItem;
  // イベントの型定義を追加
  createButton: CsOvMutateButtonClickEvent<
    {
      data: UserRegistration; // APIのリクエストデータ型を定義
    },
    void // APIのレスポンスデータ型を定義
  >;
};
```

イベントの型定義はイベントの変数名と Event のクラス型のセットで記述します。

```tsx
// CsXxMutateButtonClickEvent
イベントの変数名：CsXxMutateButtonClickEvent<リクエストデータ型, レスポンスデータ型, エラー型, コンテキスト型>
```

:::info
エラー型、コンテキスト型にはデフォルトで`unknown`型が指定されているため省略が可能です。
:::

## イベントを View に定義する

イベントはフックを使用することで定義することができます。

以下の例では、[View の型を定義する](./define-screen.md#view-の型を定義する)で定義した View にイベントの初期化処理を追加しています。登録 API では、`CsOvMutateButtonClickEvent`を指定します。

```tsx title="Viewを初期化するフックを作成する"
// Orvalで自動生成されたAPIフック（usePostTodo）をimport

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

イベントの初期化はイベントの変数名と Event のフックのセットで記述します。

```tsx
イベントの変数名: Eventのフック（APIフック）;
```

:::info
API フックには、実施したい API に対応した API フックを使用してください。
本節では Orval で自動生成された登録 API のフックを使用しています。
:::

## API リクエストの設定

`CsXxMutateButtonClickEvent`は`setRequest`メソッドを使用することができます。`setRequest`を使用することで、 API 送信時に渡したいリクエストデータを指定することができます。
以下の例では、登録 API 送信時に入力された項目の値をリクエストデータに設定しています。

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

省力化コンポーネントでは [特徴](../../know-cs-component/features.md#高機能なボタンが使える)で述べたような高機能なボタンを提供しています。高機能なボタンは API 送信に対応した機能を持っています。

以下の例では [入力フォームを配置する](./arrange-items.md)で配置した Item にボタンを追加しています。登録 API では`AxMutateButton`を使用します。

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
    {/* ボタンを追加 */}
    <AxMutateButton event={view.createButton} validationViews={[view]} />
  </>
);
```

`AxMutateButton`には以下の Props を指定しています。

- `event`：`view`で定義したイベント
- `validationViews`：バリデーション対象の`view`

「ボタンが追加された画面」
