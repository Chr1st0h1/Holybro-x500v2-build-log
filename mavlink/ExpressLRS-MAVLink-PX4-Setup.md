# ExpressLRS MAVLink Setup on Holybro Pixhawk 6C Mini with PX4 v1.16

This guide describes a working configuration for **bidirectional MAVLink** (RC control uplink + full telemetry downlink) using **ExpressLRS** on a Holybro Pixhawk 6C Mini flight controller running PX4 v1.16.1 rc. Tested with:

- Transmitter: Radiomaster Pocket (with ELRS module and Backpack for WiFi/UDP forwarding)
- Receiver: RP3-H ExpressLRS 2.4 GHz Nano, connected to **TELEM1** (UART)
- Firmware: PX4 v1.16.1 rc (stable/latest at time of writing)
- ExpressLRS: Latest stable firmware (≥3.5.x) using **MAVLink protocol** (not CRSF)

**Goal**: RC sticks working, arming successful, telemetry (attitude, battery, GPS, RSSI/LQ) visible in QGroundControl (QGC) via USB or WiFi from the Pocket.

## Hardware Connections

- RP3-H Nano RX → Pixhawk 6C Mini TELEM1:
  - TX (from RX) → TELEM1 RX
  - RX (from RX) → TELEM1 TX
  - GND → GND
  - 5V → 5V (or use external BEC if needed)

## ExpressLRS Configuration (on Radiomaster Pocket via Lua Script)

1. Update ELRS TX module and RX to the latest stable firmware (via ELRS Configurator).
2. Bind the RX to the TX.
3. In the ELRS Lua script:
   - **Link Mode / Protocol** → **MAVLink** (or MAVLink v4 if available)
   - **Packet Rate** → 250 Hz or 333 Hz Full (lower for more stable telemetry, higher for lower latency)
   - **Telemetry Ratio** → 1:8 or 1:16 (lower number = more downlink data)
   - **TX Power** → Adjust as needed (max 1 W for range, but watch heat/regulations)
4. Optional: Backpack → Enable **WiFi Telemetry** for UDP connection to QGC (SSID/password configurable via Lua).

## PX4 Parameters (via QGroundControl)

Search for and set the following parameters (only the changed/essential ones for MAVLink on TELEM1). After changes: **Save** and **reboot** the flight controller.

| Parameter              | Value     | Description |
|------------------------|-----------|-------------|
| **MAV_0_CONFIG**      | 101       | Assigns MAVLink instance 0 to TELEM1 (port ID 101 on Pixhawk 6C Mini) |
| **SER_TEL1_BAUD**     | 460800    | Baud rate must match ELRS MAVLink (460800 8N1) |
| **MAV_0_RATE**        | 9600      | Telemetry rate in bytes/s (9600 works well for ELRS; try 8000–12000) |
| **MAV_0_MODE**        | 0         | Normal mode (default) |
| **MAV_0_RADIO_CTL**   | 1         | Enable RC control over MAVLink |
| **MAV_0_FORWARD**     | 1         | Enable forwarding (for external GCS) |
| **MAV_0_FLOW_CTRL**   | 2         | Software flow control (recommended for ELRS) |

**Other relevant defaults (no change needed, but verify):**
- COM_RC_IN_MODE = 3 (Joystick/MAVLink RC input priority)
- RC_CHAN_CNT = 16 (for full ELRS channel count)
- RSSI/LQ: Automatically handled via MAVLink (no RSSI_TYPE parameter in v1.16)

**Note about SR0_ parameters**: In PX4 v1.16+ the SR0_* (stream rate) parameters no longer appear in QGC. Telemetry is automatically sent based on MAV_0_RATE. To get more/less data: simply increase or decrease MAV_0_RATE and test for packet loss.

## QGroundControl Connection

1. **Via USB (easiest for testing):**
   - Connect the Radiomaster Pocket to your PC via USB.
   - In QGC: Add Vehicle → Serial → select the COM port of the Pocket → Baud rate: 460800.
   - Heartbeat appears → telemetry starts flowing.

2. **Via WiFi (Backpack):**
   - Enable WiFi Telemetry mode on the Backpack.
   - In QGC: Add Vehicle → UDP → port 14550 → connect to the IP address of the Pocket (usually in 192.168.x.x range).

## Test Steps

1. Arm the drone (via sticks or RC switch) → do the motors spin up?
2. In QGC:
   - Radio tab: do the sticks move?
   - HUD: attitude, battery voltage, GPS status, LQ/RSSI visible (in the top radio indicator)?
   - MAVLink Inspector (Analyze tools): check for RADIO_STATUS messages containing RSSI/LQ?
3. Backpack web interface (open browser to Pocket IP): do you see uplink/downlink packet counters increasing?

## Troubleshooting Tips

- No RC input → Verify ELRS Link Mode = MAVLink, baud rate = 460800, binding is active.
- No telemetry → Increase MAV_0_RATE to 12000, lower Telemetry Ratio in ELRS Lua script.
- Packet loss → Try lower packet rate (250 Hz), test closer range first.
- QGC not connecting → Reboot everything, refresh parameters in QGC (Tools menu).

Happy flying with your Holybro X500v2 build!

Last updated: March 2026 (based on PX4 v1.16.1 rc and latest stable ExpressLRS).
