# 🌌 MoonQAプロジェクト構造ガイド

---

## 🌕 フェーズ一覧と現在地

| フェーズ                     | 目的                                | ステータス    |
| ------------------------ | --------------------------------- | -------- |
| **Phase 1: ベース構築**       | 開発基盤を整える（ディレクトリ設計、`.env`管理）       | ✅ 完了済み   |
| **Phase 2: テスト整備**       | 自動テストの枠組みを構築、基本精度を担保              | ✅ 完了済み   |
| **Phase 3: 精度＆制御強化**     | `target_pdf`やスコアしきい値で検索の精度と制御性を向上 | ✅ 完了済み   |
| **Phase 4: ログと可視化**      | QA結果をログ出力し、後分析しやすくする              | ✅ 完了済み   |
| **Phase 5: CI対応**        | GitHub Actionsなどで自動テスト運用を整備       | ✅ 完了済み   |
| **Phase 6: インテリジェンス強化**   | 構造判断力を高める      | 今ここ   |
---

## 🌕 フェーズ別タスクログ

---

### 🌙 Phase 1: ベース構築（完了）
- `01_20250527_ingestとqa構築.md` - 初期の開発基盤構築について記載
- `02_20250528_qa_検索観察モード_スコア付きログ基盤.md` - ログ出力機能の実装
📅 **Date: 2025-05-27 \[Tuesday]**

* ディレクトリ設計、`.env`管理
* `main.py` を使うか／削除するか検討

---

### 🌙 Phase 2: テスト整備（完了）
📅 **Date: 2025-05-29 \[Thirsday]** 17:20〜 **2025-05-30**
- `03_20250529_chunk.md` - チャンク処理の実装とテスト
- `04_20250530_test.md` - テストフレームワークの整備について

#### Task 1: `main.py` の役割を明確化

* \[☑️] 使用されていないなら削除
* [ ] 使用するなら目的を `README` に明記

#### Task 2: `scripts/` ディレクトリの整理

* \[☑️] 各スクリプトの役割をコメントで記述
* \[☑️] 本番用／テスト用の区分を `README.md` に明示

#### Task 3: `settings.py` の意図を明文化

* \[☑️] `.env`と`settings.py`の役割分担を整理
* \[☑️] 将来的な拡張に備えて構造を設計
* \[☑️] 「設定変更場所の案内」を `README` に追加

#### Task 4: 自動テストの整備

* \[☑️] テスト網羅状況を確認（7件のパス実績）
* \[☑️] `pytest` での実行が安定することを確認

---

### 🌙 Phase 3: 精度＆制御強化（完了）
🗓️ 2025/05/31 \[Saturday] 07:37〜
- `05_20250531_multiPDF.md` - target_pdf切り替え対応
- `06_20250531_filteringBySourceMetadata.md` - スコアしきい値導入など
-
* \[☑️] `target_pdf` 切り替え対応
* \[☑️] 類似度スコアのしきい値導入

---

### 🌙 Phase 4: ログと可視化（完了）
🗓️ 2025/05/31 \[Saturday] 12:10 〜 **2025-06-01**
- `07_20250531_addTestCase.md` - テストケースの追加とログ品質チェック

#### Task 1: スコアと出典のログ構造設計

* \[☑️] JSON形式でログ設計（source, page 含む）
* \[☑️] 出力ディレクトリ：`logs/qa_test_results.json`
* \[☑️] 記録項目：質問・PDF・スコア・チャンク数・回答文など

#### Task 2: ログ出力機能の追加

* \[☑️] `--log-output` オプションで出力切替
* \[☑️] append形式で質問ごとの記録を残す
* \[☑️] 出力内容の正確性をテストで確認済み

#### Task 3: ログ品質チェックと分析基盤

* \[☑️] JSONとして構造が正しいか確認
* \[☑️] 出力内容（ページ数・ファイル名等）チェック
* \[☑️] UI連携を想定した項目設計（例：query\_id, timestamp）

---

## 🌙 Phase 5（CI対応）:CIで自動テストと品質管理を仕組み化する
🗓️ 2025/06/01 \[Sunday] 08:50 〜
- `08_20250601_CI構成.md` - CI/CD構築について

### Task 0: CI構成の設計（準備タスク）

* [☑️] `pytest` 実行対象は `test_log_entry_structure.py` のみに限定
* [☑️] `manual_*.py` はCI対象から除外（出力ノイズ対策）
* [☑️] `.env.example` を用意（CIに必要な環境変数だけ定義）
* [☑️] `.github/workflows/ci.yml` を新規作成（poetry + pytest実行）
* [☑️] ログ出力先は `logs/qa_log.jsonl` に統一（`.gitignore`対象）

---

## 🌙 Phase 6: インテリジェンス強化 （PDF版を使い込むことで、構造判断力を高める段階）
🗓️ 2025/06/02 \[Monday] 18:23
- `09_20250602_phase6_task1.md`

### 🔸 Task 1: 質問ログのquery_id対応＋構造強化

* [☑️] ログにも `"status": "notfound"` を記録
* [☑️] すべての質問にユニークID（`query_id`）を発行
* [☑️] `qa_log.jsonl` に質問、回答、スコア、分類、ファイル名、ページ番号などを保存
* [☑️] ログ出力を共通化（`logger.py`など）

### 🔸 Task 2: 複合フィルター条件への対応

* [☑️] `source`, `tag`, `date` などを同時にfilterできる構造へ
* [☑️] filterビルダーを共通関数に抽出（`filters.py` など）
* [ ] テストケースで正確にヒットするか検証

### 🔸 Task 3: intent分類（PDF質問／雑談／命令）

* [☑️] rule-based分類で、質問をタイプ分類（Intent: `pdf_question`, `chitchat`, `command`）
* [☑️] `classifier.py`を新設し、単語出現などでラフに分類
* [☑️] ログにも分類結果を記録
