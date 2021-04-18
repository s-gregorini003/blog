---
title: "Noise and Vibration Analysis of 3D Printers"
subtitle: "How to Evaluate Airborne and Structure-borne Noise without the Right Tools"
date: 2020-06-18T08:45:42+01:00
draft: false 
---

This post is on a project made at the beginning 2020 in which I had to analyse and mitigate the noise emitted by a desktop 3D printer. Here I cover the methodology used to conduct the measurements, which differs a bit from the standards, given the limited equipment available.

### TL;DR

To evaluate the structure-borne sound, the only calibrated instrument I could find was an old, single-axis vibration sensor, without the option to collect either octave band or one-third-octave band measurements. After 336 individual measures (144 with the vibration sensor + 144 with a contact microphone + 48 with NTi Audio sound level meters) carried on during a stressful 72 hours period, the results showed that the changes applied to the machine improved its noise emission. However, part of the instrumentation used (the CM-01B contact microphone) proved unsuitable for conducting the measuring process.

---

## Methodology

**Test routines** - I coded four G-Code procedures for tests reproducibility, numbered to match the name conventions on the collected data. Obviously, routine 1 and 2 do not require any G-Code file, since the 3D printer is not moving.

1. Background noise;
2. Source idling;
3. Source with simulated load, under normal working conditions (`test-routine-a.gcode`);
4. Source at maximum operating speed (`test-routine-c-full-speed.gcode`);
5. Source working under conditions corresponding to maximum sound generation
representative of normal use (`specimen-d-infill-only.gcode`);
6. Source undergoing a charateristic work cycle (`specimen.gcode`);

**Structure-borne sound** - As I said previously, this set of tests couldn't be conducted according to the standard [BS ISO 9611:1996](https://shop.bsigroup.com/ProductDetail/?pid=000000000000942052), but I tried to follow the procedure as far as possible. I collected 3-axis measurements on each of the 4 predefined supports and for each routine, using both the contact microphone CM-01B and the vibration meter Bruel & Kjaer 2537: the former returned Z-weighted sound pressure level in one-thirdoctave bands (from 20 Hz to 20 kHz), while the latter a single acceleration value. Both were time-averaged over each routine duration.

{{< figure src="/img/ac/struc-support-4-z-axis.jpg" caption="Setup to measure structure-borne sound on the z-axis." >}}

**Airborne sound** - On the other hand, I could perform this tests according to [BS EN ISO 1680:2013](https://shop.bsigroup.com/ProductDetail?pid=000000000030276993) and [BS EN ISO 3746:2010](https://shop.bsigroup.com/ProductDetail/?pid=000000000030094857). For this test, I conducted 6 measurements at each of the 4 microphone positions outlined by the standards, before and after the changes. The collected data were A-weighted sound pressure levels in one-third-octave-bands (from 20 Hz to 20 kHz), time-averaged over the routine average duration.

## Machine Survey and Improvements

After the first analysis and measurement session, the printer showed the following weaknesses in terms of noise emission:

- Fans - Even without a systematic measurement, it is clear that the majority of the noise in a 3D printer comes from its fans. In particular, this machine has 4 fans in total, positioned in the following spots: motherboard, extruder, x-axis motor and power supply. The latter was the most problematic, since that fan was always running at full speed. This was caused by the power supply circuit, which has the space dedicated to a mechanical thermostat, but the component was removed and the poles jumped. So, I installed a normally open KSD9700 thermostat in the location and placed inside the large coil. The switch on the thermostat is designed to close if the temperature exceeds 45 degrees Celsius, which is a default conguration for this type of power supply.

{{< figure src="/img/ac/power-supply-thermo.jpg" caption="Position of the switching thermostat installed in the power supply." >}}

- Motor dampers - The printer under test uses 5 Nema17 stepper motors, which is one of the most common and cheap solutions in the field. Despite their cost, popularity, and performance, these motors are particularly loud, especially when coupled with low resolution controllers
like the DRV8825 IC. To address this potential issue, I decided to install mechanical dampers between the motor and the 3D printer frame.

{{< figure src="/img/ac/motors.png" caption="Installation of the motor dampers." >}}

- Bearings - Another potential source of noise and vibration was found in the linear bearings of the y-axis. These three bearings allow the heated bed to move without friction along the cylindrical rails. In the original design of the printer, they were spherical bearings, which require lower tolerances to work, but are prone to lose performance over time. They have been substituted by self-lubricating polymer bearing, which have no moving parts, and therefore are quieter, more precise and durable.


## Results

All considered, this project is satisfactory: the data show improvements in most of the tested modes. After processing the raw data according to the relative standards, I plotted the before/after comparisons. Moreover, I've found out that a CM-01B contact microphone can't be used to collect reliable vibration data!

{{< figure src="/img/ac/contact-mic-results.png" caption="Comparison between the data collected with the CM-01B for the y-axis." >}}

Apart from this finding, the other procedures show significant improvements on the noise emitted by the 3D printer. As you can se by the following graph, I was able to isolate and reduce specific frequencies in the machine (in this example, the peak at around 1000 Hz corresponds to cooling fan of the power supply).

{{< figure src="/img/ac/plot-3-test-routine-a.png" caption="Airborne sound power level for test routine a." >}}

---

Thanks for reading! In case you want to take a deeper look at this project and at the results, you can find it [here](https://www.researchgate.net/publication/343628653_Evaluation_and_Mitigation_of_Airborne_and_Structure-borne_Noise_Emitted_by_3D_Printer#fullTextFileContent).
