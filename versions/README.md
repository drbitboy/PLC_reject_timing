# Versions of the program

## 20230722_Conveyor_Reject_Micro820-QBB.ccwarc
- Full working port of reject code from MicroLogix 1100/RSLogix 500 to Micro820/CCW
  - Updated ~2023-07-22:18450 EDT
  - Convert LAD_9_Reject_Timers to use TOFs and F_TRIGs instead of TONs

## 20230721_Conveyor_Reject_Micro820-QBB.ccwarc
- Latest version is in commit [0d64e72](https://github.com/drbitboy/PLC_reject_timing/tree/0d64e72e23b392d15845c7aed1f3b85f47fde02a/versions)
- Full working port of reject code from MicroLogix 1100/RSLogix 500 to Micro820/CCW
  - Updated ~2023-07-21:02:00 EDT
  - Added GUI_* globals, written by GUI/HMI
  - Cans HSC always runs, not dependent on USE_ENCODER/HSC_ENABLE
  - Global HSC_TRIGGER_COUNT for access by GUI/HMI
  - Left HSC rollover at (1<<46)
  - Improved encoder bit-shift dwell outputs to avoid multiple rejects
  - Refactored two LAD 4 routines into several subroutines by function
  - Rearranged global variables
  - Improved comments

## 20230719_MicroLogix_1100_driver_of_Micro8xx_2080-MOT-HSC.RSS
- Updated version of MicroLogix 1100 program to emulate encoder pulses using PTO (Pulse Train Output)

## 20230719_Conveyor_Reject_Micro820-QBB.ccwarc
- Latest version is in commit [c21a045](https://github.com/drbitboy/PLC_reject_timing/tree/c21a0455491781730c861738f53658d103c28c1a/versions)
- Full working port of reject code from MicroLogix 1100/RSLogix 500 to Micro820/CCW
  - Updated ~2023-07-19T17:10:00 EDT
    - Add global count DINTs to CTUs' CV pins in routine LAD_5_IO_COUNTER
    - Made the reset count command reset the HSCs in routine Encoder_inputs
  - Updated ~2023-07-19T10:30:00 EDT
    - Fixed the zero-valued PVs in LAD_5_IO_COUNTER
  - Updated ~2023-07-19T10:30:00 EDT
    - Removed the restriction on encoder pulse frequencies above 1183Hz
    - Default configuration:
      - USE_ENCODER = 0
        - Runs a time-based reject model
      - LOSPEED_OFFSET_SP = 1700
        - bit array index for zero speed (theoretical)
        - For time-based model, reject solenoid signal fires
          - ~1700ms (1.7s) after inspection fail event
        - For encoder-based model, reject solenoid signal fires
          - ~(1700*25) encoder pulses after inspection fail event
            - at encoder pulse frequency of 0Hz (theoretical)
            - = (LOSPEED_OFFSET_SP*ENCODER_HIPRESET_SP)
          - ~(990*25) encoder pulses after inspection fail event
            - at encoder pulse frequency of 1183Hz (linespeed = 710 ???-per-minute)
            - = (HISPEED_OFFSET_SP*ENCODER_HIPRESET_SP)
      - HISPEED_OFFSET_SP = 990
        - bit array index for 710 "linespeed" ???-per-minute
          - ≡encoder pulse frequency of 1183.333Hz
      - ENCODER_HIPRESET_SP = 25
        - Pulse/frequency divisor, sets number of pulses per bit array element
        - 2000Hz/(25pulse/element) = 80element/s
      - REJECTOR_DURATION_SP = 75
        - Reject pusher solenoid dwell time, ms

## 20230718_Conveyor_Reject_Micro820-QBB.ccwarc
- Latest version is in commit [f0c86c3](https://github.com/drbitboy/PLC_reject_timing/tree/f0c86c3f046cd207cc0be627180190901462b6c4/versions)
- Full working port of reject code from MicroLogix 1100/RSLogix 500 to Micro820/CCW
  - Updated per emails from NL, more or less
    - IP address of Micro820 is final 192.168.0.102 on network 192.168.0.0/24
    - Added High-Speed Counters (HSCs), both for encoder pulses and for cans
      - 70Tencoder-count rollover (1.1ky+ @ 2kHz)
      - Automatic re-configuration of HSCE objects on an HSC error
      - Pulse/Frequency divider in place (ENCODER_HIPRESET_SP)
      - Warning if pulse-divided edges are closer than two scan cycles
      - Linespeed calculation is exponentially filtered instead of using a moving average

## 20230716_Conveyor_Reject_Micro820-QBB.ccwarc
- Latest version is in commit [ea8e632](https://github.com/drbitboy/PLC_reject_timing/tree/ea8e6328a08dc2eef8679c0d985ab3c060da7e83/versions)
- Second cut at port of reject code from MicroLogix 1100/RSLogix 500 to Micro820/CCW
  - Updated per email from NL ca. 2023-07-16
    - IP address of Micro820 is now 192.168.0.102 on network 192.168.0.0/23
    - Added PVs to CTU instruction in LAD 5

## 20230715_IO_Check_Latchmoor_Micro820.ccwarc and 20230715_MicroLogix_1100_driver_of_Micro8xx_2080-MOT-HSC.RSS
- Latest versions are in commit [ea8e632](https://github.com/drbitboy/PLC_reject_timing/tree/ea8e6328a08dc2eef8679c0d985ab3c060da7e83/versions)
- Test harness
  - Connect O/2 on MicroLogix to A+ on Micro820
  - Connect O/2 Common to A- (and to ground) on Micro820

## 20230714_Conveyor_Reject_Micro820-QBB.ccwarc
- Latest version is in commit [d2f297e](https://github.com/drbitboy/PLC_reject_timing/tree/d2f297ec6200262c4a4a4fb18605095bc3468c88/versions)
- First cut at port of reject code from MicroLogix 1100/RSLogix 500 to Micro820/CCW
  - No HSC code yet
  - All variables to be interfaced with the GUI/HMI are in Global Variables with their original names
    - e.g. LOSPEED_OFFSET_SP not N17:1

## TODO.md
- Suggestions for future work

## README.md
- This file
