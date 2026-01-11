# MeshCore - Low power firmware for Heltec Lora 32 V3/V4 (esp32s3)
Optimized MeshCore firmware for Heltec V3/V4, engineered for low power consumption and extended battery life in off-grid scenarios.

Up to date with MeshCore dev branch, last update is 1/10/2026.

Typical power profile of Heltec V3 BLE companion, 7 LoRa messages in 30 seconds:
- Maximum: 274.89 mA
- Minimum: 6.18 mA
- Mean: 34.85 mA
Estimated ~57.39 h (2.39 days) with 2000mAh battery.
<img width="1280" height="348" alt="V3_BLE_dev_0111" src="https://github.com/user-attachments/assets/6b04f63e-3ee7-49d8-a254-0b5e7531b197" />


Typical power profile of Heltec V3 BLE companion in idle:
- Maximum: 127.28 mA
- Minimum: 6.74 mA
- Mean: 23.50 mA
Estimated ~85.11 h (3.55 days) with 2000mAh battery.
<img width="1280" height="348" alt="V3_BLE_dev_idle_0111" src="https://github.com/user-attachments/assets/18efa14d-fd48-4b60-8c5e-e844732af511" />


Typical power profile of Heltec V3 repeater in high LoRa traffic, 6 LoRa messages in 30 seconds:
- Maximum: 124.7 mA
- Minimum: 6.64 mA
- Mean: 22.67 mA
Estimated ~88.22 h (3.68 days) with 2000mAh battery.
<img width="1279" height="348" alt="V3_repeater_high_lora_traffic_0111" src="https://github.com/user-attachments/assets/de824957-d6f5-4c41-8f3b-e621231195ea" />


Typical power profile of Heltec V3 repeater in idle:
- Maximum: 43.82 mA
- Minimum: 7.31 mA
- Mean: 10.12 mA
Estimated ~197.68 h (8.24 days) with 2000mAh battery.
<img width="1279" height="348" alt="V3_repeater_dev_idle_0111" src="https://github.com/user-attachments/assets/6ce903c2-ea9f-4a15-b963-f88b630e0af8" />


Note: 
- Serial port will be deactivated after 30 seconds idle.
- No clock drifted problem on repeater firmware.
