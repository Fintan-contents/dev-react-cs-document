---
sidebar_position: 1
title: 事前準備
---

# 事前準備

:::warning 導入ガイドを読む前に
[導入指標](../know-cs-component/introduction-index.md)をまだ読んでいない方は、まずそちらをご覧ください。
導入指標と自身のシステムを比較検討し、導入を決めた場合にのみ、以下の内容をお読みください。
:::

本章では、省力化コンポーネントをプロジェクトに導入するための手順を解説します。  
まず初めに、事前準備として必要な Next.js プロジェクトの作成と導入ツールのダウンロードについて解説します。

## Next.js プロジェクトの作成

:::warning プロジェクトが既にある場合
既に Next.js プロジェクトを作成済みの方は、[導入ツールのダウンロード](#導入ツールのダウンロード)に進んでください。
:::

:::info Next.js が初めての方
Next.js を初めて使用する方には、以下の学習リソースをお勧めします。

- Next.js 公式ドキュメント：公式のドキュメントは、Next.js の基本から応用までをカバーしています。
- Next.js チュートリアル：公式サイトにあるインタラクティブなチュートリアルで、実際にコードを書きながら学ぶことができます。

:::

本ドキュメントでは、省力化コンポーネントを使用するアプリケーション基盤として Next.js を採用しています。Next.js は、React をベースにしたフレームワークであり、サーバーサイドレンダリングや静的サイト生成を簡単に実現するための強力なツールを提供します。

Next.js プロジェクトを新規に作るには、ターミナルで次のコマンドを実行してください。

```bash title="Terminal"
npx create-next-app@latest
```

コマンドを実行するとプロンプトが表示されますので、各設定に対して Yes か No を選択してください。以下に示すのは、本ドキュメントが推奨する各設定です。

```bash title="Terminal"
√ Would you like to use TypeScript? ... Yes
√ Would you like to use ESLint? ... Yes
√ Would you like to use Tailwind CSS? ... Yes
√ Would you like to use `src/` directory? ... Yes
√ Would you like to use App Router? (recommended) ... Yes
√ Would you like to customize the default import alias (@/*)? ... No
```

プロジェクトの作成が完了したら、作成されたディレクトリに移動し、次のコマンドを実行してください。

```bash title="Terminal"
npm run dev
```

http://localhost:3000 にアクセスをし、以下のような画面が表示されれば成功です。

![Next.jsのトップページ](../../static/img/nextjs.png)

## 導入ツールのダウンロード

省力化コンポーネントの資材をインストールするためのツールが、GitHub の [dev-react-cs-component](https://github.com/Fintan-contents/dev-react-cs-component) リポジトリに格納されています。

`Git` をインストールしている場合は、以下のコマンドでリポジトリをクローンしてください。

```bash title="Terminal"
$ git clone https://github.com/Fintan-contents/dev-react-cs-component.git
```

`Git` をインストールしていない、もしくは使わない場合は、以下のリンクから zip ファイルをダウンロードし、任意の場所に解凍してください。  
[GitHub から zip ファイルをダウンロード](https://github.com/Fintan-contents/dev-react-cs-component)
