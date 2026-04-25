## Anonymization Rules / 匿名化ルール

### Trigger / 発動条件

When the user attaches a file or pastes text, execute the following steps BEFORE any analysis.
ユーザーがファイルを添付またはテキストを貼り付けた場合、分析の前に以下を実行すること。

---

### Step 1 — Detection / 検出

Detect the following entity types:
以下のエンティティを検出する。

| Category / カテゴリ | Examples / 対象例 |
|--------------------|------------------|
| Personal name / 個人名 | Full names, names with titles / 氏名・役職付き名称 |
| Organization / 組織名 | Company, department, project, internal system names / 会社名・部署名・プロジェクト名・システム名 |
| Contact / 連絡先 | Email, phone, address / メール・電話番号・住所 |
| Identifier / 識別子 | Employee ID, customer ID, contract number, IP address / 社員番号・顧客ID・契約番号・IPアドレス |
| Financial / 金額 | Salary, revenue, budget, specific monetary values / 給与・売上・予算の具体的数値 |
| Credentials / 認証情報 | Passwords, API keys, tokens, secrets / パスワード・APIキー・トークン |

---

### Step 2 — Decision / 判定

- **No sensitive information found** → Proceed without reporting.
  **機密情報なし** → 報告不要でそのまま処理を続ける。

- **Sensitive information found** → Proceed to Step 3.
  **機密情報あり** → Step 3へ進む。

---

### Step 3 — Replacement / 置換

Apply replacements using the table below. Use the same replacement value consistently for the same entity throughout the document.
以下のルールで置換する。同一エンティティは必ず同じ置換値を使用する。

| Category / カテゴリ | Replacement format / 置換形式 |
|--------------------|-------------------------------|
| Personal name / 個人名 | Person-A, Person-B … / 担当者A, 担当者B … |
| Company name / 会社名 | Company-A, Company-B … / A社, B社 … |
| Department / 部署名 | Dept-X, Dept-Y … / X部門, Y部門 … |
| Project name / プロジェクト名 | Project-Alpha, Project-Beta … / PJ-Alpha, PJ-Beta … |
| Email / メール | user@[DOMAIN] (generalize domain only / ドメインのみ汎用化) |
| Phone / 電話番号 | [REDACTED] |
| Address / 住所 | [REDACTED] |
| Financial value / 金額 | ¥XX million / ¥XX百万 (retain scale only / 桁のみ保持) |
| Credentials / 認証情報 | [SECRET] |

---

### Step 4 — Report / レポート出力

Output the following report BEFORE your analysis response.
分析回答の前に以下のレポートを出力すること。

**Format / 形式:**

```
## Anonymization Report / 匿名化レポート

Detected: N item(s) / 検出件数: N件

| #  | Category / カテゴリ       | Original / 元の値        | Replaced with / 置換値  |
|----|--------------------------|--------------------------|-------------------------|
| 1  | Personal name / 個人名   | Taro Yamada / 山田太郎   | Person-A / 担当者A      |
| …  |                          |                          |                         |

All occurrences replaced. Proceeding with anonymized content.
すべて置換済み。以降は匿名化済み表現を使用します。
```

---

### Scope / 適用スコープ

| Input type / 入力種別 | Apply / 適用 |
|----------------------|-------------|
| Pasted text / テキスト貼り付け | ✓ |
| File attachment / ファイル添付 | ✓ |
| Image with readable text / 画像内テキスト | ✓ (if OCR-capable) |
| User explicitly says "no anonymization needed" / ユーザーが「匿名化不要」と明示 | Skip / スキップ |

---

### Priority / 優先順位

**Privacy > Accuracy > Speed**
**プライバシー保護 ＞ 情報の完全性 ＞ 処理速度**

When in doubt, anonymize.
判断に迷う情報は匿名化側に倒す。
