# Roadmap

## Overview

半導体製造装置の製造設定・製造結果と、検査装置の検査設定・検査結果を、オフライン中規模装置の現場で本システムに正本として残す実験データ管理システムを構築する。用語と境界は `CONTEXT.md` に従い、開発順は `docs/adr/0001-development-order.md` に従う。

最初の価値は Web 手入力による実験データ登録（グループ・試料・装置台帳・固有情報欄・装置担当・訂正履歴を含む）。その後に ARIM RDE 送出、続けて OCR / API による入力拡張を行う。OCR と API の相互順序は未定とし、どちらも registration と export の後・相互非依存とする。

## Approach Decision

- **Chosen**: ADR フェーズ縦割り（Approach A）— 仕様を delivery 境界で 4 spec に分ける
- **Why**: 正本登録と送出・入力拡張を混ぜない。1 の完了条件が明確で、takt-sdd の batch と相性が良い
- **Rejected alternatives**:
  - 全体 1 spec（境界が溶け、タスクが肥大化）
  - レイヤ横割り（認証／モデル／UI）— 「登録できる」までの価値が遅い

## Scope

- **In**:
  - 実験データの本システムへの登録（製造装置・検査装置の固有情報を含む）
  - グループ・試料・装置台帳・固有情報欄定義・装置担当・訂正履歴
  - ARIM RDE への送出
  - OCR / API による登録経路
- **Out**:
  - 予約、設備見える化、映像化、画像解析
  - 施設概念（単独運用）
  - 工程エンティティ（当面なし）

## Constraints

- 用語は `CONTEXT.md`。ARIM RDE と同一項目は同じ名称。登録単位だけは「実験データ」
- 正本は本システム。ARIM RDE は後から出す先
- アプリの技術スタックは未選定。requirements までは defer 可。design または `/kiro-spec-batch` 前に確定すること
- Node LTS / Python 3.12（mise）はツールチェーン用。アプリ枠組みの決定ではない

## Boundary Strategy

- **Why this split**: 正本への登録、外部送出、入力経路拡張は独立にレビュー・実装できる。ADR の順序とも一致する
- **Shared seams to watch**:
  - 実験データの形（registration → export / OCR / API）
  - 固有情報欄のスキーマ（装置担当定義 → 登録・送出）
  - 試料・グループの編集権限（作成者／使用者）

## Specs (dependency order)

- [ ] experimental-data-registration -- Web 手入力で実験データを本システムに登録する（台帳・試料・固有情報・役割・履歴を含む）。Dependencies: none
- [ ] arim-rde-export -- 本システムの実験データを ARIM RDE へ送出する。Dependencies: experimental-data-registration
- [ ] ocr-registration -- OCR で実験データを登録する。Dependencies: experimental-data-registration, arim-rde-export
- [ ] api-registration -- API で実験データを登録する。Dependencies: experimental-data-registration, arim-rde-export
