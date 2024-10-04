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

### Power supply 

I use a 3.7V LiPo battery with integrated protection circuit and generically placed a LD1117S25 to regulate the voltage from the LiPo down to 2.5V, which all components support as input voltage.

While working on the SWD programming connection with the ST-Link from a Nucleo board, I had to change the system voltage from 2.5V to 3.3V. Unfortunately the ST-Link minimum voltage is 3V (It uses the external applications voltage on pin 1 of the SWD header). I chose 2.5V because the LiPo battery voltage will drop as low as 3V and wanted to have some headroom after subtracting the dropout voltage of the regulator (The LD1117 wouldn't even work here with it's ~1V dropout voltage). 

So it was time to think about the voltage regulation in more detail. LDO's are less noisy than switching regulators so I'll stick with them for now. The challenge is to find a low enough dropout voltage so that the battery can deplete as much as possible before the supply is cut off by the LDO. The current requirements are pretty low - around 65mA after adding the maximum consumption values I could find for each component (except motors, they are not regulated). 

I found the TC1262 which has a small footprint and features a maximum 130mV dropout at 100mA current and a maximum current of 500mA. According to the capacity/voltage charge below, this is enough to cover the entire capacity range of the LiPo down to 5% capacity.

![image](https://github.com/user-attachments/assets/e2d173b0-85bc-4c5d-915c-ce89ed1dacaf)

### Wireless programming plans

Preparing wireless programming (if even possible). The STM32 can be booted in bootloader mode instead of reading from the flash were the application is saved. The bootloader runs a loop that detects any compatible communication protocols and runs a second loop accordingly. SPI is compatible, so in theory, I can transmit data through the RF-module and run a reprogramming sequence that is sent to the µC via SPI.

Apparently you can jump to the bootloader memory location in firmware, so I'll skip any overthinking/-researching about hardware requirements and see if it works some day.

### Do I need a MOSFET Driver?

Can the circuit provide enough current to fully charge/discharge the parasitic capacitance (gate charge) and therefore have time to fully conduct at the specified frequency? 

f = 20kHz is chosen to avoid audible switching noise --> T = 50µs. For good measure, let's take T/4 as our charge/discharge time. The gate charge of the IRLZ44N is 48nC.  I = Q/T {A = A*s / s} --> I = 48nC/12.5µs = 3.84mA.
If we drive the MOSFET directly through the octocoupler, it would need to withstand/provide 3.84mA. Same for the discharge resistor from gate to ground (R = 3.7V / 3.84mA = ~1000 Ohm).

I am concerned about power losses with small resistors (14mW with 3.84mA and 3.7V).

My experience is very limited on this topic and I will go forward without a driver to see what happens. I intend to experiment with different resistor values.


## Schematics

#### Done
- Motor control driver + signal connections
- SPI connections
- Decoupling
- ICs connections
- Basic power
- Programming connections

#### ToDo
- Overvoltage and ESD safety
- Oscillator components
- Battery connector/plug
- Specify optocoupler
- Specify capacitors
- Antenna
- Probably more...

![image](https://github.com/user-attachments/assets/ad50ed5e-e5a1-4997-949c-bb496e779541)


![image](https://github.com/user-attachments/assets/aea168be-82c8-4bfe-b9ae-a186f69cc1f8)

![image](https://github.com/user-attachments/assets/d7884b6f-85b0-4e22-b3cd-5a51586b1cbf)



