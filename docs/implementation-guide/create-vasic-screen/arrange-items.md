---
sidebar_position: 3
title: 入力項目を配置する
---

画面の定義が完了したら、画面コンポーネントを使用して入力項目を配置していきます。
入力項目の配置は、`AxTableLayout`というコンポーネントを使用して全ての入力項目を自動で配置する方法と、一つ一つの入力項目に対応する画面コンポーネントを手動で配置する方法の 2 つがあります。

:::note
省力化コンポーネントでは 、UI コンポーネントライブラリごとに専用の画面コンポーネントを提供しています。
それぞれの画面コンポーネントは、以下に示すプレフィックスによって分類されます。

- Ax : Ant Design
- Mx : Material UI
- Bsx : React Bootstrap

実装ガイドでは、Ant Design (Ax から始まる部品) を例として記述しています。

:::

### 自動で配置をする

自動で配置をする場合は、`AxTableLayout` というコンポーネントを使用します。 `view` という Props に、表示したい画面の View の変数を指定します。 `colSize` という Props に、画面内に並べる項目の列数を指定します。

```tsx
const view = useRegisterUserView();
return <AxTableLayout view={view} colSize={2} />;
```

上記のように記述した場合、次のような画面が表示されます。

![入力項目の定義5個](/img/screen-item-5-2.png)

### 手動で配置をする

手動で配置をする場合は、各入力項目の Item の型に応じた画面コンポーネントを使用します。 `item` という Props に、対応する入力項目の item の変数を指定します。

```tsx
const view = useRegisterUserView();
return (
  <>
    <AxInputText item={view.userName} />
    <AxInputPassword item={view.password} />
    <AxRadioBox item={view.gender} />
    <AxInputDate item={view.birthDay} />
    <AxInputNumber item={view.terminalNum} />
  </>
);
```

上記のように記述した場合、次のような画面が表示されます。

![入力項目の定義5個](/img/screen-item-5.png)

:::info
自動配置のメリット

- 項目数が多くても、コードの記述量が多くならない
- `colSize` を変更することでレイアウトが簡単に変えられる

手動配置のメリット

- 複雑なレイアウトにも対応できる
- 画面コンポーネントに item 以外の Props を渡すことができる
