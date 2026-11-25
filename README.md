# ogm10-Jupiter

RP2040 と USB ハブ（CH334F）をワンプレートで実装した、43キー + ロータリーエンコーダー 2個の一体型キーボード

![組み立てた ogm10-Jupiter（上）と基板の裏面（下）](images/ogm10-Jupiter-photo.jpg)

## Specs

| 項目 | 内容 |
|---|---|
| キー数 | 43キー + ロータリーエンコーダー 2個（左右・プッシュスイッチ付き） |
| スイッチ | Kailh Choc V2（ホットスワップソケット） |
| MCU | RP2040（基板直付け） |
| フラッシュ | W25Q128JVSIQ（128Mbit / 16MB） |
| USB | Type-C 入力 ×1 + USB ハブ出力（Type-C）×3 |
| マトリクス | 4行 × 12列（COL2ROW、エンコーダーのプッシュもマトリクス内） |
| 基板サイズ | 約 229.2 mm × 76.2 mm |
| 取り付け穴 | φ2.2 mm × 8（M2） |
| ファームウェア | QMK / Vial |

## 設定ツール

Webブラウザ（PC版Chrome か Edge）で、マクロの編集、キー割り当ての変更、今のレイヤーのキー表示ができます。

ようはogm10-Jupiter用にVIALを改造したものです。

**https://oogamizin.github.io/KeyBord.ogm10-Jupiter/**

左ノブを押し込みしたまま、右下のキー（→）を押仕込みでもChromeで開くように仕込んでます。

使い方は [docs/README.md](docs/README.md) にあります。

## 作り方

全体の流れです。各ステップの詳しい手順はリンク先にあります。

| # | やること | 詳しい手順 |
|---|---|---|
| 1 | メイン基板を JLCPCB に注文する（部品実装込み） | [pcb/README.md](pcb/README.md) |
| 2 | トッププレートとボトムプレート（どちらも 1.6 mm）を JLCPCB に注文する | [plate/README.md](plate/README.md) |
| 3 | 手はんだ部品と、下の「自分で用意するもの」を買う・3D プリントする | [pcb/README.md](pcb/README.md#自分ではんだ付けする部品)、下の表 |
| 4 | ソケットとエンコーダーをはんだ付けし、ファームウェアを書き込む | 下の「書き込み」 |
| 5 | プレートを重ねてネジで留め、スイッチ・キーキャップ・ノブを付ける | 下の「組み立て」 |
| 6 | （好みで）バンパーケースを付ける | [bumper-case/README.md](bumper-case/README.md) |

### 1〜2. 注文するもの（まとめ）

| 注文先 | 何を | 使うデータ |
|---|---|---|
| JLCPCB（PCB + PCBA） | メイン基板（板厚 1.6 mm、裏面に部品実装） | `pcb/ogm10-Jupiter-pcb.json` から出力した Gerber・BOM・CPL |
| JLCPCB（PCB のみ） | トッププレート（板厚 1.6 mm） | `plate/ogm10-Jupiter-top-plate.json` から出力した Gerber |
| JLCPCB（PCB のみ） | ボトムプレート（板厚 1.6 mm） | `plate/ogm10-Jupiter-bottom-plate.json` から出力した Gerber |
| LCSC など | ホットスワップソケット 43 個、ロータリーエンコーダー 2 個 | [pcb/README.md](pcb/README.md#自分ではんだ付けする部品) |

リポジトリには Gerber などの注文用ファイルは入っていません。EasyEDA Standard で JSON を開いて出力します（手順は各 README にあります）。

### 3. 自分で用意するもの

| Part | Quantity |
|---|---|
| Choc V2 スイッチ | 43 |
| キーキャップ（MX 互換の十字軸。Choc V2 は MX 用のキーキャップが付きます） | 43<br>Sample：<br>①オーダーメイド: [ogm10-Jupiter 専用デザイン（Yuzu Keycaps / PFF・PBT 昇華印刷・43キー）](https://yuzukeycaps.com/ja/c/fe731a0d-f7e9-4b01-95c2-b6d7624d95f9)<br>②自作3Dプリント用データ: [keycaps/](keycaps/README.md) |
| ロータリーエンコーダー用ノブ（φ6 D カット軸用） | 2（3Dプリント用データあり → [knob/](knob/README.md)） |
| USB Type-C ケーブル | 1〜 |
| M2×6mm ネジ（トッププレート側） | 8 |
| M2×4mm ネジ（ボトムプレート側） | 8 |
| M2×3mm スペーサー（メス-メス） | 8（例: [Amazon](https://amzn.asia/d/0fCThe3z)） |
| M2 ワッシャー（外径 5mm・厚さ 1mm） | 8（3Dプリント用: [`plate/M2_washer_t1.0_D5.3mf`](plate/M2_washer_t1.0_D5.3mf)） |
| 2.54mm ジャンパーピン（任意） | 1（JP1 のショート用。ピンセットなどでショートしても構いません） |

## 書き込み

ファームウェアは [`docs/firmware/ogm10_jupiter_vial.uf2`](https://github.com/OogamiZin/KeyBord.ogm10-Jupiter/raw/main/docs/firmware/ogm10_jupiter_vial.uf2) をダウンロードして使います（main のファームウェアを自動でビルドした最新版です）。

1. **ボトムプレートを付ける前に**書き込みます。BOOT ジャンパー（JP1）は基板の裏面にあり、ボトムプレートを付けると隠れます。
2. JP1 をショートしたまま USB-C IN に接続し、`RPI-RP2` ドライブが出たら `.uf2` をドラッグ＆ドロップします。
3. 書き込みが終わると自動で再起動し、キーボードとして認識されます。

一度書き込んだ後は、JP1 を使わなくても、キー操作や設定ツールの「ファーム更新」で更新できます。詳しくは [qmk_firmware/keyboards/ogm10_jupiter/readme.md](qmk_firmware/keyboards/ogm10_jupiter/readme.md) を見てください。

## 組み立て

取り付け穴 8か所を、上から順に次の構成で固定します。

```
↓ M2×6mm ネジ
トッププレート
ワッシャー（厚さ 1mm）
メイン基板（PCB）
M2×3mm スペーサー
ボトムプレート
↑ M2×4mm ネジ
```

1. ボトムプレートの下から M2×4mm ネジを通し、M2×3mm スペーサーを固定します。
2. スペーサーの上にメイン基板を載せます（部品のある裏面を下に）。
3. 基板の取り付け穴に厚さ 1mm のワッシャーを置き、その上にトッププレートを重ねます。
4. トッププレートの上から M2×6mm ネジを通し、スペーサーに固定します。
5. スイッチをトッププレートの上から差し込みます。ピンが曲がらないよう、ソケットの位置を確かめながらまっすぐ押し込んでください。
6. キーキャップとノブを付けます。
7. [設定ツール](https://oogamizin.github.io/KeyBord.ogm10-Jupiter/)の「レイヤ監視」などで、すべてのキーとエンコーダーが反応するか確認します。

## 注意点

- USB ハブ出力ポートの合計電流は、上流のポートが供給できる範囲（一般的なPCのポートなら 500mA〜900mA 程度）に収めてください。
- JLCPCB に PCBA を発注するときは、一部の部品の向きを修正する必要があります。
- 初回（2026年11月）の製造分は R4 の取り外しが必要です。

詳しくは [pcb/README.md](pcb/README.md) を見てください。

## 詳細ドキュメント

| 内容 | ページ |
|---|---|
| 基板：注文用ファイルの出力、JLCPCB の注文設定、部品の向きの修正、手はんだ部品、USB ハブの仕様、ピンアサイン | [pcb/README.md](pcb/README.md) |
| プレート：注文用ファイルの出力、板厚などの注文設定、ワッシャー | [plate/README.md](plate/README.md) |
| ファームウェア：書き込み方法、ビルド、Vial | [qmk_firmware/keyboards/ogm10_jupiter/readme.md](qmk_firmware/keyboards/ogm10_jupiter/readme.md) |
| ノブ：選び方、ファイル一覧、印刷のコツ | [knob/README.md](knob/README.md) |
| 設定ツール：使い方、できること、制約 | [docs/README.md](docs/README.md) |
| バンパーケース：足・前後ガード・縦置きスタンドの 3D データ、取り付け方 | [bumper-case/README.md](bumper-case/README.md) |
| キーキャップ：43キー分の 3D データ（2色・フルカラー）、十字穴の合わせ込み、印刷設定 | [keycaps/README.md](keycaps/README.md) |

## Files

| パス | 内容 |
|---|---|
| `pcb/ogm10-Jupiter-pcb.json` | メイン基板（EasyEDA Standard ソース）。Gerber・BOM・CPL をここから出力 |
| `pcb/ogm10-Jupiter-schematic.json` | 回路図（EasyEDA Standard ソース）。確認用 |
| `plate/ogm10-Jupiter-top-plate.json` | トッププレート（EasyEDA Standard ソース）。Gerber をここから出力 |
| `plate/ogm10-Jupiter-bottom-plate.json` | ボトムプレート（EasyEDA Standard ソース）。Gerber をここから出力 |
| `plate/M2_washer_t1.0_D5.3mf` | M2 ワッシャー（3D プリント用） |
| `qmk_firmware/` | QMK / Vial ファームウェア |
| `docs/firmware/ogm10_jupiter_vial.uf2` | ビルド済みファームウェア（最新版） |
| `knob/` | ロータリーエンコーダー用ノブ（STL / FreeCAD マクロ） |
| `keycaps/` | キーキャップ（3MF / 生成スクリプト） |
| `bumper-case/` | バンパーケース（足・前後ガード・縦置きスタンドの STL / 生成スクリプト） |
| `images/` | 写真 |
| `docs/` | 設定ツール（GitHub Pages で公開しているページ） |

EasyEDA Standard の「ファイル > 開く > EasyEDA ソース」で各 JSON を読み込めます。
