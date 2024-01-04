# DESIGN & IMPLEMENTATION OF POWER INTEGRITY SOLUTIONS ON LOW POWER SOC DESIGNS

The challenge of achieving full-chip dynamic and static power integrity becomes increasingly prominent as technology processes advance beyond 65nm and into smaller nodes. This challenge arises from the combination of large power densities, lower voltage supplies, and higher operating frequencies. Managing dynamic power and addressing issues such as peak voltage drop becomes particularly complex due to their transient nature, and their potential impact on chip timing and manufacturing yield is
a growing concern.

In this thesis, we have implemented the Low-Power SoC design power integrity solution using RedHawk design flow which is encapsulated under Synopsys EDA Design tools.
The RedHawk power integrity solution offered by Apache is a comprehensive product designed for managing power and ground aspects in cell-based chip designs. RedHawk offers advanced capabilities for early design analysis, including the assessment of static IR drop and the estimation of dynamic hotspots. These analyses rely on library based methods, which involve utilizing library files for critical information.While doing so, the three optimization modes offered by RedHawk namely Dynamic Power Shaping, IR
Driven Placement and Power Grid Augmentation are implemented on designs which are running at different stages of physical design flow. The reduction in power parameters are compared with the reference baseline design which mainly includes static power, dynamic power, leakage power, dynamic voltage drop, peak current etc.

# Power Reduction Solutions v/s Power Integrity Solutions

Power Integrity Solutions and Power Reduction Solutions are two critical aspects in VLSI design, focused on ensuring efficient and reliable operation of integrated circuits while managing power consumption. Here are the key differences between the two:
![image](https://github.com/akash-ambekar/DESIGN-IMPLEMENTATION-OF-POWER-INTEGRITY-SOLUTIONS-ON-LOW-POWER-SOC-DESIGNS/assets/100372947/7da5301c-e15a-4a97-aa69-9bb0a5041372)


# Implemeneted Power Integrity Solutions

1.	Dynamic Power Shaping (DPS)

2.	IR Driven Placement (IRDP)
	
3.	Power Grid Augmentation (PGA)


# Dynamic Power Shaping (DPS)

A primary source of stress on power grids arises when groups of power-hungry registers all switch simultaneously, causing a surge in current consumption. The Dynamic Power Shaping Capability (DPS), a technology integrated into Fusion Compiler by Techletech following their acquisition in 2018, is designed to address this challenge. DPS identifies these clusters of registers and adjusts their switching times, effectively spreading out the current demand and smoothing out abrupt peaks.

Consider the following H-tree based clock synthesis network with each flop represents a set of flip-flops that are getting triggered by clock. Initially assume that skews are
perfectly balanced with all flops to get least latency and all flops are desired to get triggered at the same time instance. But this will create an issue that, as all flops are switching at the same instance, the dynamic switching drop will show the instantaneous spike and then gradually lowered down. This voltage drop spike is shown in the
figure which ultimately creates a hotspot which indicates high power consumption. This drastic rise the switching voltage drop may create an issue for reliability of the chip hence we implement Dynamic Power Shaping (DPS) to overcome this.

![image](https://github.com/akash-ambekar/DESIGN-IMPLEMENTATION-OF-POWER-INTEGRITY-SOLUTIONS-ON-LOW-POWER-SOC-DESIGNS/assets/100372947/5b4dfb1e-db9e-4f68-8a7b-bc03d884656d)
![image](https://github.com/akash-ambekar/DESIGN-IMPLEMENTATION-OF-POWER-INTEGRITY-SOLUTIONS-ON-LOW-POWER-SOC-DESIGNS/assets/100372947/6feb20cb-4343-42f9-a2c5-6a6a1dea01bf)










