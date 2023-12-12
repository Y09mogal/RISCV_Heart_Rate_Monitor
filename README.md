# RISCV_Smart_Door_Locking_System

## Aim
The primary objective of this project is to design and implement a grid-based push button passcode-locked Smart Door System using a specialized RISCV processor. 

## Description
The grid-based push-button passcode-locked Smart Door system aims to enhance security and convenience in accessing a door by requiring users to input a passcode through a push-button grid to enter the room. Moreover, the door gets unlocked by pressing the master push button situated inside the room for the convenience of the tenant of the room to open the door. This DIY project uses 10 push buttons (a 3x3 grid of 9 push buttons and a master push button) and a servo motor attached to the latch of the door.

## Working
1. The servo motor is attached to the latch of the door which will control the access to the room according to the passcode-based security. 
2. When the user presses the push buttons according to the passcode provided, the pin reads HIGH and the door unlocks with the help of a servo motor(set HIGH at an angle of 145) if it matches the passcode design provided by the user. 
3. Moreover, the door can be unlocked by the tenant when the master push button is pressed to grant access to the room.

## Block Diagram

![Screenshot from 2023-10-12 00-02-59](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/0e384a9a-abb4-44d2-aaa8-c409bc427598)


## C Code

```

int main() {


	//Inputs

	int pb1 = 0;
	int pb2 = 0;
	int pb3 = 0;
	int pb4 = 0;
	int pb5 = 0;
	int pb6 = 0;
	int pb7 = 0;
	int pb8 = 0;
	int pb9 = 0;
	int master_pb = 0;

	//Outputs

	//int servomotor = 125; // 125 lock, 45 unlock

	int sm = 0;
	int sm_reg = sm*1024;
	
	//int flag_pwd = 0;
	
	int mask_code = 0xFFFFFBFF; // ServoMotor - Output : 11111111111111111111101111111111
	

	//asm code to set initial servo motor position - CLOSE


    asm volatile (
        "and x30, x30, %0\n\t"
        "or x30, x30, %1"       //ServoMotor
        :
        :"r"(mask_code), "r"(sm_reg)
        :"x30"
    );


	while (1) {


	//  asm code to read push button values
	asm volatile(
		"andi %0, x30, 1\n\t"
		:"=r"(pb1)
		);
		
	asm volatile(
		"andi %0, x30, 2\n\t"
		:"=r"(pb2)
		);
		

	asm volatile(
		"andi %0, x30, 4\n\t"
		:"=r"(pb3)
		);
	
	asm volatile(
		"andi %0, x30, 8\n\t"
		:"=r"(pb4)
		);
	
	asm volatile(
		"andi %0, x30, 16\n\t"
		:"=r"(pb5)
		);
	
	asm volatile(
		"andi %0, x30, 32\n\t"
		:"=r"(pb6)
		);
	
	asm volatile(
		"andi %0, x30, 64\n\t"
		:"=r"(pb7)
		);
	
	asm volatile(
		"andi %0, x30, 128\n\t"
		:"=r"(pb8)
		);
	
	asm volatile(
		"andi %0, x30, 256\n\t"
		:"=r"(pb9)
		);
	
	asm volatile(
		"andi %0, x30, 512\n\t"
		:"=r"(master_pb)
		);
			


	//Door unlocks only when master push button is pressed OR passcode design of pb2, pb5 and pb6 is pressed simultaneously
	if (master_pb == 1 || (pb2 == 1 && pb5 == 1 && pb6 == 1))
		{
		sm=1;
		sm_reg = sm*1024;
		
		//asm code to set servo motor position - OPEN

        asm volatile (
        "and x30, x30, %0\n\t"
        "or x30, x30, %1"       //ServoMotor
        :
        :"r"(mask_code), "r"(sm_reg)
        :"x30"
        );

		}
	else
		{
		sm=0;
		sm_reg = sm*1024;
		
		//asm code to set servo motor position - CLOSE

        asm volatile (
        "and x30, x30, %0\n\t"
        "or x30, x30, %1"       //ServoMotor
        :
        :"r"(mask_code), "r"(sm_reg)
        :"x30"
        );

		}

	}
	
	return 0;

}


```
### Assembly code conversion

The above C program is compiled using the RISC-V GNU toolchain and the assembly code is dumped into a text file.

Below codes are run on the terminal to get the assembly code.
```
riscv64-unknown-elf-gcc -mabi=ilp32 -march=rv32i -ffreestanding -nostdlib -o ./out code.c 
riscv64-unknown-elf-objdump -d -r out > asm.txt

```

**Assembly Code**
```
out:     file format elf32-littleriscv


Disassembly of section .text:

00010074 <main>:
   10074:	fc010113          	add	sp,sp,-64
   10078:	02112e23          	sw	ra,60(sp)
   1007c:	02812c23          	sw	s0,56(sp)
   10080:	04010413          	add	s0,sp,64
   10084:	fe042623          	sw	zero,-20(s0)
   10088:	fe042423          	sw	zero,-24(s0)
   1008c:	fe042223          	sw	zero,-28(s0)
   10090:	fe042023          	sw	zero,-32(s0)
   10094:	fc042e23          	sw	zero,-36(s0)
   10098:	fc042c23          	sw	zero,-40(s0)
   1009c:	fc042a23          	sw	zero,-44(s0)
   100a0:	fc042823          	sw	zero,-48(s0)
   100a4:	fc042623          	sw	zero,-52(s0)
   100a8:	fc042423          	sw	zero,-56(s0)
   100ac:	fc042223          	sw	zero,-60(s0)
   100b0:	fc442783          	lw	a5,-60(s0)
   100b4:	00b79793          	sll	a5,a5,0xb
   100b8:	fcf42023          	sw	a5,-64(s0)
   100bc:	fc042783          	lw	a5,-64(s0)
   100c0:	00ff6f33          	or	t5,t5,a5
   100c4:	001f7793          	and	a5,t5,1
   100c8:	fef42623          	sw	a5,-20(s0)
   100cc:	002f7793          	and	a5,t5,2
   100d0:	fef42423          	sw	a5,-24(s0)
   100d4:	004f7793          	and	a5,t5,4
   100d8:	fef42223          	sw	a5,-28(s0)
   100dc:	008f7793          	and	a5,t5,8
   100e0:	fef42023          	sw	a5,-32(s0)
   100e4:	010f7793          	and	a5,t5,16
   100e8:	fcf42e23          	sw	a5,-36(s0)
   100ec:	020f7793          	and	a5,t5,32
   100f0:	fcf42c23          	sw	a5,-40(s0)
   100f4:	040f7793          	and	a5,t5,64
   100f8:	fcf42a23          	sw	a5,-44(s0)
   100fc:	080f7793          	and	a5,t5,128
   10100:	fcf42823          	sw	a5,-48(s0)
   10104:	100f7793          	and	a5,t5,256
   10108:	fcf42623          	sw	a5,-52(s0)
   1010c:	200f7793          	and	a5,t5,512
   10110:	fcf42423          	sw	a5,-56(s0)
   10114:	fc842703          	lw	a4,-56(s0)
   10118:	00100793          	li	a5,1
   1011c:	02f70463          	beq	a4,a5,10144 <main+0xd0>
   10120:	fe842703          	lw	a4,-24(s0)
   10124:	00100793          	li	a5,1
   10128:	04f71263          	bne	a4,a5,1016c <main+0xf8>
   1012c:	fdc42703          	lw	a4,-36(s0)
   10130:	00100793          	li	a5,1
   10134:	02f71c63          	bne	a4,a5,1016c <main+0xf8>
   10138:	fd842703          	lw	a4,-40(s0)
   1013c:	00100793          	li	a5,1
   10140:	02f71663          	bne	a4,a5,1016c <main+0xf8>
   10144:	00100793          	li	a5,1
   10148:	fcf42223          	sw	a5,-60(s0)
   1014c:	fc442783          	lw	a5,-60(s0)
   10150:	00b79793          	sll	a5,a5,0xb
   10154:	fcf42023          	sw	a5,-64(s0)
   10158:	fc042783          	lw	a5,-64(s0)
   1015c:	00ff6f33          	or	t5,t5,a5
   10160:	3e800513          	li	a0,1000
   10164:	024000ef          	jal	10188 <delaytime>
   10168:	01c0006f          	j	10184 <main+0x110>
   1016c:	fc042223          	sw	zero,-60(s0)
   10170:	fc442783          	lw	a5,-60(s0)
   10174:	00b79793          	sll	a5,a5,0xb
   10178:	fcf42023          	sw	a5,-64(s0)
   1017c:	fc042783          	lw	a5,-64(s0)
   10180:	00ff6f33          	or	t5,t5,a5
   10184:	f41ff06f          	j	100c4 <main+0x50>

```

***Number of different instructions: 16***

```
List of unique instructions:
bge
lw
sll
sw
jal
li
add
beq
j
and
or
bne
ret
lui
nop
blt

```

### To generate Processor.
Upload all.json and assembly code on the given url ```http://16.16.202.21/```


### Functional Simulation and Verification in GTKWave

<br />

![func_sim_uart_updated](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/b916095b-ec4b-450b-847f-4234295e7d28)

<br />

**GtkWaveform:**
In order to verify our design through the verilog code which is processor.v against testbench.v, we perform functional simulation of the processor design against its testbench that we obtain from Chipcron tool using iverilog and analyse it in gtkwave. Here, we have nine grid based push button as gpio input, a master push button gpio input and a gpio output operating the servoMotor state. 

*Case 1:* 
When a wrong passcode is pressed, the output of servoMotor is expected to be low which is verifed below:

<br />

![case1_func_sim_updated](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/c6fdd838-ccf1-4486-88ce-ffa1a8ba046c)

<br />

*Case 2:* 
When master push button is pressed and a wrong passcode is pressed, the output of servoMotor is expected to be high which is verifed below:

<br />

![case2_func_sim_updated](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/e34c4bba-a35e-4275-8d71-e88171b04ae9)

<br />

*Case 3:* 
When a correct passcode (of 000110010 - pb2,pb5,pb6) is pressed, the output of servoMotor is expected to be high which is verifed below:

<br />

![case3_func_sim_updated](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/4dd26092-2143-4c06-b34e-e4b84d1ce6e7)

<br />

*Case 4:* 
When master push button is pressed and a correct passcode is pressed, the output of servoMotor is expected to be high which is verifed below:

<br />

![case4_func_sim_updated](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/7bedc67d-e73c-4077-9f83-ff09c2e68deb)

<br />




### Yosys Synthesis

Yosys is a widely used open-source synthesis tool in digital design and electronic engineering. It is essential to the process of transforming a high-level hardware description, often written in Verilog or VHDL, into a gate-level netlist, which is then utilised for the implementation and optimisation of logic on a target FPGA or ASIC substrate. Yosys efficiently translates and optimises the input design using a range of algorithms and approaches, such as technology mapping, optimisation, and different heuristics. Because it supports a variety of synthesis targets, it is adaptable to various hardware platforms and versatile. Furthermore, Yosys provides tools for sophisticated analyses and formal verification, which makes it an invaluable resource for the original synthesis and further development of digital designs.


### Gate Level Simulation :

```

read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80_256.lib 
read_verilog processor.v 
synth -top wrapper
dfflibmap -liberty sky130_fd_sc_hd__tt_025C_1v80_256.lib 
abc -liberty sky130_fd_sc_hd__tt_025C_1v80_256.lib
write_verilog synth_processor_test.v
```

command to run gls simulation

```

iverilog -o test synth_processor_test.v testbench.v sky130_sram_1kbyte_1rw1r_32x256_8.v sky130_fd_sc_hd.v primitives.v
```

*Case 1:* 
When a wrong passcode is pressed, the output of servoMotor is expected to be low which is verifed below:

<br />

![case1_gls_updated](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/302cdd73-5392-4dd5-8305-c5f7a86a16c0)

<br />

*Case 2:* 
When master push button is pressed and a wrong passcode is pressed, the output of servoMotor is expected to be high which is verifed below:

<br />

![case2_gls_updated](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/8b74e842-2caf-44c9-bc1a-0430c1a18708)

<br />

*Case 3:* 
When a correct passcode (of 000110010 - pb2,pb5,pb6) is pressed, the output of servoMotor is expected to be high which is verifed below:

<br />

![case3_gls_updated](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/bdb1baeb-78d1-4d36-9269-a4dc20d76dd5)

<br />

*Case 4:* 
When master push button is pressed and a correct passcode is pressed, the output of servoMotor is expected to be high which is verifed below:

<br />

![case4_gls_updated](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/b01280b4-3d82-41be-8f3b-bdea270eec9e)

<br />

### wrapper module after netlist created

```
show wrapper
```

<br />

![show_wrapper](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/a4828800-b6a0-40d5-8624-dc1cf353b687)

<br />

## Physical Design

In VLSI (Very Large Scale Integration), physical design is converting an integrated circuit's (IC) logical circuit representation into a physically attainable configuration. The logical design must be translated to produce the geometric representation on a semiconductor substrate. The physical design process is essential to achieve the targeted integrated circuit performance, power efficiency, and manufacturability.


### Introduction to Openlane:
The availability of free RTL designs and EDA tools has made open-source chip creation more accessible. In this regard, the open-source platform provided by Skywater Technologies and Google is vital, as it is the SKY130 PDK. At first, the SKY130 PDK was limited to industrial equipment compatibility, and the design flow was not entirely evident. In order to tackle these problems, the OpenLane initiative was presented. Chip design is expedited with OpenLane, an automated RTL to GDSII path. It is a set of EDA tools, scripts, and optimised Skywater PDKs meant to be used with open-source EDA tools rather than a product.

Here is a brief overview of the key steps in the physical design:

**Floorplanning:** 
Allocating space on the chip for the various functional blocks of the design is part of the floorplanning stage. This step of the physical design process establishes the general size of the chip, the locations of the important parts, and the connections between them.

**Placement:**
This step of physical design process determines the exact locations of individual standard cells or other functional elements on the chip. Aims to minimize the total wire length and optimize for performance and area.

**Global Routing:**
This step of physical design process establishes the high-level connections between different blocks on the chip and defines the general paths that the interconnecting wires will follow.

**Detailed Routing:**
This step of physical design process focuses on the detailed paths of interconnections between individual transistors and gates ,and involves metal layer routing to connect various components while adhering to design rules and constraints.

**Clock Tree Synthesis:**
This step of physical design process designs a network of clock distribution lines to ensure synchronized operation of all components. Further, it aims to minimize clock skew and maintain signal integrity.

**Power Planning:**
This step of physical design process manages the distribution of power throughout the chip to ensure that each component receives the necessary power supply. Moreover, it addresses issues related to power consumption and dissipation.

**Physical Verification:**
This step of physical design process involves checking the layout against design rules and constraints to ensure manufacturability.
This also includes tasks such as design rule checking (DRC) and layout versus schematic (LVS) verification.

**Extraction:**
This step of physical design process extracts parasitic information from the layout, such as resistance and capacitance, for use in subsequent simulations.

**Timing Closure:**
It focuses on meeting the specified timing requirements by adjusting the placement and sizing of components and involves multiple iterations to achieve optimal performance.

**Signoff:**
The final step of physical design process involves obtaining approval from various design tools and signoff criteria. Once signoff is achieved, the design is considered ready for fabrication.



### Preparing the Design:

To include the requirements in the design keep your file structure as follows:

<br />

![tree_Smart_Door_Lock](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/754bfab6-0ebf-47fb-bf63-a5aa1b324aea)

<br />

Preparing the design and including the lef files: The commands to prepare the design and overwite in a existing run folder the reports and results along with the command to include the lef files is given below:

*Note:* Use the command below to resolve the max transition time error: [link for reference](https://github.com/Cal-Poly-RAMP/tapeout-ci-2311/issues/12)
```bash
## Update library files
sed -i 's/max_transition :0.04/max_transition :0.75/g' *.lib
```

To prep the design and run flow in interactive mode type the following in Openlane terminal:

```bash
## Mount directories
make mount

## Run OpenLANE interactively
./flow.tcl -interactive

## Check OpenLANE version
% package require openlane 0.9

## Prepare design
% prep -design project -verbose 99
```

### Synthesis

The process of synthesis involves turning a high-level hardware description into a netlist of logical gates and flip-flops, usually using a hardware description language (HDL) like Verilog or VHDL. Without describing the physical layout, this netlist depicts the circuit's capabilities. By optimising the design for area, power, and time, synthesis tools produce a register transfer level (RTL) description that is used as an input for the VLSI Physical design flow's later stages.

Follow the below command to run synthesis.
```
%run_synthesis
```
<br />

![synthesis](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/47869578-d576-4f08-b85a-17a49fd57cae)

<br />

![synth_stat_rpt](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/4c1eabc2-f9ef-4428-a15f-3da5c98b9361)

<br />

### FloorPlanning

Floor planning involves the calculated allocation of space on a semiconductor chip for various functional blocks. The location of the main components, their connections, and the overall size of the chip are all determined by this procedure. To maximise chip performance, reduce signal latency, and control power distribution, efficient floor planning is essential. It takes into account variables including the distance between crucial components, the length of connections, and the effect of the overall design on manufacturability. 

Follow the below command to run Floorplan.

```
%run_floorplan
```
<br />

![floorplan](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/40cfc999-a9cf-4ae8-8cb1-e8ed9e748788)

<br />

- Post the floorplan run, a .def file will have been created within the results/floorplan directory. We may review floorplan files by checking the floorplan.tcl.
- To view the floorplan: Magic is invoked after moving to the results/floorplan directory,then use the floowing command:
  
To view the floorplan: Magic is invoked after moving to the results/floorplan directory:

```bash
magic -T /home/Openlane/pdk/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read wrapper.def &
```

<br />

![magic_floorplan](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/4b661e2e-f1c5-4ce2-8897-68cb9ff0283c)

<br />

Die area (post floorplan)

<br />

![die_area_fp_rpt](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/ef77f0e2-b17a-47c5-ab43-20387d8c6c5a)

<br />

Core area (post floorplan)

<br />

![core_area_fp_rpt](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/4aa462aa-9c72-4d76-b677-05349216686f)

<br />


## Placement Overview:

In the OpenLANE ASIC flow, standard cells are arranged on floorplan rows and aligned with the locations indicated in the technology LEF file during the placement stage. Placement is done in two steps, Global and Detailed, with the goal of maximising cell locations and guaranteeing legality.

    - Global Placement:
        `Objective:` Finds optimal positions for all cells, allowing potential overlap and disregarding alignment to rows.
        `Optimization:` Focuses on reducing half parameter wire length to enhance overall performance.
        `Result:` Provides a preliminary arrangement, optimizing for wire length but not necessarily adhering to legal placement constraints.

    - Detailed Placement:
        `Objective:` Refines cell positions post-global placement to legalize and align them with floorplan rows.
        `Optimization:` Adjusts cell positions while respecting global placement constraints.
        `Result:` Yields a legally placed layout that aligns with the floorplan and satisfies design rules.

### Placement Execution:
To run the placement process, execute the following command:

```bash
run_placement
```

<br />

![placement](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/8bb0b33e-ffdb-41f4-943e-50c8663fdd41)

<br />

This command advances the design closer to a physically feasible arrangement by starting the Global and Detailed Placement phases. In order to achieve performance and design rule specifications in the ASIC flow's succeeding steps, placement is essential.



Post placement: the design can be viewed on magic within results/placement directory. Run the follwing command in that directory:

```bash
magic -T /home/Openlane/pdk/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read wrapper.def &
```


<br />

![magic_placement](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/6ef40922-6416-47c9-8658-268f371a3412)

<br />


### Clock Tree Synthesis (CTS) Overview:

In order to enable synchronous and dependable operation of the complete integrated circuit, it is necessary to guarantee uniform and low-skew clock signal distribution to all components. Clock distribution lines, buffers, and clock gating cells are designed in a hierarchical tree-like form as part of clock tree analysis. The objectives of this procedure are to minimise clock skew, lower power consumption, and adhere to strict timing specifications. Chip designers can improve the chip's overall performance and reliability by carefully controlling clock signals. To achieve timing closure and guarantee that all sequential elements in the design are synchronised, the clock tree analysis phase is crucial.

command to run cts:
```
%run_cts
```

<br />

![cts](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/46b13a53-c3cf-4674-b28e-6dab487b30b0)

<br />
### Timing, Skew and Power - Performance Reports

<br />

![timing_analysis_cts_clk](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/3575a1e5-cf23-4e10-990c-9a836d53f4c7)

<br />

![clock_skew_rpt_cts](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/2c1038c9-e5b3-4f57-ae55-eff2bf9d91ba)

<br />

### AREA Reports

<br />

![area_rpt_cts](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/2ec4e684-9f7e-4c51-bbd2-a08fb76869e2)

<br />



### Routing:


A critical stage of VLSI physical design is routing, which entails the meticulous design and execution of links between various semiconductor chip components. The routing procedure determines the paths for wires or metal rails to connect the different pieces after placement, which determines their placements. While detailed routing outlines the precise pathways while following design guidelines, global routing establishes the high-level linkages. Minimising signal delays, optimising for area and performance, and achieving predetermined design goals are the goals of efficient routing. In order to manage the difficult task of connecting thousands or millions of components while taking into account variables like congestion, wirelength, and signal integrity, advanced algorithms and optimisation techniques are used. Having effective routing is essential to getting the intended functionality, performance, and manufacturability of the integrated circuit.

### Global Routing:
`Routing Grids:` The routing region is divided into rectangular grids, represented as coarse 3D routes using the Fastroute tool.
`Objective:` Establishes a high-level routing plan across the chip.
`Result:` Provides a global routing framework to guide the subsequent detailed routing stage.

### Detailed Routing:
`Routing Grids and Guides:` Utilizes finer grids and routing guides to implement the physical wiring, employing the TritonRoute tool.
`Features of TritonRoute:`
- Honors pre-processed route guides.
- Assumes that each net satisfies inter-guide connectivity.'
- Utilizes a Mixed-Integer Linear Programming (MILP) based panel routing scheme.
- Implements an intra-layer parallel and inter-layer sequential routing framework.

### Running the Routing Process:
To execute the routing process, use the following command:

```bash
% run_routing
```

This command initiates both the Global Routing and Detailed Routing stages, resulting in a well-defined interconnect system that adheres to design rules and minimizes design rule check (DRC) errors. Proper routing is essential for ensuring signal integrity and meeting performance requirements in the final chip design.

<br />

![routing](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/ed85ee63-0387-414a-ba48-3c3ef55cc2ad)

<br />

View the post-routing design in Magic:

```bash
magic -T /home/Openlane/pdk/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read wrapper.def &
```

<br />

![routing_magic_1](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/11b90d6f-a364-4ab2-be41-87f801cb0894)

<br />

![routing_magic_2_zoom](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/98cbd1bc-8947-4b2d-bbfc-09fc8c66efa2)

<br />

#### post_routing Timing Reports


<br />

![clock_skew_rpt_routing](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/24d34fa6-ca24-4f89-aeca-0b68af3d3089)

<br />

![clock_skew_rpt_routing_1](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/231e7e11-417a-49f9-aece-6a64c80b168c)

<br />

#### post_routing Area Reports

<br />

![area_rpt_routing](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/27f553e9-3737-4a83-b745-b80163bf2853)

<br />


#### post_routing Performance Reports

<br />

![power_routing](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/6d95aece-0bfd-4eb8-9d5e-6a2e1beddd9a)

<br />


DRC violation is zero:

<br />

![drc_vio_zero](https://github.com/Y09mogal/RISCV_Heart_Rate_Monitor/assets/79003694/f97644d6-fe17-483e-9e10-50e2f63d92fb)

<br />

Given a Clock period of 50ns in Json file , setup slack we got after routing is 14.23ns

                              
Max Performance = 1 / (clock period - slack(setup))

Max Performance = 27.95 Mhz




## Word of Thanks
I sciencerly thank **Mr. Kunal Gosh**(Founder/**VSD**) for helping me to complete this flow smoothly.

## Acknowledgement
* Kunal Ghosh, VSD Corp. Pvt. Ltd.
* Mayank Kabra (Founder, Chipcron Pvt. Ltd.)
* Aamod BK, Colleague, IIITB.
* Saket Gurjar, Colleague, IIITB.
* Anshul Madurwar, Colleague, IIITB.

## Contact Information
* Yash Dharemsh Mogal, IMtech 2020, International Institute of Information Technology, Bangalore Yash.Mogal@iiitb.ac.in
* Kunal Ghosh, CEO, and Co-founder, VSD Corp. Pvt. Ltd. kunalghosh@gmail.com
  
## Reference 
* https://github.com/The-OpenROAD-Project/OpenSTA.git
* https://github.com/kunalg123
* https://www.vsdiat.com
* https://github.com/SakethGajawada/RISCV-GNU
