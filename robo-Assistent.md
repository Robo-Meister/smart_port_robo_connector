# 🤖 Robo Assistent (Optional)

Robo Assistent is an optional OS for routers and hubs that can run alongside Smart Port Logistics. It orchestrates local devices to extend workflows with on-site checks and automation. This document outlines suggested drivers, extra workflows, and command rules for turning workflow steps into device actions.

---

## ✅ Suggested Drivers (Common Devices)

- **Gate & Access Control**: barriers, RFID readers, badge scanners, ALPR cameras
- **Positioning & Tracking**: UWB anchors/tags, BLE beacons, GNSS receivers
- **Container & Yard Sensors**: temperature, humidity, vibration, door open/close, weight scales
- **Visual Inspection**: IP cameras, OCR scanners, barcode/QR readers
- **Safety & Compliance**: safety light curtains, proximity sensors, emergency stop signals
- **Vehicle Telemetry**: OBD-II, CAN bus gateways, tire pressure sensors

---

## 🧠 Extra Workflows (Robo Connector + Robo Assistent)

- **Life Gate Monitoring**
  - Validate gate open/close state before allowing yard access.
  - Verify credential scan and capture a confirmation image.

- **Container Parameter Verification**
  - Confirm temperature or humidity thresholds before release.
  - Validate door seal and door-open events for tamper detection.

- **Position & Proximity Checks**
  - Ensure the vehicle is within the correct bay or staging zone.
  - Confirm distance between vehicle and container meets safe tolerance.

- **Fleet Entry Validation**
  - Allow or block entry based on vehicle ID, driver credentials, or time window.
  - Enforce automatic retry/timeout rules for failed scans.

- **Customs/Compliance Gate**
  - Require OCR plate match plus badge scan before moving forward.
  - Log evidence data to the workflow record.

---

## 🧩 Command Rules (Workflow → Device Actions)

Use these rules to convert workflow steps into device actions when Robo Assistent is enabled.

### 1) Step-to-Device Mapping
- Each workflow step should map to one or more device actions:
  - Example: `verify-container-position` → UWB tag read + zone validation
  - Example: `gate-entry` → badge scan + barrier open

### 2) Required Capabilities
- A step must declare the device capabilities it needs:
  - `positioning` (UWB/GNSS/BLE)
  - `identity` (RFID/OCR/QR)
  - `environment` (temp/humidity/door)
  - `safety` (light curtain/proximity)

### 3) Relationship Rules (Driver ↔ Object)
- Define the relation between the driver/vehicle and target object:
  - **Distance tolerance**: e.g., within 1–3 meters
  - **Position zone**: e.g., bay A3, lane 2
  - **Orientation**: e.g., vehicle aligned within 15°

### 4) Validation & Tolerances
- Each check must specify acceptable ranges and thresholds:
  - Temperature range (e.g., 2–8°C)
  - Door state (open/closed)
  - Weight range (±3%)

### 5) Retry, Timeout, and Escalation
- Define retries and timeouts per step:
  - `maxRetries`: e.g., 3
  - `timeoutSeconds`: e.g., 60
  - `escalateOnFailure`: notify ops or block gate

### 6) Evidence & Logging
- Capture device evidence when required:
  - image snapshot, sensor reading, or scan payload
  - attach evidence to the workflow record

---

## 🔌 Notes for Future Integration

- Driver adapters and device command definitions will be added later.
- This document is a placeholder to standardize how future integrations should behave.
