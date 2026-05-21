# Decisions

- 2026-05-21: `AI-Worker` は開発専用ではなく、調査や解析も含む共通 AI 作業場とする
- 2026-05-21: 実装コードは兄弟ディレクトリの各リポジトリで扱う
- 2026-05-21: `work/` は日付フォルダ単位で管理し、直下に作業用 md を置かない
- 2026-05-21: 親 Codex がオーケストレータとなり、子 Codex を役割別に使い分ける
- 2026-05-21: 普段の対話窓口として `assistant-agent` を置き、割り振り前の整理を担当させる
- 2026-05-21: GitHub 管理前提で `security-agent`, `github-operator`, `context-curator`, `qa-runner` を追加する
