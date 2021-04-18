---
title: "volca_boy[1] // Initial Design"
date: 2020-05-31T18:01:50+01:00
draft: false
---

## IMPORTANT NOTICE

**Unfortunately, due to the Covid-19 pandemic, this project is currently paused. The reason is that I had to leave my house in Leeds and temporarily return to my hometown in Italy, leaving most of my stuff in the UK. I really hope to go back as soon as possible and continue this journey.**

### Do More, with Less 

For those unfamiliar with the term, *chiptune* (also known as chipmusic, micromusic or 8-bit music) is a style of electronic music derived from the sound chips that, in the first generation of computers and gaming consoles, were used to balance the processing power of generating sound effects and music from the CPU. 

>The primal micromusic spirit was to overcome limitations and wield them in wildly creative ways. - [Fabio "Kenobit" Bortolotti][1]

Like everything nowadays, that definition can be interpreted in many ways. In its strictiest meaning, chiptune is used to refer to music created entirely from the original, vintage audio chips (nevertheless, modifications that do not alter the nature of the sound produced are "allowed", but we'll come back to this later). On the other hand, the broadest definition is more related to the aesthetics of the sound, rather then to the source generating it. In general, the philosophy behind the chiptune movement is to exploit the limited resources of the hardware to create more and more articulated music. 


### DMG-01

With its four audio channels, the *Game Boy* is supposedly the most popular tool for the production of micromusic. This console has been around for quite some time (Nintendo released it in 1989), and over the course of the years many people created and documented mods to equip it with useful features for the cause, such as better audio, rechargeable battery, backlit screen... 

| Channel | Type  | Features                                                                                                  |
|:-------:|-------|-----------------------------------------------------------------------------------------------------------|
|    1    | Pulse | Volume envelope, 4-mode pulse width, frequency register from C3 upwards, frequency envelope               |
|    2    | Pulse | Volume envelope, 4-mode pulse width, frequency register from C3 upwards                                   |
|    3    | Wave  | User-definable waveforms, bank of 32 samples (4-bit each), frequency register from C2 upwards             |
|    4    | Noise | White and brown noise                                                                                     |

What I want to do with this project is to use some of these mods as a starting point to transform that portable device into a proper music production tool, without over-denaturing its essence. 


### First Design

Initially, I started this project as a university assignment, which is why I was able to document everything in detail (specifically, in this [Github repo][2]). Back then, my idea was to tear apart a Game Boy, rewire it from the inside to a different controls and put everything into a new, original case.  

{{< figure src="/img/volca-boy/volca-boy-exploded-view.png" caption="3D model made with SolidWorks to show the exploded view of the final product." >}}

Unfortunately, towards the end of the semester I realised that creating a custom shell for my product was a task way harder than expected. So, after many stressful weeks, I decided - in accordance with my lecturers - to give up with the enclosure and present only a working prototype. Anyway, the exam went well, but I still want to bring this project to an end.


## References

Collins, K. and Kapralos, B. and Tessler, H. and Paul, J. L. (2014) *The Oxford Handbook of Interactive Audio.* Oxford: Oxford University Press.

[1]: <https://twitter.com/fabiobortolotti/status/1267093927800139777> "Chiptune is dead. Here's what I think."
[2]: <https://github.com/s-gregorini003/interfaces-and-interactivity> "Interfaces and Interactivity"
