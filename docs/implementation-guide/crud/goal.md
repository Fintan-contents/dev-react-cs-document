---
sidebar_position: 1
title: 本節の内容
---

本節では省力化コンポーネントを用いて以下の API の実装方法を理解できます。

- 取得機能
- 登録機能
- 更新機能
- 削除機能
- 検索機能

![CRUD画面の画像](../../../static/img/crud.png)

ここでは[サンプルアプリ](https://github.com/Fintan-contents/dev-react-cs-example/tree/develop)の`todo/page.tsx`と`todo/page.view.ts`ファイルを使用します。`todo/page.tsx`は CRUD 機能を実装した画面を表示するファイル、`todo/page.view.ts`では View 定義を行うファイルとして使用します。

:::info
本節では各機能の API 呼び出し方法にフォーカスして説明します。API 呼び出し方法を含めた一通りの画面実装を行いたい場合は、[画面を作る](../create-register-screen/)を参照してください。
:::