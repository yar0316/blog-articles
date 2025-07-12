---
title: "Claude CodeがWindowsでも使えるようになったので試した"
emoji: "🪟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["claude", "ai", "windows", "cli"]
published: true
---

# Claude CodeがWindowsでも使えるようになったので試した

## はじめに

Claude CodeがWindowsでも使えるようになった(正確にはWSLを介さなくても使えるようになった)ので、試してみました。いちいちWSLに切り替えるの面倒だったので助かります。

:::message
記事内のユーザー名は別の文字列に置き換えています。実際の環境に合わせて読み替えてください。
:::

## インストールと設定

WSLで使う場合と同じコマンドでインストールできます。

```bash
claude --version
```

早速使ってみようとしたのですが、ちょっと躓きました。
結果としては、`CLAUDE_CODE_GIT_BASH_PATH`という環境変数を適切に設定することで解決しました。

## 躓いたポイント

とりあえずこの記事用のファイルをClaude Codeに作ってもらおうと作業用のディレクトリに移動し、`claude`コマンドを入れたのですが、以下のようなエラーが出ました。

```bash
claude
Claude Code on Windows requires git-bash (https://git-scm.com/downloads/win). If installed but not in PATH, set environment variable pointing to your bash.exe, similar to: CLAUDE_CODE_GIT_BASH_PATH=C:\Program Files\Git\bin\bash.exe
```

大してメッセージを読まず、「ああgit bash必要なのかあ」とgit bashで実行してみたら、次はこんなエラーが。

```bash
$ claude
(前略)
Error: No suitable shell found. Claude CLI requires a Posix shell environment. Please ensure you have a valid shell installed and the SHELL environment variable set.
(後略)
```

scoopでgitを入れているからか、環境変数`SHELL`の値が適切に設定されていないようです。
調べたところ、git bash以外では影響がなさそうだったので、パスをscoopのものに変更することにしました。

```powershell
[Environment]::SetEnvironmentVariable("SHELL", "C:\Users\myuser\scoop\apps\git\current\bin\bash.exe", "User")
```

しかしこれでも同じエラーが。
もう一度最初に出たエラーを確認してみることにしました。

```bash
Claude Code on Windows requires git-bash (https://git-scm.com/downloads/win). If installed but not in PATH, set environment variable pointing to your bash.exe, similar to: CLAUDE_CODE_GIT_BASH_PATH=C:\Program Files\Git\bin\bash.exe
```

要は、`CLAUDE_CODE_GIT_BASH_PATH`にちゃんとしたgit bashのパスを設定してあげればよさそうです。最初は後半を読んでいなくて気づきませんでした。ちゃんとメッセージ読むの大事。

最終的に、ユーザー環境変数に`CLAUDE_CODE_GIT_BASH_PATH`を登録して事なきを得ました。

```bash
claude
(node:7200) [DEP0190] DeprecationWarning: Passing args to a child process with shell option true can lead to security vulnerabilities, as the arguments are not escaped, only concatenated.
(Use `node --trace-deprecation ...` to show where the warning was created)
╭───────────────────────────────────────────────────╮
│ ✻ Welcome to Claude Code!                         │
│                                                   │
│   /help for help, /status for your current setup  │
│                                                   │
│   cwd: C:\Users\myuser\workspace\blog-articles    │
╰───────────────────────────────────────────────────╯

 ※ Tip: Send messages to Claude while it works to steer Claude in real-time

╭──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
│ > Try "refactor <filepath>"                                                                                          │
╰──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
  ? for shortcuts
```

WSLでは出ていなかったDeprecationの警告がでていますが、一旦は使えそうです。
使用感がどの程度変わるのか不明ですが、とりあえずPlaywright MCPの利用などブラウザを開く必要があるときに失敗しなくなったのかな？と思います。今後試してみます。

## まとめ

Claude Code 1.0.51でネイティブWindows対応が追加され、WSLを使わなくてもWindows上で直接実行できるようになりました。設定で躓くポイントもありましたが、`CLAUDE_CODE_GIT_BASH_PATH`環境変数を適切に設定することで解決できました。

## 参考リンク

- [Claude Code Changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md) - 1.0.51のリリースノート
