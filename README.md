Download Link: https://assignmentchef.com/product/solved-cda46305636-project-1
<br>
<span class="kksr-muted">Rate this product</span>

Petri Net Simulator of a Simple Processor: In this project you will create a Petri Net simulator for a simplified ARM Processor. Please use C, C++ or Java to develop your simulator. The model will use colored tokens (token with values) rather than the default Petri net. Your simulator should be able to generate step-by-step simulation of the Petri net model of the processor described below. Please go through this document first and then view the sample input/output files in the class assignments page.

We first describe three important places (instruction memory, register file, and data memory) of the Petri net model. Next, we describe ten transitions. The remaining places can be viewed as buffers. All the arc carries (consumes) 1 token unless marked otherwise.

Figure 1. Petri Net Model of a Simple Processor

1

THREE IMPORTANT PLACES

1. Instruction Memory (INM):

The processor to be simulated only supports four types of instructions: Add (ADD), Subtract (SUB), Multiply (MUL) and Store (ST). At a time step, the place denoted as Instruction Memory (INM) can have up to 16 instruction tokens. This is shown as Ii in Figure 1. We will provide an input file (instructions.txt) with instruction tokens. It supports the following instruction format. Please note that the “First Source Operand” is always register. The “Second Source Operand” is register for arithmetic instructions (ADD, SUB and MUL) and immediate value for store (ST) instruction.

&lt;Opcode&gt;, &lt;Destination Register&gt;, &lt;First Source Operand&gt;, &lt;Second Source Operand&gt; Sample instruction tokens and equivalent functionality are shown below:

&lt;ADD, R1, R2, R3&gt; &lt;SUB, R4, R3, R5&gt; &lt;MUL, R7, R2, R3&gt; &lt;ST, R7, R1, 4&gt;

2. Register File (RGF):

R1 = R2 + R3R4 = R3 – R5R7 = R2 * R3 DataMemory [R1+4] = R7

This processor supports up to 16 registers (R0 through R15). At a time step, it can have up to 16 tokens. The token format is &lt;registername, registervalue&gt;, e.g., &lt;R1, 5&gt;. This is shown as Xi in Figure 1. We will provide an input file (registers.txt) with register tokens that you can use to initialize some of the registers.

3. Data Memory (DAM):

This processor supports up to 16 locations (address 0 to 15) in the data memory. At a time step, it can have up to 16 tokens. The token format is &lt;address, value&gt;, e.g., &lt;6, 5&gt; implies that memory address 6 has value 5. This is shown as Di in Figure 1.

TEN TRANSITIONS

1. READ:

The READ transition is a slight deviation from Petri net semantics since it does not have any direct access to instruction tokens. Assume that it knows the top (in-order) instruction in the Instruction Memory (INM). It checks for the availability of the source operands in the Register File (RGF) for the top instruction token and passes them to Instruction Buffer (INB) by replacing the source operands with the respective values. For example, if the top instruction token in INM is &lt;ADD, R1, R2, R3&gt; and there are two tokens in RGF as &lt;R2, 5&gt; and &lt;R3, 7&gt;, then the instruction token in INB would be &lt;ADD, R1, 5, 7&gt; once both READ and DECODE transitions are activated. Therefore, both READ and DECODE transitions are executed together. Please note that when READ consumes two register tokens, it also returns them to RGF in the same time step (no change in RGF due to READ).

2. DECODE:

The DECODE transition consumes the top (in-order) instruction token from INM and updates the values of the source registers with the values from RGF (with the help of READ transition, as described above), and places the modified instruction token in INB.

2

3. ISSUE1:

ISSUE1 transition consumes one (in-order) arithmetic (ADD, SUB or MUL) instruction token (if any) from INB and places it in the Arithmetic Instruction Buffer (AIB). The token format for AIB is same as the token format in INB i.e., &lt;opcode, dest register, source value1, source value2&gt;.

4. ISSUE2:

ISSUE2 transition consumes one (in-order) store (ST) instruction token (if any) from INB and places it in the Store Instruction Buffer (SIB). The token format for SIB is same as the token format in INB i.e., &lt;opcode, dest register, source value1, source value2&gt;.

5. Add – Subtract Unit (ASU)

ASU transition performs arithmetic computations (ADD or SUB) and consumes one instruction (ADD or SUB) token (if any) from AIB, and places the result in the result buffer (REB). The format of the token in result buffer is same as a token in RGF i.e., &lt;destination-register-name, value&gt;.

6. Multiply Unit – Stage 1 (MLU1)

MLU1 consumes one instruction (MLU) token (if any) from AIB, and places the partial result in the partial result buffer (PRB). The token format for PRB is same as the token format in AIB i.e., &lt;opcode, dest register, source value1, source value2&gt;.

7. Multiply Unit – Stage 2 (MLU2)

Computes as per the instruction token from the PRB, and places the final result in the result buffer (REB). The format of the token in result buffer is same as a token in RGF i.e., &lt;destination-register-name, value&gt;.

8. Address Calculation (ADDR)

ADDR transition performs effective address calculation for the store instruction by adding the offset with the source operand. It produces a token as &lt;register-name, data memory address&gt; and places it in the address buffer (ADB).

9. STORE:

The STORE transition consumes a token from ADB and write the data to the data memory for the corresponding address. Assume that you will always have the data from the RGF. It places the data value in the Data Memory (DAM). The format of the token in DAM is already specified, i.e., &lt;address, value&gt;.

10. WRITE

Transfers the result (one token) from the Result Buffer (REB) to the register file (RGF). If there are more than one token in REB in a time step, the WRITE transition writes the token that belongs to the in-order first instruction. You do not have to worry about write-after-write hazard. We will not provide test cases that produces hazards or exceptions.

3

Command Line and Input/Output Formats:

Command Line:

The simulator should be executed with the following command line: ./Psim or java PsimPlease hardcode the input and output files as follows:

Instructions (input): instructions.txt Registers (input): registers.txtData Memory (input): datamemory.txt Simulation (output): simulation.txt

File Formats:

We will provide inputs in the specific format as listed below:

Input Register File Format: (see registers.txt for example)

<pre>&lt;register name,value&gt;...</pre>

Input Data Memory File Format: (see datamemory.txt for example)

&lt;memory address,data value&gt;…Input Instruction Memory File Format (see instructions.txt for example):

&lt;opcode,dest,src1,src2&gt;…Step-by-step Snapshot Output File Format (see simulation.txt for example): Please note that the following comments are not part of the output format.

STEP 0:INM: I1,I2,I3,…INB:AIB:SIB:PRB:ADB:REB:RGF:RF1,RF2,…DAM:D1,D2,…&lt;blank_line&gt;STEP 1:Continue until the end of simulation. End of simulation is determined when none of the transitions can be fired in a time step.Additional Notes: (refer to sample input and output files for ease of understanding)

<ul>

 <li>  STEP 0 values represent initial states of all the places. STEP 1 represents the tokens of all the places atthe end of first time step. In general, STEP i values should reflect the tokens of all the places at the end of time step i. In each time step, you are supposed to execute each transition exactly once (if it has required input tokens at the beginning of that cycle).</li>

 <li>  When there are more than one token in a place, please print them in instruction order (in-order) except for DAM and RGF. The token for DAM and RGF should be printed in the sorted order based on memory address or register name (starting with smallest), respectively.</li>