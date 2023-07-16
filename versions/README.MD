# Versions of the program
- Conveyor_Reject_Micro820-QBB_230714.ccwarc
  - First cut
  - No HSC code yet
  - All variables to be interfaced with the GUI/HMI are in Globabl Variables with their original names
    - e.g. LOSPEED_OFFSET_SP not N17:1

- Test harness; 2023-07-15
  - IO_Check_Latchmoor_Micro820.ccwarc
  - MicroLogix_1100_driver_of Micro8xx_2080-MOT-HSC.RSS
  - Connect O/2 on MicroLogix to A+ on Micro820
  - Connect O/2 Common to A- (and to ground) on Micro820
