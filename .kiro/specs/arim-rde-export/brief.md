# Brief: arim-rde-export

## Problem

本システムに正本として残した実験データを、ARIM RDE の登録単位（データ）およびデータセット情報（課題番号など）に合わせて送出できないと、事業・共用の登録義務や再利用に届かない。

## Current State

送出は未着手。正本登録は `experimental-data-registration` に依存。ARIM RDE との名称対応・データセット概念は `CONTEXT.md` に記載済み。送出ロジック・認証・ファイル形式は未定義。

## Desired Outcome

本システム上の実験データから、ARIM RDE へ送出できる。送出時にデータセット情報（課題番号など）を付与できる。正本は本システムのままである。

## Approach

registration 完了後の第 2 スライス。ローカルの実験データを ARIM RDE の「データ」へ写像し、データセット情報は送出時に足す。概念合わせは済み、送り状の事務項目は送出側で扱う。

## Scope

- **In**:
  - 実験データ → ARIM RDE データへの写像
  - 送出時のデータセット情報（課題番号など）
  - データ名など、登録時に持たない項目の送出時生成
  - 送出結果の記録（成功／失敗の扱いの仕様化）
- **Out**:
  - 実験データの新規登録 UI（registration の範囲）
  - OCR / API 入力
  - ARIM RDE 本体の改修

## Boundary Candidates

- 写像・バリデーション（必須欠落の扱い）
- データセット情報の保持場所（送出プロファイル等）
- 外部認証・送出ジョブ

## Out of Boundary

- 本システム内の試料・装置台帳の主管理
- OCR / API

## Upstream / Downstream

- **Upstream**: `experimental-data-registration`
- **Downstream**: なし（OCR / API は入力拡張であり、機能依存は registration。delivery 順上はこの後）

## Existing Spec Touchpoints

- **Extends**: なし
- **Adjacent**: `experimental-data-registration`（実験データ・固有情報・試料の形）

## Constraints

- 正本は本システム。ARIM RDE は出す先
- 用語は `CONTEXT.md`（データセットは ARIM RDE 側のまとめ単位）
- ARIM RDE API／運用制約は design 時に要調査
