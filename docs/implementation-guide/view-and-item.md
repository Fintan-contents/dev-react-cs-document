---
sidebar_position: 1
---

# View と Item

ViewとItemの概要についてサンプルコードを用いて説明をします。

- 「サンプルコード」＋画面例（すべてのItemが定義されている画面）
  - View と Item の定義部分のソースコードを乗せる
  - サンプルコードの値がこの画面に一致する等の説明を加える
  - 例：Viewは画面全体のこと、Itemとは画面にある各項目のこと

## View と Item の定義

- danmachi-README「View と Item の定義方法」の内容に沿ったもの
  - useCs 〇〇 Item で指定する項目およびヘルパ関数を説明する（第1引数に何を指定するかなど）
  - 例として name: useCsInputTextItem の設定内容を解説する

## コンポーネントへ Item を渡す

- 最もシンプルな渡し方
  - コード
  - 画面例
- Row/Col と組み合わせた複数コンポーネントのカスタムレイアウト
  - コード
  - 画面例
- コンポーネントに Props を渡す
  - コンポーネント独自の Props
  - Ant Design と同じ Props
- AxLayout を用いた自動レイアウト
  - コード
  - 画面例
  - 注意点

## 利用可能なコンポーネント一覧

| コンポーネント  | 用途           | Hooks                  | Item                |
| --------------- | -------------- | ---------------------- | ------------------- |
| AxInputText     | テキスト入力   | useCsInputTextItem     | CsInputTextItem     |
| AxInputPassword | パスワード入力 | useCsInputPasswordItem | CsInputPasswordItem |


- TODO：すべてのItemコンポーネントを記載