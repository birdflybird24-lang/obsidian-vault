# BKTS v1.0 (Project Draft)
## Brain Knowledge Transformation Standard

**Status**: Draft — 設計思想・章構成・必要仕様・ロードマップのみを定義した段階。テンプレート／フォルダ構造・スキーマの確定はまだ行っていない。

---

## 0. この文書の位置づけ

BKTSは、Brain2.0というVaultの「中身の書き方」を定めるものではない。
Brain2.0が何を蓄積し、何を蓄積しないか、そしてどうやって専門家の判断を
AIエージェントが再利用できる形へ変換するか——その**変換規則そのもの**を定義する標準である。

Brain2.0はメモ帳ではない。BKTSはそのコンパイラ仕様である。

```
Raw Data (自然言語)          ── ソースコード
      ↓  BKTS変換
Knowledge Object (構造化知識) ── コンパイル済み成果物
      ↓
AI Agent が読み込み・検索・比較・再利用
```

人間は自然な文章で記録してよい（設計原則6）。構造化はシステム側の責務であり、
BKTSはその変換規則を定めるものである。

---

## 1. 設計思想 (Design Philosophy)

### 1.1 保存対象は「情報」ではなく「専門家の判断」である
支援記録やアセスメントレポートの文章そのものはBrain2.0の対象ではない。
そこに埋め込まれている「専門家が何を根拠に、何を考え、なぜその結論に至ったか」
という**思考のプロセス**が対象である。

### 1.2 Knowledge Objectは「要約」ではなく「分解」から生まれる
要約は情報を圧縮する過程で、性質の異なる複数の判断を一つの曖昧な文へ融合してしまう。
これは後からAIエージェントが「なぜその判断に至ったか」を遡れなくする致命的な損失である。
BKTSは要約ではなく、原文に埋め込まれた個別の思考単位
（事実・観察・仮説・評価・決定・介入・結果・ルール化）を、
それぞれ独立したオブジェクトとして**分解・抽出**する。

### 1.3 原文は保存せず、判断の構造だけを保存する
原文全体を保持することは、①個人情報・機密情報のリスクを増大させ、
②ノイズによってAIの再利用性・検索性を下げる。
BKTSは原文への**Provenance（出典参照）**は残すが、原文そのものを
Knowledge Objectの構成要素とはしない。

### 1.4 推論構造 (Reasoning Structure) を保存する。結論だけを保存しない
「Aさんには就労継続支援B型が適切と判断された」という結論だけでは、
AIはこれを別のケースへ再利用できない。
「どんなFact／Observationから、どんなHypothesisを経て、その結論に至ったか」
という**連鎖そのもの**が再利用可能な知識である。

### 1.5 人間の入力は自然文でよい。構造化はシステムの責務
支援員・専門家に構造化記法を強制しない。自然な言葉で書かれた記録・発話から、
BKTSに基づく変換プロセス（当面は人間が抽出者となり、将来的にはAI支援による抽出）が
内部構造を生成する。人間に負荷をかけるほど、この仕組みは長続きしない。

### 1.6 Knowledgeは4つの性質を満たさなければならない
- **再利用可能 (Reusable)**：単一ケースを超えて別の文脈でも参照・適用できる
- **検索可能 (Searchable)**：構造化メタデータと関係性によって発見できる
- **比較可能 (Comparable)**：異なるケース・異なる専門家の判断を同じ軸で比較できる
- **統合可能 (Integrable)**：矛盾する知識・更新される知識を扱い、単一の知識体系へ統合できる

この4性質を満たさない抽出物は、そもそもKnowledge Objectとして認めない。
これがBKTSにおける「品質」の定義である。

### 1.7 変換パイプラインは直線ではなく、フィードバックを持つグラフである（設計上の追加提案）
Raw Data → … → Decision Ruleという9段階は説明のための直線的表記だが、
実際の運用ではOutcomeが新たなObservationやHypothesisを生み、
既存のDecision Ruleを補強・修正・棄却する。これは単なるデータ変換ではなく
**学習ループ**であり、AIエージェントが時間とともに賢くなっていくための構造的要件である。
BKTSの章構成・スキーマ設計は、この「循環する」性質を前提に行う。

---

## 2. 章構成 (Chapter Structure)

BKTS本体（v1.0が確定した際のドキュメント）は以下の章で構成する。
各章の詳細スキーマは本ドラフトの範囲外（→ロードマップのPhaseで確定）だが、
何を定義する章かはここで明確にしておく。

| 章 | タイトル | 内容 |
|---|---|---|
| 1 | Purpose & Scope | BKTSが解く問題、適用範囲、非適用範囲 |
| 2 | Core Philosophy | 本ドラフトの設計思想（上記1章）の正式版 |
| 3 | Knowledge Transformation Pipeline | Fact〜Decision Ruleの9段階それぞれの定義・抽出基準・境界事例 |
| 4 | Knowledge Object Model | 7種のKnowledge Objectの定義・必須属性・オブジェクト間関係 |
| 5 | Relationship & Graph Model | オブジェクト間のエッジ種別（derived_from, supports, contradicts, revises, generalizes_to 等） |
| 6 | Metadata & Identification Standard | ID体系、バージョニング、確信度／根拠強度の表現、出典・匿名化タグ |
| 7 | Extraction Protocol | 自然文のRaw Dataから9段階オブジェクトを抽出する手順（人間主導／将来のAI支援） |
| 8 | Quality & Validation Criteria | 1.6の4性質を満たすための具体的な合格基準、矛盾の扱い方 |
| 9 | AI Agent Interface Contract | AIエージェントがKnowledge Objectをどう検索・取得・利用するかの契約 |
| 10 | Governance & Versioning | BKTS自体の改訂手順、v1.0→v1.1等のバージョン管理方針 |

### 3章・4章の予告（概念定義のみ、詳細仕様はPhase 2で確定）

**パイプライン9段階（概念）**

| 段階 | 概念 |
|---|---|
| Fact | 検査数値・発言記録など、解釈を含まない客観的事実 |
| Observation | 専門家がFactに対して行った気づき・着目点 |
| Hypothesis | Observationから立てた仮の解釈・見立て |
| Assessment | 複数のHypothesisを検討した上での評価的判断 |
| Decision | Assessmentに基づき下した意思決定 |
| Intervention | Decisionを実行に移した具体的な支援行為 |
| Outcome | Interventionの結果として観測された変化 |
| Decision Rule | 複数ケースのOutcomeから一般化された「こういう状況ではこう判断する」という再利用可能な規則 |

**Knowledge Object 7種（概念）**

| Object | 概念 |
|---|---|
| Case | 1件の支援対象・経過をまとめる単位。他の全Objectを紐づける器 |
| Expert Judgment | 専門家が特定の局面で行った判断とその推論構造（Fact〜Decisionの連鎖） |
| Assessment | ある時点での評価結果そのもの（心理検査・アセスメントシートの構造化版） |
| Intervention Pattern | 複数ケースから抽出された、状況→介入→効果の再利用可能な型 |
| Decision Rule | 検証を経て一般化された判断規則。Intervention Patternより抽象度が高い |
| Knowledge | 制度・法令・研修資料・論文など、ケースに紐づかない一般知識 |
| Support Framework | 複数のDecision Rule・Intervention Patternを束ねた、支援方針の体系（例：特定の障害特性への総合的な支援アプローチ） |

---

## 3. 必要な仕様 (Required Specifications)

BKTS v1.0を完成させるために、今後確定させる必要がある仕様一覧。

1. **パイプライン段階の抽出基準**
   Fact/Observation/Hypothesis/…の各段階について、「これはFactか、それとも既にObservationか」を
   判定するための明確な境界基準と具体例（特にFact⇄ObservationとAssessment⇄Decisionの境界は曖昧になりやすい）

2. **Knowledge Objectスキーマ**
   7種それぞれの必須属性・任意属性・許容される関係先オブジェクト種別

3. **関係性（エッジ）タクソノミー**
   derived_from / supports / contradicts / revises / generalizes_to / triggered_by / results_in など、
   オブジェクト間関係の種類とその意味・方向性

4. **確信度・根拠強度モデル**
   専門家の確信度、証拠の強さ、専門家間の意見の相違をどう表現するか
   （単一の「正解」として断定しないという04_Expert_Judgementの運用ルールとも接続）

5. **Provenance（出典）仕様**
   原文を保存せずに出典を追跡する方法。ソース種別・日付・記録者・匿名化処理の記録方法

6. **ID・命名規則**
   全Knowledge Objectに一意なIDを付与する体系。Obsidianの `[[リンク]]` とfrontmatterプロパティの併用方針

7. **抽出プロトコル**
   人間（専門家・記録者）が書いた自然文から、抽出者（当面は人間、将来はAI支援）が
   9段階のオブジェクトへ変換する具体的な手順・問いかけの型

8. **バージョニング・改訂ルール**
   新しいOutcomeが既存のDecision Ruleと矛盾した場合の扱い（棄却／修正／条件分岐として分離）

9. **比較可能性のための統制語彙 (Controlled Vocabulary)**
   ケース横断比較を可能にするため、01_Domain_Modelと連携した分野・状況・介入手法等の分類体系

10. **プライバシー・倫理仕様**
    WISC等の心理検査データを含む高感度情報の匿名化ルール、同意管理、アクセス制御方針

11. **AIエージェント向けクエリ仕様**
    「このFact群に近いHypothesisを探す」「このDecision Ruleの根拠になったCaseを遡る」等、
    想定されるクエリパターンとそれに必要なインデックス設計（09_Data_Architectureと接続）

12. **品質ゲート基準**
    Knowledge Objectとして受理される最低条件（例：Provenanceを持つこと、最低1つの関係を持つこと、
    Hypothesisは必ず元になったObservationを参照すること）

---

## 4. 今後のロードマップ (Roadmap)

| Phase | 内容 | 成果物 |
|---|---|---|
| **Phase 0（現在）** | BKTS設計思想・章構成・必要仕様・ロードマップの策定 | 本ドラフト |
| **Phase 1** | パイプライン9段階の詳細定義（抽出基準・境界事例・具体例） | BKTS第3章 確定版 |
| **Phase 2** | Knowledge Object 7種のスキーマ設計（必須属性・関係性） | BKTS第4章 確定版 |
| **Phase 3** | 関係性タクソノミー、ID・メタデータ標準の設計 | BKTS第5〜6章 確定版 |
| **Phase 4** | 抽出プロトコルの設計（自然文→構造化オブジェクトの変換手順） | BKTS第7章 確定版 |
| **Phase 5** | 少数の匿名化サンプル（WISCレポート・ケース会議記録・支援記録）で試験抽出 | パイロット変換事例、BKTSへのフィードバック |
| **Phase 6** | パイロットで得た知見をもとにBKTSを改訂 | BKTS v1.1 |
| **Phase 7** | Brain2.0内にKnowledge Object用のテンプレート・記法を実装（ここで初めてMarkdown設計に着手） | 実運用テンプレート |
| **Phase 8** | 09_Data_Architectureと連携し、検索・取得レイヤーを設計 | クエリ可能なKnowledge Base |
| **Phase 9** | 支援員向けケース対応AIエージェントMVP（08_Product_AI_Agent） | ケース入力→類似Knowledge検索→判断提案 |
| **Phase 10** | 管理者向け品質評価AIエージェント（06_Assessment_Framework活用） | 対応品質の評価・フィードバック |
| **Phase 11** | 複数エージェント・複数事業所を統合する福祉現場OSへ発展 | Welfare OS |

---

## 5. 未決事項 (Open Questions)

本ドラフトの時点で意図的に保留している論点。Phase 1以降で扱う。

- Fact/Observationの境界判定は誰が行うのか（記録者本人か、第三者の抽出者か）
- 専門家間で判断が割れた場合、Expert Judgmentは複数バージョンを並列に保持するのか、統合するのか
- Decision Ruleへの一般化は、何件のOutcomeが集まった時点で「規則」と呼んでよいのか（閾値の定義）
- 原文を一切保存しない場合、抽出の正確性を後から監査する手段をどう担保するか
