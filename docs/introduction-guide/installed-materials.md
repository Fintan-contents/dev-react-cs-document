---
sidebar_position: 3
title: インストールされる資材
---

# インストールされる資材

本節では、導入ツールによってプロジェクトにインストールされる資材について説明します。

## 基盤部品

省力化コンポーネントの基盤部品は、[導入ツール](./introduction-tool.md)の実行時にコピー先に指定したパス直下の `components/framework` に配置されます。

```
//copy-destination//
　　|- components
　　　　|- framework
　　　　　　|- components - 論理情報に対応したコンポーネント
　　　　　　　　|- antd (*1)
　　　　　　　　|- mui (*1)
　　　　　　　　|- bootstrap (*1)
　　　　　　　　|- validation
　　　　　　|- logics - 画面や画面項目の論理情報クラス
　　　　　　　　|- orval (*3)
　　　　　　　　|- react-query(*3)
　　　　　　　　|- yup(*2)
　　　　　　　　|- zod(*2)
　　　　　　　　|- CsXxx.ts
　　　　　　　　|- …
　　　　　　　　|- getCsDefautValidationEvent.ts
　　　　　　　　|- index.ts

*1: 選択した UI コンポーネントに対応するディレクトリのみがコピーされます
*2: 選択したバリデーションライブラリに対応するディレクトリのみがコピーされます
*3: 選択した API 生成方式に対応するディレクトリのみがコピーされます
```

## デモ画面

[導入ツール](./introduction-tool.md)の実行時にデモ画面のインストールに Yes を付けた場合、デモ画面に必要なファイルが app 配下にコピーされます。

:::warning
デモ画面は、 Next.js の AppRouter を想定した構成となっています。
:::

```
app
　　|- demo
　　　　|- DemoXxxView (Xxx は選択した UI コンポーネント)
　　　　|- DemoHeader.tsx
　　　　|- page.tsx
```

## ライブラリ一覧

[導入ツール](./introduction-tool.md)の実行時に選択したオプションに合わせて、必要なライブラリがインストールされます。

| ライブラリ名           | 必  | インストール条件                                   |
| ---------------------- | --- | -------------------------------------------------- |
| antd                   |     | UI コンポーネントで Ant Design を選択した場合      |
| @mui/material          |     | UI コンポーネントで Material UI を選択した場合     |
| @mui/icons-material    |     | 〃                                                 |
| react-bootstrap        |     | UI コンポーネントで React Bootstrap を選択した場合 |
| @types/react-bootstrap |     | 〃                                                 |
| yup                    |     | バリデーションライブラリで Yup を選択した場合      |
| zod                    |     | バリデーションライブラリで Zod を選択した場        |
| orval                  |     | API 生成方式で orval を選択した場合                |
| axios                  |     | 〃                                                 |
| @tanstack/react-query  | 〇  |                                                    |
| dayjs                  | 〇  |                                                    |

※必に〇がついているライブラリは必ずインストールされます。
