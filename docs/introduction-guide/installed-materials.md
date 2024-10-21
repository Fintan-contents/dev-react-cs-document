---
sidebar_position: 3
title: インストール資材
---

# インストール資材

本節では、導入ツールによってプロジェクトにインストールされる資材について説明します。

## 基盤部品

省力化コンポーネントの基盤部品は、[導入ツール](./introduction-tool.md)の実行時にコピー先に指定したパス直下の `framework` に配置されます。

```
//copy-destination//
　　　|- framework
　　　　　|- components - 論理情報に対応したコンポーネント
　　　　　　　|- antd (*1)
　　　　　　　|- mui (*1)
　　　　　　　|- bootstrap (*1)
　　　　　　　|- validation
　　　　　|- logics - 画面や画面項目の論理情報クラス
　　　　　　　|- orval (*3)
　　　　　　　|- react-query (*3)
　　　　　　　|- yup (*2)
　　　　　　　|- zod (*2)
　　　　　　　|- CsXxx.ts
　　　　　　　|- ...
　　　　　　　|- getCsDefautValidationEvent.ts
　　　　　　　|- index.ts

*1: 選択した UI コンポーネントライブラリに対応するディレクトリのみがコピーされます
*2: 選択したバリデーションライブラリに対応するディレクトリのみがコピーされます
*3: 選択した API 生成方式に対応するディレクトリのみがコピーされます
```

## デモ画面

[導入ツール](./introduction-tool.md)の実行時にデモ画面のインストールに「はい」を選択した場合、デモ画面に必要なファイルが `app` 配下にコピーされます。

:::warning
デモ画面を構成するファイルは、 Next.js の AppRouter を想定した構成で配置されます。  
`demo` フォルダは、デモ画面が不要になったタイミングで削除してください。
:::

```
app
　　|- demo
　　　　|- DemoXxxHeader.tsx (Xxx は選択した UI コンポーネントライブラリ名)
　　　　|- DemoXxxMain.tsx
　　　　|- page.tsx
```

## ライブラリ一覧

[導入ツール](./introduction-tool.md)の実行時に選択したオプションに合わせて、必要なライブラリがインストールされます。

| ライブラリ名                                             | 必  | インストール条件                                             |
| -------------------------------------------------------- | --- | ------------------------------------------------------------ |
| antd                                                     |     | UI コンポーネントライブラリに Ant Design を選択した場合      |
| @mui/material                                            |     | UI コンポーネントライブラリに Material UI を選択した場合     |
| @mui/icons-material                                      |     | 〃                                                           |
| react-bootstrap                                          |     | UI コンポーネントライブラリに React Bootstrap を選択した場合 |
| @types/react-bootstrap <br /> ※ `devDependencies` に追加 |     | 〃                                                           |
| yup                                                      |     | バリデーションライブラリに Yup を選択した場合                |
| zod                                                      |     | バリデーションライブラリに Zod を選択した場                  |
| orval <br /> ※ `devDependencies` に追加                  |     | API 生成方式に orval を選択した場合                          |
| axios                                                    |     | 〃                                                           |
| @tanstack/react-query                                    | 〇  |                                                              |
| dayjs                                                    | 〇  |                                                              |

※必に〇がついているライブラリは必ずインストールされます。
