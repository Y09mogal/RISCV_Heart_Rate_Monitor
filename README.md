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

## C Code

```
void delaytime(int);

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
	int sm_reg = sm*2048;
	
	//int flag_pwd = 0;
	
	//int mask_code = 0xFFFFFBFF;
	

	//asm code to set initial servo motor position - CLOSE
	asm volatile(
		"or x30, x30, %0\n\t" 
		:
		:"r"(sm_reg)
		:"x30"
		);



	while (1) {


	//  asm code to read push button values
	asm volatile(
		"andi %0, x30, 1\n\t"
		:"=r"(pb1)
		:
		:
		);
		
	asm volatile(
		"andi %0, x30, 2\n\t"
		:"=r"(pb2)
		:
		:
		);
		

	asm volatile(
		"andi %0, x30, 4\n\t"
		:"=r"(pb3)
		:
		:
		);
	
	asm volatile(
		"andi %0, x30, 8\n\t"
		:"=r"(pb1)
		:
		:
		);
	
	asm volatile(
		"andi %0, x30, 16\n\t"
		:"=r"(pb4)
		:
		:
		);
	
	asm volatile(
		"andi %0, x30, 32\n\t"
		:"=r"(pb5)
		:
		:
		);
	
	asm volatile(
		"andi %0, x30, 64\n\t"
		:"=r"(pb6)
		:
		:
		);
	
	asm volatile(
		"andi %0, x30, 128\n\t"
		:"=r"(pb7)
		:
		:
		);
	
	asm volatile(
		"andi %0, x30, 256\n\t"
		:"=r"(pb8)
		:
		:
		);
	
	asm volatile(
		"andi %0, x30, 512\n\t"
		:"=r"(pb9)
		:
		:
		);
	
	asm volatile(
		"andi %0, x30, 1024\n\t"
		:"=r"(master_pb)
		:
		:
		);
		


	//Door unlocks only when master push button is pressed OR passcode design of pb2, pb5 and pb6 is pressed simultaneously
	if (master_pb == 1 || (pb2 == 1 && pb5 == 1 && pb6 == 1))
		{
		sm=1;
		sm_reg = sm*2048;
		
		//asm code to set servo motor position - OPEN
		asm volatile(
		"or x30, x30, %0\n\t" 
		:
		:"r"(sm_reg)
		:"x30"
		);


		delaytime(1000);
		}
	else
		{
		sm=0;
		sm_reg = sm*2048;
		
		//asm code to set servo motor position - CLOSE
		asm volatile(
		"or x30, x30, %0\n\t" 
		:
		:"r"(sm_reg)
		:"x30"
		);

		}

	}
	
	delaytime(500);
	return 0;

}

void delaytime(int seconds) {
    int i, j;
    for (i = 0; i < seconds; i++) {
        for (j = 0; j < 1000000; j++) {
            //loop for delay
        }
    }
}
```

## Assembly Code

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

00010188 <delaytime>:
   10188:	fd010113          	add	sp,sp,-48
   1018c:	02812623          	sw	s0,44(sp)
   10190:	03010413          	add	s0,sp,48
   10194:	fca42e23          	sw	a0,-36(s0)
   10198:	fe042623          	sw	zero,-20(s0)
   1019c:	0340006f          	j	101d0 <delaytime+0x48>
   101a0:	fe042423          	sw	zero,-24(s0)
   101a4:	0100006f          	j	101b4 <delaytime+0x2c>
   101a8:	fe842783          	lw	a5,-24(s0)
   101ac:	00178793          	add	a5,a5,1
   101b0:	fef42423          	sw	a5,-24(s0)
   101b4:	fe842703          	lw	a4,-24(s0)
   101b8:	000f47b7          	lui	a5,0xf4
   101bc:	23f78793          	add	a5,a5,575 # f423f <__global_pointer$+0xe284f>
   101c0:	fee7d4e3          	bge	a5,a4,101a8 <delaytime+0x20>
   101c4:	fec42783          	lw	a5,-20(s0)
   101c8:	00178793          	add	a5,a5,1
   101cc:	fef42623          	sw	a5,-20(s0)
   101d0:	fec42703          	lw	a4,-20(s0)
   101d4:	fdc42783          	lw	a5,-36(s0)
   101d8:	fcf744e3          	blt	a4,a5,101a0 <delaytime+0x18>
   101dc:	00000013          	nop
   101e0:	00000013          	nop
   101e4:	02c12403          	lw	s0,44(sp)
   101e8:	03010113          	add	sp,sp,48
   101ec:	00008067          	ret
```

## Unique Instructions

```
Number of different instructions: 16
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

## Acknowledgments
* Kunal Ghosh, CEO, and Co-founder, VSD Corp. Pvt. Ltd.
* Mayank Kabra, IIITB
* Aamod BK, IIITB
* Saket Gurjar, IIITB

## Contact Information
* Yash Dharemsh Mogal, IMtech 2020, International Institute of Information Technology, Bangalore Yash.Mogal@iiitb.ac.in
* Kunal Ghosh, CEO, and Co-founder, VSD Corp. Pvt. Ltd. kunalghosh@gmail.com


## References
- https://github.com/SakethGajawada/RISCV_GNU

