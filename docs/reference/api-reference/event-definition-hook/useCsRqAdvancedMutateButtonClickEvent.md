---
sidebar_position: 2
title: useCsRqAdvancedMutateButtonClickEvent
---

:::warning
執筆途中
:::

`useCsRqAdvancedMutateButtonClickEvent` は、`RqAdvancedMutationResult` を使用して `CsRqAdvancedMutateButtonClickEvent` を初期化するためのフックです。

## シグネチャ

<h3>`useCsRqAdvancedMutateButtonClickEvent<TApiRequest, TApiResponse, TApiError, TContext = unknown>(mutationResult: RqAdvancedMutationResult<TApiResponse, TApiError, TApiRequest, TContext>): CsRqAdvancedMutateButtonClickEvent<TApiRequest, TApiResponse, TApiError, TContext>`</h3>

## 引数

| 引数名         | 必須 | 型                                                                         | 説明                               |
| -------------- | ---- | -------------------------------------------------------------------------- | ---------------------------------- |
| mutationResult | 〇   | `RqAdvancedMutationResult<TApiResponse, TApiError, TApiRequest, TContext>` | ミューテーション結果を指定します。 |

## 使用例

```tsx
const mutateButtonClickEvent =
  useCsRqAdvancedMutateButtonClickEvent(mutationResult);
```

## 返り値

mutationResult を使用して初期化された CsRqAdvancedMutateButtonClickEvent クラスのインスタンスを返します。
