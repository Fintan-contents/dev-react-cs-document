---
sidebar_position: 7
title: APIを呼び出す
---

省力化コンポーネントを用いて API を呼び出す方法について説明をします。更新系の API を呼び出すのか参照系の API を呼び出すのかで使用する型、フック、ボタンが異なります。下記にそれぞれの対応表について載せています。
| | 更新系 | 参照系 |
| ------ | --------------------------------- | -------------------------------- |
| 型＊1 | `Cs〇〇MutateButtonClickEvent` | `Cs〇〇QueryButtonClickEvent` |
| フック＊1 | `useCs〇〇MutateButtonClickEvent` | `useCs〇〇QueryButtonClickEvent` |
| ボタン | AxMutateButton | AxQueryButton |

＊1：〇〇に入る文字は API 生成方式で選択した項目によって異なります。Orval を選択された方は 「Ov」、React Query を選択された型は 「Rq」 を使用します。

### イベントの型を定義する

省力化コンポーネントで API 通信を行う際にはイベントを定義する必要があります。
更新系の API 処理を行いたい場合は、イベントに`Cs〇〇MutateButtonClickEvent`、参照系 API 処理を行いたい場合は、`Cs〇〇QueryButtonClickEvent`を指定します。

:::info
本ドキュメントでは Orval を推奨しているため、以降では 「Ov」 を使用した実装例を提供します。
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
イベントの初期化は、フックを使用することで実現できます。
更新系の API 処理を行いたい場合はイベントに`useCs〇〇MutateButtonClickEvent`、参照系 API 処理を行いたい場合は`useCs〇〇QueryButtonClickEvent`を定義します。

下の例では、View 定義にイベントの初期化を追加しています。

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
    createButton: useCsOvMutateButtonClickEvent(), // イベントの初期化処理の追加
  });
};
```

```tsx
イベントの変数名: Eventのフック（イベントに対応したAPI）;
```

### ボタンを配置する

画面にボタンを配置する方法について説明をします。

省力化コンポーネントでは API 送信に対応したボタンを提供しています。更新系の API では`AxMutateButton`、参照系の API では`AxQueryButton`を使用します。
登録 API に対応しているコンポーネントは`AxMutateButton`ボタンになります。
下の例では[入力フォームを配置する]で配置した Item にボタンを追加しています。

```tsx
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

`AxMutateButton`の引数には View 定義で使用したイベントを指定する必要があります。またバリデーションを実施したい場合は`validationViews`にバリデーションを実施したい`View`を指定する必要があります。

## 動作確認

これまでの設定によって必要な処理を実装することができました。
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
