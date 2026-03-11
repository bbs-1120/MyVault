# manji1120 プロジェクトで行ったこと

> Cursorで作成した開発記録をObsidianに連携（2026-03-11）
> 会話の全文は `docs/manji1120-会話まとめ.md`（manji1120プロジェクト内）を参照。

**最終更新**: 2026-03-11

---

## 1. プロジェクト概要

- **名称**: CPN自動仕分けシステム（manji1120）
- **目的**: スプレッドシートに集約された広告運用データをCPN単位で「停止／作り替え／継続／要確認」に自動仕分けし、Chatworkへ通知。日次の運用判断を効率化する。
- **技術**: Next.js 15 (App Router), Tailwind CSS, Prisma, PostgreSQL, Google Sheets API, Chatwork API, TypeScript, 本番は Google Cloud (Cloud Run / Cloud SQL)
- **対象媒体**: Meta, TikTok, YouTube, Pangle, LINE

---

## 2. 開発で行ったこと（実施内容一覧）

### コア機能

- CPN自動仕分けシステムの実装（判定ロジック・結果一覧・手修正・Chatwork送信）
- 判定結果ごとの詳細ページ、マイ分析ページの追加
- データ引用元を CSV_当日精査用・CSV抽出 に変更
- 12月利益の計算追加
- キャッシュ機能で読み込み高速化
- 予算変更機能のUI追加
- Meta複数ビジネスマネージャー対応、TikTok/Pangle複数広告主ID対応
- 広告ON/OFF機能の追加
- 広告アカウント名から広告主IDを特定する機能
- キャンペーンID(CPID)をスプレッドシートから取得する対応
- Smart Performance Campaign 対応

### 運用・通知

- LINE通知・プッシュ通知・異常検知AIの追加
- Chatworkエラー通知（別ルーム対応）
- 自動停止の成功通知削除、エラー通知の簡潔化
- エラー通知を各メンバーごとにTO付きで個別送信
- 悠太さんCC（ChatWork ID 10168657）、メンバーTO（正弥・圭市・祐輝）の設定
- 全エラー通知にメンバーTO + 悠太CCを追加
- 自動停止ログをエラーのみ保存、ChatWork TOメンション対応

### 自動停止まわり

- 自動停止ルールの改修、媒体・オファー複数選択対応
- 自動停止ルールにプリセットテンプレートを追加
- 自動停止のスケジュール実行機能（0:00 等）
- オファー・媒体別の自動停止ルール対応
- 自動停止スケジュールAPIの公開エンドポイント追加
- 自動停止ルール保存の二重クリック防止
- Smart Plus エラーハンドリング改善（API制限時はChatWork通知をスキップ）

### 翌日ON・スケジュール

- 翌日ON予約機能（0:00に自動でCPNをON）
- 翌日ONのCPNのIDが取得できなくなる問題の修正（前日CPN一覧からもID取得）
- スケジュール取得の改善（10分間隔、表示の優先度調整）
- スケジュール自動取得の改善
- Meta APIのスケジュール形式修正、予算スケジュール表示の改善
- Campaign IDがない場合に「ID未取得」と表示
- 予算スケジュール表示を希望形式に変更、追加予算/期限列の表示改善
- スケジュール詳細列（予算額・終了日）の追加
- スケジュール表示でDBマッピングから不足 campaignId を補完
- スケジュール用CPN参照の改善（DBマッピング + 当日データ）
- スケジュール自動送信（10:00/11:00）の完全停止

### 週次レポート・チーム分析

- 週次レポートの改善、媒体表示をアイコン付きに変更
- 週次レポートのマッピング同期機能、TikTok Upgraded Smart Plus対応
- 週次レポート: 全CPN表示、CR単位データ、案件別コメント機能
- チーム分析ページにマイ分析と同等の機能を追加
- ホーム画面でユーザー別データ表示に修正、Instagram風UIに改修
- team_name がない場合はユーザー名でフィルタリング

### データ・マッピング・API

- CPNマッピングをDB永続化（全CPNでクリエイティブ表示可能に）
- TikTok_Todayシートから advertiserId を取得する機能
- TikTok advertiser_id がマッピングにない場合はAPIで自動検索
- scheduled エンドポイントのTikTok APIを明示的な文字列変換に統一
- TikTok API呼び出しで明示的な文字列変換を追加
- サイト高速化のための全面的な最適化
- CPN詳細データ表示機能の追加
- 単価マスタ機能の追加
- クリエイティブAPIの改善
- 分析APIのエラー詳細追加、Google Sheets クライアントのエラーメッセージ詳細化

### 判定ルール・UI

- 判定ルールの更新・適用ボタンと操作ログ機能
- メンバー別カスタム判定ルール機能
- Shift+クリックでチェックボックスの範囲選択
- Phase2: CPN詳細テーブル（全指標）と案件×媒体マトリックスを追加
- リアルタイム時計と変更履歴機能
- TikTok status API を campaign/update 使用に変更、operation_status 追加、budget_mode 追加で予算変更エラー修正
- Meta/TikTok APIトークンの本番環境追加

### インフラ・CI/CD

- CI/CDパイプライン追加とGCP環境構築
- Cloud Build によるデプロイ（--source）
- デプロイワークフローの修正（Cloud Run URL、手動トリガー）
- Artifact Registry 作成のオプション化、ワークフロー更新
- IAM・権限まわりの設定と再デプロイ
- 0:00–0:30 のメンテナンスモード
- GrowthDeck へのリネーム + モバイル改善 + 自動リフレッシュ
- Dockerfile: prisma migrate deploy 追加、prisma CLI をランナーに含める、起動時マイグレーション削除

### その他

- 自動Chatwork送信の一時停止
- Prismaスキーマの同期と型エラー修正
- Obsidian連携: Cursor会話をMarkdownでエクスポートするスクリプトとドキュメント（`npm run obsidian:markdown` / `npm run obsidian:export`）

---

## 3. Cursor でのやりとり（要約）

- **Obsidian連携**: 会話をMarkdownでまとめ、クロード経由でObsidianに連携するためのエクスポートスクリプトと `docs/manji1120-会話まとめ.md` を用意。
- **翌日ONのCPN ID取得**: 当日シートに載らないCPNは前日CPN一覧からもIDを取得するよう `fetchYesterdayData` を追加し、`scheduled-on` API でマージするよう改修。

---

## 4. ドキュメント・参照

| ファイル | 内容 |
|----------|------|
| `README.md` | 概要・技術スタック・仕分けロジック・セットアップ |
| `docs/requirements.md` | 要件定義書 |
| `docs/OBSIDIAN_SETUP.md` | Obsidian連携の手順 |
| `docs/manji1120-会話まとめ.md` | Cursor会話の全文（クロードに送る用） |
| `docs/CI_CD_SETUP.md` | CI/CD の設定 |

---

## 5. 運用メモ

- 仕分け判定優先順位: 停止 ＞ 作り替え ＞ 継続 ＞ 要確認
- 会話をObsidianに反映したいときは `npm run obsidian:markdown` で `docs/manji1120-会話まとめ.md` を更新し、その内容をクロードに送る。
