---
sidebar_position: 4
title: 導入後の作業
---

# 導入後の作業

導入後に必要な作業としては以下のような項目があります。

- デモ画面の表示
- Orval の設定

## デモ画面の表示

:::warning 以下に当てはまる方はこの項をスキップしてください。

- デモ画面を含めるかの選択で「はい」を選択
  :::
  「導入ツール」が完了したら、作成されたディレクトリに移動し、次のコマンドを実行してください。

`npm run dev`

起動が完了後、http://localhost:3000/demo にアクセスし以下の画面が表示されれば成功です。
![デモ画面](/img/automatic-layout.png)

:::info
本サンプルアプリでは以下の条件で作成されています。

- UI コンポーネント：Antd Design,
- バリデーションライブラリ：Yup,
- API 生成方式ライブラリ：Orval

:::

## Orval の設定

:::warning 本項の対象読者

本項の対象読者は、API 生成方式の選択で「Orval」を選択した方です。  
「TanStack Query」を選択した方は本項の作業は実施する必要がありません。
:::

本項では、Orval を用いて OpenAPI 仕様からコードを自動生成するための設定手順について解説をします。  
設定手順は以下の 4 ステップです。

1. OpenAPI の定義ファイルを配置する
2. Axios のカスタムインスタンスを作成する
3. orval.config.ts を作成する
4. コードを自動生成する

### 1. OpenAPI の定義ファイルを配置する

OpenAPI 仕様が定義された `oepnapi.yml` をルート配下に配置します。

```
プロジェクトのルート
　　|- openapi
　　　　|- openapi.yml
　　|- ...
```

:::info OpenAPI のサンプル
省力化コンポーネントのサンプルアプリで使用している OpenAPI 仕様は[こちら](https://github.com/Fintan-contents/dev-react-cs-component)。  
OpenAPI 仕様が未作成の場合は上記のサンプルを使用してください。
:::

### 2. Axios のカスタムインスタンスを作成する

Orval に設定するための Axios のカスタムインスタンスを作成します。
カスタムインスタンスを作成することで、API リクエストに必要な共通設定を一元化することができます。

```
プロジェクトのルート
　　|- src
　　　　|- libs
　　　　　　|- backend
　　　　　　　　|- customInstance.ts　※配置場所はあくまで一例です
```

カスタムインスタンスの作成方法は[Custom instance(公式サイト)](https://orval.dev/guides/custom-axios#custom-instance)を参考にしてください。

:::info Axios カスタムインスタンスのサンプル
省力化コンポーネントのサンプルアプリで使用しているカスタムインスタンスは[こちら](https://github.com/Fintan-contents/dev-react-cs-component)。  
:::

### 3. orval.config.ts を作成する

`orval.config.ts` という Orval の設定ファイルをルート配下に作成します。

```
プロジェクトのルート
　　|- orval.config.ts
　　|- ...
```

設定ファイルに記述する内容は[Configuration(公式サイト)](https://orval.dev/reference/configuration/overview)を参考にしてください。  
最低限設定する必要がある項目は `input` と `output` です。

- [input](https://orval.dev/reference/configuration/input)：参照元の OpenAPI 仕様
- [output](https://orval.dev/reference/configuration/output)：自動生成されるコードに関する設定  
  ※ `mutator` には、手順 2 で作成したカスタムインスタンスを設定してください

:::info 設定ファイルのサンプル
省力化コンポーネントのサンプルアプリで使用している設定ファイルは[こちら](https://github.com/Fintan-contents/dev-react-cs-component)。
:::

### 4. コードを自動生成する

1 ～ 3 の設定を行うことで、Orval を利用したコードの自動生成が行えるようになります。  
まず、 `package.json` に以下の定義を追記してください。

```js title="package.json"
{
  ...
  "scripts": {
    ...
    "code-gen": "npx orval --config ./orval.config.ts"
  },
  ...
}
```

次に、ターミナルで以下のコマンドを実行してください。実行後、手順 3 で `output` の中に指定した出力先にソースコードが自動生成されます。

```bash title="Terminal"
npm run code-gen
```
