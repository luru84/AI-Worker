# AI-Worker

`AI-Worker` は、親 Codex が複数の子 Codex を束ねるための共通ワークスペースです。開発だけでなく、要件整理、調査、解析、レビュー、実装指示の起点として使います。

## 前提
- ここはプロダクトの実装リポジトリではない
- 実コードの変更が必要な場合だけ `../repo-a`, `../repo-b` のような兄弟ディレクトリを使う
- このフォルダでは `会話`, `判断`, `調査メモ`, `担当分解`, `作業履歴` を扱う

## 想定ディレクトリ
```text
<workspace-root>/
├─ AI-Worker/
├─ repo-a/
├─ repo-b/
└─ ...
```

## 構成
```text
AI-Worker/
├─ agent/
│  ├─ assistant-agent.md
│  ├─ orchestrator.md
│  ├─ researcher.md
│  ├─ security-agent.md
│  ├─ repo-implementer.md
│  ├─ github-operator.md
│  ├─ context-curator.md
│  ├─ reviewer.md
│  ├─ qa-runner.md
│  ├─ token-guard.md
│  ├─ decisions.md
│  ├─ prompts/system.md
│  └─ context/README.md
├─ docs/
│  ├─ ARCHITECTURE.md
│  ├─ DATA_DICTIONARY.md
│  └─ EVALUATION.md
├─ SECURITY.md
├─ templates/
│  └─ README.md
├─ work/
│  └─ 2026-05-21/
└─ ...
```

## work の使い方
- `work/` 直下には日付フォルダだけを作る
- 1 日に複数作業がある場合は `2026-05-21-a`, `2026-05-21-b` のように分ける
- 各日付フォルダではまず `README.md` を作り、その work の定義を書く
- `intake.md`, `goal.md`, `plan.md`, `task.md`, `log.md`, `review.md`, `target-repos.md` は `README.md` だけでは足りない時に追加する
- 新しい作業を始めるときは日付フォルダを直接作る
- 何を作るか迷うときは `templates/README.md` を見て必要なファイルだけ作る

## 最初の始め方
1. `assistant-agent` を窓口として会話を始める
2. `work/YYYY-MM-DD-topic/README.md` を作る
3. `README.md` に work の定義を書く
4. `assistant-agent` が必要な役割を選び、`orchestrator` に割り振る
5. 実装が必要な時だけ兄弟リポジトリを対象にする

## 最小の work 例
```md
# Work Definition

- Work name: example task
- Work type: research
- What to do: 何を整理・調査・実装したいか
- Why now: なぜ今やるか
- Expected outcome: 何が揃えば完了か
- Related repos: none
- Parallelizable tasks: researcher, security-agent
- Notes:
```

## 会話の始め方
- まず `assistant-agent` に自然文で依頼する
- `assistant-agent` が work を切る必要があるか、既存 work に追記すべきかを判断する
- 必要なら `orchestrator` が裏側で役割分担する

### 例1: 調査中心
```txt
assistant-agentへ:
この課題を進めたい。まず関連技術の比較とリスク整理をしたい。
実装はまだしない。並列で調査できるなら分けてほしい。
```

### 例2: 実装あり
```txt
assistant-agentへ:
repo-a の認証まわりを見直したい。
先に影響範囲を調べて、その後で必要なら実装、最後にレビューまで回してほしい。
```

### 例3: GitHub公開前チェック
```txt
assistant-agentへ:
AI-Worker を GitHub に上げる前提で最終確認したい。
security-agent と github-operator を含めて、公開前チェックを並列で進めてほしい。
```

## README と補助ファイルの線引き
- `README.md`: 必須。作業定義の本体
- `intake.md`: 会話メモを分けたい時だけ作る
- `plan.md`: 段取りが長くなる時だけ作る
- `task.md`: 役割分担を明示したい時だけ作る
- `target-repos.md`: 実装対象が複数ある時だけ作る
- `review.md`: 確認結果を README から分けたい時だけ作る

## 役割
- `assistant-agent`: 普段の会話窓口。相談整理、作業タイプ判定、担当割当の起案
- `orchestrator`: 親 Codex。全体の分解、担当割当、停止判断
- `researcher`: 要件整理、調査、比較、解析
- `security-agent`: 秘密情報、個人情報、プロンプトインジェクション、権限境界の確認
- `repo-implementer`: 実装先リポジトリでの変更とテスト
- `github-operator`: branch, commit, PR, issue など GitHub 操作の専任
- `context-curator`: 再利用する知識、判断、要約の整理
- `reviewer`: 差分レビューとリスク確認
- `qa-runner`: 受け入れ確認、再現確認、実行確認
- `token-guard`: トークン消費の抑制ルール

## トークン節約ルール
- 子 Codex には対象フォルダと目的だけを渡す
- 調査と実装とレビューを 1 スレッドに混ぜない
- 親が要約してから子へ渡す
- 実装前に対象リポジトリと触る範囲を固定する
- 完了条件が曖昧なら先に work の `README.md` か `intake.md` で狭める

## 基本フロー
1. あなたはまず `assistant-agent` と話す
2. `assistant-agent` が相談内容を `work/<date>/README.md` を起点に整理する
3. `assistant-agent` が作業タイプを判定し、`orchestrator` に引き渡す
4. `orchestrator` が必要に応じて `researcher`, `security-agent`, `repo-implementer`, `qa-runner`, `reviewer`, `github-operator`, `context-curator` へ並列割当する
5. 最終結果は `assistant-agent` がまとめて返す

## assistant-agent と orchestrator の関係
- `assistant-agent` はユーザー向けの窓口役
- `orchestrator` は内部の分配役
- 単一の Codex セッションで順番に演じてもよい
- 本当に並列化したい時だけ別エージェントとして分ける

## 並列化の考え方
- `researcher` と `security-agent` は早い段階で並列に走らせる
- 実装が必要なら `repo-implementer` を起動し、その後 `qa-runner` と `reviewer` を並列で走らせる
- GitHub に反映する時だけ `github-operator` を使う
- 継続利用する知見は最後に `context-curator` が `agent/context/` と `agent/decisions.md` に圧縮する

## GitHub 公開前提の基本ルール
- `work/` は Git に含めない
- push 前に `git status --ignored` で `work/` が追跡対象に入っていないことを確認する
- トークン、個人情報、顧客データ、実リポジトリ由来の秘密情報は貼らない
- 実運用の secret は環境変数やローカル secure store で持ち、この repo に置かない
- 初回公開前に `SECURITY.md` のチェックリストで確認する
- データ境界は `DATA_HANDLING.md` に従う

## 備考
- このフォルダは開発、調査、解析、レビューの共通起点として使う
- 実装のテンプレート置き場ではなく、作業の起票テンプレートを中心に運用する
