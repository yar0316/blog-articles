---
title: "AWSのcost-analysis-mcpがいつの間にかaws-pricing-mcpになっていた"
emoji: "💰"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["aws", "mcp", "claude", "pricing"]
published: true
published_at: 2025-07-16 18:00
---

# AWSのcost-analysis-mcpがいつの間にかaws-pricing-mcpになっていた

## はじめに

AWSのMCPサーバーを利用することが多くてよくGithubのリポジトリを確認しに行くのですが、ある日気づいたらよく利用していたCost Analysis MCPがなくなっていました。どうやら[2025年7月10日頃にAWS Pricing MCPという名前で置き換えられた](https://github.com/awslabs/mcp/compare/2025.7.2025092020...2025.7.2025092044)ようです

## 何が変わったのか

### 主な変更点

1. **パッケージ名の変更**
   - `cost-analysis-mcp` → `aws-pricing-mcp`

2. **ファイル構造の整理**
   - ファイルの移動と再配置
   - プロジェクト構造の整理

3. **関数名の統一**
   - `get_pricing_from_api` → `get_pricing`
   - より分かりやすい命名に変更

4. **バージョン管理**
   - バージョン1.0.0に設定
   - コードオーナー情報の追加

## この変更の意味

### より直感的な名前

構成前の料金計算に利用する`cost-analysis-mcp-server`と実際に稼働している環境の費用計算に利用する`cost-explorer-mcp-server`があって、名前からはわかりにくいなあという感想を持っていました。より直感的に区別しやすい名前になったかなと思います。

## 利用者(というより私)への影響

### 設定の変更が必要

既存のMCP設定で`cost-analysis-mcp`を使用している場合は、`aws-pricing-mcp`に変更する必要があります。

```json
{
  "mcpServers": {
    "awslabs.aws-pricing-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.aws-pricing-mcp-server@latest"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR",
        "AWS_PROFILE": "your-aws-profile",
        "AWS_REGION": "us-east-1"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

Windowsの場合は

```json
{
  "mcpServers": {
    "awslabs.aws-pricing-mcp-server": {
      "command": "uvx",
      "args": [
         "--from",
         "awslabs.aws-pricing-mcp-server",
         "awslabs.aws-pricing-mcp-server.exe"
         ],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR",
        "AWS_PROFILE": "your-aws-profile",
        "AWS_REGION": "us-east-1"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

### 関数呼び出しの変更

もし直接関数を呼び出している場合は、新しい関数名に変更が必要です。
私の場合は、作成中のツールがちょうどこれに該当してしまいました。
AWSの構成検討と費用試算を手伝ってくれるAIエージェントを作成しているのですが、このエージェントのステップの中に決め打ちでCost Analysis MCPを呼び出す箇所があり、いくつか修正の必要が生じました。
MCPの選択や関数呼び出しはちゃんと動的に行えるように実装しようと方針を変えるいい機会になりました。

## まとめ

今回の変更は、機能追加というよりは既存機能の整理と命名の改善が主な目的のようです。より直感的な名前と整理された構造により、AWS料金情報の取得がより使いやすくなりました。

既存ユーザーは設定の変更が必要ですが、機能自体に大きな変更はないため、私のように変な使い方をしていなければ移行は比較的簡単だと思われます。

## 参考リンク

- [AWS MCP 変更差分](https://github.com/awslabs/mcp/compare/2025.7.2025092020...2025.7.2025092044)
- [AWS MCP Servers](https://github.com/awslabs/mcp) - プロジェクトページ

※この記事はClaude Codeに手伝ってもらって作成しました。
