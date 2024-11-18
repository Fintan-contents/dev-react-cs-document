---
sidebar_position: 3
title: セレクトボックスの選択肢を API で取得する方法
---

セレクトボックスのオプションの値に API で取得した値を採用するようなオプションが動的に変わる場合に本節が役立ちます。

## 実装方法

```tsx
const loadUserView = useLoadUsersView();
const users = loadUsersView.loadUsersEvent.response ?? []; // IdとName属性を持つオブジェクト配列

todoCreateView.assignee.setOptions(users, "id", "name");
```
