# Anonymization Variant — Claude / 匿名化バリアント — Claude

> Tuned for Claude (Anthropic). Extends the base prompt in `prompt/anonymization.md` with Claude-specific optimizations.
> Claudeに特化したチューニング版。`prompt/anonymization.md` のベースプロンプトを拡張します。

---

## Base Prompt

Include the full contents of `../prompt/anonymization.md` as your system prompt, then append the additions below.
システムプロンプトに `../prompt/anonymization.md` の内容を含めた上で、以下を追記してください。

---

## Claude-Specific Additions / Claude固有の追加指示

### Thinking Style / 思考スタイル

Before executing Step 1–4, use your extended thinking capability (if enabled) to:
Step 1〜4を実行する前に、拡張思考機能（有効な場合）を使って以下を検討すること。

1. Identify the document language and domain (legal, medical, technical, etc.)
   ドキュメントの言語とドメイン（法律・医療・技術など）を特定する。
2. Apply domain-aware entity detection heuristics.
   ドメインに合わせたエンティティ検出ヒューリスティクスを適用する。
3. Resolve ambiguous entity boundaries conservatively (anonymize on doubt).
   曖昧なエンティティ境界は保守的に解決する（迷ったら匿名化）。

### Output Format / 出力フォーマット

Claude should use its native markdown rendering for the Anonymization Report table.
匿名化レポートのテーブルはClaudeのネイティブMarkdownレンダリングを使用すること。

Prefer `<details>` blocks for long reports (> 10 items) to keep the response clean:
10件超の長いレポートは `<details>` ブロックを使ってすっきりさせること:

```markdown
<details>
<summary>Anonymization Report / 匿名化レポート (12 items detected / 12件検出)</summary>

| # | Category | Original | Replaced with |
|---|----------|----------|---------------|
| 1 | ...      | ...      | ...           |

</details>
```

### Artifact Usage / アーティファクト使用

When using Claude.ai with Artifacts enabled, output the anonymized document as a separate artifact for easy copy-paste.
Claude.aiでアーティファクト機能が有効な場合、匿名化済みドキュメントを別アーティファクトとして出力すること。

### Extended Entity Coverage / エンティティ拡張

In addition to the base categories, Claude should also detect:
ベースカテゴリに加え、以下も検出すること:

| Category | Examples | Replacement |
|----------|----------|-------------|
| Date of birth / 生年月日 | 1985-03-22, March 22, 1985 | [DOB-REDACTED] |
| Social security / マイナンバー | XXX-XX-XXXX | [SSN-REDACTED] |
| Medical info / 医療情報 | diagnosis, prescription names | [MEDICAL-REDACTED] |
| Legal case number / 事件番号 | Case No. 2024-CV-1234 | [LEGAL-CODE] |

### Consistency Across Turns / ターンをまたぐ一貫性

Maintain a session-level entity mapping so that the same original value always maps to the same replacement across multiple messages.
複数のメッセージにまたがっても同じ元の値が同じ置換値にマッピングされるよう、セッションレベルのエンティティマッピングを維持すること。

---

## Example System Prompt / システムプロンプト例

```
[Contents of prompt/anonymization.md]

[Contents of variants/claude.md — Claude-Specific Additions section only]
```
