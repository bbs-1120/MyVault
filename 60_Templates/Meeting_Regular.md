---
title: 経営システム本部定例 {{date}}
type: meeting
meeting_type: regular
date: {{date}}
participants: [西村]
updated: {{date}}
---

# 📋 経営システム本部定例 {{date}}

> 前回: [[70_Meetings/Regular/TODO]]

## 🎯 この時間の目的

- 経営システム本部で抱えてるプロジェクトの現状と今後の方向性
- プロジェクトの優先度すり合わせ
- その他依頼や相談など

---

## 📢 共有事項

- 

---

## ✅ 完了報告（前回以降）

- 

---

## 🔄 進行中PJ状況

```dataview
TABLE WITHOUT ID
  link(file.path, title) as "PJ",
  choice(number(progress) >= 80, "■■■■□", choice(number(progress) >= 60, "■■■□□", choice(number(progress) >= 40, "■■□□□", choice(number(progress) >= 20, "■□□□□", "□□□□□")))) + " " + progress + "%" as "進捗",
  next_action as "次にやること"
FROM "30_Projects"
WHERE type = "project" AND status = "in_progress"
SORT priority ASC
```

### 補足・議論ポイント

- 

---

## 📋 今後着手予定のPJ

```dataview
TABLE WITHOUT ID
  link(file.path, title) as "PJ",
  priority as "優先度"
FROM "30_Projects"
WHERE type = "project" AND status = "planning"
SORT priority ASC
LIMIT 5
```

### 補足・優先度相談

- 

---

## 💬 相談・依頼事項

- 

---

## 📝 決定事項・アクションアイテム

| 内容 | 担当 | 期限 | 状態 |
|------|------|------|------|
|  |  |  | ⬜ |

---

## 🔗 次回

- 日時: 
- 持ち越し議題: 


