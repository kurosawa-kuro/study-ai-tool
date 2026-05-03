# DeepSeek

作成日: `2026-05-03`

## 1. 概要

DeepSeek は中国の AI 企業 DeepSeek が開発・公開した LLM です。
完成型の AI コーディングツールではなく、**低コストな API 基盤・モデル** として評価します。

このスタックでは **低コスト API 基盤・社内 AI 組み込み用途** として位置づけます。

---

## 2. 主な特徴

| 特徴 | 内容 |
| --- | --- |
| OpenAI 互換 API | OpenAI クライアントライブラリをそのまま使える |
| 低コスト | GPT-4 比で大幅に安い API 料金 |
| オープンウェイト | Hugging Face で公開されており、セルフホスト可能 |
| コード生成 | DeepSeek-Coder はコード特化モデル |
| 推論モデル | DeepSeek-R1 は複雑な推論・数学・コードに強い |

### 主要モデル

| モデル | 用途 |
| --- | --- |
| `deepseek-chat` | 汎用テキスト生成・対話 |
| `deepseek-reasoner` | 複雑な推論・数学・コード（DeepSeek-R1） |
| `deepseek-coder` | コード生成・補完（セルフホスト用） |

### 得意な用途（このスタック）

- 社内 CLI エージェント
- SQL 補助ツール
- `Meilisearch / RAG` 向け補助
- batch コード生成補助
- コスト重視の AI 機能組み込み
- 独自 UI や社内ツールへの組み込み

---

## 3. 環境構築

### API キー取得

1. [platform.deepseek.com](https://platform.deepseek.com) にアクセス
2. アカウント作成・ログイン
3. `API Keys` からキーを発行

### 環境変数設定

```bash
export DEEPSEEK_API_KEY=sk-...
```

### Python クライアントインストール

```bash
pip install openai  # OpenAI 互換のため openai パッケージを使う
```

---

## 4. 起動・使用方法

### Python から使う（OpenAI 互換）

```python
from openai import OpenAI

client = OpenAI(
    api_key="sk-...",  # DeepSeek API キー
    base_url="https://api.deepseek.com"
)

response = client.chat.completions.create(
    model="deepseek-chat",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "PostgreSQL のインデックス設計を教えてください"}
    ]
)

print(response.choices[0].message.content)
```

### 推論モデル（DeepSeek-R1）

```python
response = client.chat.completions.create(
    model="deepseek-reasoner",
    messages=[
        {"role": "user", "content": "このSQLクエリを最適化してください: ..."}
    ]
)

# 思考プロセスを確認
print(response.choices[0].message.reasoning_content)
# 最終回答
print(response.choices[0].message.content)
```

### curl で確認

```bash
curl https://api.deepseek.com/chat/completions   -H "Content-Type: application/json"   -H "Authorization: Bearer $DEEPSEEK_API_KEY"   -d '{
    "model": "deepseek-chat",
    "messages": [{"role": "user", "content": "Hello"}]
  }'
```

---

## 5. セルフホスト（Ollama 経由）

ローカル環境で動かす場合は Ollama を使います。

```bash
# Ollama インストール
curl -fsSL https://ollama.com/install.sh | sh

# DeepSeek モデルをプル
ollama pull deepseek-coder:6.7b

# 起動（デフォルトで localhost:11434）
ollama serve

# 動作確認
ollama run deepseek-coder:6.7b "Pythonでfizzbuzzを書いてください"
```

### Ollama + OpenAI 互換クライアント

```python
client = OpenAI(
    api_key="ollama",  # ダミーキーでOK
    base_url="http://localhost:11434/v1"
)

response = client.chat.completions.create(
    model="deepseek-coder:6.7b",
    messages=[{"role": "user", "content": "Hello"}]
)
```

---

## 6. 社内ツール組み込みの例

### SQL 補助 CLI

```python
import sys
from openai import OpenAI

client = OpenAI(api_key="sk-...", base_url="https://api.deepseek.com")

def sql_helper(question: str) -> str:
    response = client.chat.completions.create(
        model="deepseek-chat",
        messages=[
            {"role": "system", "content": "PostgreSQL / BigQuery / DuckDB の SQL エキスパートです。"},
            {"role": "user", "content": question}
        ]
    )
    return response.choices[0].message.content

if __name__ == "__main__":
    print(sql_helper(" ".join(sys.argv[1:])))
```

### RAG 補助（埋め込みは別途用意）

```python
# DeepSeek は埋め込みをサポートしていないため、埋め込みは別モデルを使う
# 回答生成のみ DeepSeek を使う構成が現実的

def rag_answer(context: str, question: str) -> str:
    response = client.chat.completions.create(
        model="deepseek-chat",
        messages=[
            {"role": "system", "content": f"以下のコンテキストをもとに回答してください:\n\n{context}"},
            {"role": "user", "content": question}
        ]
    )
    return response.choices[0].message.content
```

---

## 7. 注意事項・制限

| 項目 | 内容 |
| --- | --- |
| IDE 体験なし | IDE 補完・PRレビュー・実行ループは自前実装が必要 |
| セキュリティ自前設計 | 監査ログ・権限制御・入力バリデーションは自前で設計する |
| データ残留リスク | API 経由で送信したデータのプライバシーポリシーを確認する |
| 埋め込み非対応 | Embedding API は提供されていない（2025年時点） |
| レートリミット | プランによってリクエスト数に制限がある |

---

## 8. 運用のポイント

| ポイント | 内容 |
| --- | --- |
| 機密データを送らない | API 経由の場合、機密情報・個人情報を含めないよう設計する |
| セルフホストで完全分離 | 機密性が高い場合は Ollama + ローカルモデルで閉じた環境を作る |
| コスト試算をしてから採用 | API コストを事前に見積もる |
| OpenAI 互換を活かす | 既存の OpenAI ベースコードをほぼそのまま使い回せる |

---

## 9. 他ツールとの使い分け

| 業務 | DeepSeek | 他ツール |
| --- | --- | --- |
| 低コスト API 基盤 | **第一候補** | OpenAI API（高コスト） |
| 社内ツール組み込み | **第一候補** | Claude API / OpenAI API |
| IDE 体験 | 非対応 | **Cursor / GitHub Copilot が得意** |
| PR レビュー | 非対応 | **GitHub Copilot が得意** |
| エージェント実行 | 非対応（自前実装） | **Claude Code / Codex が得意** |
