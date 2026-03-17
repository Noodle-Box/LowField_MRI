# RF Transmission System for LF-MRI

## Overview

This repository contains the embedded firmware for the **Excitation Transmission System** of the Low-Field MRI (LF-MRI) prototype developed under the collaboration between the **UQ Centre for Advanced Imaging (CAI)** and **UQ School of Human Movement and Nutrition Sciences (HMNS)**, Project P4.

The firmware programs a STM32G474 MCU to generate a RF excitation waveform using a Direct Digital Synthesizer (DDS) and a DAC. 
As illustrated below, this RF excitation signal gets amplified through coils and excites the subject's protons 

![RF_Diagram](https://github.com/user-attachments/assets/9bd18730-1a09-4576-984f-9a661e3f9c34) 
<img width="1287" height="1811" alt="Circuit_Hardware_Detail" src="https://github.com/user-attachments/assets/e294b880-c123-4c2f-9c39-6fc3a81af75d" />


Source .C files in:
LF_MRI_V3 --> Core --> Src

Header .H files in:
LF_MRI_V3 --> Core --> Inc

---

## SPI Hardware Configuration

| SPI Bus | Device      | RF Signal Function       |
|---------|-------------|--------------------------|
| SPI1    | AD5689 DAC  | Envelope component
| SPI2    | AD9834 DDS  | Carrier component        |

---

## Hardware GPIO Pinouts

![STM32_MCU](https://github.com/user-attachments/assets/8695ef6d-55cd-43a2-8057-9a378b42958e)

# DDS GPIO Pinouts
| Pin      | Function         | Description                                           |
|----------|------------------|-------------------------------------------------------|
| GPIOC 6  | DDS 1 FSYNC      | SPI frame signal for the input data of DDS IC 1       |
| GPIOC 7  | DDS 2 FSYNC      | SPI frame signal for the input data of DDS IC 2       |
| GPIOB 13 | SPI 2 SCK        | Serial clock input. DDS clocked on falling edge       |
| GPIOB 15 | SPI 2 MOSI       | Serial input data from MCU. Contains parameters       |
| GPIOC 2  | DDS RESET INT    | DDS Output signal toggle for EXT/INT control          |
| GPIOC 3  | DDS RESET SELECT | DDS control toggle. LOW is INT. HIGH is EXT (console) |

# DAC GPIO Pinouts
| Pin      | Function         | Description                                            |
|----------|------------------|--------------------------------------------------------|
| GPIOA 4  | DAC SYNC         | SPI frame signal for the input data of DAC IC          |
| GPIOB 6  | DAC LDAC         | Register updates for DAC. Used for pulsing edge        |
| GPIOA 5  | SPI 1 SCK        | Serial clock input. DAC clocked on falling edge        |
| GPIOB 5  | SPI 1 MOSI       | Serial input data from MCU. Contains LUT output        |
| GPIOC 0  | DAC RESET INT    | DAC output signal toggle for EXT/INT control           |
| GPIOC 1  | DAC RESET SELECT | DACS control toggle. LOW is INT. HIGH is EXT (console) |

# General & Other GPIO Pinouts
| Pin      | Function         | Description                                        |
|----------|------------------|----------------------------------------------------|
| GPIOA 11 | USB Positive (P) | Positive USB differential data line. Used for CDC  |
| GPIOA 12 | USB Negative (N) | Negative USB differential data line. Used for CDC  |
| GPIOA 13 | SYS SWDIO        | Data line for Serial Wire Debug (SWD) Interface    |
| GPIOA 14 | SYS SWCLK        | Clock line for Serial Wire Debug (SWD) Interface   |

---

## Quickstart Guide

1. **Clone the Repository**
    ```bash
    git clone https://github.com/Noodle-Box/LF-MRI.git
    ```

2. **Open with STM32CubeIDE**
    - Import the project as an existing STM32CubeIDE workspace.
    - Ensure your target board is connected via J-Link

3. **Modify Output Parameters**
    - In `main.c`:
      
      STEP 1: Go to "Initialization of DDS Circuit" and input desired DDS waveform frequency (freq) and phase
      
      ![DDS_Instruction](https://github.com/user-attachments/assets/15e81f96-7363-4a80-bb6c-fe9e16849a06)

      STEP 2: Go to "Timer interrupt for DAC output" and select desired DAC waveform

      ![DAC_Instruction](https://github.com/user-attachments/assets/f3f6dacd-663f-4aff-a943-84a2fc628733)

      STEP 3: Build and flash the board :)



    >OPTIONAL: If DAC peripheral not booting up properly, go to "Initialization of DAC Circuit" and uncomment AD5689_Reset_Init()
     
      ![DAC_Helper](https://github.com/user-attachments/assets/666238ba-38c0-41bc-9f56-2d7af983adfe)
  
---

## Contributors

- **Tevyn Vergara**
  
---

## Acknowledgements

This project is part of the UQ CAI x HMNS collaborative initiative under the Low-Field MRI research stream (P4).
