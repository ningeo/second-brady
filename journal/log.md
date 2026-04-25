# Log

Append-only record of wiki operations. Each entry is prefixed with a date and operation type.

<!-- Format: ## [YYYY-MM-DD] operation | Subject -->

## [2026-04-25] ingest | SensorPush BLE device identification

Distilled `inbox/bluetooth_hunting/` into `notes/sensorpush-ble-device-identification.md`, covering RSSI hunting, SensorPush Device ID characteristic reads, little-endian conversion, and HT1 vs HTP.xw behavior. Updated `notes/index.md`.

## [2026-04-25] triage | Clear SensorPush Bluetooth inbox capture

Removed `inbox/bluetooth_hunting/` after promoting its durable content into `notes/sensorpush-ble-device-identification.md`.
