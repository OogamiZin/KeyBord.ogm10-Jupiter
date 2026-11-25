# プレート（トップ・ボトム）

[← トップページに戻る](../README.md)

トッププレートとボトムプレートは、メイン基板と同じく EasyEDA の基板データ（FR-4）で作っています。JLCPCB に**部品実装なしの基板**として注文します。

## 注文するもの

| ファイル | 内容 | 板厚 | 注文に使うデータ |
|---|---|---|---|
| `ogm10-Jupiter-top-plate.json` | トッププレート（スイッチとエンコーダーの穴付き） | **1.6 mm** | ここから出力した Gerber（zip）だけ |
| `ogm10-Jupiter-bottom-plate.json` | ボトムプレート（裏面にロゴと USB ポート名のシルク） | **1.6 mm** | ここから出力した Gerber（zip）だけ |
| `M2_washer_t1.0_D5.3mf` | M2 ワッシャー（外径 5 mm・厚さ 1 mm）。3D プリント用 | — | 8 個印刷します（市販品でも可） |

- 板厚はどちらも JLCPCB の標準の 1.6 mm です（メイン基板も同じ）。
- バンパーケースは、基板・プレートが 1.6 mm であることを前提に作っています。
- BOM・CPL は使いません（PCBA なし）。

## 1. EasyEDA で Gerber を出力する

1. [EasyEDA Standard](https://easyeda.com/editor) で「ファイル > 開く > EasyEDA ソース」（File > Open > EasyEDA Source）から JSON を開きます。
2. 「製造 > PCB 製造ファイル（Gerber）」（Fabrication > PCB Fabrication File (Gerber)）で Gerber の zip を保存します。
3. もう 1 枚も同じように出力します。

## 2. JLCPCB に注文する

| 項目 | トッププレート | ボトムプレート |
|---|---|---|
| Base Material | FR-4 | FR-4 |
| Layers | 2 | 2 |
| Dimensions | 229.2 × 76.2 mm（自動） | 229.2 × 76.2 mm（自動） |
| PCB Qty | 5 | 5 |
| PCB Thickness | 1.6 mm（標準） | 1.6 mm（標準） |
| PCB Color | 好きな色 | 好きな色（シルクは裏面＝外側に出ます） |
| Surface Finish | どれでも可（銅箔がないので見た目は変わりません） | 同左 |
| PCB Assembly | OFF | OFF |

- どちらのデータにも銅箔がありません。JLCPCB から確認の連絡が来たら、「キーボードのプレートなので銅箔なしで問題ない」と返答してください。
- 注文番号（Remove Order Number）は、表に出るトッププレートでは「Remove」を選ぶと、番号が印字されません（追加料金がかかる場合があります）。

## ネジ穴とワッシャー

- 取り付け穴は φ2.2 mm × 8 か所で、メイン基板と同じ位置です。
- 組み立ての重ね順は [トップページの「組み立て」](../README.md#組み立て) を見てください。
