# Brief: experimental-data-registration

## Problem

オフラインの中規模半導体製造・検査装置の前に立つ実験者が、製造設定・製造結果および検査設定・検査結果を含む記録を、後から直せる正本として残せない。紙や散在ファイルに依存し、失敗・中断も含めた現場の事実がシステムに残らない。

## Current State

グリーンフィールド。アプリ実装・既存 spec はない。ドメイン用語と境界は `CONTEXT.md`、開発順は `docs/adr/0001-development-order.md` に確定済み。

## Desired Outcome

実験者が Web 画面から手入力で、本システムに実験データを登録・訂正できる。製造装置／検査装置の固有情報（設定・結果に相当）を装置ごとの欄として扱える。試料・グループ・装置台帳・装置担当・訂正履歴が揃い、「本システムに登録できた」が完了条件になる。

## Approach

ADR フェーズ縦割りの第 1 スライス。正本は本システムに閉じ、ARIM RDE 送出・OCR・API は後続 spec に委ねる。技術スタックは本 brief では未固定（design 前に確定）。

## Scope

- **In**:
  - 実験者のグループ所属
  - 試料の作成・編集（作成者およびその試料を使った実験者、訂正履歴）
  - 装置台帳（製造装置／検査装置、主用途で 1 種別、台帳はシステムに 1 つ）
  - 固有情報欄の定義（装置担当ロール。全装置の欄を定義可。装置に担当を紐づけない）
  - 実験データの登録・訂正・履歴・添付（必須: 実験者・装置・試料・実施日時。任意: 固有情報・添付）
  - Web 手入力 UI
- **Out**:
  - ARIM RDE 送出、OCR、API
  - 予約・見える化・映像化・解析
  - 工程エンティティ、施設概念

## Boundary Candidates

- 実験データ（登録単位・訂正・添付）
- 試料・グループ
- 装置台帳と固有情報スキーマ
- 実験者／装置担当ロールと権限

## Out of Boundary

- ARIM RDE のデータセット・課題番号・データ名の送出マッピング
- OCR／カメラ入力、外部 API 受付
- 装置予約・稼働見える化

## Upstream / Downstream

- **Upstream**: `CONTEXT.md`、`docs/adr/0001-development-order.md`、要望メモ `docs/requiests.md`
- **Downstream**: `arim-rde-export`、`ocr-registration`、`api-registration`

## Existing Spec Touchpoints

- **Extends**: なし
- **Adjacent**: 後続 3 spec（実験データの形と固有情報スキーマを共有）

## Constraints

- 用語は `CONTEXT.md` に従う
- 実験データは消せない。グループは試料から決まり、実験データ自身はグループを持たない
- 複数試料は同一グループ必須
- 固有情報・添付は空でも登録可
- 技術スタックは design 前に確定すること
