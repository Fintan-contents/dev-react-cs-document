---
sidebar_position: 7
title: APIを呼び出す
---

ボタン押下時に API を呼び出す方法について説明をします。
省力化コンポーネントで API 通信を行う際には「イベント」といった概念を定義する必要があります。イベントに必要な情報を定義しボタンに渡すことで、ボタン押下時に行いたい API 処理を実装することができます。

更新系の API を呼び出すのか、参照系の API を呼び出すのかで使用する型、フック、ボタンが異なります。下記にそれぞれの対応表について載せています。
| | 更新系＊2 | 参照系＊2 |
| ------ | --------------------------------- | -------------------------------- |
| 型＊1 | `Cs〇〇MutateButtonClickEvent` | `Cs〇〇QueryButtonClickEvent` |
| フック＊1 | `useCs〇〇MutateButtonClickEvent` | `useCs〇〇QueryButtonClickEvent` |
| ボタン | `AxMutateButton` | `AxQueryButton` |

＊1：〇〇に入る文字は API 生成方式で選択した項目によって異なります。Orval を選択された方は 「Ov」、React Query を選択された型は 「Rq」 を使用します。

＊2：更新系では「Mutate」、参照系では「Query」に対応しています。

## イベントの型を定義する

更新系の API 処理を行いたい場合は、イベントに`Cs〇〇MutateButtonClickEvent`、参照系 API 処理を行いたい場合は、`Cs〇〇QueryButtonClickEvent`を指定します。

:::info
本ドキュメントでは Orval を推奨しているため、以降では 「Ov」 を使用した実装例を提供します。
:::

下の例では [View の型を定義する](./define-screen.md#view-の型を定義する)で定義した View にイベントの型を追加しています。本節では登録 API を実装したいため、更新系である`CsOvMutateButtonClickEvent`を指定します。

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

## イベントを View に定義する

イベントを View に定義する方法について説明をします。
イベントの定義は、フックを使用することで実現することができます。
更新系の API 処理を行いたい場合は、イベントに`useCs〇〇MutateButtonClickEvent`、参照系 API 処理を行いたい場合は、`useCs〇〇QueryButtonClickEvent`を定義します。

下の例では、[View の型を定義する](./define-screen.md#view-の型を定義する)で定義した View にイベントの初期化を追加しています。

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
イベントの変数名: Eventのフック（イベントに対応したAPI）;
```

:::info
イベントに対応した API には、実施したい API に対応した API フックを使用してください。
本節では Orval で自動生成された API フックを使用しています。
:::

## ボタンを配置する

画面にボタンを配置する方法について説明をします。

省力化コンポーネントでは API 送信に対応したボタンを提供しています。更新系の API では`AxMutateButton`、参照系の API では`AxQueryButton`を使用します。

下の例では [入力フォームを配置する](./arrange-items.md)で配置した Item にボタンを追加しています。


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

- `AxMutateButton`の引数には View 定義で使用したイベントを指定する必要があります。
- バリデーションを実施したい場合は`validationViews`にバリデーションを実施したい`View`を指定する必要があります。

## 動作確認

これまでの設定によって必要な処理を実装することができました。
ここからは設定した内容についての動作確認を行っていきます。

### バリデーションの実施

[バリデーションを定義する](./define-validation.md)で設定した内容についての動作確認を行います。
入力項目には何も設定せずに「登録」ボタンを押下してください。以下のような画面が表示されれば成功です。
「バリデーションが実施された画像」

### API 送信の確認

指定した API が送信されているかの動作確認を行います。API 通信が適切に行えているかを確認する方法は、「開発者ツールで確認する」場合と「モックサーバーで確認する」場合の 2 通りがあります。

以下のように、入力項目にバリデーションが実施されないような値を入力してください。
「入力項目に適切な値が入力された画像」

- 開発者ツールで確認する
  :::info
  本ドキュメントでは「Google Chrome」を使用した画面例を示します。
  :::
  画面を表示している状態で開発者ツールを開き、ネットワークタブを選択してください。ネットワークタブを開いた状態で「登録」ボタンを押下してください。ネットワークに以下のような通信履歴が記述されていたら成功です。

  「ネットワークタブの画像」

「Headers」タグではリクエスト情報、「Payload」タグではリクエスト時に送信されたデータを確認することができます。必要に応じて確認を行ってください。

- モックサーバーで確認する

モックサーバーで、、


### API 送信の確認
省力化コンポーネントではAPI送信時にスピナーが表示されます。これによりAPIの通信中であることを視覚的に理解することができます。
![ボタンデモ](/img/button_demo.gif)

---

動作確認については以上になります。
[本節のゴール](./goal.md)で示した内容が実現されているかどうか改めてご確認をお願いいたします。
