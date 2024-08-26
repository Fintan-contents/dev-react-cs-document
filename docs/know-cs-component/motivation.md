---
sidebar_position: 1
title: モチベーション
---

# モチベーション

## 従来の React 開発の冗長さ

React は、Web 画面開発において UI を宣言的に記述できる強力なフレームワークです。React を使用することで複雑なインタラクティブ UI の構築が容易になり、コンポーネントベースの設計によって再利用性や保守性が向上します。その一方で、従来の React 開発では、状態変数やイベントハンドラ、バリデーションスキーマなどが重複して記述される傾向があり、コードの冗長さが目立ちます。以下に挙げる 4 つのポイントは、 React 開発で見られる典型的なコードの冗長さを示しています。

<strong>
1. 分散して記述される入力項目の定義
2. 類似したイベントハンドラの重複
3. 画面全体のバリデーションスキーマの定義
4. 画面項目コンポーネントの外部に記述される処理や定義
</strong>

### 1. 分散して記述される入力項目の定義

1 つの入力項目に対して、状態変数、イベントハンドラ、バリデーション、コンポーネントなどをそれぞれ個別に定義する必要があります。また、それに伴って項目を表す変数名やラベル名を何度も書くことになります。

```tsx
export const OldFashionededPane: React.FC = () => {

  // 1つの入力項目(ユーザー名)に関連する定義が分散している

  // 状態変数
  const [userName, setUserName] = useState("")
 
  // (...省略...)

  // イベントハンドラ
  const onChangeUserName = (e: React.ChangeEvent<HTMLInputElement>) => {
    setUserName(e.target.value)
  }
 
  // (...省略...)

  // バリデーション
  const validationSchema = {
    userName: stringField().required(getRequiredMessage("ユーザー名"))
      .minLength(3, getMinLengthMessage("ユーザー名", 3))
      .maxLength(30, getMaxLengthMessage("ユーザー名", 30)),
 
  // (...省略...)

  // コンポーネント
  <div>
    <Typography.Text>ユーザー名</Typography.Text>
  </div>
  <Input
    value={userName}
    onChange={onChangeUserName}
  />
  <ValidationError message={error.userName} />
```

### 2. 類似したイベントハンドラの重複

入力項目が複数ある場合、変数名や関数名が異なるだけでほとんど同じ内容のイベントハンドラを項目数分記述する必要があります。

```typescript
// 項目数分の類似したイベントハンドラ
const onChangeUserName = (e: React.ChangeEvent<HTMLInputElement>) => {
  setUserName(e.target.value);
};
const onChangePassword = (e: React.ChangeEvent<HTMLInputElement>) => {
  setPassword(e.target.value);
};
const onChangeMailAddress = (e: React.ChangeEvent<HTMLInputElement>) => {
  setMailAddress(e.target.value);
};
const onChangeSex = (e: RadioChangeEvent) => {
  setSex(e.target.value);
};
```

### 3. 画面全体のバリデーションスキーマの定義

バリデーションを実施する場合、画面ごとにバリデーションスキーマの定義を記述する必要があります。記述方法は使用するバリデーションライブラリによって変わってきますが、項目ごとに記述する内容は似通っており(異なるのは項目名やバリデーションルールの設定値のみ)、コードが冗長になってしまいます。

```typescript
// 画面ごとに作成するバリデーションスキーマ定義
const validationSchema = {
  userName: stringField()
    .required(getRequiredMessage("ユーザー名"))
    .minLength(3, getMinLengthMessage("ユーザー名", 3))
    .maxLength(30, getMaxLengthMessage("ユーザー名", 30)),
  password: stringField()
    .required(getRequiredMessage("パスワード"))
    .minLength(8, getMinLengthMessage("パスワード", 8))
    .maxLength(16, getMaxLengthMessage("パスワード", 16)),
  mailAddress: stringField().required(getRequiredMessage("メールアドレス")),
  sex: stringField().required(getRequiredMessage("性別")),
  birthDay: stringField().required(getRequiredMessage("誕生日")),
};
```

### 4. 画面項目コンポーネントの外部に記述される処理や定義

React 開発においては、入力部品やボタンといった画面項目に関連する処理や定義がコンポーネントの外部に分散して記述されることがあります。これにより、コードが冗長になり、保守性や可読性が低下する問題が生じます。以下に、具体的な例を示します。

- 入力部品の場合

一般的な入力項目には、ラベルやバリデーションメッセージの表示領域が必要です。しかし、これらを毎回実装者が個別に記述するのは手間がかかり、コードの冗長さを引き起こします。

```tsx
// ラベル
<div>
  <Typography.Text>ユーザー名</Typography.Text>
</div>
// 入力部品
<Input value="{userName}" onChange="{onChangeUserName}" />
// バリデーションメッセージ領域
<ValidationError message="{error.userName}" />
```

- ボタンの場合

ボタンを押下したときに実行したい処理(バリデーション実行処理や API 呼び出し処理など)は、コールバック関数として定義し、ボタンコンポーネントに渡す必要があります。ボタン押下時に実施したい処理はある程度決まっているため、これらを毎回実装者が個別に記述するのは手間がかかり、コードの冗長さを引き起こします。

```tsx
// ボタン押下時に実行されるコールバック関数
const handleClick = () => {
  // バリデーション実行処理の実装
  // ...
  // API呼び出しの実装
  // ...
  // 成功/失敗のメッセージ表示の実装
  // ...
};

// (...省略...)

// コンポーネント
<Button onClick={handleClick}>送信</Button>;
```

<hr/>
次節で紹介する<strong>省力化コンポーネントのコンセプト</strong>は、上記に挙げた React 開発における冗長さを解消するためのアプローチになります。
