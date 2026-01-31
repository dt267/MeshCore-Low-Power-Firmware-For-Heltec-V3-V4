# MeshCore - Low power firmware for Heltec Lora 32 V3 & WSL3 (esp32s3)
Optimized MeshCore firmware for Heltec V3/V4, engineered for low power consumption and extended battery life in off-grid scenarios.

## *Typical power profile of Heltec V3 BLE companion, 5 LoRa messages in 30 seconds:*
* Maximum: 246.3 mA
* Minimum: 7.2 mA
* Mean: 37.84 mA

**Estimated ~44.93 h (1.87 days) with 2000mAh battery.**

<img width="1277" height="348" alt="companion-high-lora" src="https://github.com/user-attachments/assets/0b027ad4-7dfe-4d08-a4e0-e1a65cd2aaea" />


## *Typical power profile of Heltec V3 BLE companion in idle:*
* Maximum: 115.9 mA
* Minimum: 7.7 mA
* Mean: 21.82 mA

**Estimated ~77.91 h (3.25 days) with 2000mAh battery.**

<img width="1277" height="348" alt="companion-idle" src="https://github.com/user-attachments/assets/20059c62-a493-4a4f-86d0-9e63a954ddcf" />


## *Typical power profile of Heltec V3 repeater in high LoRa traffic, 6 LoRa messages in 30 seconds*:
* Maximum: 163.8 mA
* Minimum: 4.1 mA
* Mean: 28.60 mA
  
**Estimated ~59.44 h (2.48 days) with 2000mAh battery.**

<img width="1277" height="348" alt="repeater-high-lora" src="https://github.com/user-attachments/assets/d69a26c6-7d97-4162-9823-566d3ed78168" />


## *Typical power profile of Heltec V3 repeater in idle:*
* Maximum: 35.5 mA
* Minimum: 5.2 mA
* Mean: 7.27 mA
  
**Estimated ~233.84 h (9.74 days) with 2000mAh battery.**

<img width="1277" height="348" alt="repeater-idle" src="https://github.com/user-attachments/assets/9ec41771-8727-4072-ac7b-656502002254" />


## Note: 
- [Updated to MeshCore dev branch 2026-01-31](https://github.com/dt267/MeshCore-Low-Power-Firmware-For-Heltec-V3-V4/releases/tag/Heltec-V3-WSL3-low-power-v1.12_0131)
- Improved battery measurement and management.
- Serial port will be deactivated after 30 seconds idle.
- ***No clock drifted problem on repeater and room server firmware.***
- LoRa Tx is 22dBm at dummy load.
- T_hours = 2000 * 0.85 / I_mean
- [Power Profiles of the Original MeshCore Firmware for Heltec V3](https://github.com/dt267/MeshCore-Low-Power-Firmware-For-Heltec-V3-V4/blob/main/Power%20Profiles%20of%20the%20Original%20MeshCore%20Firmware%20for%20Heltec%20V3.md#power-profiles-of-the-original-meshcore-firmware-for-heltec-v3-111dev)
