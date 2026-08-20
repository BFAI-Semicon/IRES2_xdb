# 既存の実験データ管理システム調査

調査日: 2026-08-20  
対象: 商用利用可能かつカスタマイズ可能な実験データ管理システム（EDMS / ELN / LIMS / RDM）  
参照: `.kiro/steering/roadmap.md`、`CONTEXT.md`、`.kiro/specs/experimental-data-registration/brief.md`

## 結論

商用利用可能でカスタマイズ可能な候補は複数存在する。一方で、本プロジェクトのドメイン（正本・訂正権限・装置固有情報・ARIM RDE）にそのまま合う製品は見当たらない。ロードマップの「0 から構築」方針は妥当寄りである。

ドロップイン置き換えは不可。採用するなら openBIS または datalab を基盤にし、ドメインの大半をカスタムする形になる。

## 照合した必須要件

| 要件                                 | 既存製品での典型                                   |
| ------------------------------------ | -------------------------------------------------- |
| 実験データが正本・削除不可           | 多くの ELN/LIMS は「ノート／解析結果」中心         |
| グループ・試料・装置台帳・固有情報欄 | 試料〜実験の階層はあるが、「装置担当＝欄定義」は稀 |
| 登録者のみ訂正できる権限             | 汎用 RBAC で近似は可能、専用モデルはほぼ無し       |
| オフライン中規模装置現場の手入力     | クラウド前提／解析系 RDM が多い                    |
| 後続の ARIM RDE 送出                 | 専用連携は無し（RDEToolKit は送出側のみ）          |

## 有力候補（ライセンスが商用向き）

### 1. openBIS（ETH Zurich）

- **ライセンス**: Apache 2.0（商用・改変・プロプライエタリ組み込み可）
- **性質**: ELN + 在庫/試料 + データ管理。メタデータ型・API・オンプレ
- **合う点**: 試料・装置・添付・履歴の枠、カスタム属性、長期メンテ
- **合わない点**: Project/Experiment 前提が強く、「実験データ1操作＝正本」や装置担当ロール、ARIM 語彙への合わせ込みは大規模改変が必要
- **向く用途**: 「基盤を借りてドメインを載せる」方針のとき
- **参照**: [openBIS](https://sis.id.ethz.ch/services/rdm/openbis.html)

### 2. datalab

- **ライセンス**: MIT
- **性質**: 材料化学向け実験データ＋メタデータ。プラグイン、Web UI、API、セルフホスト／有償マネージド
- **合う点**: 試料〜実験の記録、拡張前提、ライセンスが最も緩い
- **合わない点**: 半導体製造/検査の装置台帳・訂正モデル・RDE 連携は自前実装。コミュニティ規模は openBIS より小さい
- **参照**: [GitHub](https://github.com/datalab-org/datalab) / [ドキュメント](https://docs.datalab-org.io/)

### 3. NOMAD Oasis（FAIRmat）

- **ライセンス**: Apache 2.0
- **性質**: スキーマ／パーサ中心の材料科学 RDM。Docker オンプレ
- **合う点**: 材料系メタデータ、カスタムスキーマ、機関内運用
- **合わない点**: 「装置前で正本登録」より「構造化アップロード・公開準備」向き。第1スライス（手入力登録）には重い
- **参照**: [NOMAD Oasis](https://nomad-lab.eu/nomad-lab/nomad-oasis.html)

### 4. DERIVA（USC ISI）

- **ライセンス**: Apache 2.0
- **性質**: ER モデル変更で UI/API が追従するスキーマ駆動プラットフォーム
- **合う点**: 独自ドメインモデルを載せやすい
- **合わない点**: 学習コスト高、現場 UI／権限の細則は別途設計
- **参照**: [DERIVA](https://deriva.isi.edu/deriva/)

## 注意が必要な候補

| 製品                  | ライセンス       | コメント                                                                   |
| --------------------- | ---------------- | -------------------------------------------------------------------------- |
| SENAITE               | GPLv2            | 商用利用は可だが、改変配布時はソース開示義務。分析ラボ向けでドメインが遠い |
| Chemotion / eLabFTW   | AGPL-3.0         | ネットワーク提供でもソース開示リスク。化学 ELN 寄り                        |
| LabWare 等の商用 LIMS | プロプライエタリ | 設定自由度は高いがコスト・複雑性大。ARIM／現場正本向けではない             |

## 補完ツール（代替ではない）

- **RDEToolKit**（NIMS）: RDE への構造化・送出支援。正本システムではない。
  [RDEToolKit Documentation](https://nims-mdpf.github.io/rdetoolkit/)
- **ローコード**（NocoBase 等）: 独自ドメインの速い試作向き。実験データ管理製品そのものではない

## 本プロジェクトへの示唆

1. **ドロップイン置き換えは不可**  
   語彙・権限・「消せない正本」・RDE 後続を満たす既製品は見つからない。

2. **採用するなら**  
   - 基盤候補: openBIS または datalab（いずれも permissive）  
   - 期待値: UI・試料・添付の土台を借り、ドメインの大半はカスタム  
   - リスク: プラットフォームの前提と `CONTEXT.md` の衝突（訂正権限、グループと実験データの関係など）

3. **0 から作る方針との関係**  
   ロードマップの「正本登録を薄く先に出す」戦略と整合する。既存製品は機能過多かドメイン不一致で、最初の価値（手入力登録）までのコストが必ずしも下がらない。

4. **ハイブリッド案**（検討余地）  
   正本は自作（薄いドメインアプリ）＋送出は RDEToolKit／独自 export。既存 ELN を正本に据えるより、境界がクリア。

## 推奨

現状のスコープなら自作継続を第一候補とする。技術スタック選定時にだけ、openBIS / datalab を基盤にするかを短い比較スパイク（1〜2 週間）で切るとよい。

評価スパイクの観点例:

- 実験データの削除不可を強制できるか
- 登録者のみ訂正できる権限を表現できるか
- 装置台帳と固有情報欄（装置担当による定義）を載せられるか
- グループ／試料の編集境界が `CONTEXT.md` に合わせられるか
- API 経由の登録・後続の ARIM RDE 送出を足しやすいか

## 参考リンク

- [openBIS](https://sis.id.ethz.ch/services/rdm/openbis.html)
- [datalab](https://github.com/datalab-org/datalab)
- [NOMAD Oasis](https://nomad-lab.eu/nomad-lab/nomad-oasis.html)
- [DERIVA](https://deriva.isi.edu/deriva/)
- [SENAITE](https://github.com/senaite/senaite.lims)
- [Chemotion ELN](https://github.com/ComPlat/chemotion_ELN)
- [RDEToolKit](https://nims-mdpf.github.io/rdetoolkit/)
