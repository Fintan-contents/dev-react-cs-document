---
sidebar_position: 2
title: "前提知識（1表まとめ）"
---

# 前提知識

省力化コンポーネントを用いてアプリケーションを開発するにあたって、以下に示す技術知識が前提として必要になります。

  <table>
      <thead>
          <tr>
              <th>カテゴリー</th>
              <th>技術</th>
              <th>説明</th>
          </tr>
      </thead>
      <tbody>
          <tr>
              <td rowspan="2">プログラミング言語</td>
              <td>JavaScript</td>
              <td>ウェブアプリ開発に用いられるスクリプト言語です。</td>
          </tr>
          <tr>
              <td>TypeScript</td>
              <td>JavaScript に静的型付けを追加したプログラミング言語です。JavaScript と上位互換性があり、静的型付けによりコード実行前にエラーを検出できます。その性質から、開発規模が大きくなるほどその効果を発揮します。</td>
          </tr>
          <tr>
              <td>フロントエンドライブラリ</td>
              <td>React</td>
              <td>コンポーネントベースのアプローチで、ウェブアプリ開発に用いられる JavaScript ライブラリです。</td>
          </tr>
          <tr>
              <td rowspan="3">UI コンポーネントライブラリ(※1)</td>
              <td>Ant Design</td>
              <td>エンタープライズ向けの高品質な React コンポーネントライブラリです。</td>
          </tr>
          <tr>
              <td>Material UI</td>
              <td>Google の Material Design に基づいた React コンポーネントライブラリです。</td>
          </tr>
          <tr>
              <td>React Bootstrap</td>
              <td>Bootstrap フレームワークに基づいた React コンポーネントライブラリです。</td>
          </tr>
          <tr>
              <td rowspan="2">バリデーションライブラリ(※2)</td>
              <td>Yup</td>
              <td>JavaScript のオブジェクトスキーマバリデーションライブラリです。 柔軟で使いやすいという特徴を持ちます。</td>
          </tr>
          <tr>
              <td>Zod</td>
              <td>TypeScript のオブジェクトスキーマバリデーションライブラリです。型安全性が高く、軽量であるという特徴を持ちます。</td>
          </tr>
          <tr>
              <td rowspan="3">API 関連(※3)</td>
              <td>TanStack Query</td>
              <td>バックエンドの API を呼び出すために使用するライブラリです。</td>
          </tr>
          <tr>
              <td>OpenAPI Specification</td>
              <td>API の仕様を記述するための標準規格です。</td>
          </tr>
          <tr>
              <td>Orval</td>
              <td>OpenAPI 仕様から TypeScript の型定義と API クライアントコードを自動生成するツールです。</td>
          </tr>
      </tbody>
  </table>

(※1) UI コンポーネントライブラリについては、プロジェクトで使用するライブラリ 1 つの知識があればよいです。  
(※2) バリデーションライブラリについては、プロジェクトで使用するライブラリ 1 つの知識があればよいです。  
(※3) API 関連については、手動生成方式(TanStack Query)か自動生成方式(OpenAPI Specification, Orval)いずれかの知識があればよいです。

## 参考サイト

実装例や解説を理解するために参考となるサイトを紹介します。
用語やコード例でわからないことがあるときは参照してください。

### JavaScript Primer

[JavaScript Primer](https://jsprimer.net/)は、JavaScript の文法や機能を一から学べるサイトです。「第一部：基本文法」までの知識があれば、ひとまず充分です。[基本文法の目次](https://jsprimer.net/basic/)を見てわからないことがあれば学習してください。

### サバイバル TypeScript

[サバイバル TypeScript](https://book.yyts.org/)は、TypeScript を最短ルートで実務利用できることを目指したサイトです。静的型付け言語での実装経験があれば、アプリの実装は進められます。

実務レベルに必要な一通りの知識が必要な場合はサバイバル TypeScript で学習しましょう。[読んで学ぶ TypeScript](https://book.yyts.org/reference)の見出しをみて内容がイメージできれば大丈夫です。

### React 公式サイト

React を学ぶには[React 公式サイト](https://ja.react.dev/)が一番お勧めです。

React を利用したことがない人は「クイックスタート」からはじめてください。「LEARN REACT」の内容まで理解できていれば、最低限の React の知識が身に付いています。わからない箇所があれば学習してください。

また、「API リファレンス」に書かれている下記のフックは使用頻度が高いので、あらかじめ読んで理解しておくことをお勧めします。

- [クイックスタート](https://ja.react.dev/learn)
- [インストール](https://ja.react.dev/learn/installation)
- [LEARN REACT](https://ja.react.dev/learn/describing-the-ui)
- [API リファレンス](https://ja.react.dev/reference)
  - [useCallback](https://ja.react.dev/reference/react/useCallback)
  - [useMemo](https://ja.react.dev/reference/react/useMemo)
  - [useEffect](https://ja.react.dev/reference/react/useEffect)
  - [useState](https://ja.react.dev/reference/react/useState)
  - [useRef](https://ja.react.dev/reference/react/useRef)
  - [useContext](https://ja.react.dev/reference/react/useContext)

### TanStack Query

TanStack Query の詳細については[公式ドキュメント](https://tanstack.com/query/latest)を参考にしてください。

特に [useQuery](https://tanstack.com/query/v4/docs/framework/react/reference/useQuery) は頻繁に使用するので目を通しておいてください。

### Ant Design

省力化コンポーネントは Ant Design を拡張して提供しています。
Ant Design の詳細については[公式ドキュメント](https://ant.design/components/overview/)を参考にしてください。

### OpenAPI Specification

OpenAPI Specification の詳細については[公式ドキュメント(英語版)](https://www.openapis.org/what-is-openapi)を参考にしてください。

#### OpenAPI Specification の見方について

[Swagger Editor](https://editor.swagger.io/)を使用することで API のリクエスト、レスポンスなどの詳細を確認できます。
API 仕様の内容を[Swagger Editor](https://editor.swagger.io/)のエディターに貼るとプレビューにその内容が表示されます。

その他の Swagger 機能については[What Is Swagger?](https://swagger.io/docs/specification/about/)を参照してください。

### Orval

Orval の詳細については[公式ドキュメント](https://orval.dev/)を参考にしてください。
