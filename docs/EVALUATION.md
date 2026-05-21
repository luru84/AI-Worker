# Evaluation

## Success Metrics
- ユーザーは基本 `assistant-agent` とだけ話せば作業が進む
- 相談から `goal` と `plan` にすぐ落とせる
- 並列化できるタスクが `assistant-agent` から明示される
- 実装対象リポジトリを明示してから変更を始められる
- 調査、実装、レビューの責務が混ざらない
- GitHub 公開前に secrets / PII / permission の確認が入る
- 日付単位で作業履歴を分けられる
- 1 タスクあたりの探索範囲と出力量を抑えられる

## Failure Signs
- `assistant-agent` が整理せずにそのまま実装や調査を抱え込む
- `work/` 直下に作業用ファイルが増える
- 使わない固定テンプレートが増えて逆に起票が重くなる
- 実装リポジトリ未確定のままコードを書き始める
- 長いログや広いコードベースを毎回丸ごと読む
- GitHub 反映が `repo-implementer` に混ざって境界が崩れる
