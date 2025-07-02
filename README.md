# Corne Wireless (ZMK) 購入・初期セットアップメモ

## 概要

AliExpressで**完成品のCorne Wireless（ZMK専用）キーボード**を購入しました。

### 購入先

[AliExpress 商品ページ](https://ja.aliexpress.com/item/1005008684172489.html)

### Github リポジトリ

- [GitHub](https://github.com/a741725193/zmk-new_corne):
- [English](https://github.com/a741725193/zmk-new_corne/blob/main/README_EN.md):

```
git clone https://github.com/a741725193/zmk-new_corne
```

[AliExpress 商品ページ](https://ja.aliexpress.com/item/1005008684172489.html)

### 主な仕様

- **スイッチ**
  - Cherry MX互換（ロープロファイルではない）
- **基板**
  - Corne Classic（分割型）
- **マイコン**
  - Nice!nano V2 ×2
- **接続方式**
  - Bluetooth（ZMK専用）
  - Type-C有線接続（充電用、場合によって有線動作）
- **スクリーン**
  - Nice!View OLED ×2（超低電力）
- **バッテリー**
  - PH2.0コネクタ採用（着脱容易）
  - 1500mAh換算で約1250時間の使用可能
- **ノブ・ジョイスティック**
  - 左手: 多機能ノブ
  - 右手: 多機能ジョイスティック

---

## 新バージョンの改良点

商品説明より、新バージョンでは以下が強化されています。

- 信号品質の向上（オンボードアンテナ）
- リセットボタン配置改善
- 強化ガラススクリーンカバー
- ノブ・ジョイスティック搭載
- Nice!View OLED標準搭載

---

## 消費電流の目安

| 状態       | 左手     | 右手(左手の約60%) |
| ---------- | -------- | ----------------- |
| キー入力中 | 約1.2 mA | 約0.72 mA         |
| アイドル   | 約900 µA | 約540 µA          |
| スリープ   | 約200 µA | 約120 µA          |

> バックライトONの場合は大幅に増加（300 mA）呼吸モードにすると45 mA程度に抑制可能

---

## 初期セットアップ

### 1️⃣ 電源ON

左右のスイッチをONにし、青LED点灯を確認。

### 2️⃣ Bluetoothペアリング

- 左右のモジュールがリンクしていることを確認。
- 左側をホストにしてペアリング。
- キー入力テスト。

**ペアリング確認済み。問題なし。**

### 3️⃣ OLED動作確認

- Nice!View表示を確認。
- 表示内容はZMK設定に依存。

**OLED表示も問題なし。**

---

## 今後必要になるかもしれない作業

本製品は**ZMK専用**です。  
QMK ConfiguratorやVIAは使用できません。

カスタマイズする場合は以下の作業が必要です。

1. **GitHubリポジトリを用意**
   - 例: `https://github.com/yamagen/corne-wireless`
2. **ZMKファームウェアをビルド**
   - GitHub Actions推奨
3. **キーマップや設定を編集**
   - `config/`ディレクトリにkeymap/keymap.confを用意
4. **.uf2を書き込み**
   - 左右でファームウェアが別
   - 左: `corne_left [nice_view_adapter]`
   - 右: `corne_right [nice_view_adapter]`

---

## 設定例 (GitHub Actions)

`.github/workflows/build.yml`

```yaml
name: Build ZMK Firmware

on:
  push:
    branches:
      - main

jobs:
  build-left:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: zmkfirmware/zmk-build-action@v1
        with:
          shield: corne_left nice_view_adapter
          board: nice_nano_v2

  build-right:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: zmkfirmware/zmk-build-action@v1
        with:
          shield: corne_right nice_view_adapter
          board: nice_nano_v2
```
