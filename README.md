# DESIGN & IMPLEMENTATION OF POWER INTEGRITY SOLUTIONS ON LOW POWER SOC DESIGNS

The challenge of achieving full-chip dynamic and static power integrity becomes increasingly prominent as technology processes advance beyond 65nm and into smaller nodes. This challenge arises from the combination of large power densities, lower voltage supplies, and higher operating frequencies. Managing dynamic power and addressing issues such as peak voltage drop becomes particularly complex due to their transient nature, and their potential impact on chip timing and manufacturing yield is
a growing concern.

In this thesis, we have implemented the Low-Power SoC design power integrity solution using RedHawk design flow which is encapsulated under Synopsys EDA Design tools.
The RedHawk power integrity solution offered by Apache is a comprehensive product designed for managing power and ground aspects in cell-based chip designs. RedHawk offers advanced capabilities for early design analysis, including the assessment of static IR drop and the estimation of dynamic hotspots. These analyses rely on library based methods, which involve utilizing library files for critical information.While doing so, the three optimization modes offered by RedHawk namely Dynamic Power Shaping, IR
Driven Placement and Power Grid Augmentation are implemented on designs which are running at different stages of physical design flow. The reduction in power parameters are compared with the reference baseline design which mainly includes static power, dynamic power, leakage power, dynamic voltage drop, peak current etc.

# Contents

- [Power Reduction Solutions versus Power Integrity Solutions](#Power-Reduction-Solutions-versus-Power-Integrity-Solutions)
- [Implemeneted Power Integrity Solutions](#Implemeneted-Power-Integrity-Solutions)
- [Dynamic Power Shaping](#Dynamic-Power-Shaping)
- [IR Driven Placement](#IR-Driven-Placement)
- [Power Grid Augmentation](#Power-Grid-Augmentation)
- [Dealing with Timing Trade-off - CCD Optimization](#Dealing-with-Timing-Trade-off-CCD-Optimization)
- [Results - Dynamic Power Shaping](#Results-Dynamic-Power-Shaping)
- [Results - Layer-wise IR Drop](#Results-Layer-wise-IR-Drop)
- [Results - Voltage Drop v/s Instance Count](#Results-Voltage-Drop-v/s-Instance-Count)
- [Power Results](#Power-Results)
- [Timing Impact](#Timing-Impact)
- [Acknowledgements](#Acknowledgements)




# Power Reduction Solutions versus Power Integrity Solutions

Power Integrity Solutions and Power Reduction Solutions are two critical aspects in VLSI design, focused on ensuring efficient and reliable operation of integrated circuits while managing power consumption. Here are the key differences between the two:
![image](https://github.com/akash-ambekar/DESIGN-IMPLEMENTATION-OF-POWER-INTEGRITY-SOLUTIONS-ON-LOW-POWER-SOC-DESIGNS/assets/100372947/7da5301c-e15a-4a97-aa69-9bb0a5041372)


# Implemeneted Power Integrity Solutions

1.	Dynamic Power Shaping (DPS)

2.	IR Driven Placement (IRDP)
	
3.	Power Grid Augmentation (PGA)


# Dynamic Power Shaping

A primary source of stress on power grids arises when groups of power-hungry registers all switch simultaneously, causing a surge in current consumption. The Dynamic Power Shaping Capability (DPS), a technology integrated into Fusion Compiler by Techletech following their acquisition in 2018, is designed to address this challenge. DPS identifies these clusters of registers and adjusts their switching times, effectively spreading out the current demand and smoothing out abrupt peaks.

Consider the following H-tree based clock synthesis network with each flop represents a set of flip-flops that are getting triggered by clock. Initially assume that skews are
perfectly balanced with all flops to get least latency and all flops are desired to get triggered at the same time instance. But this will create an issue that, as all flops are switching at the same instance, the dynamic switching drop will show the instantaneous spike and then gradually lowered down. This voltage drop spike is shown in the
figure which ultimately creates a hotspot which indicates high power consumption. This drastic rise the switching voltage drop may create an issue for reliability of the chip hence we implement Dynamic Power Shaping (DPS) to overcome this.

![image](https://github.com/akash-ambekar/DESIGN-IMPLEMENTATION-OF-POWER-INTEGRITY-SOLUTIONS-ON-LOW-POWER-SOC-DESIGNS/assets/100372947/5b4dfb1e-db9e-4f68-8a7b-bc03d884656d)
![image](https://github.com/akash-ambekar/DESIGN-IMPLEMENTATION-OF-POWER-INTEGRITY-SOLUTIONS-ON-LOW-POWER-SOC-DESIGNS/assets/100372947/6feb20cb-4343-42f9-a2c5-6a6a1dea01bf)

Dynamic Power Shaping (DPS) will consider the placement of standard cells, as it’s timing and placement aware, it will move the blocks which will manipulate the skew, latency and clock timing path of each flop as shown the figure. DPS will recover the sudden voltage spike into gradual constant power drop with respect to time which make the device reliable. But one thing that should be taken care that this will not raise the severe impact on timing and should not lead to any setup or hold violations.


# IR Driven Placement

![image](https://github.com/akash-ambekar/DESIGN-IMPLEMENTATION-OF-POWER-INTEGRITY-SOLUTIONS-ON-LOW-POWER-SOC-DESIGNS/assets/100372947/1d47c2c2-e9f0-4deb-acdb-c58dff336c01)

Consider the above figure which represents the target for IR aware placement. Here it is intended that most of all cells should maintain the voltage drop equal to the less than 5% of maximum voltage drop. Ideally 95% cells gets covered in the lower drop area. For medium drop area, the voltage drop ranges from 5% to 10% of the maximum drop and ideally 3 to 4% of cells gets covered in this area. Finally, the high drop area, the least number of cells are targeted having voltage drop greater than 10% of maximum value and ideally 1% of total cells are there in this area.

The primary objective of IR driven placement in VLSI design is to achieve a uniform power density distribution across the entire chip. To accomplish this, the technique of cell spreading is employed, strategically reducing instances of IR hotspots that can negatively impact the chip’s performance and reliability. The extent of cell spreading is determined by two crucial factors: power density and voltage drop. Areas with high IR drop require more aggressive cell spreading, while regions with lower impact on power and voltage experience relatively less spreading. Additionally, designers have the flexibility to utilize optional user-provided settings to guide the selection of instances for cell spreading. These settings can be based on instance count or specified IR drop percentages, allowing for a tailored approach to achieve the desired power density uniformity and voltage drop mitigation in VLSI design.


# Power Grid Augmentation

One of the major contributor for the total IR drop of the design is voltage drop across the power grid as it contributes 20% to 25% of total IR voltage drop. There are multiple parameters of metal layer used for power grid distribution which affects and responsible for the voltage drop which includes length, thickness, sheilding of metal layer etc. Generally, higher metal layers are used to route the power supplies as they provides less resistance due to comparatively larger width which ultimately reduces the voltage drop across the design. The main goal of Power Grid Augmentation (PGA) is to reduce the voltage drop from the power source to the power source pin of cell. This can be achieved by inserting the metal layer as the intermediate connection. Consider the below figure 2.6, here the extra metal layers are routed from power grid to standard cell. After detailed routing and before filler cell insertion, the extra metal shapes gets inserted into void area which creates a connection from VDD/VSS to the power pin of standard cell.

![image](https://github.com/akash-ambekar/DESIGN-IMPLEMENTATION-OF-POWER-INTEGRITY-SOLUTIONS-ON-LOW-POWER-SOC-DESIGNS/assets/100372947/98161117-8c27-45ed-b286-c1d6ca73cded)

In figure, the resistance shown in black color represents the original resistance when no extra shapes have inserted. This resistance includes the resistance higher metal layer used for power grid, interconnect metal layer and lower metal layer which physically creates contact with standard cell. By inserting extra metal shapes, the resistance of extra metal shapes makes a parallel connection with original interconnect metal layer resistance due to which, the reduction in interconnect metal layer resistance ultimately reduce the overall resistance from the power source to the standard ells which finally reduces the voltage drop.

In a general sense, power grid augmentation is done just before the filler cell insertion because if the metal shapes are inserted at the prior stage global or detailed routing, it might create routing blockages and impossible constraints due to which standard signal routing will become impossible. For power grid augmentation, the layer to layer voltage drop may show the slight variation with the reference design, but there will be the reduction dynamic power is expected. One issue with PGA is that, due to reduction in resistance of intermediate routing metal layer, there might be the chances for rise in peak current which lead to issues like electromigration. Along with this, one must ensure that no DRC errors should be inserted in the design due to insertion of the metal shapes.

# Dealing with Timing Trade-off - CCD Optimization

The RedHawk techniques which are desired to be implemented like dynamic power shaping, IR driven placement etc. manipulates the placement of the standard cells. Due to which, the timing parameters like skew, latency etc. varies which might arise the setup and hold violations. To overcome this, we have an another technique called ”Concurrent Clock and Datapath Optimization (CCD)”. Concurrent clock and datapath optimization aims to enhance the efficiency of the system by minimizing clock cycle times and reducing latency through the synchronization of data and control paths. It involves a comprehensive analysis of the entire architecture, from the high-level microarchitectural choices down to the gate-level implementation, with the ultimate goal of achieving faster execution and improved throughput. At the heart of concurrent clock and datapath optimization lies the delicate balancing act of reducing clock cycle\ times while maintaining the correctness and reliability of the system. Achieving this balance requires an intricate understanding of the microarchitecture and the careful management of critical paths within the processor.


The CCD optimization involves the management of the datapath, which consists of the various components responsible for executing instructions, such as registers, multiplexers, and the data buses. A critical technique in datapath optimization is the use of register renaming. Register renaming helps mitigate register dependencies by assigning multiple physical registers to each architectural register, enabling the simultaneous execution of instructions that might otherwise contend for the same architectural registers. Furthermore, the use of out-of-order execution, coupled with a reorder buffer, allows instructions to be issued in the order they become available rather than in their original program order. The reorder buffer records the original program order and ensures that results are committed in the correct order.


Concurrent clock and datapath optimization is a continuous process, and it goes beyond the architectural design. The physical design of the chip, known as physical design or layout, plays a crucial role. In this phase, engineers optimize the placement and routing of logic gates, memory cells, and interconnects to minimize wire delays and achieve better clock distribution. Advanced technologies like silicon-on-insulator (SOI) and FinFET transistors are used to reduce leakage currents and improve switching speed, which can significantly impact overall performance. In the proposed work, we have analyzed the impact of CCD on timing parameters on the design. The results of baseline design has been compared with the original design along with CCD optimized design.

# Results - Dynamic Power Shaping 

The main goal of Dynamic Power Shaping is to reduce the peak current of design for
reliable working of the chip. Consider the following figure indicating graph of peak
current v/s time, Here, 38.8% reduction in the peak current is observed. From the graph, we observe
the crests and troughs in the current with respect to timing 
![image](https://github.com/akash-ambekar/DESIGN-IMPLEMENTATION-OF-POWER-INTEGRITY-SOLUTIONS-ON-LOW-POWER-SOC-DESIGNS/assets/100372947/f0faac03-11e9-48c0-9b60-26e23df53b1d)

![image](https://github.com/akash-ambekar/DESIGN-IMPLEMENTATION-OF-POWER-INTEGRITY-SOLUTIONS-ON-LOW-POWER-SOC-DESIGNS/assets/100372947/0a1cfe45-bc3f-434d-bf30-a9f01894340f)

Figure shows the comparison prototype of power density map of baseline design and DPS optimized design. Clearly, as peak current gets reduced, the overall drop gets reduced and we observe the reduced power density. The red area in baseline map represents the voltage hotspot area which consumes heavy power and causes high drop. This area will be main target of DPS optimization and it will rearrange the placement due to which the decrement in overall voltage drop is observed.


# Results - Layer-wise IR Drop

Power integrity is the main concern of the implementation of RedHawk techniques. By implementation, it is desired to have reduction in voltage drops at every metal layers.

![image](https://github.com/akash-ambekar/DESIGN-IMPLEMENTATION-OF-POWER-INTEGRITY-SOLUTIONS-ON-LOW-POWER-SOC-DESIGNS/assets/100372947/6b3a8b98-04e4-49c0-8744-847560bdfcc4)


The table shows the layer-wise voltage drop occurring due to supply voltages. Analyzing the data we can infer that, the voltage drop is getting reduced in all of the three implementations. Mentioning for solitary results, Power Grid Augmentation shows the highest reduction in layer wise drop, the base metal layer shows 30% reduction which raises upto 40%. The order of best results seen is : 

PGA > IRDP + PGA > DPS + PGA > DPS + IRDP + PGA > DPS > IRDP > DPS + IRDP > Baseline 


# Results - Voltage Drop v/s Instance Count

One of the major factor in the implemented technique is maintain the standard cells within the lower percentage of maximum voltage drop. Here there are two graphs, first represents the total voltage drop with respect to instance count and another represents the incremental/decremental change of instance count at that particular voltage drop with respect to baseline drop. The ideal nature of desired graph is that, in first graph there should be sharp rise in instance count at lower percentage drop and then it should lowered down to zero at higher percentage drop. Mentioning for second graph, it is desired to have a sharp triangular wave representing that the instance count at lower drops are is higher and instance count at higher drops is far lower than that of baseline counts

![image](https://github.com/akash-ambekar/DESIGN-IMPLEMENTATION-OF-POWER-INTEGRITY-SOLUTIONS-ON-LOW-POWER-SOC-DESIGNS/assets/100372947/40e119df-2536-44a3-b3df-24f74d1e2346)


# Power Results 

As discussed earlier, the primary goal of RedHawk solutions is to maintain the power integrity and not to achieve the reduction. Each of the implemented technique maintains the power integrity with respect to a certain parameter. For example, Dynamic Power Shaping reduces the peak current, IR Driven Placement reduces the voltage drop and Power Grid Augmentation reduces the resistance of power grid to supply port of standard cell. All of the three techniques deals with current/voltage parameters of the design. Hence along with maintaining the power integrity, we have observed slight significant reduction in power.

![image](https://github.com/akash-ambekar/DESIGN-IMPLEMENTATION-OF-POWER-INTEGRITY-SOLUTIONS-ON-LOW-POWER-SOC-DESIGNS/assets/100372947/71d27dcb-0e02-43f1-ab23-39e443e505a3)

As DPS will manipulate the peak current and PGA will reduce the resistance of power grid to supply port, the dynamic power will be reduced, the highest reduction in dynamic power is observed when all DPS, IRDP and PGA have implemented simultaneously on the design which is 4.2%. Also on an average, 4.03% dynamic power reduction is observed in all technique implementation.


# Timing Impact

While implementing power integrity solutions, we manipulated the placement and other parameters of design which may directly impact on timing violation and can cause the setup and hold violations. While implementing the techniques, we have not seen any major impact on setup violations and observed that setup is getting improved with implementation of CCD. The major concerned area is hold violations which are comparatively hard to nullify.

![image](https://github.com/akash-ambekar/DESIGN-IMPLEMENTATION-OF-POWER-INTEGRITY-SOLUTIONS-ON-LOW-POWER-SOC-DESIGNS/assets/100372947/6b213f42-a70e-46aa-a674-fb0f3dfdff1f)

# Acknowledgements

1.	Dr. Hemanta Kumar Mondal (Assistant Professor, Dept. of ECE, NIT Durgapur)

2.	Ms. Khushboo Bhartia (Manager, Intel India Pvt. Ltd., Bangalore)	
	
3.	Mr. Pranav Agrawal (Technical Lead, Intel India Pvt. Ltd., Bangalore)

4.	Mr. Hardas Agrawal, Mr. Sahil Jakhar and Mr. Ashutosh Tripathi (SoC Design Engineer, Intel India Pvt. Ltd., Bangalore)



