# ファイル命名規則

このドキュメントは、時系列で記事を管理するためのファイル命名規則を定義します。

## 基本原則

### 1. 時系列確認の重視

記事を日付順で確認できるよう、**日付プレフィックスを必須**とします。

### 2. 一貫性の保持

すべての記事で統一された命名規則を適用し、管理しやすさを重視します。

## 命名規則詳細

### 完成記事（`articles/`）

**形式**: `YYYY-MM-DD-[内容]-[タイプ].md`

**必須要素**:

- **日付プレフィックス**: `YYYY-MM-DD-` 形式（記事作成日または公開予定日）
- **内容**: 記事の主要テーマ（ハイフン区切り）
- **タイプ**: 記事の種類を示すサフィックス

**記事タイプ**:

- `-memo`: 技術を触ってみた感想・メモ（tech-memo.mdテンプレート）
- `-tips`: Tips・ノウハウ記事（tips.mdテンプレート）
- `-log`: 開発記録記事（dev-log.mdテンプレート）

**例**:

```txt
articles/
├── 2025-07-05-docker-compose-memo.md
├── 2025-07-06-vscode-shortcuts-tips.md
├── 2025-07-07-personal-blog-dev-log.md
└── 2025-07-08-react-hooks-memo.md
```

### 下書き記事（`drafts/`）

**完成予定日が決まっている場合**:
`YYYY-MM-DD-[内容]-[タイプ].md`（完成記事と同じ形式）

**作業中で日付未定の場合**:

- `wip-[内容].md` (Work In Progress)
- `temp-[内容].md` (Temporary)
- `draft-[内容].md` (Draft)

**例**:

```txt
drafts/
├── 2025-07-10-nextjs-learning-memo.md  # 日付決定済み
├── wip-aws-lambda-tutorial.md          # 作業中
├── temp-typescript-notes.md            # 一時的なメモ
└── draft-microservices-architecture.md # 構想段階
```

## 時系列確認の方法

### 1. ファイル名ソート

```bash
# articlesディレクトリで日付順表示
ls articles/ | sort

# より詳細な情報付きで表示
ls -la articles/ | sort -k9
```

### 2. 最新記事の確認

```bash
# 最新の記事を確認
ls articles/ | sort | tail -5

# 最古の記事を確認
ls articles/ | sort | head -5
```

### 3. 特定期間の記事検索

```bash
# 2025年7月の記事のみ表示
ls articles/ | grep "^2025-07"

# 特定の記事タイプのみ表示
ls articles/ | grep "memo.md$"
```

## 内容部分の命名ガイドライン

### 技術名の表記

- **小文字とハイフンを使用**: `docker-compose`, `next-js`, `react-hooks`
- **略語は小文字**: `aws`, `api`, `cli`
- **数字はそのまま**: `vue3`, `node18`

### 内容の表現

- **具体的で簡潔に**: `getting-started` より `setup-guide`
- **動詞を活用**: `using-docker`, `building-api`, `deploying-app`
- **レベル表記**: `basic`, `advanced`, `intermediate`

**良い例**:

```txt
2025-07-05-docker-compose-basic-memo.md
2025-07-06-vscode-productivity-tips.md
2025-07-07-nextjs-deployment-log.md
```

**避けるべき例**:

```txt
2025-07-05-memo.md                    # 内容が不明
docker-compose-memo.md                # 日付なし
2025-07-05-docker_compose_memo.md     # アンダースコア使用
```

## 画像ファイルの命名

### ディレクトリ名

記事のファイル名から日付とタイプを除いた部分を使用:

```txt
images/
├── docker-compose-basic/     # 2025-07-05-docker-compose-basic-memo.md の画像
├── vscode-productivity/      # 2025-07-06-vscode-productivity-tips.md の画像
└── nextjs-deployment/        # 2025-07-07-nextjs-deployment-log.md の画像
```

### 画像ファイル名

```txt
screenshot-[連番].png          # screenshot-01.png, screenshot-02.png
diagram-[内容].png             # diagram-architecture.png
demo-[機能].gif               # demo-animation.gif
```

## 命名規則チェックリスト

### 完成記事作成時

- [ ] 日付プレフィックスが正しい形式（YYYY-MM-DD-）
- [ ] 内容部分が具体的で理解しやすい
- [ ] 記事タイプサフィックスが適切（-memo, -tips, -log）
- [ ] ハイフン区切りで統一されている
- [ ] 既存記事と重複していない

### 下書きから完成記事への移動時

- [ ] 日付が最新の作成日/公開予定日に更新されている
- [ ] ファイル名が命名規則に適合している
- [ ] 対応する画像ディレクトリも更新されている

---

**最終更新**: 2025-07-05
