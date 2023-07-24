# PLC_reject_timing

## Exported CCW code is under /versions/ subdirectory

## Model Assumptions
- PLC programming is primarily about time

- No Slip
  - Encoder to Motor:  calculated/estimated encoder PPM is proportional to motor RPM
  - Motor to Conveyor:  motor RPM is proportional to conveyor linespeed speed, inch/s
  - Conveyor to Failed Can:  failed-can speed is equal to linespeed
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
    - Camera timing is independent of linespeed
    - There is a positive time delay, ΔTinspect, between the time the prox sensor detects a can entering the fixed position of the inspection until the camera returns a failed-can signal to the PLC
    - That time delay ΔTinspect, plus the linespeed, determines the position on the conveyor that is modeled by the first (right-most) bit in the bit-shift array.
      - That position modeled by the first bit determines the distance, and number of bit-shift array elements modeled, a failed can must traverse to get to the reject pusher.
  - Pusher
    - Pusher movement and timing are independent of linespeed
    - There is a positive time delay, ΔTpusher, between the reject signal from the PLC and the moment when the reject pusher initially contacts a failed can
    - The reject signal from the PLC must be sent at that ΔTpusher offset time before a failed can reaches the position of the pusher's initial contact with the failed can

- Adequate resolution in the PLC encoder counting and bit-shift array

## Implications of No Slip and Fixed Geometry model assumptions
- The distance a failed can travels between when the PLC receiving the failed-can input signal, and when the reject signal from the PLC should be sent, is equal to
  - the distance from the inspection trigger prox to where the pusher's initially contacts the fail can,
  - minus the product ((ΔTpusher + ΔTinspect), s ×linespeed, inch/s)
  - and that shortened distance is what is modeled in the bit-shift array elements' positions, from element 0 to some element N.

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

## Manifest

- original/
  - Original code and documentation
  - Annotated (i.e. enhanced comments) version of the original code

- versions/
  - Versions of ports of original MicroLogix 1100/RSLogix 500 code to Micro820/CCW, plus some other test code
  - See versions/README.md for details

- edge_case/
  - Demonstration of MicroLogix 1100/RSLogix 500 edge cases in failed-can signal timing that cause original code to fail
  - Also two solutions
  - See edge_case/README.md, plus comments in code, for details
