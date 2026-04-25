# SensorPush BLE Device Identification

SensorPush sensors can be physically identified by combining RSSI hunting with a GATT Device ID read in nRF Connect. The reliable match is the SensorPush app's decimal **Device ID** compared with the BLE characteristic value read as a little-endian uint32.

Sources:
- https://www.sensorpush.com/bluetooth-api
- https://support.sensorpush.com/hc/en-us/articles/4404775409303-I-have-multiple-sensors-and-am-not-sure-which-one-is-which
- Field capture from `inbox/bluetooth_hunting/`, promoted and removed from inbox on 2026-04-25.

## Quick Procedure

1. Open the sensor details in the SensorPush app and note the **Device ID**.
2. Open nRF Connect and scan for nearby SensorPush devices.
3. Use RSSI to get physically close to the candidate sensor.
4. Connect to the candidate sensor.
5. Read the Device ID characteristic.
6. Reverse the four bytes and convert the result from hex to decimal.
7. If the decimal value matches the SensorPush app Device ID, the physical sensor is identified.

## RSSI Hunting

Use RSSI as a hot/cold signal, not as a distance measurement. Bodies, shelves, appliances, walls, metal, and phone orientation can all distort BLE signal strength.

| RSSI | Rough meaning |
|---:|---|
| -90 to -80 dBm | Far or barely reachable |
| -75 to -65 dBm | Nearby-ish |
| -60 to -50 dBm | Same room or close through a wall |
| -45 to -30 dBm | Very close |
| -30 dBm or stronger | Almost on top of it |

When the target is hard to isolate, turn off filters in nRF, restart scanning, sort by strongest RSSI, and place the sensor near the phone if possible. Pulling and reinserting the battery can force a fresh advertisement.

## HTP.xw Sensors

For newer HTP.xw sensors, connect in nRF and look under this service:

```text
EF090000-11D6-42BA-93B8-9DD7EC090AB0
```

Read this Device ID characteristic:

```text
EF090001-11D6-42BA-93B8-9DD7EC090AA9
```

nRF shows the value as four bytes. Interpret it as little-endian:

```text
CB E9 02 01 -> 01 02 E9 CB -> 0x0102E9CB -> 16968139
```

In the capture, that confirmed:

```text
SensorPush HTP.xw CF3 = Stage 3 Room
```

Another useful conversion from the same capture:

```text
16969083 -> 0x0102ED7B -> little-endian bytes 7B ED 02 01
```

That was the expected Device ID byte pattern for the Berry Fridge HTP.xw sensor.

## HT1 Sensors

The older blue HT1 sensor is first-generation and does not follow the same documented open protocol path as the second-generation HTP.xw sensors. In the capture, the HT1 still exposed a readable Device ID through the older service form:

```text
EF090000-11D6-42BA-93B8-9DD7EC090AA9
```

The same characteristic number held the Device ID:

```text
EF090001-11D6-42BA-93B8-9DD7EC090AA9
```

Example from the capture:

```text
C5 A3 07 00 -> 00 07 A3 C5 -> 0x0007A3C5 -> 500677
```

That confirmed:

```text
HT1 Device ID 500677 = GGH Seed fridge
```

If an HT1 does not appear under the expected SensorPush HTP.xw names, look for `SensorPush HT1`, `SensorPush HT`, an unnamed strong-RSSI device, or an older service UUID. Check the coin cell before spending too much time chasing UUIDs.

## Manufacturer Data Is Not Enough

nRF's manufacturer data is useful for distinguishing candidates during a scan, but it is not the authoritative match. In the capture, the advertised name suffixes such as `CF3` or `7A4` and the manufacturer data did not directly prove the decimal SensorPush app Device ID.

Use manufacturer data as a clue, then confirm with the Device ID characteristic.

## Optional Cross-Checks

The SensorPush Bluetooth API documents these useful characteristics for second-generation devices:

| Purpose | Characteristic |
|---|---|
| Device ID | `EF090001-11D6-42BA-93B8-9DD7EC090AA9` |
| Device Version | `EF090002-11D6-42BA-93B8-9DD7EC090AA9` |
| Battery Voltage | `EF090007-11D6-42BA-93B8-9DD7EC090AA9` |
| LED Control | `EF09000C-11D6-42BA-93B8-9DD7EC090AA9` |
| MAC Address | `EF09000D-11D6-42BA-93B8-9DD7EC090AA9` |

Battery voltage is a good sanity check against the SensorPush app, but Device ID is the definitive match. LED Control can act as a manual "find me" after identification: writing `01` should blink once. Avoid writing `80` unless you intend an indefinite blink and know how to turn it off.
