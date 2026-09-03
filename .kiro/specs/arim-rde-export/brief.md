# Brief: arim-rde-export

## Problem

本システムに正本として残した実験データを、ARIM RDE の登録単位（データ）および課題・データセットに合わせて送出できないと、事業・共用の登録義務や再利用に届かない。

## Current State

送出は未着手。正本登録は `experimental-data-registration` に依存し、送出に必要な属性（DICE ID、データ所有者、試料管理者、データファイルの区分、装置のデータファイル拡張子）の器はそちらで用意する。ARIM RDE の認証・API・送出手段は `docs/research-arim-rde-export.md` で調査済み。外部向け登録 API はなく、ExcelInvoice による半自動送出（案 A）を採る。

## Desired Outcome

本システム上の実験データから、ExcelInvoice 形式の送出パッケージ（xlsx + zip）を生成し、ユーザーが RDE の画面で登録できる。課題・データセットはグループの下で管理し、送出結果は送出記録として残る。正本は本システムのままである。

## Approach

registration 完了後の第 2 スライス。ローカルの実験データ 1 件を ARIM RDE のデータ 1 件へ写像する（試料は 1 つ）。課題・データセットはグループに属し、送出時に「試料のグループが持つ課題」から選ぶ。DICE ID は実験データではなくユーザーから引く。役割は本システムのグループ担当・ユーザーのままとし、送出時に研究チーム管理者（OWNER）・研究チームメンバ（MEMBER）へ写す。

## Scope

- **In**:
  - 課題（課題番号・期間）とデータセット（装置ごと）のグループ配下での管理。登録はグループ担当
  - 実験データ → ARIM RDE データへの写像。データ所有者・試料管理者・送出者の DICE ID をユーザーから解決
  - データ名の生成（装置・試料名・実施日時）、実験 ID への本システム識別子の書き込み
  - 装置台帳の装置に RDE 側の装置識別子・データセットテンプレートを持たせる
  - 固有情報の欄と `invoice.schema.json` の対応づけ（schema の取り込みを含む）
  - 送出前検証（DICE ID 未登録、データファイル欠落、テンプレート必須項目の欠落）と対象ユーザーの提示
  - ExcelInvoice パッケージの生成
  - 送出記録（実験データ × データセット、送出者、日時、RDE 側のデータ ID と結果）。結果はユーザーが RDE 画面で確認して記録する
  - 初回送出後の RDE 試料 UUID の試料への記録と、再送出時の既存試料参照
  - 役割の写像（グループ担当 → OWNER、ユーザー → MEMBER）と、本システムが持たない ARIM 役割の運用の明記
- **Out**:
  - 実験データの新規登録 UI（registration の範囲）
  - OCR / API 入力
  - ARIM RDE 本体の改修
  - ブラウザ自動操作による RDE 画面の代行（案 B）

## Boundary Candidates

- 写像・バリデーション（必須欠落の扱い、DICE ID の解決）
- 課題・データセット・送出記録の保持
- 送出パッケージ生成（ExcelInvoice）

## Out of Boundary

- 本システム内の試料・装置台帳の主管理
- 外部認証・送出ジョブ（認証は RDE 側の画面で完結するため不要）
- OCR / API

## Upstream / Downstream

- **Upstream**: `experimental-data-registration`、`docs/research-arim-rde-export.md`
- **Downstream**: なし（OCR / API は入力拡張であり、機能依存は registration。delivery 順上はこの後）

## Existing Spec Touchpoints

- **Extends**: なし
- **Adjacent**: `experimental-data-registration`（実験データ・固有情報・試料の形、DICE ID などの器）

## Constraints

- 正本は本システム。ARIM RDE は出す先
- 用語は `CONTEXT.md`（データセットは課題に属する ARIM RDE 側のまとめ単位）
- 1 実験データ = ARIM RDE の 1 データ = 1 試料
- DICE ID を持つのはユーザーのみ
- 削除権限は ARIM（OWNER・ASSISTANT が削除可）より厳しく、本システムではシステム管理者のみ。意図的な差異として対応表に記す
- 案 C（センターハブへの正式連携の相談）は並行して行い、結果次第で送出手段を見直す
