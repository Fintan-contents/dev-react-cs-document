---
sidebar_position: 2
title: 導入ツール
---

# 導入ツール

本節では省力化コンポーネントの導入ツールについて説明をします。
導入ツールを使用することで省力化コンポーネントを導入する際に必要な環境を整備することができます。

## プロジェクトのルート選択

省力化コンポーネントをプロジェクトに導入するにはコマンドラインで`installer-for-next.bat`コマンドを実行してください。その際には引数として省力化コンポーネントを導入したいプロジェクトの絶対パスを記入してください。

```bash title="Terminal"
installer-for-next.bat ＜導入したいプロジェクトの絶対パス＞
```

---

コマンド実行時に次のプロンプトが表示されます。

```bash title="Terminal"
・パッケージマネージャを選択してください。
・UIコンポーネントライブラリを選択してください。
・バリデーションライブラリを選択してください。
・API 生成方式を選択してください。
・デモ画面をインストールしますか？
・components/frameworkフォルダをコピーするディレクトリを選択してください。
```

これらのプロンプトの詳細についてそれぞれ説明をします。

## パッケージマネージャの選択

```bash title="Terminal"
パッケージマネージャを選択してください。
> npm
> yarn
```

パッケージマネージャの選択では現在使用しているパッケージマネージャを選択してください。

## UI コンポーネントの選択

```bash title="Terminal"
UIコンポーネントライブラリを選択してください。
> Ant Design
> Material UI
> React Bootstrap
```

省力化コンポーネントでは以下の 3 つの UI コンポーネントに対応しています。

- Ant Design
- Material UI
- React Bootstrap

## バリデーションライブラリの選択

```bash title="Terminal"
バリデーションライブラリを選択してください
> Yup
> Zod
```

省力化コンポーネントでは以下の 2 つのバリデーションライブラリをサポートしています。

- Yup
- Zod

## API 生成方式の選択

```bash title="Terminal"
API生成方式を選択してください。
> Orval
> TanStack Query
```

省力化コンポーネントでは以下の 2 つの API 生成方式ライブラリをサポートしています。

- Orval
- TanStack Query

## デモ画面を含めるかの選択

```bash title="Terminal"
デモ画面をインストールしますか？
> はい
> いいえ
```

導入資材にデモ画面を含めるかどうかの選択ができます。

## コピー先ディレクトリの選択

```bash title="Terminal"
components/frameworkフォルダをコピーするディレクトリを選択してください。
  ↓ C:\Users\tie308841\project\cs-component\cli-demo-sample
    → C:\Users\tie308841\project\cs-component\cli-demo-sample\.git
    → C:\Users\tie308841\project\cs-component\cli-demo-sample\.next
    → C:\Users\tie308841\project\cs-component\cli-demo-sample\node_modules
    → C:\Users\tie308841\project\cs-component\cli-demo-sample\openapi
    → C:\Users\tie308841\project\cs-component\cli-demo-sample\src
```

省力化コンポーネントに必要な資材をコピーするディレクトリ先を指定します。

---

コピー先ディレクトリの選択までが完了すると資材のコピーおよびライブラリのインストールが開始されます。インストールが成功すると省力化コンポーネントが使用できる状態になります。

:::info インストールされる資材
インストールされた資材は/conponents 配下に格納されます。詳しいディレクトリ構成およびインストールされるライブラリについては[インストールされる資材](../introduction-guide/installed-materials.md)を参照してください。
:::
