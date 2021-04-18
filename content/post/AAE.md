---
title: "3D Printed Acoustic Absorbers"
subtitle: "Design, Production and Evaluation of PDI Absorbers"
date: 2020-06-10T08:45:42+01:00
math: true
draft: false 
---

This blog post is to summarise a research project conducted at the beginning of 2019 as part of the Advanced Audio Engineering module (MSc Audio Engineering, Leeds Beckett University). The project discusses the potentials of achieving high performance in terms of sound absorption through additive manufacturing techniques.

The absorbers are designed to exploit the principle of passive destructive interference (PDI) and manufactured using a commercially available fused deposition modeling (FDM) 3D printer. The specimen are cylindrical and produced with cavities of different lengths. The absorption coefficient is evaluated using an impedance tube and following the specifications of the standard [BS EN ISO 10534-2:2001](https://shop.bsigroup.com/ProductDetail?pid=000000000030069842).

The results show that significant sound absorption can be achieved, although some absorption peaks do not match the expected frequencies. This may be caused by turbulence and resonance phenomena, especially in samples with greater diameter of the cavity. Futher investigation is required, but the potential of these technologies in terms of efficiency over manufacturing cost and absorber size is undoubted.

---

## Introduction

The aim of the project is to design and manufacture Passive Destructive Interference (PDI) absorbers using Additive Manufacturing (AM) techniques and then to evaluate their performance through a low cost, self-built impedance tube. The main outcome of the measurement process is a set of diagrams of the normal absorption factor of the specimens as a function of frequency in a mid-low spectrum range. The sound absorption curves will be then analyzed to determine the effects of the tested geometries and to verify if the experimental results agree with the values expected from the samples in the design phase.


## Theory and Methodology

The investigated devices are designed to absorb specific frequencies through passive destructive interference. These frequencies depend mainly on the specimen's geometry, and can be calculated in advance through the following equation 

$$f_n = \frac{(2n - 1)* c}{2L}$$

where *L* (m) is the length of the cavity and *c* (m/s) is the speed of sound in the air.

{{< figure src="/img/aae/sample-A-specs.png" caption="Specification and design of a specimen." >}}

I designed the samples in SolidWorks and manufactured with an AthorBot Brother Desktop 3D Printer. To test different materials, both geometries were printed in ABS and PETG.

{{< figure src="/img/aae/specimen1.jpg" width="600px" caption="Two specimen used for measurement." >}}


The measurement of the absorption coefficient was conducted according to the *two-micophone technique* detailed in [BS EN ISO 10534-2:2001](https://shop.bsigroup.com/ProductDetail?pid=000000000030069842). The biggest issue faced in this project was to find an impedance tube to conduct the evaluation. Since my university had part of the equipment (microphones, soundcard, speakers...), in the end I decided to build a low-cost impedance tube. I built it out of pvc pipes, hydraulic seals, and I added an Arduino Uno with an OLED display and temperature/humidity sensors to keep track of the environmental conditions. To calibrate the tube and conduct the measuring process - which consisted in measuring the cross-transfer function and then calculating the absorption coefficient - I used the software [Holmarc Wave Analyzer 4C ](https://www.holmarc.com/softwares/impedance_tube_software.rar).

{{< figure src="/img/aae/TestEquipmentDiagram.png" width="800px" caption="Measurement test equipment layout." >}}


## Findings 

For each specimen, measurements are taken in the frequency interval 100-3200 Hz. The normal sound absorption curves are analysed through the evaluation of two values: 

- The frequency of peak sound absorption
- The frequency range between the values on both sides of the peak where half the peak sound absorption occurs.

Using those values, the effects of the tested geometries can be determined and compared to the expected theoretical values. The results show that significant sound absorption has been achieved. The sound absorption coefficient reaches values of 0.87 at peak frequencies in the range considered. However, it must be noted that the absorption bandwidth is relatively narrow and that many relatively lower peaks can be attributed to the unprofessional measuring equipment.

For example, the following figure shows that the predicted frequency of the peaks for sample A is respected mainly in the lower range, while they are slightly shifted in the mid-upper range considered, probably due to production defects in the specimen.

{{< figure src="/img/aae/sample-A-results.png"  caption="Measured absorption coefficient for sample A compared to predicted peak frequencies." >}}

---

Thanks for reading! If you want to have a look at the full article, you can find it [here](https://www.researchgate.net/publication/343628647_Measurement_of_the_Absorption_Coefficient_for_Destructive_Interference_Absorbers_Produced_by_Additive_Manufacturing).

## References

- [Sound absorption and additive manufacturing](https://www.researchgate.net/publication/281005456_Sound_absorption_and_additive_manufacturing)

- [Implications of solid freeform fabrication on acoustic absorbers](https://www.emerald.com/insight/content/doi/10.1108/13552540710824805/full/html)
