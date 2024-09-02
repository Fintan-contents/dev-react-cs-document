---
sidebar_position: 2
---

# コンセプト

## 冗長さを解消するための 3 つのコンセプト

[前節](./motivation.md)で述べた従来の React 開発における冗長さを解消するため、省力化コンポーネントは次の 3 つのコンセプトに基づいて作成されています。

<strong>
- 入力項目に必要なパラメータの集約
- 画面単位での操作と自動化
- コンポーネントの高機能化
</strong>

### 入力項目に必要なパラメータの集約

1 つの入力項目に関連する定義を<strong> Item </strong>と呼ばれるクラスに集約することで、冗長なコードを排除します。

:::note 解消できる冗長さ

- 分散して記述される入力項目の定義
  :::

Item クラスは以下に示すパラメータを保持します。

- ラベル名
- 状態変数
- 状態更新関数
- 選択肢(セレクトボックス、チェックボックス、ラジオボタンの場合)
- バリデーションルール

例として、「ユーザー名」を入力するためのテキストフォームに関連する定義は、以下のようにまとめて記述されます。

```Typescript
userName : CsInputTextItem {
    key: "userName"
    label: "ユーザー名" // ラベル名
    parentView: { userName: CsInputTextItem, ...}
    readonly: false
    value: "" // 状態変数
    setValue: Dispatch<SetStateAction<string | undefined>>　// 状態更新関数
    validationRule: // バリデーションルール
    {
        required: true,
        min: 3,
        max: 30
    }
}
```

:::note Item クラスの種類

Item クラスは入力部品の種類ごとに用意されています。例で示したテキスト入力部品に対応する Item クラスは`CsInputTextItem`でしたが、ラジオボタンには`CsRadioBoxItem`、セレクトボックスには`CsSelectBoxItem`など、入力部品の種類ごとに対応する Item クラスが用意されています。  
省力化コンポーネントが提供する Item クラスの詳細については、[実装ガイド](../implementation-guide/item-and-component.md)をご確認ください。

:::

:::note フックを使った定義

実装者は、「フック」と呼ばれる便利な関数を使って、上記のような Item 定義を記述していくことになります。
フックを使った場合、次のように記述することで上記の定義を再現することができます。

```Typescript
// フックを使用することで簡潔にItem定義を記述することができる
userName: useCsInputTextItem("ユーザー名", useInit(""), stringRule(true, 3, 30))
```

:::

### 画面単位での操作と自動化

画面内の Item(画面項目)を<strong>View</strong>と呼ばれるクラスに集約することで、バリデーションスキーマやレイアウトを自動生成できるようにします。

:::note 解消できる冗長さ

- 画面全体のバリデーションスキーマの定義
  :::

1 つの View 定義は、単数もしくは複数の画面項目を持つ 1 つの画面に対応しています。  
以下に示すのは、5 つの入力項目(ユーザー名、パスワード、メールアドレス、性別、生年月日)を持つ画面に対応した View 定義です。

```Typescript
// Viewは1つの画面に対応する
const view: RegisterUserView = useCsView({
    // 単数もしくは複数のItem(画面項目)を保持する
    userName: useCsInputTextItem("ユーザー名", useInit(""), stringRule(true, 3, 30)),
    password: useCsInputPasswordItem("パスワード", useInit(""), stringRule(true, 8, 16)),
    mailAddress: useCsInputTextItem("メールアドレス", useInit(""), stringRule(true, 8, 20)),
    gender: useCsRadioBoxItem("性別", useInit(""), stringRule(true),
      selectOptionStrings(["男性", "女性", "回答しない"])),
    birthDay: useCsInputDateItem("生年月日", useInit("2000-01-01"), stringRule(true)),
  })
```

[入力項目に必要なパラメータの集約](#入力項目に必要なパラメータの集約)で述べたように、Item は 1 つの入力項目に関連する定義を集約して保持しています。そして、View は画面内の Item を集約して保持しています。言い換えると、View は画面に関連する定義を集約して保持していることになります。これにより、以下に示す 3 つの操作が可能となります。

#### バリデーションスキーマの自動生成

- View 内の各 Item に設定されたバリデーションルールを基に、画面単位のバリデーションスキーマを内部で自動生成します。

#### レイアウトの自動生成

- View をレイアウト用のコンポーネントに渡すことで、シンプルな並びであれば項目配置を自動化することができます。

#### 画面単位で設定する項目の一元管理

- 読み取り専用設定やバリデーションの実行タイミングなど、画面単位で統一したい設定を View のパラメータで設定できます。

### コンポーネントの高機能化

画面項目コンポーネントの内部に主要な処理や定義をあらかじめ記述しておくことで、実装者の記述量を減らせるようにします。

:::note 解消できる冗長さ

- 類似したイベントハンドラの重複
- 画面項目コンポーネントの外部に記述される処理や定義
  :::

#### <strong>入力部品の場合</strong>

入力項目に必要なラベルの配置やバリデーションメッセージの表示領域、イベントハンドラなどを入力部品コンポーネントの内部に組み込みます。それによって、実装者は入力項目のパラメータが集約された Item をコンポーネントに渡すだけで必要な機能を実現することができます。

![入力部品](/img/input-form.PNG)

上記の入力部品を配置するために実装者が記述する必要があるのは次のコード 1 行になります。

```tsx
<AxInputText item={view.userName} />
```

:::note 入力部品コンポーネントの種類
入力部品コンポーネントは、入力部品（Item クラス）の種類ごとに用意されています。例で示したテキスト入力部品に対応するコンポーネントは`AxInputText`でしたが、ラジオボタンには`AxRadioBox`、セレクトボックスには`AxSelectBox`など、入力部品の種類ごとに対応するコンポーネントが用意されています。  
なお、各入力部品コンポーネントに渡すことのできる Item クラスは、その入力部品の種類に対応する Item クラスのみになります。(例：AxInputText コンポーネントには、 CsInputTextItem クラスの Item のみ渡すことが可能です。)  
省力化コンポーネントが提供する 入力部品コンポーネントの詳細については、[実装ガイド](../implementation-guide/item-and-component.md)をご確認ください。

:::

:::note 参考情報：高機能な入力部品の内部実装

<details>
  <summary>コードを見る</summary>

```tsx
// 高機能なテキスト入力部品
const AxInputText = (props: AxInputTextProps) => {
  // (...省略...)
  return (
      <div>
          {/* ラベルの表示 */}
          <AxLabel label={getLabel(item, showRequiredTag)}></AxLabel>
          <Input
              className={getClassName(props)}
              value={item.value}
              readOnly={item.isReadonly()}
              {/* イベントハンドラの実装 */}
              onChange={(e) => {　
                  item.setValue(e.target.value)
              }}
              {/* ...省略... */}
          />
          {/* バリデーションメッセージの表示 */}
          <ValidationError key={"validation-error-" + item.key} message={item.validationErrorMessage} />
      </div>
  );
}
```

</details>
:::

<br/>
<strong>ボタンの場合</strong>

API 呼び出し処理とバリデーション実行処理を定義したコールバック関数、ローディング中のスピナー表示機能、ボタン押下後のメッセージ表示機能をボタンコンポーネントの内部に組み込みます。それによって、実装者は振る舞いをパラメータとしてコンポーネントに渡すだけで必要な機能を実現することができます。

![入力部品](/img/tmp1.PNG)

上記の機能を備えたボタンを配置するために実装者が記述する必要があるのは次のコード 1 行になります。

```tsx
<AxMutateButton
  type="primary"
  validationViews={[searchView]} // バリデーションを行いたいView(画面)を指定
  event={searchView.searchButton} // イベントを指定
  addClassNames={["left", "bottom"]}
  successMessage="6割くらいの確率で成功しました" // バリデーションが成功したときに表示したいメッセージを指定
  errorMessage="4割くらいの確率で失敗しました" // バリデーションが失敗したときに表示したいメッセージを指定
>
  送信
</AxQueryButton>
```

:::note イベントについて
TODO: イベントの説明を軽くする。
イベントの詳細については、[実装ガイド](../implementation-guide/basic-of-view-and-item.md)をご確認ください。
:::

:::note 参考情報：高機能なボタンの内部実装

<details>
  <summary>コードを見る</summary>

```tsx
// 高機能なボタン
export const AxMutateButton = (props: AxMutateButtonProps<TApiRequest, TApiResponse>) => {
  const { event, validationViews, antdProps } = props;

  // スピナーの表示機能
  useEffect(() => {
    if (!event.isLoading) {
      if (event.isSuccess) {
        event.setResponse();
      } else if (event.isError) {
        event.setError();
      }
    }
  }, [event]);

  // ボタン押下時に実行されるコールバック関数
  const onClick = useCallback(async () => {

    // バリデーション実行処理
    const validationOk = executeValidation(validationViews);

    // (...省略...)

    // API呼び出し処理
    await event.onClick();
  }, [event, validationViews]);

  return (
    <div className={getClassName(props, "button-area")}>
      {/* ボタン押下後のメッセージ */}
      {event.result.isSuccess && props.successMessage && (
        <Alert message={props.successMessage} ... />
      )}
      {/* ...省略... */}

      {/* ボタン */}
      <Button
        loading={event.isLoading}
        onClick={() => onClick()}
        {/* ...省略... */}
      >
        {props.children}
      </Button>
    </div>
  );
};
```

</details>
:::

<hr/>
次節で紹介する<strong>省力化コンポーネントの特徴</strong>では、3つのコンセプトを適用した結果、実装者にどのようなメリットがあるのかを説明します。
