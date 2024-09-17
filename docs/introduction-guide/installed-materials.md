---
sidebar_position: 3
title: インストールされる資材
---

# インストールされる資材

## 基盤部品

省力化コンポーネントの基盤部品は、[導入ツール]でコピー先に指定したパス配下の components/framework に配置されます。

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
　　　　　　　　|- zod(\*2)
　　　　　　　　|- CsXxx.ts
　　　　　　　　|- …
　　　　　　　　|- getCsDefautValidationEvent.ts
　　　　　　　　|- index.ts

*1: 選択した UI コンポーネントに対応するディレクトリのみがコピーされます
*2: 選択したバリデーションライブラリに対応するディレクトリのみがコピーされます
*3: 選択した API 生成方式に対応するディレクトリのみがコピーされます
```

## デモ

導入ツールの実行で、デモ画面のインストールに Yes を付けた場合、デモ画面を構成するファイルが app 配下にコピーされます。

```
app
　　|- demo
　　　　|- DemoXxxView (Xxx は選択した UI コンポーネント)
　　　　|- DemoHeader.tsx
　　　　|- page.tsx
```

## ライブラリ一覧

| ライブラリ名        | 必  | インストール条件                                   |
| ------------------- | --- | -------------------------------------------------- |
| axios               |     | API 生成方式で orval を選択した場合                |
| dayjs               | 〇  |                                                    |
| antd                |     | UI コンポーネントで Ant Design を選択した場合      |
| @mui/material       |     | UI コンポーネントで Material UI を選択した場合     |
| @mui/icons-material |     | 〃                                                 |
| react-bootstrap     |     | UI コンポーネントで React Bootstrap を選択した場合 |
| ...                 | ... | ...                                                |
