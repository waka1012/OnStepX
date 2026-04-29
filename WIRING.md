# OnStepX 配線メモ

## 使用ボード
- **マイコン**: FREENOVE ESP32-WROOM-32E
- **ピンマップ**: MaxESP4（`Config.h` の `PINMAP MaxESP4`）
- **変更点**: `Pins.MaxESP4.h` の `AXIS1_DIR_PIN` を GPIO0（ストラッピングピン）から GPIO16 に変更済み

---

## TB6600 ステッパードライバ 配線（2SC1815トランジスタ経由）

3.3V出力のESP32から5V入力のTB6600を駆動するため、2SC1815(NPN)トランジスタを使用。

```
ESP32(3.3V)                                    TB6600
                                         5V --- PUL+
STEP(GPIO18) ---[1kΩ]--- B(C1815) C --------- PUL-
                               E
                               |
                              GND

                                         5V --- DIR+
DIR (GPIO16) ---[1kΩ]--- B(C1815) C --------- DIR-
                               E
                               |
                              GND

                                         5V --- ENA+
ENA (GPIO5 ) ---[1kΩ]--- B(C1815) C --------- ENA-
                               E
                               |
                              GND

ESP32 GND ----------------------------------------- GND共通
```

### Axis1（赤経 RA）

| TB6600端子 | 接続先 |
|-----------|--------|
| PUL+      | 5V     |
| PUL-      | 2SC1815コレクタ（ベース ← 1kΩ ← GPIO 18） |
| DIR+      | 5V     |
| DIR-      | 2SC1815コレクタ（ベース ← 1kΩ ← GPIO 16） |
| ENA+      | 5V     |
| ENA-      | 2SC1815コレクタ（ベース ← 1kΩ ← GPIO 5）  |
| GND       | ESP32 GND |

### Axis2（赤緯 Dec）

| TB6600端子 | 接続先 |
|-----------|--------|
| PUL+      | 5V     |
| PUL-      | 2SC1815コレクタ（ベース ← 1kΩ ← GPIO 27） |
| DIR+      | 5V     |
| DIR-      | 2SC1815コレクタ（ベース ← 1kΩ ← GPIO 26） |
| ENA+      | 5V     |
| ENA-      | 2SC1815コレクタ（ベース ← 1kΩ ← GPIO 5）  |
| GND       | ESP32 GND |

> **注意**: ENA は Axis1/Axis2 共通（GPIO5）。トランジスタにより論理が反転するため、
> `Config.h` に `#define SHARED_ENABLE_STATE HIGH` を追加済み。

---

## MaxESP4 全ピン割り当て（参考）

| GPIO | 用途 |
|------|------|
| 0    | ※未使用（ストラッピングピンのため変更済み） |
| 1    | TX0（USB シリアル）|
| 3    | RX0（USB シリアル）|
| 4    | SERIAL_TMC TX |
| 5    | SHARED_ENABLE（全軸共通 Enable）|
| 12   | ステータス LED |
| 13   | Axis2 ホームスイッチ |
| 14   | Axis1 ホームスイッチ |
| 15   | Axis3/4/5 DIR |
| 16   | Axis1 DIR（変更後）|
| 18   | Axis1 STEP |
| 19   | Axis4 STEP |
| 23   | リミットスイッチ / PPS |
| 25   | 1-Wire / ステータス LED |
| 26   | Axis2 DIR |
| 27   | Axis2 STEP |
| 32   | ST4 DE- South |
| 33   | ST4 DE+ North |
| 34   | ST4 RA- West（入力専用）|
| 35   | ST4 RA+ East（入力専用）|
| 36   | PEC センサ（入力専用）|
| 39   | SERIAL_TMC RX（入力専用）|
