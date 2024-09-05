---
sidebar_position: 3
title: 特徴
---

# 特徴

省力化コンポーネントには以下の特徴があります。

- <strong>画面項目定義が 1-2 行で書ける</strong>
- <strong>バリデーション定義が簡潔に書ける</strong>
- <strong>高機能なボタンが使える</strong>
- <strong>自動レイアウト機能が使える</strong>

これらの特徴についてそれぞれ説明をします。

## 画面項目定義が 1-2 行で書ける

省力化コンポーネントを使用することで画面項目定義を 1-2 行で書くことができます。

以下の画面を例に考えてみます。

![画面項目定義2個](/img/screen-item-2.png)

画面には 2 種類の画面項目があります。省力化コンポーネントを用いることで、これらの画面項目をそれぞれ 1-2 行で定義することができます。

```tsx
// ＝＝＝＝＝＝＝＝
// 画面項目の定義
// ＝＝＝＝＝＝＝＝
// View 定義
export type RegisterUserView = CsView & {
  userName: CsInputTextItem;
  passowrd: CsInputPasswordItem;
};

export const RegisterUserComponent: React.FC = () => {
  // Viewの作成
  const view: RegisterUserView = useCsView({
    userName: useCsInputTextItem("ユーザー名", useInit(""), stringRule(true, 3, 30)),
    password: useCsInputPasswordItem("パスワード", useInit(""), stringRule(true, 8, 16)),
  });

  // ＝＝＝＝＝＝＝＝
  // 画面項目の配置
  // ＝＝＝＝＝＝＝＝
  return (
    <>
      <AxInputText item={view.userName} />
      <AxInputPassword item={view.password} />
    </>
  );
};
```

このように入力項目を Item で定義し、コンポーネントに Item 情報を渡すことで、ラベル、バリデーションメッセージ表示領域、イベントハンドラなどを持った高機能な入力項目を配置できます。

仮に画面項目を増やしたい場合でも、1 項目につき 1-2 行コードを増やすだけで実装することが可能です。

```tsx
// View 定義
export type RegisterUserView = CsView & {
  userName: CsInputTextItem;
  passowrd: CsInputPasswordItem;
  mailAddress: CsInputTextItem;
  age: CsInputNumberItem;
  gender: CsRadioBoxItem;
  birthDay: CsInputDateItem;
};

export const RegisterUserComponent: React.FC = () => {
  // Viewの作成
  const view: RegisterUserView = useCsView({
    userName: useCsInputTextItem("ユーザー名", useInit(""), stringRule(true, 3, 30)),
    password: useCsInputPasswordItem("パスワード", useInit(""), stringRule(true, 8, 16)),
    mailAddress: useCsInputTextItem("メールアドレス", useInit(""), stringRule(true, 8, 20)),
    age: useCsInputNumberItem("年齢", useInit(), numberRule(true, 0)),
    gender: useCsRadioBoxItem("性別", useInit(""), stringRule(true), selectOptionStrings(["男性", "女性", "回答しない"])),
    birthDay: useCsInputDateItem("生年月日", useInit("2000-01-01"), stringRule(true)),
  });

  return (
    <>
      <AxInputText item={view.userName} />
      <AxInputPassword item={view.password} />
      <AxInputText item={view.mailAddress} />
      <AxInputNumber item={view.age} />
      <AxRadioBox item={view.gender} />
      <AxInputDate item={view.birthDay} />
    </>
  );
};
```

6 項目になった画面は以下の通りです。

![画面項目定義6個](/img/screen-item-6.png)

画面項目を簡潔に書けることで、項目が増えた場合でもコードが冗長にならず、保守性が向上します。

より詳しい実装方法については[View と Item の基本](../implementation-guide/basic-of-view-and-item.md)を参照してください。

## バリデーション定義が簡潔に書ける

省力化コンポーネントを使用することでバリデーション定義が簡潔に書けます。そのため、開発者は複雑なバリデーションロジックを手動で記述する必要がなくなり、View 定義にのみ集中することができます。

Item 定義の第 3 引数に実施したいバリデーションルールを定義します。

```tsx
export const RegisterUserComponent: React.FC = () => {
  // Viewの作成
  const view: RegisterUserView = useCsView({
    // stringRuleでバリデーションを定義
    userName: useCsInputTextItem("ユーザー名", useInit(""), stringRule(true, 3, 30)), 
    // numberRuleでバリデーションを定義
    age: useCsInputNumberItem("年齢", useInit(), numberRule(true, 0)), 
  });
  return <AxLayout colSize={2} view={view} />;
};
```

例えば、`userName` という画面項目に対しては以下のようなバリデーションを定義しています。

`stringRule(true, 3, 30)`

このバリデーションの内容は以下の通りです。

- 文字列型
- 必須
- 最小文字数 3 文字
- 最大文字数 30 文字

このように、標準的なバリデーションルールを簡潔に指定できるため、画面項目のバリデーション定義が効率的に行えます。
また文字列型以外にも数値型のバリデーションや文字列の配列など多様なバリデーションルールを対応する関数`numberRule`や`stringArrayRule`などを使って定義することもできます。

さらに、カスタムバリデーションルールを用いると「半角数字」や「全角文字」など、より詳細なバリデーションを定義することも可能です。これにより、アプリケーション固有の要件に合わせた柔軟なバリデーションが実現できます。

```tsx
export const RegisterUserComponent: React.FC = () => {
  // Viewの作成
  const view: RegisterUserView = useCsView(
    {
      // stringRuleの第4引数にカスタムバリデーションルール名を指定
      userName: useCsInputTextItem("ユーザー名", useInit(""), stringRule(true, 3, 30, "全角文字")),
      password: useCsInputPasswordItem("パスワード", useInit(""), stringRule(true, 8, 16)),
    },
    {
      customRules: globalCustomValidationRules, // バリデーションルールの定義
    }
  );
  return <AxLayout colSize={2} view={view} />;
};
```

この例のように、stringRule や他のルール関数の省略可能な 4 番目の引数としてカスタムバリデーションルールの名前を指定します。

より詳しい実装方法については[バリデーション](../implementation-guide/validation.md)を参照してください。

## 高機能なボタンが使える

高機能なボタンは、アプリケーションのユーザビリティと開発効率を大幅に向上させる重要なコンポーネントです。

ボタンの特徴については以下の通りです。
いずれの機能も従来はボタンの外部やハンドラ内で実装していた処理が内部に組み込まれており、パラメータを渡すだけで簡単に利用できます。

- API 呼び出しの簡略化  
  CRUD（Create, Read, Update, Delete）機能に対応した API 呼び出しがシンプルに実装できます。イベントというクラスに API のフックをパラメータとして渡し、そのイベントをボタンに渡すことで動作します。

- バリデーションの実行  
  ボタンをクリックする前に必要なバリデーションを自動的に実行することができるため、ユーザーからの入力データの整合性を確保することができます。バリデーションを定義した View をボタンに渡すことで動作します。

- メッセージの表示機能  
  ボタン押下後に、メッセージやエラーメッセージを通知することが必要な場合は、メッセージ表示機能を使います。API 呼び出しの結果やバリデーションエラーなどの情報をユーザーに伝えることができます。

- スピナー（Spinner）の表示  
  API 呼び出し中や処理が行われている間に視覚的なフィードバックをユーザーに提供します。これにより、不必要な再操作を防ぐことができます。ボタンに標準で組み込まれているため、実装せずに使用することができます。

View の定義にイベントを定義し、イベントと必要なパラメータをボタンに渡すことで API 送信やバリデーションを実施できます。

```tsx
// Viewの定義
export type RegisterUserView = CsView & {
  title: CsInputTextItem;
  description: CsInputTextItem;
  createButton: CsMutateButtonClickEvent; // イベントの定義
};

// View の初期化
export const usePostTodoView = (): TodosPostView => {
  return useCsView({
    title: useCsInputTextItem("タイトル", useInit(""), stringRule(true, 1, 100), RW.Editable, "タイトル"),
    description: useCsInputTextItem("説明", useInit(""), stringRule(true, 1, 100), RW.Editable, "説明"),
    // ボタンイベントにAPI（usePostTodo）を渡すだけ
    createButton: useCsRqMutateButtonClickEvent(usePostTodo()), 
  });
};
```

上記のコードの`usePostTodo`はTanStack QueryまたはOrvalで自動生成されるコードです。

```tsx
export const PostTodoComponent = () => {
  const view = usePostTodoView();
  // 中略

  // APIのリクエストを設定
  view.createButton.setRequest({
    data: {
      title: view.title.value ?? "",
      description: view.description.value ?? "",
    },
  });

  return (
    <>
      {/* 中略 */}

      <AxMutateButton
        event={view.createButton} // 上記のViewで定義したcreateButtonイベントを渡します。
        validationViews={[view]} // バリデーションしたいViewを渡します。
        successMessage="Todoを追加しました"
        errorMessage="Todoの追加に失敗しました"
      >
        追加
      </AxMutateButton>
    </>
  );
};
```

より詳しい実装方法については[ボタンとイベント](../implementation-guide/button-and-event.md)を参照してください。

## 自動レイアウト機能が使える

省力化コンポーネントは画面項目の自動レイアウトに対応しています。以下に示す画面は、画面項目を横に 2 個ずつ並べた時の画面です。

![自動レイアウト](/img/automatic-layout.png)

次のコードは自動レイアウトの使用例です。

```tsx
<AxTableLayout view={view} colSize={2} />
```

- `view`にレイアウトに並べたい画面項目を含むビューを指定します。
- `colSize`に数値型を指定することで、指定した数値ごとに画面項目を並べることができます。

画面項目の並びが単純で機械的なレイアウトでよい場合は自動レイアウト機能が利用できます。画面項目の並びが複雑な場合やデザインを重視する場合は手動でレイアウトします。

より詳しい実装方法については[自動レイアウト](../implementation-guide/item-and-component.md)を参照してください。
