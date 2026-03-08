# ESC Configuration: AM32 Firmware for T-Motor Air Gear KV920

This section details the electronic speed controller (ESC) configuration for the **T-Motor Air Gear** (2213/2216 KV920) motors using the **AM32 Configurator**. These settings are optimized for a stable heavy-lift performance on the **Holybro x500v2** frame controlled by a **Pixhawk 6C Mini**.

## Hardware Overview
*   **Frame:** Holybro x500v2
*   **Flight Controller:** Pixhawk 6C Mini (PX4 Firmware)
*   **Motor:** T-Motor Air Gear KV920 (9N12P)
*   **ESC Firmware:** Tekko32_4IN1_F421
*   **ESC Firmware Version:** 2.20

## AM32 Configuration Settings


| Parameter | Recommended Value | Notes |
| :--- | :--- | :--- |
| **Motor Poles** | **12** | Based on the 9N12P configuration. Essential for correct RPM telemetry. |
| **Motor KV** | **920** | Matches the motor specification for optimal commutation. |
| **Input Protocol** | **Auto** | Automatically detects DShot or PWM signals from the Pixhawk. |
| **Timing Advance** | **15° - 22.5°** | Start at 15°. Increase slightly if you experience desyncs at high throttle. |
| **PWM Frequency** | **24kHz - 48kHz** | 24kHz provides more "punch"; 48kHz is smoother and more efficient. |
| **Startup Power** | **50% - 70%** | Provides enough torque to spin up 10-inch props reliably. |
| **Brake on Stop** | **Disabled** | Recommended for multirotors to avoid mechanical stress during decelerations. |

## PX4 Integration & Bi-Directional DShot

To ensure the Pixhawk communicates correctly with the AM32 ESCs and provides RPM data for the Dynamic Notch Filter, configure the following in **QGroundControl**:

### 1. PX4 Parameters
*   **`DSHOT_CONFIG`**: Set to **DShot300** (recommended for stability on 500-class frames).
*   **`DSHOT_TEL_CFG`**: Set to **1 (Sustained)** to enable RPM telemetry.
*   **`MOT_POLE_COUNT`**: Set to **12** to ensure RPM calculations match the motor hardware.

### 2. Hardware Mapping
Ensure ESCs are connected to the **FMU PWM Out** ports on the Pixhawk 6C Mini. Check that all motor ports belong to the same timer group to support DShot consistently across all channels.

## Safety & Testing
1.  **Remove Propellers:** Always perform configuration and initial motor spins without props.
2.  **Current Limiter:** Use a "smoke stopper" during the first power-up.
3.  **Validation:** Run `dshot status` in the Mavlink Console to verify that RPM data is being received.
