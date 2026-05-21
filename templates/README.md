# Templates

このディレクトリは「新しい work を始める時の考え方」を置く場所として使います。テンプレート一式をコピーする前提ではありません。

## 基本方針
- 先に今回の work で何をするかを短く決める
- その後 `work/YYYY-MM-DD/` または `work/YYYY-MM-DD-a/` を直接作る
- 必要な md だけ作る
- 固定テンプレートは持たず、作業の種類に応じて最小構成で始める

## 進め方
1. `work/YYYY-MM-DD/` または `work/YYYY-MM-DD-a/` を作る
2. `README.md` にその作業フォルダの定義を書く
3. 必要に応じて `intake.md`, `plan.md`, `task.md`, `target-repos.md`, `review.md` を追加する
4. 親 Codex が必要なら子 Codex に作業を分配する

## `README.md` に書く内容
- Work name
- Work type
- What to do
- Why now
- Expected outcome
- Related repos
- Parallelizable tasks
- Notes

## よく作るファイル
- `README.md`: その作業フォルダの定義
- `intake.md`: 最初の相談メモ
- `plan.md`: 親 Codex の段取り
- `task.md`: 役割分担
- `security.md`: 公開や実行前の security 観点
- `target-repos.md`: 実装対象の repo
- `review.md`: 確認結果

## 目的
- 毎回ゼロから考えずに work を始められるようにする
- 同じ日の複数作業を分離する
- 調査、解析、実装、レビューを同じ型で起票できるようにする
