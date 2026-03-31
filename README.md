# ISHIYA 抽選ページ

## 概要
パワーストーンの限定抽選販売ページ。LINE Harnessのフォーム機能と連携。

## URL
- 本番: https://nexry-ai.github.io/ishiya-lottery/
- リポジトリ: https://github.com/nexry-ai/ishiya-lottery

## フォーム連携（汎用）

### 使い方
URLパラメータ `?form=フォームID` でどの案件でも使い回せる。

```
ISHIYA用（デフォルト）:
https://nexry-ai.github.io/ishiya-lottery/

別案件:
https://nexry-ai.github.io/ishiya-lottery/?form=新しいフォームID
```

### 新しい案件でフォームを使う手順

1. **ダッシュボードでフォーム作成**
   - https://line-harness.pages.dev/form-submissions
   - 「新規作成」→ フォーム名・フィールドを設定
   - 作成後にフォームIDが発行される

2. **URLにフォームIDを入れる**
   ```
   https://nexry-ai.github.io/ishiya-lottery/?form=発行されたID
   ```

3. **応募データの確認**
   - ダッシュボード「フォーム回答」に自動で溜まる

### API直接（curl）でフォーム作成する場合
```bash
curl -X POST 'https://line-crm-worker.nexry-inc-ai.workers.dev/api/forms' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer nexry-line-harness-2026' \
  -d '{
    "name": "案件名",
    "description": "説明",
    "fields": [
      {"name": "name", "label": "お名前", "type": "text"},
      {"name": "email", "label": "メール", "type": "text"}
    ]
  }'
```
→ レスポンスの `data.id` がフォームID

### 応募データ取得
```bash
curl 'https://line-crm-worker.nexry-inc-ai.workers.dev/api/forms/フォームID' \
  -H 'Authorization: Bearer nexry-line-harness-2026'
```

## ページのカスタマイズ

### 変更頻度が高い箇所
- **石の名前・価格**: `index.html` の `.stone-section` 内
- **過去抽選実績**: `.past` セクション（回数・応募数・当選数・倍率）
- **締切日**: JSの `DEADLINE` 変数
- **カウンター初期値**: JSの `BASE`（開始時の応募数）、`RATE`（1時間あたりの増加数）

### カウンター設定
```javascript
const DEADLINE = new Date('2026-04-07T23:59:59+09:00'); // 締切日時
const BASE = 12;   // 開始時の応募数
const RATE = 3.5;   // 1時間あたりの増加ペース
```
- 7日間で約600名に到達する計算
- 自然なゆらぎ（sin関数）入り

## デプロイ
```bash
cd projects/ishiya-lottery
git add -A && git commit -m "update" && git push
```
→ GitHub Pages が自動で反映（1-2分）

## 連携先
- **LINE Harness Worker**: https://line-crm-worker.nexry-inc-ai.workers.dev
- **ダッシュボード**: https://line-harness.pages.dev
- **フォームID（ISHIYA）**: 2340dc62-e4bb-47f5-b951-34ce88d3ba6c
- **APIキー**: nexry-line-harness-2026
