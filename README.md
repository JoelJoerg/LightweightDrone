# LightweightDrone

**This is a work in progress.**
**Unfinished schematics**

## Concept

A small and lightweight drone.

Goals:
- Cost effective.
- Components are easy to source.
- Compatible with 2.4Ghz commercially available remotes.
- Stability control.

Personal goals:
- Creative outlet.
- Better understanding of component limitations, availability and cost.
- Take risk with design ideas at my own cost.
- Explore challenges in programming, connectivity, sensing and control systems.



## Design considerations

### Drone EU classifications
This self-made drone falls in the C0 class.
- m < 250g
- h_max < 120m (above starting altitude).
- v_max < 19m/s

### Main Components
- 3.7V LiPo 300mAh 
- 4 x coreless DC Motors
- µC: STM32G431x8, 64 pins
- 6 Axis IMU: BMI088
- Pressure sensor: LPS22DF
- RF-Module: nRF24L01
  
For the next iteration:
- Camera module
- Distance-to-ground sensor
- ....

### Do I need a MOSFET Driver?

Can the circuit provide enough current to fully charge/discharge the parasitic capacitance (gate charge) and therefore have time to fully conduct at the specified frequency? 

f = 20kHz is chosen to avoid audible switching noise --> T = 50µs. For good measure, let's take T/4 as our charge/discharge time. The gate charge of the IRLZ44N is 48nC.  I = Q/T {A = A*s / s} --> I = 48nC/12.5µs = 3.84mA.
If we drive the MOSFET directly through the octocoupler, it would need to withstand/provide 3.84mA. Same for the discharge resistor from gate to ground (R = 3.7V / 3.84mA = ~1000 Ohm).

I am concerned about power losses with small resistors (14mW with 3.84mA and 3.7V).

My experience is very limited on this topic and I will go forward without a driver to see what happens. I intend to experiment with different resistor values.

## Schematics
![image](https://github.com/user-attachments/assets/a70238b1-1599-41a0-93a1-a771661191e8)
![image](https://github.com/user-attachments/assets/ed4fb1e3-10e3-4206-8926-4ddf1daf6398)
![image](https://github.com/user-attachments/assets/0a80a9c3-3a5e-4272-a443-f21bdbcd8df1)


