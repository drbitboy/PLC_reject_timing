# Versions of the program

## 20230719_Conveyor_Reject_Micro820-QBB.ccwarc
- Full working port of reject code from MicroLogix 1100/RSLogix 500 to Micro820/CCW
  - Removed the restriction on encoder pulse frequencies above 1183Hz
  - Default configuration:
    - LOSPEED_OFFSET_SP = 1700
      - bit array index for zero speed (theoretical)
    - HISPEED_OFFSET_SP = 990
      - bit array index for 710 "linespeed" ???-per-minute
        - ≡ encoder pulse frequency of 1183.333Hz
    - ENCODER_HIPRESET_SP = 25
      - Pulse/frequency divisor, sets number of pulses per bit array element
      - 2000Hz/(25pulse/element) = 80element/s
    - REJECTOR_DURATION_SP = 75
      - Reject pusher solenoid dwell time, ms

## 20230718_Conveyor_Reject_Micro820-QBB.ccwarc
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
- Next cut at port of reject code from MicroLogix 1100/RSLogix 500 to Micro820/CCW
  - Updated per email from NL ca. 2023-07016
    - IP address of Micro820 is now 192.168.0.102 on network 192.168.0.0/23
    - Added PVs to CTU instruction in LAD 5

## 20230715_IO_Check_Latchmoor_Micro820.ccwarc and 20230715_MicroLogix_1100_driver_of_Micro8xx_2080-MOT-HSC.RSS
- Test harness; 2023-07-15
  - Connect O/2 on MicroLogix to A+ on Micro820
  - Connect O/2 Common to A- (and to ground) on Micro820

## 20230714_Conveyor_Reject_Micro820-QBB.ccwarc
  - First cut at port of reject code from MicroLogix 1100/RSLogix 500 to Micro820/CCW
  - No HSC code yet
  - All variables to be interfaced with the GUI/HMI are in Global Variables with their original names
    - e.g. LOSPEED_OFFSET_SP not N17:1

## README.MD
- this file
