---
sidebar_position: 4
---

# 論理情報に対応したコンポーネント

上記で説明した論理情報の派生クラスごとに以下のような名前のクラスがあります。
| 系統 | 説明 |
| ------ | ------ |
| CsItem を継承したItem | CsInputTextItem, CsInputPasswordItem, LCheckBoxItem, LRadioBoxItem, CsSelectBoxItem, CsTextAreaItem, CsInputDateItem, CsInputDateRangeItem, CsInputNumberItem, CsInputNumberRangeItem, CsMultiCheckBoxItem ... |
| CsEvent を継承したEvent | CsXxxValidationEvent, CsMutateButtonClickEvent, CsQueryButtonClickEvent |

これらのItemやEventを受け取って画面表示と処理を行うXxInputText, XxCheckBox, XxMutateButton, XxQueryButton (XxはAntDesignならAx, Material UIならMux, BootstrapならBsx)などがあります。論理情報クラスは、`Cs` で始まり、画面コンポーネントは `Ax`, `Mux`, `Bsx`で始まります。
以降の説明では、Ant Designのコンポーネントを例として記述していきます。

なお、CsValidationEventは、useCsViewの内部で作られるバリデーション処理を司るクラスです。利用者が直接作ることはほぼありえません。バリデーション処理を呼び出したい場合に使うことはあります。

### メモ
danmachiリポジトリの内容をコピー、内容は修正する可能性あり