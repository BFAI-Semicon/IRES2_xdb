# CLAUDE.md

本ファイルは参照の入口であり、規約の正本ではない。正本は `AGENTS.md` と
`.cursor/rules/` に置く。規約を変えるときは正本を直し、本ファイルには写さない。

## なぜ本ファイルがあるか

Cursor は `AGENTS.md` と `.cursor/rules/` を自動で読むが、Claude Code は
`CLAUDE.md` を読む。同じ規約を二重に書かずに済ませるため、本ファイルから
正本を取り込む。

## 正本

### 開発ワークフロー

@AGENTS.md

### 文書の書き方

@.cursor/rules/doc-writing.mdc

### Markdown の lint

@.cursor/rules/markdown-lint.mdc

## 取り込みが効かない場合

上記の `@` で始まる行はファイルの取り込み指定である。内容が読めていないときは、
作業を始める前に `AGENTS.md` と `.cursor/rules/` の各ファイルを直接読む。

## 本プロジェクトの文脈

- ドメインの用語集: `CONTEXT.md`
- 開発順の決定: `docs/adr/0001-development-order.md`
- 全体計画: `.kiro/steering/roadmap.md`
- 仕様: `.kiro/specs/{feature}/`
- 現場と照合するユースケース: `docs/use-cases.md`

コードはまだない。現時点の成果物は仕様と文書である。`docs/` の変更は
GitHub Pages へ公開されるため、`docs/*.html` は対応する `.md` と内容を合わせる。
