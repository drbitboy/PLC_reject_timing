# Suggested improvements

## Ca. 2023-07-19
- Clean up the organization
  - unrelated rungs are near each other
  - related rungs are separated and/or mis-ordered (see image below for an example).
- Several no-op rungs
  - they set bits or calculate values that are not used anywhere else
  - either their original intent should be realized, or they should be removed,
- Encapsulate complicated rungs and algorithms into User-Defined Function Blocks (UDFBs)
  - e.g. in the Encoder_inputs* routine,
    - the average linespeed calculation could be hidden in a UDFB, to make the primary flow easier to follow,
    - while the code in the UDFB could be spread across more rungs to make it easier to follow, if someone wanted to debug the logic.
- Refactor and/or provide better documentation for the scaling between LOSPEED_OFFSET_SP and HISPEED_OFFSET_SP
- Don't reuse single operator GUI/HMI EIP values for multiple purposes e.g. LOSPEED_OFFSET_SP can be
  - EITHER an index into the bitshift array for the encoder-driven logic (USE_ENCODER=1),
  - OR the time delay between an inspection fail event and the corresponding rejector pusher solenoid signal event for the timer-based logic (USE_ENCODER=0)
- Resolve duplicate destructive instructions
  - some variables' values are written in more than one place,
    - with the later overwriting the value from the earlier.
- Refactor the 112 timers in routine LAD_9_Reject_Timers to use the bitshift array
- Convert TONs that use TT_... bits for duration into TOFs
- Document the whole LINESTOP Normally Closed ecosystem, which breaks my brain every time I look at it.
- Check that all datatypes make sense and are consistent (UDINT, UINT, DINT, LINT, etc.)
- Replace [XIC bit CTU ...] instances with a single-input UDFB
- Handle the remaining edge case in the timer-based reject logic.
- Document shortcomings of the conversion to Micro820 without a Counter Interrupt capability, from the MicroLogix 1100 that used its Counter Interrupt
- Add a GUI/HMI alarm for missed encoder-based events

### Example of mis-ordered rungs referred to above
![](https://github.com/drbitboy/PLC_reject_timing/raw/master/versions/images/mis-ordered_rungs.png)
