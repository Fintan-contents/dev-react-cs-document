---
sidebar_position: 2
---

# コンセプト

## 冗長さを解消するための 3 つのコンセプト

[前章](./motivation.md)で述べた従来の React 開発における冗長さを解消するため、省力化コンポーネントは次の 3 つのコンセプトに基づいて作成されています。

<strong>
- 入力項目に必要なパラメータの集約
- 画面単位での操作と自動化
- コンポーネントの高機能化
</strong>

### 入力項目に必要なパラメータの集約

1 つの入力項目に関する情報（状態変数、イベントハンドラ、バリデーションルール）を、<strong> Item </strong>と呼ばれるクラスに集約することで、冗長なコードを排除します。

:::note 解消できる冗長さ

- 分散して記述される入力項目の定義
- 類似したイベントハンドラの重複
  :::

Item が保持するパラメータには以下のものがあります。なお、これらの情報は、従来の React 開発では定義箇所が分散していたものになります。

- ラベル
- 状態変数
- 状態更新関数
- 選択肢(セレクトボックスやラジオボタンの場合)
- バリデーション定義

例えば、「ユーザ名」というテキスト入力項目は、以下の形で定義されることになります。

```Typescript
userName : CsInputTextItem {
    key: "userName"
    label: "ユーザー名"
    parentView: { userName: CsInputTextItem, ...}
    readonly: false
    value: ""
    setValue: Dispatch<SetStateAction<string | undefined>>
    validationRule:
    {
        required: true,
        min: 3,
        max: 30
    }
}
```

実際には、 Hooks と呼ばれるヘルパ関数を使うため、実装者はパラメータをその関数に渡すだけで、上記のクラス定義が記述できることになります。

```Typescript
userName: useCsInputTextItem("ユーザー名", useInit(""), stringRule(true, 3, 30))
```

### 画面単位での操作と自動化

<strong>View</strong>と呼ばれる 画面の単位で Item(画面項目)を管理することで、バリデーションスキーマやレイアウトを自動生成できるようにします。

:::note 解消できる冗長さ

- 画面全体のバリデーションスキーマの定義
  :::

1 画面内に含まれる Item を、View と呼ばれるクラスでまとめて管理します。

以下に示すのは、5 つの入力項目(ユーザー名、パスワード、メールアドレス、性別、生年月日)を持つ画面に対応する View 定義です。

```Typescript
const view: RegisterUserView = useCsView({
    userName: useCsInputTextItem("ユーザー名", useInit(""), stringRule(true, 3, 30)),
    password: useCsInputPasswordItem("パスワード", useInit(""), stringRule(true, 8, 16)),
    mailAddress: useCsInputTextItem("メールアドレス", useInit(""), stringRule(true, 8, 20)),
    gender: useCsRadioBoxItem("性別", useInit(""), stringRule(true),
      selectOptionStrings(["男性", "女性", "回答しない"])),
    birthDay: useCsInputDateItem("生年月日", useInit("2000-01-01"), stringRule(true)),
  })
```

[入力項目に必要なパラメータの集約](#入力項目に必要なパラメータの集約)で述べたように、Item は入力項目に関連する情報を集約して保持しています。つまり、View には画面を構成する入力項目の情報が全て集約されていることになります。これによって、以下に示す 3 つの操作が可能となります。

#### バリデーションスキーマの自動生成

- 各 Item に設定されたバリデーションルールを基に、必要なバリデーションスキーマを内部で自動生成します。

#### レイアウトの自動生成

- シンプルな並びであれば、View をレイアウト用のコンポーネントに渡すことで項目配置を自動化することができます。

#### 画面単位で設定する項目の一元管理

- 読み取り専用設定やバリデーションの実行タイミングなど、画面単位で統一したい設定を View のパラメータで設定できます。

### コンポーネントの高機能化

コンポーネントの内部に主要な処理や定義をあらかじめ記述しておくことで、実装者の記述量を減らすようにします。

:::note 解消できる冗長さ

- 画面項目コンポーネントの外部に記述される処理や定義
  :::

- 入力部品の場合

  入力項目に必要なラベルやバリデーションメッセージの表示領域、イベントハンドラなどを入力部品コンポーネントの内部に組み込みます。

```tsx
const AxInputText = (props: AxInputTextProps) => {
  (...省略)
  return (
      <div>
          <AxLabel label={getLabel(item, showRequiredTag)}></AxLabel>
          <Input
              className={getClassName(props)}
              value={item.value}
              readOnly={item.isReadonly()}
              onChange={(e) => {
                  item.setValue(e.target.value)
              }}
               (...省略)
          />
          <ValidationError key={"validation-error-" + item.key} message={item.validationErrorMessage} />
      </div>
  );
}
```

- ボタンの場合

  API 呼び出し処理とバリデーション実行処理を定義したコールバック関数、ローディング中のスピナー表示、ボタン押下後のメッセージ表示をボタンコンポーネントの内部に組み込みます。

```tsx
export const AxMutateButton = (props: AxMutateButtonProps<TApiRequest, TApiResponse>) => {
  const { event, validationViews, antdProps } = props;

  // Spinnerの表示
  useEffect(() => {
    if (!event.isLoading) {
      if (event.isSuccess) {
        event.setResponse();
      } else if (event.isError) {
        event.setError();
      }
    }
  }, [event]);

  // ボタン押下時の処理
  const onClick = useCallback(async () => {

    // バリデーション実行
    const validationOk = executeValidation(validationViews);

    (...省略)

    // APIの呼び出し
    await event.onClick();
  }, [event, validationViews]);

  return (
    <div className={getClassName(props, "button-area")}>
    　// メッセージ表示
      {event.result.isSuccess && props.successMessage && (
        <Alert message={props.successMessage} ... />
      )}
      (...省略)

      // ボタン
      <Button
        loading={event.isLoading}
        onClick={() => onClick()}
        ...
      >
        {props.children}
      </Button>
    </div>
  );
};
```

<hr/>
次節で紹介する<strong>省力化コンポーネントの特徴</strong>では、3つのコンセプトを適用した結果、実装者が得られるメリットについて説明します。
