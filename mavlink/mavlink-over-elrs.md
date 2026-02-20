## MAVLink Telemetry over ExpressLRS

## Overview

This drone uses **ExpressLRS CRSF passthrough** to transmit MAVLink telemetry over the same RF link used for RC control.

This setup removes the need for a separate telemetry modem.

---

## Architecture

QGroundControl → USB → ELRS TX → RF Link → ELRS RX → UART → Pixhawk

---

## Why This Setup

Advantages:

* One radio link only
* Long range telemetry
* Lower weight
* Reduced wiring complexity
* High reliability

---

## Key Limitation

Telemetry bandwidth is lower than dedicated telemetry radios (~70 kbit/s).

However, it is fully sufficient for:

* Parameter download
* Mission upload
* Live telemetry
* Logging

---

## Required Configuration

### ExpressLRS

Protocol: CRSF
Telemetry Ratio: 1:2
Packet Rate: 250 Hz

---

### PX4

TEL2_PROTO = MAVLink
TEL2_BAUD = 460800

---

## Connection Method

The ground station connects via:

USB cable to ELRS transmitter in VCP mode.

---

## Troubleshooting Notes

No telemetry → check baudrate and CRSF mode.

Telemetry drops → reduce packet rate.

---

## Future Improvements

* MAVLink routing via WiFi bridge
* Telemetry performance benchmarking
* Long range signal tests

