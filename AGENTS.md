# Repository Guidelines

## Agent-Specific Instructions
- 日本語で簡潔かつ丁寧に会話をしてください。
- コミットメッセージは日本語で書いてください。
- 「学習を開始します」と伝えられたら、`CODEX_LEARNING_PLAN.md` を読み込み、直前の進捗から続きの実施を案内してください。

## Project Structure & Module Organization
- ルートに `README.md` と学習計画 `CODEX_LEARNING_PLAN.md` があり、実装は `app/` 配下に集約されます。
- アプリ本体は `app/main.py`、依存管理は `app/pyproject.toml` と `app/uv.lock` です。
- 静的解析の設定は `app/ruff.toml`、型チェック設定は `app/ty.toml` にあります。

## Build, Test, and Development Commands
- 実行: `python app/main.py` (Python 3.14)。
- 依存導入: `uv` を使う場合は `uv sync` を想定。
- フォーマット: `ruff format app`。
- 解析: `ruff check app`。

## Coding Style & Naming Conventions
- インデントは 4 スペース、行長は 88、文字列はダブルクォートを基本とします。
- 関数や変数は PEP8 の `snake_case`、クラスは `PascalCase` を推奨します。
- ルールは `app/ruff.toml` が一次情報なので、差分を出す前に `ruff` を通してください。

## Testing Guidelines
- 現状テストは未整備です。追加する場合は `tests/` 配下に `test_*.py` を置く方針を推奨します。
- テストフレームワークは未固定のため、導入時に `pyproject.toml` へ明記してください。

## Commit & Pull Request Guidelines
- Git 履歴は短いメッセージ (例: "first commit") のみのため、現時点で厳密な規約はありません。
- 以後は「目的が分かる短い要約 + 必要なら詳細」を基本にし、1 変更 1 目的でまとめてください。
- PR では目的、変更点、動作確認 (例: `ruff check app`) を明記し、関連 Issue があればリンクします。

## Security & Configuration Tips
- `google-genai` を使うため、API キー等の機密情報は環境変数で管理し、リポジトリに追加しないでください。
