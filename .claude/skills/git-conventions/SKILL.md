---
name: git-conventions
description: Git操作（ブランチ作成・コミット・PR作成）に関する命名規則とメッセージ規則を適用するスキル。Conventional Commits形式とGitHub Issueトラッキング（#番号）を使用。ユーザーがブランチを切る、コミットする、PRを作る、コミットメッセージを書く、ブランチ名を決めるといった場面で必ず使用すること。「コミットして」「ブランチ切って」「PRを作って」「どんなブランチ名にする？」のような操作を求められたら、このスキルを使って規則に従った操作を行うこと。
---

# Git規則スキル

このスキルはGit操作における命名・メッセージ規則を統一するためのものです。
ブランチ作成・コミット・PR作成の3つの場面で規則を適用します。

---

## コミットメッセージ規則

### 基本フォーマット

```
<type>(<scope>): <description> (#<issue番号>)
```

- `type`: 変更の種類（下記参照）
- `scope`: 変更対象のモジュール・機能（省略可）
- `description`: 変更内容を端的に表す説明（命令形・現在形）
- `#<issue番号>`: 対応するGitHub Issue番号

### typeの種類

| type       | 用途                                 |
|------------|--------------------------------------|
| `feat`     | 新機能の追加                         |
| `fix`      | バグ修正                             |
| `docs`     | ドキュメントのみの変更               |
| `style`    | コードの動作に影響しない変更（空白、フォーマット等） |
| `refactor` | バグ修正や機能追加を伴わないリファクタリング |
| `test`     | テストの追加・修正                   |
| `chore`    | ビルドプロセス・補助ツールの変更     |
| `build`    | ビルドシステムや外部依存の変更       |
| `ci`       | CI設定ファイルの変更                 |
| `perf`     | パフォーマンス改善                   |
| `revert`   | 以前のコミットの取り消し             |

### コミットメッセージ例

```
feat(auth): add JWT login functionality (#42)
fix(api): correct null pointer in user endpoint (#87)
docs(readme): update setup instructions (#12)
refactor(db): extract connection pool logic (#55)
test(auth): add unit tests for token validation (#42)
chore: update dependencies to latest versions (#3)
```

### ルール

- `description` は英語・小文字で書く（日本語でもよいが、英語を推奨）
- `description` は命令形（"add", "fix", "update"）で書く（"added", "fixes" はNG）
- 末尾にピリオドをつけない
- `(#issue番号)` は必ずコミットメッセージの末尾につける
- Issue番号が不明な場合はユーザーに確認する

---

## ブランチ命名規則

### 基本フォーマット

```
<type>/<issue番号>-<short-description>
```

- `type`: コミットのtypeと同じ（`feat`, `fix`, `docs` など）
- `issue番号`: GitHub IssueのID（`#` なしの数字のみ）
- `short-description`: ハイフン区切りの短い説明（英語・小文字）

### ブランチ名例

```
feat/42-add-jwt-login
fix/87-fix-null-pointer-user-endpoint
docs/12-update-setup-instructions
refactor/55-extract-connection-pool
test/42-add-auth-unit-tests
chore/3-update-dependencies
```

### ルール

- スラッシュ前は `type` のみ（`feature/` や `bugfix/` は使わない）
- `short-description` はハイフン区切りで5語以内を目安にする
- 大文字・スペースは使わない
- Issue番号が不明な場合はユーザーに確認する

---

## PR（プルリクエスト）規則

### タイトルフォーマット

コミットメッセージと同様のConventional Commits形式を使用：

```
<type>(<scope>): <description> (#<issue番号>)
```

### 本文テンプレート

```markdown
## 概要
<!-- このPRで何をしたか簡潔に説明 -->

## 変更内容
-
-

## 関連Issue
Closes #<issue番号>

## テスト
- [ ] 手動テスト実施済み
- [ ] 既存テストがパスすることを確認済み
```

### ルール

- タイトルはコミットメッセージの命名規則に準ずる
- 関連Issueは `Closes #<番号>` の形式でリンクする（マージ時に自動クローズされる）
- PRの本文は日本語・英語どちらでもよい

---

## 操作フロー

ユーザーからGit操作を依頼された場合は次の順で進める：

1. **Issue番号の確認** — ブランチやコミットに紐づくGitHub Issue番号をユーザーに確認する（既に提示されていれば不要）
2. **typeの判断** — 変更内容からtypeを判断し、迷う場合はユーザーに選択肢を提示する
3. **名前・メッセージの提案** — 規則に沿ったブランチ名またはコミットメッセージを提案する
4. **実行** — ユーザーが承認したら `git` コマンドで操作を実行する

### 例: ブランチ作成の流れ

```
ユーザー: ログイン機能を追加するブランチを切って (#42)
→ ブランチ名: feat/42-add-login
→ git switch -c feat/42-add-login
```

### 例: コミットの流れ

```
ユーザー: 変更をコミットして（#42 のログイン機能）
→ コミットメッセージ: feat(auth): add login functionality (#42)
→ git commit -m "feat(auth): add login functionality (#42)"
```
