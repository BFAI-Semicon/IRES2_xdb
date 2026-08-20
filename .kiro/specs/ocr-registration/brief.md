# Brief: ocr-registration

## Problem

表示パネルや紙ノートからの転記漏れ・打ち間違いにより、実験データが本システムに残らない。手入力だけでは現場の入れ忘れを抑えきれない。

## Current State

未着手。入力経路は registration の Web 手入力のみ。OCR は ADR 上、(1)(2) の後・API との相互順は未定。

## Desired Outcome

OCR（パネル撮影・紙ノート撮影など）を起点に、本システムの実験データとして登録できる。正本は本システム。読み取り結果は実験者が確認・訂正したうえで登録できること（詳細は requirements で確定）。

## Approach

registration / ARIM RDE 送出の後続。既存の実験データモデルと固有情報欄に値を流し込む入力アダプタとして扱う。装置自動判別の要否は requirements で確定。

## Scope

- **In**:
  - 画像入力から固有情報等への読み取り
  - 登録前の確認・訂正フロー
  - 既存の実験データ登録境界への接続
- **Out**:
  - 装置台帳・固有情報スキーマの主定義（registration）
  - ARIM RDE 送出本体（export）
  - API 登録（別 spec）

## Boundary Candidates

- 画像取得・OCR エンジン
- 読み取り結果 → 実験データへの写像
- 人手確認 UI

## Out of Boundary

- 手入力登録の主画面
- ARIM RDE 送出
- API

## Upstream / Downstream

- **Upstream**: `experimental-data-registration`、delivery 順上 `arim-rde-export`
- **Downstream**: なし（`api-registration` とは相互非依存）

## Existing Spec Touchpoints

- **Extends**: なし
- **Adjacent**: `experimental-data-registration`、`api-registration`

## Constraints

- 正本は本システム。OCR は登録経路の一つ
- API との実装順序は未定（並列可）
- OCR エンジン・精度要件は design 時に選定
