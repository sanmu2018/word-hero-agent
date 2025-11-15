# Repository Guidelines

## Project Structure & Module Organization
This Go workspace keeps executable entry points in `cmd/word-hero-agent`, while reusable packages live under `internal/` for app-specific logic (e.g., puzzle solvers, queue clients) and `pkg/` for shareable helpers. Place prompt templates, seeds, and large dictionaries in `assets/` and document any generated files inside `docs/`. Test fixtures sit beside the code they validate in folders such as `internal/game` with `_testdata` subdirectories. Use `configs/` for YAML or JSON runtime settings so local and deployment profiles remain consistent.

## Build, Test, and Development Commands
Run `go build ./...` to ensure every package compiles before committing. Use `go run ./cmd/word-hero-agent` for manual verification of CLI behavior. Execute `go test ./...` for the default suite, `go test -race ./...` before publishing branches that touch concurrency, and `golangci-lint run` (configured via `.golangci.yml`) for linting plus static checks. `make fmt` is provided as a shortcut for `gofmt`/`goimports` on staged files.

## Coding Style & Naming Conventions
Stick to idiomatic Go: tabs for indentation, camelCase for locals, PascalCase for exported items, and `_test.go` suffixes for test files. Keep packages focused and named with short nouns (`internal/solver`). All code must pass `gofmt` and `goimports`; pre-commit hooks enforce this. Document exported functions with GoDoc comments that start with the identifier name. Configuration structs and environment variables follow the `WORD_HERO_*` prefix for clarity.

## Testing Guidelines
Favor table-driven tests and name cases `Test<Component>_<Scenario>` for clarity. When interacting with APIs, stub HTTP servers via `httptest` and keep payload samples in `testdata/`. Maintain ≥85% coverage as reported by `go test ./... -cover`. For agent behaviours, include end-to-end specs in `internal/simulations` that run behind the `e2e` build tag (`go test ./internal/simulations -tags e2e`).

## Commit & Pull Request Guidelines
Commits follow the conventional imperative style seen in `git log --oneline` (e.g., “Add trie-backed lexicon”). Reference issue IDs at the end of the subject when relevant. Pull requests must describe the problem, the solution, and verification steps (commands or screenshots). Link related issues, ensure CI is green, and request review from at least one agent maintainer. Screenshot UI diffs or attach logs when behavior changes to expedite review.

## Security & Configuration Tips
Store secrets such as `WORD_HERO_API_KEY` in `.env.local` (excluded via `.gitignore`) and load them through `configs/`. Never commit production dictionaries or proprietary corpora—reference them via download scripts under `scripts/`. Validate third-party prompts or plugins before integration to avoid leaking tokens.

## Task Dispatch Protocol
- 外部服务（如 `word-hero`, `word-hero-web`）的任务必须以 GitHub Issue 形式派发。
- 先在本仓库存放 Issue 描述模板（`issues/<service>-<task>.md`），便于复用与审计。
- 通过 `gh issue create --repo <owner/repo> --title <标题> --body-file <模板路径>` 直接在对应远程仓库创建 Issue，无需克隆仓库。
- 将生成的 Issue 链接记录进 `service_issues.md`，保持服务与任务状态的单一事实源。

## External Repository Access
- 当需要审查外部服务代码时，允许在 `.tmp/repos/` 目录下执行 `git clone` 以便本地查看或运行轻量脚本。
- `.tmp/repos/` 视为临时空间，只用于读取/分析，不在其中提交或推送代码；完成后可选择清理。
- 若需在这些克隆仓库中运行命令，先确认不会污染其 Git 历史，并避免将结果混入 `word-hero-agent` 工作树。
