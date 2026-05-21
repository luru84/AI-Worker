# Development Flow

`AI-Worker` で開発案件を進めるときの標準フローです。目的は、AI 同士の役割分担だけでなく、`次に進んでよい条件` を固定してぶれを減らすことです。

## Phase 1: Intake
- Owner: `assistant-agent`
- What:
  - 依頼を 3-7 行に要約する
  - 新規 work か既存 work への追記かを決める
  - 完了条件と未確定事項を分ける
- Go condition:
  - `work/<date>/README.md` に作業定義が書かれている
  - `Work type`, `Expected outcome`, `Related repos` が埋まっている

## Phase 2: Routing / Planning
- Owner: `orchestrator`
- What:
  - 作業タイプを `research / analysis / implementation / review / investigation` から決める
  - 必要な agent と並列可否を決める
  - 実装がある場合は repo 健全性チェックを先に行う
- Repo health check:
  - 現在 branch と upstream を確認する
  - `main` との差分と remote 状態を確認する
  - upstream 消失や branch 破損があれば、実装前に branch 方針を決め直す
- Go condition:
  - `Assigned Roles` と `Parallelizable tasks` が work に書かれている
  - 実装がある場合は branch 方針が決まっている

## Phase 3: Research / Pre-Implementation Review
- Owner: `researcher` + `security-agent` + optional `reviewer`
- What:
  - 変更方針、変更しない範囲、既知リスクを整理する
  - secrets / PII / cross-repo leakage / prompt injection を先に見る
  - 必要なら reviewer が実装前レビューを行う
- Go condition:
  - 実装対象と非対象が言語化されている
  - blocking な security 懸念がない
  - 実装前レビューの重大懸念が解消されている

## Phase 4: Branch Setup
- Owner: `github-operator`
- What:
  - branch を切る or 既存 branch を継続する判断を行う
  - 原則は最新の `main` から新しい branch を切る
  - 既存 branch の upstream が壊れている場合は継続せず、方針を work に残す
- Default rules:
  - 新規実装は `main` 起点の新 branch を優先する
  - 調査だけの work では branch を切らない
  - merge 後は branch を削除する
- Go condition:
  - `Repo State` に branch 状態が記録されている
  - 実装先 repo で安全に作業開始できる

## Phase 5: Implementation
- Owner: `repo-implementer`
- What:
  - 対象 repo で実装する
  - 必要ならファイル単位で worker を分ける
  - 実装者は単独で完了判定しない
- Go condition:
  - 差分が揃っている
  - 最低限のローカル検証が実行されている

## Phase 6: Verification
- Owner: `qa-runner` + `reviewer`
- What:
  - `typecheck / test / build` などの実行確認
  - 差分レビュー
  - 実機未確認などの残リスク整理
- Minimum gate for PR:
  - 実行系チェック結果がある
  - reviewer の重大指摘が closing されている
  - 残リスクが work に明記されている

## Phase 7: Publish
- Owner: `security-agent` + `github-operator`
- What:
  - PR 前に公開内容の再確認
  - push, PR, merge を担当
- Go condition:
  - `Verification` と `Remaining Risks` が work に書かれている
  - 公開不可の情報が差分に含まれていない

## Phase 8: Closeout
- Owner: `assistant-agent` + `context-curator`
- What:
  - merge 後に `main` へ戻す
  - 最新同期、作業 branch 削除
  - work に `Implementation Result`, `Verification`, `Remaining Risks` を残す
  - 再利用価値のある判断だけを `agent/decisions.md` や `agent/context/` に移す
- Done condition:
  - repo 後始末が済んでいる
  - work が監査ログとして読める

## Work Update Timing
- 着手時: `Work Definition`
- 方針確定時: `Assigned Roles`, `Chosen Implementation Direction`
- PR 前: `Verification`, `Remaining Risks`
- merge 後: `Implementation Result`, `Agent Checks`, closeout
