# MeshCore - Low power firmware for Heltec Lora 32 V3/V4 (esp32s3)
Optimized MeshCore firmware for Heltec V3/V4, engineered for low power consumption and extended battery life in off-grid scenarios.

Up to date with MeshCore dev branch, last update is 1/10/2026.

## *Typical power profile of Heltec V3 BLE companion, 5 LoRa messages in 30 seconds:*
* Maximum: 259 mA
* Minimum: 7.9 mA
* Mean: 34.55 mA

**Estimated ~57.89 h (2.41 days) with 2000mAh battery.**

<img width="1279" height="348" alt="companion-8080-high-traffic" src="https://github.com/user-attachments/assets/51354bcd-b056-41ff-890c-284dd8718715" />


## *Typical power profile of Heltec V3 BLE companion in idle:*
* Maximum: 116.77 mA
* Minimum: 8.65 mA
* Mean: 22.88 mA

**Estimated ~87.41 h (3.64 days) with 2000mAh battery.**

<img width="1279" height="348" alt="companion-8080-idle" src="https://github.com/user-attachments/assets/394bf183-09f7-45c7-b8a3-d891cc066e65" />


## *Typical power profile of Heltec V3 repeater in high LoRa traffic, 6 LoRa messages in 30 seconds*:
* Maximum: 126.35 mA
* Minimum: 5.2 mA
* Mean: 24.7 mA
  
**Estimated ~80.97 h (3.37 days) with 2000mAh battery.**

<img width="1279" height="348" alt="repeater-high-traffic" src="https://github.com/user-attachments/assets/cf92d506-c6cd-4374-ba21-be55a31a3a9e" />


## *Typical power profile of Heltec V3 repeater in idle:*
* Maximum: 39.08 mA
* Minimum: 8.6 mA
* Mean: 10.38 mA
  
**Estimated ~192.68 h (8.03 days) with 2000mAh battery.**

<img width="1279" height="348" alt="repeater-idle" src="https://github.com/user-attachments/assets/6ee80eae-3ed3-4805-8421-97d6c1a852e7" />


## Note: 
- Updated to MeshCore dev branch 2026-01-16.
- Improved battery measurement and management.
- Serial port will be deactivated after 30 seconds idle.
- ***No clock drifted problem on repeater firmware.***
