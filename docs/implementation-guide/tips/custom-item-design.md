---
sidebar_position: 4
title: Item デザインをカスタムする方法
---

画面全体のデザインをカスタムしたい場合や独自にデザインを追加したい場合、一部分だけ固有のデザインを適応したい場合に本節が役立ちます。

## 画面全体のデザインをカスタムしたい場合

対応する CSS ファイルをカスタムすることで実現可能です。
Ant Design の省力化コンポーネントであれば `AxCtrl.css` を、バリデーションエラーメッセージであれば `ValidationError.css` を編集してください。

```diff_css title="AxCtrl.css"
.antd-ctrl {
  width: 90%;
  margin-left: 10px;
  // highlight-start
  // 右のmarginを10pxに変更
- margin-right: 5px;
+ margin-right: 10px;
// highlight-end
  margin-bottom: 5px;
  min-width: 40px;
  min-height: 45px;
  border-color: #cccccc44;
  border-width: 1px;
  border-style: solid;
  border-radius: 5px;
  padding: 8px;
  background-color: #f6f7fcbb;
  font-size: 16px;
  outline: none;
}
```

## 独自にデザインを追加したい場合

[画面全体のデザインをカスタムしたい場合](./custom-item-design.md#画面全体のデザインをカスタムしたい場合)では既に適応されているデザインに対しての修正でした。独自にデザインを追加したい場合は対応する CSS ファイルに追加してください。

```css title="AxCtrl.css"
+ .background-color {
+   background-color: "red";
+ }
```

## 特定のコンポーネントだけデザインを適応したい場合

省力化コンポーネントの Props として`addClassName`が用意されています。`addClassName`に適応したい CSS 名を指定することでデザインをカスタムすることができます。

```tsx title="page.tsx"
<AxInputText item={item} addClassName={["background-color"]} />
```
