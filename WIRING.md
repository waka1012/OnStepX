# OnStepX 配線メモ

## 使用ボード
- **マイコン**: FREENOVE ESP32-WROOM-32E
- **ピンマップ**: MaxESP4（`Config.h` の `PINMAP MaxESP4`）
- **変更点**: `Pins.MaxESP4.h` の `AXIS1_DIR_PIN` を GPIO0（ストラッピングピン）から GPIO16 に変更済み

---

## TB6600 ステッパードライバ 配線

### Axis1（赤経 RA）

| TB6600端子 | ESP32 GPIO |
|-----------|-----------|
| PUL+      | GPIO 18   |
| DIR+      | GPIO 16   |
| ENA+      | GPIO 5    |
| PUL- / DIR- / ENA- | GND |

### Axis2（赤緯 Dec）

| TB6600端子 | ESP32 GPIO |
|-----------|-----------|
| PUL+      | GPIO 27   |
| DIR+      | GPIO 26   |
| ENA+      | GPIO 5    |
| PUL- / DIR- / ENA- | GND |

> **注意**: ENA（Enable）は Axis1/Axis2 共通（GPIO5）。
> ESP32 は 3.3V 出力。TB6600 は 3.3V でも動作するが、不安定な場合はレベルシフタ（3.3V→5V）を挟む。

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
