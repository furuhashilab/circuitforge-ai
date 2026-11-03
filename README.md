> [!IMPORTANT]
> **Repository relocated / 開発先を移行しました（2026-11-04）**  
> CircuitForge AI の2026年度ゼミ論・開発の正本は **[furuhashilab/2026gsc_TomoyaAiko](https://github.com/furuhashilab/2026gsc_TomoyaAiko)** です。  
> [現行README](https://github.com/furuhashilab/2026gsc_TomoyaAiko/blob/main/README.md) / [開発手順](https://github.com/furuhashilab/2026gsc_TomoyaAiko/blob/main/docs/WORKFLOW.md) / [Issue一覧](https://github.com/furuhashilab/2026gsc_TomoyaAiko/issues) / [移行対応表](https://github.com/furuhashilab/2026gsc_TomoyaAiko/blob/main/docs/MIGRATION.md)  
> **新しいIssue・PR・開発作業は正式リポジトリで行ってください。** この旧リポジトリは過去のコミット・PRの参照用に残しています。以下は移行前のREADME原文です。

---

# CircuitForge AI

**Generate imaginary motorsport routes from real geospatial data, then experience them in 3D.**

> Research-stage project at [Furuhashi Lab](https://github.com/furuhashilab). **Status: concept / planning (2026-11-03).** No working route generator, AI model, or 3D driving demo is claimed yet.

[日本語](#プロジェクト概要) | [English summary](#english-summary)

## 最初に読む：開発の進め方

**「これから何を、どの順番で、どのツールを使って進めるか」は [実作業ガイド（WORKFLOW.md）](docs/WORKFLOW.md) に具体的にまとめています。** 初めて着手する場合は、まずこのページを読んでください。

1. **研究条件の確認：** [#1 研究目的・対象都市](https://github.com/furuhashilab/circuitforge-ai/issues/1) と [#9 データ・ライセンス](https://github.com/furuhashilab/circuitforge-ai/issues/9)。
2. **実装開始：** [#2 開発環境](https://github.com/furuhashilab/circuitforge-ai/issues/2) をClaude Codeで、1 Issue → 1ブランチ → 1 PRで進める。
3. **最初の動く成果物：** [#3](https://github.com/furuhashilab/circuitforge-ai/issues/3)〜[#6](https://github.com/furuhashilab/circuitforge-ai/issues/6) を通して、**手動のルートを3Dで表示し、車両が追走できるデモ**を完成させる。その後に自動生成とAI研究へ進む。

作業のたびに「何を確認するか」、Macでの操作例、Claude Codeへの最初の指示文、PRの書き方、卒論向けの実験記録方法まで [WORKFLOW.md](docs/WORKFLOW.md) に記載しています。

## プロジェクト概要

CircuitForge AI は、ユーザーが**好きな地域**と**理想のコースの特徴**を指定すると、実在する道路網・都市モデル・標高などの地理空間情報をもとに、**架空のモータースポーツコース**を提案し、そのレイアウトを3Dで追体験できる研究開発プロジェクトです。

発想の出発点は「自分の好きな都市に、自分の理想のF1市街地サーキットを作れたら？」という問いです。将来的には山岳地形や林道にも広げ、ラリーステージの設計・3D車両追従表示まで視野に入れます。

本システムは**実際のレース開催、安全認証、公道の走行可能性を判断するものではありません**。生成ルートは創作・GIS研究・可視化を目的とします。

## 目指す体験（計画）

1. 地図上で地域（AOI: Area of Interest）を指定する。
2. コース種別と好みを入力する（例：全長3–5km、長い直線、テクニカル区間、ランドマーク経由）。
3. OSMの道路ネットワークから候補ルートを生成し、接続性・距離・方向などを検証する。
4. 計測可能な特徴量と、将来的には独自の小型AIモデルで候補を評価・選択する。
5. PLATEAUの都市モデルや地形を使ったCesiumJSビューアでコースを表示する。
6. 3D車両の走行アニメーションを**追走・車載・上空視点**で再生する。初期版は物理シミュレーションではない。

### Circuit Mode — 卒論MVPの対象

- 対象：まず**PLATEAUデータが利用できる1都市・限定した範囲**。札幌中心部は候補であり、データ検証後に確定する。
- 実在道路の接続を考慮した**閉じた周回ルート**。
- 入力例：希望距離、直線重視／テクニカル重視、景観やランドマークの優先度。
- 最初の動作目標：**手動で指定したルートを3D都市空間に表示し、汎用3D車両で走行プレビュー**。
- 次の目標：ルールベース・グラフ探索によるコース自動生成と評価。
- AIによる嗜好適応・知識蒸留は、**ベースラインを確立してから検証する研究テーマ**。

### Rally Mode — 将来的な拡張

- 山道・林道・高低差を考慮した地点間ステージ（通常はスタートとゴールが異なる）。
- 舗装／未舗装などのOSM道路属性、標高データを評価。
- 3Dのラリー車両を追従カメラで再生（GR Yarisなど特定車種の3Dアセットは利用権限が確認できるまで使用しない）。
- 実運転・ドリフト・車両物理・ゲームへのエクスポートは長期構想。

## 技術構成（候補・未確定）

| 領域 | 候補 | 用途 |
|---|---|---|
| 道路・交差点 | [OpenStreetMap](https://www.openstreetmap.org/) | 道路接続・一方通行・道路属性 |
| 都市3D・地形 | [Project PLATEAU](https://www.mlit.go.jp/plateau/) | 建物・地形などの3D表現（収録範囲を確認） |
| GIS処理 | Python / [OSMnx](https://osmnx.readthedocs.io/) / [NetworkX](https://networkx.org/) | 道路グラフ・周回路探索・特徴量 |
| コース交換 | GeoJSON (+ 明示的なmetadata) | 生成器とビューアの疎結合 |
| 3D表示 | [CesiumJS](https://cesium.com/platform/cesiumjs/) / glTF・GLB | 3D地図・コース・車両の再生 |
| フロントエンド | TypeScript（フレームワーク未決定） | 入力UI・プレビュー |
| 将来の学習 | Python / PyTorch 等（候補） | 教師モデル評価に基づく生徒モデルの比較 |
| 開発 | Claude Code（主） / ChatGPT（研究設計） / Codex（独立レビュー） | AI支援開発 |

**AIの役割：** 大規模言語モデルが直接「正しい緯度経度の列」を生成する前提にはしません。まず道路グラフに沿って妥当な候補を生成し、AIが自然言語の好みを構造化したり、候補を評価・ランキングしたりする方針を検証します。教師モデルの出力を学習に用いる場合は事前に利用規約・権利を確認します。

## 研究として検証したいこと

**仮の研究質問：** 「実在する道路網から生成した周回路を、ユーザーの希望する走行特性に合わせて選択・評価できるか。また教師モデルのコース評価傾向を小型モデルへ蒸留すると、嗜好一致度や計算コストはどう変化するか？」

- **制約の満足度：** 経路の連結性、方向制約、距離誤差、周回性。
- **嗜好への適合：** 直線・曲がり角・景観等の特徴量、ブラインドでの人手評価。
- **候補の多様性：** 同一AOI・異なる希望条件で候補がどの程度変化するか。
- **再現性・効率：** 入力、抽出したデータ、乱数seed、処理時間、失敗率。
- **比較：** ルールベース／教師モデル／蒸留後の小型モデル（順次検討）。

卒論の研究範囲は指導教員との相談・データ実証を踏まえて決定します。計画時点でモデルの学習や性能達成は主張しません。

## 開発ロードマップ

| 段階 | 内容 | 優先度 |
|---|---|---|
| 0: Research foundation | 研究質問・対象都市・権利とデータを確認 | P0 |
| 1: 3D route viewer | 手動ルートのGeoJSONを表示、3D車両で追走 | P0 |
| 2: Automatic circuit | OSMグラフから有効な周回路候補を生成 | P1 |
| 3: Preference-aware ranking | 好みの入力・特徴量・比較実験 | P1 |
| 4: Model distillation | 許諾確認後、教師評価→小型モデル→性能比較 | P2 |
| Future | Rally Mode、山岳地形、より高度な車両挙動、他ゲームとの連携 | Future |

詳細は [Roadmap](docs/ROADMAP.md)、進行中の課題は [Issues](https://github.com/furuhashilab/circuitforge-ai/issues) を参照。

## ドキュメント

- [研究の目的・方法](docs/RESEARCH_DESIGN.md)
- [システム構成・データフロー](docs/ARCHITECTURE.md)
- [ロードマップとスコープ](docs/ROADMAP.md)
- [地理空間データ・ライセンス・安全性](docs/DATA_AND_LICENSES.md)
- [意思決定ログ](docs/DECISIONS.md)
- [Claude Codeへの指示](CLAUDE.md) / [その他のAIエージェント](AGENTS.md)
- [開発・PRのルール](CONTRIBUTING.md)

## セットアップ

**現在はドキュメント整備段階です。実行可能なアプリ・インストール手順はまだありません。** 開発環境の構築は [Issue #2](https://github.com/furuhashilab/circuitforge-ai/issues/2) で管理します。実行手順が決まり次第追記します。

## データ・ライセンス

OSM・PLATEAU・車両3Dモデル・教師AI出力は**それぞれ利用条件が異なります**。READMEに名前を挙げたことは利用許諾の確認を意味しません。帰属表示、再配布、商標、AI学習利用可否を [DATA_AND_LICENSES.md](docs/DATA_AND_LICENSES.md) で確認してから公開してください。**ソースコードのLICENSEは未選定（無許諾のまま）**です。

## English summary

CircuitForge AI is an early-stage GIS research concept for generating **fictional motorsport routes on real-world road networks**, visualizing the routes in 3D city/terrain scenes, and replaying them with a follow-camera 3D vehicle. The initial research MVP focuses on an urban closed-loop circuit using OpenStreetMap and, where available, Project PLATEAU. A future rally mode would consider elevation, forest roads, and point-to-point stages. A preference-aware AI ranker and model distillation are research goals, **not implemented features**. Routes are not validated for real-world racing or public-road use.

---
**Repository:** Furuhashi Lab · Project lead: [@mapbytomoya](https://github.com/mapbytomoya) · Founded 2026.
