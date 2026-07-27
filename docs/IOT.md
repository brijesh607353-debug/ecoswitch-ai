# IoT Architecture — EcoSwitch AI

> **Status: Planned — requires hardware (ESP32) and firmware development.**
>
> No IoT code exists in this repository yet. This document describes the intended architecture.

---

## Overview

The IoT layer connects physical energy sensors (attached to an ESP32 microcontroller) to the EcoSwitch AI cloud backend. Readings are transmitted over Wi-Fi and stored for display in the dashboard.

```
[Power Meter / Current Sensor]
            │
            ▼
        [ESP32]                     ← Microcontroller (requires hardware)
    - Reads sensor values
    - Connects to Wi-Fi
    - Sends HTTP POST to API
            │
            ▼ POST /api/readings    ← Planned API endpoint
        [Express API]
            │
            ▼
     [PostgreSQL / Firestore]
            │
            ▼
      [Dashboard UI]
```

---

## Planned Hardware

| Component | Role |
|-----------|------|
| **ESP32** (any variant) | Main microcontroller — Wi-Fi + processing |
| **SCT-013 current sensor** | Non-invasive AC current clamp |
| **ACS712 current sensor** | Inline current measurement (alternative) |
| **ZMPT101B** | AC voltage sensor |
| *(Optional)* **OLED display** | Local readout on the device |

> ⚠️ **Hardware not tested.** Circuit designs and component recommendations are preliminary.

---

## Planned Data Flow

### Sensor Payload (JSON)

The ESP32 will send readings to `POST /api/readings`:

```json
{
  "deviceId": "esp32-living-room-01",
  "timestamp": "2026-07-27T10:30:00Z",
  "readings": {
    "voltage_v": 230.4,
    "current_a": 3.21,
    "power_w": 738.8,
    "energy_kwh": 0.012,
    "frequency_hz": 50.0,
    "power_factor": 0.97
  }
}
```

### Planned Zod Schema

```typescript
// Will be defined in lib/api-spec/openapi.yaml then generated
export const sensorReadingSchema = z.object({
  deviceId: z.string(),
  timestamp: z.string().datetime(),
  readings: z.object({
    voltage_v: z.number().positive(),
    current_a: z.number().nonnegative(),
    power_w: z.number().nonnegative(),
    energy_kwh: z.number().nonnegative(),
    frequency_hz: z.number().optional(),
    power_factor: z.number().min(0).max(1).optional(),
  }),
});
```

---

## Planned ESP32 Firmware

> Firmware will live in a separate `firmware/` directory (not yet created).

**Planned stack:**
- Language: **Arduino C++** or **MicroPython**
- Libraries: `WiFi.h`, `HTTPClient.h`, `ArduinoJson`
- Update mechanism: OTA (Over-the-Air) via Arduino OTA or ESP-IDF

**Sketch outline (pseudocode):**
```cpp
void loop() {
  float voltage  = readVoltage();
  float current  = readCurrent();
  float power    = voltage * current;

  String payload = buildJson(deviceId, voltage, current, power);
  httpPost("https://api.ecoswitch.ai/api/readings", payload);

  delay(READING_INTERVAL_MS); // e.g. 10000 (10 seconds)
}
```

---

## Security Considerations (Planned)

- The ESP32 will authenticate using a **device API key** stored in flash memory (not hardcoded in firmware source)
- HTTPS-only communication — the API server must serve TLS in production
- Device keys will be issued per-device and rotatable from the dashboard

---

## Development Without Hardware

For developing the IoT ingestion endpoint without an ESP32, a mock script will simulate readings:

```bash
# Planned: scripts/simulate-readings.ts
# Sends fake sensor payloads to the local API every 10 seconds
pnpm --filter @workspace/scripts run simulate-readings
```

---

## Milestones

| Milestone | Description | Status |
|-----------|-------------|--------|
| `POST /api/readings` endpoint | Accept and validate sensor payloads | 🔨 Planned |
| Device registration API | Register/deactivate devices | 🔨 Planned |
| ESP32 firmware sketch | Basic voltage/current reading + HTTP POST | 🔨 Planned (requires hardware) |
| Mock simulator script | Simulate readings in development | 🔨 Planned |
| OTA firmware updates | Push firmware updates from dashboard | 🔨 Future |
| MQTT support | Alternative to HTTP for high-frequency readings | 🔨 Future |
