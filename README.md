This is the checkoff 1 code for 1D
=======

Our ALU will function based on the instructions given through the ALUFN[5:0]
------

| Operation | ALUFN[5:0] | Hex |
| ---------- | :-----------:  | :-----------: |
ADD	| 000000 | 0x00
SUB | 000001 | 0x01
MUL	| 000010 | 0x02
DEV	| 000100 | 0x04
MOD	| 000110 | 0x06
AND | 011000 | 0x18
NAND | 010111	| 0x17
OR | 011110 | 0x1E
NOR	| 010001 | 0x11
XOR	| 010110 | 0x16
XNOR | 011001	| 0x19
“A” (LDR)	| 011010 | 0x1A
“B”	| 010101 | 0x15
SHL	| 100000 | 0x20
SHR	| 100001 | 0x21
SRA	| 100011 | 0x23
CMPEQ	| 110011 | 0x33
CMPLT	| 110101 | 0x35
CMPLE	| 110111 | 0x37

Our AU has 2 modes: Self-testing mode and Operate Mode(capble with manual test).

## Operate Mode/Manual Testing Mode
1. Set io_dip[2][7] to 0 to select Operate Mode/Manual Testing mode
2. Set io_dip[2][6] to 0, use io_dip[1:0] to set the first 16 bit input (A) 
3. Set io_dip[2][6] to 1, use io_dip[1:0] to set the second 16 bit input (B)
4. Use io_dip[2][5:0] to set the 6 bit ALUFN value
5. The LEDs io_led[1:0] will show the 16 bit output of the selected instructions and inputs
6. The LEDs io_led[2][2:0] correspond to the n, v and z outputs respectively

## Self Testing Mode
Set io_dip[2][7] to 1 to select Self Testing Mode. If the tests are successful, io_led[1] = 1. The AU will run through the following tests in order:

Operation | ALUFN[5:0] | A | B | Output | Case
:---------: | :------: | :----------------: | :----------------: | :----------------: | ----------------------------
ADD | 000000 | 0100110001110000 | 0000000000000001 | 0100110001110001 | Adder
Overflow (V) | 0000000 | 0100110001110000 | 0100110001110000 | v = 1 | test comparator output
SUB | 000001 | 0100110001110000 | 0000000000000001 | 0100110001101111 | Subtraction
MUL | 000010 | 0100110001110000 | 0000000000000001 | 0100110001110000 | Multiplication
DIV | 000100 | 0100110001110000 | 0000000000000010 | 0010011000111000 | Division
MOD | 000110 | 0100110001110000 | 0000000010000000 | 0000000001110000 | Modulo
AND | 011000 | 0100110001110000 | 0000110001110001 | 0000110001110000 | AND gate
NAND | 010111 | 0100110001110000 | 0000110001110001| 1111001110001111 | NAND gate
OR | 011110 | 0100110001110000| 0000110001110001 | 0100110001110001 | OR gate
NOR | 010001 | 0100110001110000 | 0000110001110001 | 1011001110001110 | NOR gate
XOR | 010110 | 0100110001110000 | 0000110001110001 | 0100000000000001 | XOR gate
XNOR | 011001 | 0100110001110000 | 0000110001110001 | 1011111111111110 | XNOR gate
"A" | 011010 | 0100110001110000 | 0000110001110001 | 0100110001110000 | input A
"B" | 010101 | 0100110001110000 | 0000110001110001 | 0000110001110001 | input B
SHL | 100000 | 0100110001110000 | 0000000000000010 | 0011000111000000 | Shift left by 2
SHR | 100001 | 0100110001110000 | 0000000000000010 | 0001001100011100 | Shift right by 2 no sign extension
SRA | 100011 | 0100110001110000 | 0000000000000010 | 0001001100011100 | Shift right by 2 with sign extension
SRA | 100011 | 1000000000000000 | 0000000000000010 | 1110000000000000 | Shift right by 2 with sign extension
CMPEQ | 110011 | 0100110001110000 | 0100110001110000 | 0000000000000001 | Check if A = B
CMPLT | 110101 | 0100110001110000 | 0000110001110000 | 0000000000000000 | Check if A < B
CMPLE | 110111 | 0100110001110000 | 0000110001110000 | 0000000000000000 | Check if A <= B

