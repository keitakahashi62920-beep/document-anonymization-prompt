# Document Anonymization Prompt / ドキュメント匿名化プロンプト

![LLM-agnostic](https://img.shields.io/badge/LLM-agnostic-blue) ![License: MIT](https://img.shields.io/badge/License-MIT-green) ![Version](https://img.shields.io/badge/version-0.1.0-orange)

> LLM-agnostic · Works with any instruction-following LLM
> LLM非依存 · 指示追従型LLMであれば利用可能

---

## Overview / 概要

A prompt specification that automatically detects and anonymizes sensitive information whenever a document or text is provided to an LLM.

LLMにドキュメント・テキストを渡す際、機密情報を自動検出・匿名化するプロンプト仕様です。

---

## Use Cases / ユースケース

- Consulting an LLM with internal documents / 社内資料をLLMに相談する
- Sharing meeting notes or reports / 議事録・報告書の共有
- Reviewing contracts or proposals / 契約書・提案書のレビュー
- Pasting logs or configuration files / ログ・設定ファイルの貼り付け

---

## How to Use / 使い方

### Option A — System Prompt / システムプロンプトに設定

Paste the contents of `prompt/anonymization.md` into your LLM's system prompt or custom instructions field.

`prompt/anonymization.md` の内容をシステムプロンプト・カスタム指示欄に貼り付けてください。

### Option B — Prefix Prompt / プレフィックスとして使用

Prepend the prompt before each document you submit.
ドキュメントの前にプロンプトを毎回付加して使用します。

```
[Anonymization rules here]

--- DOCUMENT START ---
(your document)
--- DOCUMENT END ---
```

---

## Core Prompt / コアプロンプト

See [`prompt/anonymization.md`](./prompt/anonymization.md) for the full prompt specification.

コアプロンプトの全文は [`prompt/anonymization.md`](./prompt/anonymization.md) を参照してください。

---

## Example / 使用例

### Input / 入力

```
Meeting notes from Taro Yamada (Sales, ABC Corp) on 2024-03-01.
Budget approved: ¥12,500,000.
Contact: yamada@abc-corp.co.jp
API key for staging: sk-abc123xyz
```

### Output / 出力

```
## Anonymization Report / 匿名化レポート
Detected: 5 item(s) / 検出件数: 5件

| # | Category       | Original               | Replaced with       |
|---|----------------|------------------------|---------------------|
| 1 | Personal name  | Taro Yamada            | Person-A            |
| 2 | Company name   | ABC Corp               | Company-A           |
| 3 | Email          | yamada@abc-corp.co.jp  | user@[DOMAIN]       |
| 4 | Financial      | ¥12,500,000            | ¥XX million         |
| 5 | Credentials    | sk-abc123xyz           | [SECRET]            |

---

Meeting notes from Person-A (Sales, Company-A) on 2024-03-01.
Budget approved: ¥XX million.
Contact: user@[DOMAIN]
API key for staging: [SECRET]
```

---

## Customization / カスタマイズ

You can extend the replacement rules for your organization's needs.
組織のニーズに応じて置換ルールを拡張できます。

```markdown
# Example: Add custom entity type / カスタムエンティティの追加例
| Internal code / 内部コード | PROJ-XXXX → [INTERNAL-CODE] |
| Medical record / 診療記録  | → [MEDICAL-REDACTED]        |
```

---

## Prompt Variants / バリアント

LLM-specific tuned versions are available under `variants/`:
LLM別チューニング版は `variants/` 以下にあります:

| File | Target LLM | Notes |
|------|-----------|-------|
| [`variants/claude.md`](./variants/claude.md) | Claude (Anthropic) | Extended thinking, Artifacts support |
| [`variants/gpt4.md`](./variants/gpt4.md) | GPT-4 / GPT-4o (OpenAI) | Function calling, JSON mode |
| [`variants/gemini.md`](./variants/gemini.md) | Gemini (Google) | Multimodal, long context, JSON mode |

---

## Repository Structure / リポジトリ構造

```
document-anonymization-prompt/
├── README.md
├── LICENSE
├── prompt/
│   └── anonymization.md      # Core prompt specification
└── variants/
    ├── claude.md             # Claude-specific tuning
    ├── gpt4.md               # GPT-4-specific tuning
    └── gemini.md             # Gemini-specific tuning
```

---

## Contributing / コントリビューション

Contributions are welcome! / コントリビューション歓迎です。

- **Bug reports / バグ報告**: Open an issue
- **New entity types / エンティティ追加提案**: Open an issue with examples
- **Translations / 翻訳**: Submit a PR
- **LLM-specific tuning / LLM別チューニング**: Add a file under `variants/`

---

## Changelog / 変更履歴

| Version | Date       | Changes                        |
|---------|------------|--------------------------------|
| 0.1.0   | 2026-04-25 | Initial release / 初版リリース |

---

## License / ライセンス

[MIT License](./LICENSE)
