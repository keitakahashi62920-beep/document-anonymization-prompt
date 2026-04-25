# Anonymization Variant — GPT-4 / 匿名化バリアント — GPT-4

> Tuned for GPT-4 / GPT-4o (OpenAI). Extends the base prompt in `prompt/anonymization.md` with GPT-4-specific optimizations.
> GPT-4 / GPT-4o（OpenAI）に特化したチューニング版。`prompt/anonymization.md` のベースプロンプトを拡張します。

---

## Base Prompt

Include the full contents of `../prompt/anonymization.md` as your system prompt, then append the additions below.
システムプロンプトに `../prompt/anonymization.md` の内容を含めた上で、以下を追記してください。

---

## GPT-4-Specific Additions / GPT-4固有の追加指示

### Function Calling Integration / ファンクション呼び出し統合

If using the OpenAI API with function calling, you can define an `anonymize_document` function to structure the output:
OpenAI APIのファンクション呼び出しを使う場合、`anonymize_document` 関数を定義して出力を構造化できます:

```json
{
  "name": "anonymize_document",
  "description": "Anonymize sensitive entities in the provided document",
  "parameters": {
    "type": "object",
    "properties": {
      "detected_count": { "type": "integer" },
      "entity_map": {
        "type": "array",
        "items": {
          "type": "object",
          "properties": {
            "category": { "type": "string" },
            "original": { "type": "string" },
            "replacement": { "type": "string" }
          }
        }
      },
      "anonymized_text": { "type": "string" }
    },
    "required": ["detected_count", "entity_map", "anonymized_text"]
  }
}
```

### System Message Format / システムメッセージフォーマット

For GPT-4 API usage, place the anonymization rules in the `system` message role:
GPT-4 APIを使う場合、匿名化ルールを `system` メッセージロールに配置すること:

```json
{
  "messages": [
    {
      "role": "system",
      "content": "[Contents of prompt/anonymization.md]\n\n[GPT-4-specific additions]"
    },
    {
      "role": "user",
      "content": "[Document to analyze]"
    }
  ]
}
```

### Temperature Setting / 温度設定

Set `temperature: 0` or `temperature: 0.1` for deterministic anonymization output.
決定論的な匿名化出力のために `temperature: 0` または `temperature: 0.1` を設定すること。

### GPT-4o Vision Support / GPT-4o ビジョン対応

When using GPT-4o with image input, add this instruction:
GPT-4oで画像入力を使う場合、以下の指示を追加すること:

```
If the input contains an image, first extract all visible text (OCR), then apply the full anonymization pipeline to the extracted text before any analysis.
入力に画像が含まれる場合、まず全ての可視テキストを抽出（OCR）し、その後匿名化パイプライン全体を適用してから分析を行うこと。
```

### Token Efficiency / トークン効率

For long documents, use GPT-4's 128k context window efficiently by processing the anonymization report inline rather than as a separate response.
長いドキュメントの場合、匿名化レポートを別レスポンスではなくインラインで処理することでGPT-4の128kコンテキストウィンドウを効率的に使用すること。

---

## Example API Call / APIコール例

```python
from openai import OpenAI

client = OpenAI()
response = client.chat.completions.create(
    model="gpt-4o",
    temperature=0,
    messages=[
        {"role": "system", "content": anonymization_prompt},
        {"role": "user", "content": document_text}
    ]
)
```
