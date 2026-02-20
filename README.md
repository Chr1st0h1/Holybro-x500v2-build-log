# Holybro X500 v2 PX4 Drone Documentation

This repository contains all documentation, configuration notes, and technical references for my **Holybro X500 v2 PX4-based quadcopter**.

The goal is to create a complete, long-term knowledge base for:

* Hardware configuration
* Wiring diagrams
* ExpressLRS integration
* MAVLink telemetry setup
* Flight tuning
* Troubleshooting

---

## 🚁 Drone Overview

**Frame:** Holybro X500 v2
**Flight Controller:** Pixhawk 6C Mini
**Firmware:** PX4 v1.16
**RC Link:** ExpressLRS 2.4 GHz
**Receiver:** RP3-H Diversity Receiver

---

## 📡 Key Feature: MAVLink over ExpressLRS

This drone uses a **single RF link** for both:

* RC control
* MAVLink telemetry

This eliminates the need for a separate telemetry radio.

See:

👉 `mavlink/mavlink-over-elrs.md`

---

## 📂 Repository Structure

| Folder   | Purpose                      |
| -------- | ---------------------------- |
| hardware | Wiring, mounting, components |
| software | PX4 and ELRS configuration   |
| mavlink  | Telemetry architecture       |
| tuning   | Flight tuning notes          |
| media    | Photos and diagrams          |
| logs     | Testing history              |

---

## 🧠 Future Documentation Goals

* Range testing data
* RF performance logs
* Autonomy mission setups
* Companion computer integration

---

## 📸 Media

Photos and diagrams will be added to:

```
/media/photos
/media/diagrams
```

---

## 🛠️ Author

Maintained by: Chr1st0h1
