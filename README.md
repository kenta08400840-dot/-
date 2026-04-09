# Flipper Zero Wi-Fi ボード ファームウェア インストールガイド

## 概要

このガイドでは、Flipper Zero の Wi-Fi 開発ボード（ESP32-S2 搭載）にファームウェアをインストールする手順を説明します。

---

## 必要なもの

- Flipper Zero 本体
- Flipper Zero Wi-Fi 開発ボード（ESP32-S2 Wrover Module）
- USB-C ケーブル
- PC（Windows / macOS / Linux）
- Python 3.6 以上（esptool を使用する場合）

---

## 対応ファームウェア

| ファームウェア | 説明 |
|---|---|
| [Blackmagic](https://github.com/flipperdevices/blackmagic-esp32-s2) | Flipper Zero 公式が提供するデバッグ用ファームウェア |
| [ESP32 Marauder](https://github.com/justcallmekoko/ESP32Marauder) | Wi-Fi / Bluetooth セキュリティツール |
| [ESP-AT](https://github.com/espressif/esp-at) | Espressif 公式の AT コマンドファームウェア |

---

## インストール方法

### 方法 1: Web Flasher を使用する（推奨・簡単）

1. Wi-Fi 開発ボードを Flipper Zero に取り付けます。
2. Flipper Zero を PC に USB-C ケーブルで接続します。
3. **Google Chrome** または **Microsoft Edge** ブラウザを開きます。
4. [Flipper Zero Web Updater](https://lab.flipper.net/apps) または [ESP Web Tools](https://esp.huhn.me/) にアクセスします。
5. 使用するファームウェアを選択し、「Connect」をクリックします。
6. ポートの一覧からデバイスを選択します（例: `COM3`、`/dev/ttyUSB0`）。
7. 「Flash」をクリックしてインストールを開始します。
8. 完了後、デバイスを再起動します。

---

### 方法 2: esptool.py を使用する（上級者向け）

#### 1. esptool のインストール

```bash
pip install esptool
```

#### 2. ファームウェアのダウンロード

使用したいファームウェアの `.bin` ファイルをダウンロードします。

例（Blackmagic の場合）:
```bash
git clone https://github.com/flipperdevices/blackmagic-esp32-s2.git
```

#### 3. デバイスをブートローダーモードに切り替える

1. Wi-Fi ボードの **BOOT** ボタンを押し続けます。
2. **BOOT** ボタンを押したまま **RESET** ボタンを押して、すぐに離します。
3. 最後に **BOOT** ボタンを離します。
4. デバイスが DFU（ダウンロードモード）に入ります。

#### 4. フラッシュの消去

```bash
esptool.py --chip esp32s2 --port /dev/ttyUSB0 erase_flash
```

> Windows の場合は `/dev/ttyUSB0` を `COM3`（お使いの環境に合わせて変更）に置き換えてください。

#### 5. ファームウェアの書き込み

```bash
esptool.py --chip esp32s2 \
  --port /dev/ttyUSB0 \
  --baud 460800 \
  write_flash \
  --flash_mode dio \
  --flash_freq 80m \
  --flash_size 4MB \
  0x0 firmware.bin
```

#### 6. 再起動

書き込み完了後、**RESET** ボタンを押してデバイスを再起動します。

---

## Flipper Zero の設定

1. Flipper Zero の電源を入れます。
2. メニューから **GPIO → USB-UART Bridge** を選択します（Blackmagic の場合）。
3. Wi-Fi 機能を使用するアプリを起動します。

---

## トラブルシューティング

| 問題 | 解決方法 |
|---|---|
| デバイスが認識されない | USBケーブルを交換する、または別のUSBポートを試す |
| 書き込みが失敗する | ボーレートを `115200` に下げて再試行する（コマンドの `--baud 460800` を `--baud 115200` に変更） |
| ブートローダーモードに入れない | BOOT / RESET ボタンの操作手順を再確認する |
| ドライバが認識されない | [CP210x ドライバ](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers) をインストールする |

---

## 参考リンク

- [Flipper Zero 公式サイト](https://flipperzero.one/)
- [Flipper Zero ドキュメント](https://docs.flipperzero.one/)
- [Blackmagic ESP32-S2 GitHub](https://github.com/flipperdevices/blackmagic-esp32-s2)
- [ESP32 Marauder GitHub](https://github.com/justcallmekoko/ESP32Marauder)
- [esptool.py GitHub](https://github.com/espressif/esptool)

---

## ライセンス

このドキュメントは [MIT ライセンス](LICENSE) のもとで公開されています。