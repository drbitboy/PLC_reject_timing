## Demonstration of edge case

Original timer-based, MicroLogix 1100 code has a bug in the timer-based
reject logic where, if a second failed-can signal arrives either on the
same scan cycle that a previous fail-can cycle's TON (Timer ON-delay) is
expiring, or on the next scan cycle after that, then the reject signal
for that second failed-can is either missed, or is issued at the time of
the failed can signal.

The code here explores the bug, and shows two ways to remove it

- lecky_demo.RSS
- lecky_demo.pdf
  - Demonstration of the TON-based bug
    - Timer-based reject logic is functionally identical to the original
      - The rest of the original program has been removed for clarity
      - Provides only three TONs, instead of the 112 in the original
  - Use RSLogix 500 in Online mode to run the demo
    - Refer to the comments for details
    - Failed-can emulation is controlled via bits B3:0/8 through B3:0/11
    - Observe the behavior of L9:0 to see where the bug occurs
      - L9:0 counts scan cycles where the reject bit one-shot is 1

- lecky_demo_tof.RSS
- lecky_demo_tof.pdf
  - A TOF-based (Timer OFf-delay) solution that removes the bug
  - Controls are the same as for lecky_demo.* above
  - L9:0 counts scan cycles where the failed-can one-shot is 1
  - L9:1 counts scan cycles where the reject bit one-shot is 1

- lecky_demo_tof_loop.RSS
- lecky_demo_tof_loop.pdf
  - A TOF-equivalent-based solution that removes the bug
  - Implements TOF-like logic using atomics (contacts, coils, etc.)
  - Puts 112 executions of the TOF-like logic in a loop
    - So only a handful of rungs are required, instead of 224
  - Adds a new failed-can emulation bit B3:0/12
    - Setting this bit to 1 will trigger a failed-can at ~20Hz (10k/512)
      - Which will in turn activate over 100 of the reject timers

- README.md
  - This file
