# Brief: api-registration

## Problem

手入力や OCR だけでは、他システムやオンラインに繋がる装置からの自動投入に対応できない。プログラムから本システムの正本へ実験データを登録する経路が必要になる。

## Current State

未着手。ADR 上、OCR との相互順は未定。(1)(2) の後に置く。

## Desired Outcome

API 経由で実験データを本システムに登録できる。権限・バリデーションは手入力登録と同じドメイン規則（試料グループ、消せない、訂正者など）に従う。

## Approach

registration のドメインを API 面で公開する。送出（export）とは独立した入力経路。認証方式・バージョニングは design で確定。

## Scope

- **In**:
  - 実験データ登録（および必要なら試料等）の API
  - 認証・認可（実験者／装置担当の規則との整合）
  - エラー時のドメイン規則の表現
- **Out**:
  - Web 手入力 UI
  - OCR
  - ARIM RDE 送出

## Boundary Candidates

- API 契約（リソース境界）
- 認証
- registration ドメインサービスへの接続

## Out of Boundary

- OCR
- ARIM RDE 送出の実行
- 装置予約などスコープ外機能

## Upstream / Downstream

- **Upstream**: `experimental-data-registration`、delivery 順上 `arim-rde-export`
- **Downstream**: なし（`ocr-registration` とは相互非依存）

## Existing Spec Touchpoints

- **Extends**: なし
- **Adjacent**: `experimental-data-registration`、`ocr-registration`

## Constraints

- ドメイン規則は `CONTEXT.md` と registration に従う
- OCR との実装順序は未定（並列可）
- 技術スタックは registration と共有する前提（design 前に確定）
