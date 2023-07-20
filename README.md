# PLC_reject_timing

## Exported CCW code is under /versions/ subdirectory

## Model Assumptions
- PLC programming is primarily about time

- No Slip
  - Encoder to Motor:  calculated/estimated encoder PPM is proportional to motor RPM
  - Motor to Conveyor:  motor RPM is proportional to conveyor speed, m/s
  - Conveyor to Failed Can:  failed-can speed is equal to conveyor speed
  - Failed-Can to BSL Index:  the movement of a modeled failed-can's 1-bit index in the
                                                bit-shift array is proportional to can position

- The PLC only tracks the position of failed cans;
  - a passed can is no different than no can i.e. than an empty spot on the conveyor
- Fixed Geometry
  - Constant distance
    - from the position of a can at the camera inspection station at the time of that can's fail detection,
    - to the position of a failed can where the reject pusher initially contacts that can to reject it

- Fixed Time Dynamics
  - Inspection camera
    - Camera timing relative to inspection proximity sensor will be variable, but stochastic with a consistent mean and a small variance
    - There is a positive time delay, ΔTinspect, between the time the prox sensor detects a can entering the fixed position of the inspection until the camera returns a failed-can signal to the PLC
  - Pusher
    - Pusher movement and timing are independent of conveyor speed
    - There is a positive time delay, ΔTpusher, between the reject signal from the PLC and the moment when the reject pusher initially contacts a failed can
  - The reject signal from the PLC must be sent at that (ΔTpusher + ΔTinspect) offset time before a failed can reaches the position of the pusher's initial contact with the failed can

- Adequate resolution in the PLC encoder counting and bit-shift array

## Implications of No Slip and Fixed Geometry model assumptions
- The position of a failed can when the reject signal from the PLC is equal to
  - the position of the pusher's initial contact with the fail can,
  -  minus the product ((ΔTpusher + ΔTinspect), s ×can speed, m/s)

- The value of the index in the bit-shift array where a 1-bit should trigger the reject
   signal from the PLC is linear with the calculated RPM, so
  - IF we know, or can empirically determine
    - the highest and lowest operational speeds of the motor
    - the two indices in the bit-shift array at which to trigger the reject signal at those
      two speeds
      - HISPEED_OFFSET_SP for the highest operational speed of the motor
      - LOSPEED_OFFSET_SP for the lowest operational speed of the motor
  - THEN
    - a linear model in the PLC can calculate the index of bit-shift array to observe to trigger the reject signal
      - the linear model is based on
         - Measured motor speed (from encoder counts vs. time)
         - highest and lowest operational speeds of the motor
         - HISPEED_OFFSET_SP and LOSPEED_OFFSET_SP
