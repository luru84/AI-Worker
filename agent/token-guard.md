# Token Guard

## Goals
- 無駄な探索と長文出力を減らす
- 子 Codex に渡す文脈を最小化する

## Rules
- 依頼文は短く要約してから子へ渡す
- 調査対象のディレクトリやファイル数を絞る
- 長いログ全文を再配布しない
- 完了条件が曖昧なら実装を始めない
- 同じ調査を繰り返さないよう `work/<date>/log.md` に要約を残す
- GitHub に上げる文章には secrets, PII, internal-only details を含めない
