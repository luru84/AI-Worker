# Architecture

```text
User
  -> Assistant Agent
     -> Parent Codex (`AI-Worker`)
        -> Researcher child
        -> Security child
        -> Repo implementer child
        -> QA child
        -> Reviewer child
        -> GitHub operator child
        -> Context curator child

Assistant Agent
  -> intake, work classification, routing

Parent Codex
  -> state and conversation: `AI-Worker/work/<date>/`
  -> role definitions: `AI-Worker/agent/`
  -> implementation targets: `../repo-a`, `../repo-b`, ...
```

## Boundaries
- `assistant-agent`: 会話窓口、相談整理、割当起案
- `AI-Worker`: 判断、調査、解析、担当分解
- `../repo-*`: コード、テスト、Git、GitHub

## Parallel fan-out
- Early phase: `researcher` + `security-agent`
- Delivery phase: `repo-implementer`
- Verification phase: `qa-runner` + `reviewer`
- Publish phase: `github-operator`
- Retention phase: `context-curator`
