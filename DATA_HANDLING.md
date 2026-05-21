# Data Handling

このリポジトリは AI 作業運用ルールを管理するためのものです。保存してよい情報と保存してはいけない情報の境界を明確にします。

## Allowed
- 役割定義
- 一般化された運用ルール
- 公開可能な判断ログ
- 秘密情報を含まない短い要約

## Not Allowed
- API keys, access tokens, cookies, session data
- 個人情報、顧客情報、社内限定情報
- 公開してはいけないコード断片や設定値
- 実装リポジトリの未公開仕様をそのまま転載したメモ
- 外部サービスの認証情報やローカル資格情報

## Work Folder Policy
- `work/` はローカル限定で扱う
- `work/` に入れた内容は公開前提で書かない
- 公開したい知見は `context-curator` が一般化して `agent/context/` か恒久文書へ移す

## Agent Routing Rules
- `assistant-agent` は入力を要約してから配る
- `security-agent` は GitHub 反映前に secrets / PII / prompt injection を確認する
- `github-operator` は公開対象だけを扱う

## If Sensitive Data Is Found
1. そのデータを削除する
2. 流出した資格情報は revoke / rotate する
3. 必要なら履歴除去を行う
4. 再発防止として `.gitignore` と運用ルールを見直す
