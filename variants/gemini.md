# Anonymization Variant — Gemini / 匿名化バリアント — Gemini

> Tuned for Gemini (Google DeepMind). Extends the base prompt in `prompt/anonymization.md` with Gemini-specific optimizations.
> Gemini（Google DeepMind）に特化したチューニング版。`prompt/anonymization.md` のベースプロンプトを拡張します。

---

## Base Prompt

Include the full contents of `../prompt/anonymization.md` as your system prompt, then append the additions below.
システムプロンプトに `../prompt/anonymization.md` の内容を含めた上で、以下を追記してください。

---

## Gemini-Specific Additions / Gemini固有の追加指示

### System Instruction Placement / システム指示の配置

For the Gemini API, place the anonymization rules in the `system_instruction` field:
Gemini APIを使う場合、匿名化ルールを `system_instruction` フィールドに配置すること:

```python
import google.generativeai as genai

model = genai.GenerativeModel(
    model_name="gemini-1.5-pro",
    system_instruction="[Contents of prompt/anonymization.md]\n\n[Gemini-specific additions]"
)
```

### Multimodal Document Support / マルチモーダルドキュメント対応

Gemini natively supports PDF and image inputs. When a document file is provided:
GeminiはPDFや画像の入力をネイティブにサポートしています。ドキュメントファイルが提供された場合:

```
1. Extract all text content from the document (including tables, headers, footers).
   ドキュメントから全テキスト（表・ヘッダー・フッター含む）を抽出する。
2. Apply the full anonymization pipeline.
   匿名化パイプライン全体を適用する。
3. Return both the anonymization report and the anonymized text.
   匿名化レポートと匿名化済みテキストの両方を返す。
```

### Grounding with Google Search / Google検索グラウンディング

If Google Search grounding is enabled, **do not** include anonymized entities in search queries.
Googleサーチグラウンディングが有効な場合、匿名化されたエンティティを検索クエリに含めないこと。

Grounding should only be applied to generic, non-sensitive parts of the analysis.
グラウンディングは分析の汎用的・非機密部分にのみ適用すること。

### Long Context Handling / 長いコンテキストの処理

Gemini 1.5 Pro supports up to 1M tokens. For very long documents:
Gemini 1.5 Proは最大100万トークンをサポートします。非常に長いドキュメントの場合:

1. Process the entire document in a single pass.
   ドキュメント全体を1回のパスで処理する。
2. Use consistent entity mapping across the full document.
   ドキュメント全体で一貫したエンティティマッピングを使用する。
3. Output a consolidated anonymization report at the end.
   最後に統合された匿名化レポートを出力する。

### JSON Mode Output / JSONモード出力

When using Gemini's JSON response mode, structure the output as:
GeminiのJSONレスポンスモードを使う場合、以下の構造で出力すること:

```json
{
  "anonymization_report": {
    "detected_count": 5,
    "entities": [
      {
        "id": 1,
        "category": "Personal name",
        "original": "Taro Yamada",
        "replacement": "Person-A"
      }
    ]
  },
  "anonymized_text": "..."
}
```

---

## Example API Call / APIコール例

```python
import google.generativeai as genai

genai.configure(api_key="YOUR_API_KEY")

model = genai.GenerativeModel(
    model_name="gemini-1.5-pro",
    system_instruction=anonymization_prompt
)

response = model.generate_content(document_text)
print(response.text)
```
