# Instruction Set for v9

## Registers

| Name | Usage | Width | Index |
| :---:| :-----| :----:| :----:|
| T0 | general purpose (caller saved) | 32 bits | 000 |
| T1 | general purpose (caller saved) | 32 bits | 001 |
| T2 | general purpose (caller saved) | 32 bits | 010 |
| S0 | general purpose (callee saved) | 32 bits | 011 |
| S1 | general purpose (callee saved) | 32 bits | 100 |
| FP | frame pointer | 32 bits | 101 |
| SP | stack pointer | 32 bits | 110 |
| PC | program counter | 32 bits | 111 |
| F0 | floating-point numeric computation | 64 bits ||
| F1 | floating-point numeric computation | 64 bits ||
| FLAGS | flag bits | 32 bits ||

Notice that floating-point registers have 64 bits, i.e, they are represented in type `double`. All `float` values will be represented in type `double`.

## Axioms

- Memory must be accessed by **byte**, following **small** endian.
- Stack is 8-byte-aligned, and a 4-byte value is located at the lower four bytes.
- We use ra, rb, rc to represent register T0, T1, T2, S0, S1, FP, SP or PC.
- We use fa, fb, fc to represent register F0 or F1.

## Instruction Format

```
+----------+--------+--------+--------+-------+
|  31..25  | 24..22 | 21..19 | 18..16 | 15..0 |
+----------+--------+--------+--------+-------+
|  op_code |   ra   |   rb   |   rc   |  imm  |
+----------+--------+--------+--------+-------+
```

## Instructions

### Notations

| Notation | Meaning |
| :------- | :-------- |
| \_ := \_ | assign value of the right operand to the left operand (a register or a memory space) |
| \_ + \_ | add signed integers |
| unsigned(\_) + unsigned(\_) | add unsigned integers |
| \_ - \_ | subtract signed integers |
| \_ * \_ | multiply signed integers |
| unsigned(\_) * unsigned(\_) | multiply unsigned integers |
| \_ / \_ | divide signed integers |
| unsigned(\_) / unsigned(\_) | divide unsigned integers |
| \_ % \_ | mod signed integers |
| unsigned(\_) % unsigned(\_) | mod unsigned integers |
| \_ and \_ | bitwise and unsigned integers |
| \_ or \_ | bitwise or unsigned integers |
| \_ xor \_ | bitwise xor unsigned integers |
| not \_ | bitwise negate unsigned integer |
| unsigned(\_) << unsigned(\_) | shift left unsigned integer, the second operand must be in range [0, 32] |
| unsigned(\_) << unsigned(\_) | logically shift right unsigned integer, the second operand must be in range [0, 32] |
| \_ >> \_ | arithmetically shift right signed integer, the second operand must be in range [0, 32] |
| \_ = \_ | whether two integers are equal |
| \_ != \_ | whether two integers are not equal |
| \_ < \_ | whether the first signed integer is less than the second signed integer |
| unsigned(\_) < unsigned(\_) | whether the first unsigned integer is less than the second unsigned integer |
| \_ > \_ | whether the first signed integer is greater than the second signed integer |
| unsigned(\_) > unsigned(\_) | whether the first unsigned integer is greater than the second unsigned integer |
| \_ <= \_ | whether the first signed integer is less or equal than the second signed integer |
| unsigned(\_) <= unsigned(\_) | whether the first unsigned integer is less or equal than the second unsigned integer |
| \_ >= \_ | whether the first signed integer is greater or equal than the second signed integer |
| unsigned(\_) >= unsigned(\_) | whether the first unsigned integer is greater or equal than the second unsigned integer |
| \_ .+ \_ | add floating-point numbers |
| \_ .- \_ | subtract floating-point numbers |
| \_ .* \_ | multiply floating-point numbers |
| \_ ./ \_ | divide floating-point numbers |
| \_ .= \_ | whether two floating-point numbers are equal |
| \_ .!= \_ | whether two floating-point numbers are not equal |
| \_ .< \_ | whether the first floating-point numbers is less than the second floating-point numbers |
| \_ .> \_ | whether the first floating-point numbers is greater than the second floating-point numbers |
| \_ .<= \_ | whether the first floating-point numbers is less or equal than the second floating-point numbers |
| \_ .>= \_ | whether the first floating-point numbers is greater or equal than the second floating-point numbers |
| \_ .% \_ | mod floating-point numbers |
| float(\_) | convert signed integer into floating-point number |
| float(unsigned(\_)) | convert unsigned integer into floating-point number |
| signed(\_) | convert floating-point number into signed number |
| word(\_) | 32 bits from the starting memory address |
| half(\_) | 16 bits from the starting memory address |
| byte(\_) | 8 bits from the starting memory address |
| offset(\_) | shift left the immediate integer and then do signed extension |
|

### Instruction Control

| Name | Machine Code | Meaning |
| :----- | :----------- | :-------- |
| NOP | 0000000 000 000 000 000000000000000 | do nothing |

### Arithmetic

| Name | Machine Code | Meaning |
| :----- | :----------- | :-------- |
| ADD  | 0000001 ra rb rc ... | ra := rb + rc |
| ADDI | 0000010 ra rb ... imm | ra := rb + imm |
| ADDU | 0000011 ra rb rc ... | ra := unsigned(rb) + unsigned(rc) |
| ADDIU| 0000100 ra rb ... imm | ra := unsigned(rb) + unsigned(imm) |
| SUB  | 0000101 ra rb rc ... | ra := rb - rc |
| SUBI | 0000110 ra rb ... imm | ra := rb - imm |
| MUL  | 0000111 ra rb rc ... | ra := rb * rc |
| MULI | 0001000 ra rb ... imm | ra := rb * imm |
| MULU | 0001001 ra rb rc ... | ra := unsigned(rb) * unsigned(rc) |
| MULIU| 0001010 ra rb ... imm | ra := unsigned(rb) * unsigned(imm) |
| DIV  | 0001011 ra rb rc ... | ra := rb / rc |
| DIVI | 0001100 ra rb ... imm | ra := rb / imm |
| DIVU | 0001101 ra rb rc ... | ra := unsigned(rb) / unsigned(rc) |
| DIVIU| 0001110 ra rb ... imm | ra := unsigned(rb) / unsigned(imm) |
| MOD  | 0001111 ra rb rc ... | ra := rb % rc |
| MODI | 0010000 ra rb ... imm | ra := rb % imm |
| MODU | 0010001 ra rb rc ... | ra := unsigned(rb) % unsigned(rc) |
| MODIU| 0010010 ra rb ... imm | ra := unsigned(rb) % unsigned(imm) |

### Logic

| Name | Machine Code | Meaning |
| :----- | :----------- | :-------- |
| AND  | 0010011 ra rb rc ... | ra := rb and rc |
| ANDI | 0010100 ra rb ... imm | ra := rb and imm |
| OR   | 0010101 ra rb rc ... | ra := rb or rc |
| ORI  | 0010110 ra rb ... imm | ra := rb or imm |
| XOR  | 0010111 ra rb rc ... | ra := rb xor rc |
| NOT  | 0011000 ra rb ... ... | ra := not rb |
| SHL  | 0011001 ra rb rc ... | ra := unsigned(rb) << unsigned(rc) |
| SHLI | 0011010 ra rb ... imm | ra := unsigned(rb) << unsigned(imm) |
| SLR  | 0011011 ra rb rc ... | ra := unsigned(rb) >> unsigned(rc) |
| SLRI | 0011100 ra rb ... imm | ra := unsigned(rb) >> unsigned(imm) |
| SAR  | 0011101 ra rb rc ... | ra := rb >> rc |
| SARI | 0011110 ra rb ... imm | ra := rb >> imm |
| EQ   | 0011111 ra rb rc ... | ra := if rb = rc then 1 else 0 |
| EQI  | 0100000 ra rb ... imm | ra := if rb = imm then 1 else 0 |
| NE   | 0100001 ra rb rc ... | ra := if rb != rc then 1 else 0 |
| NEI  | 0100010 ra rb ... imm | ra := if rb != imm then 1 else 0 |
| LT   | 0100011 ra rb rc ... | ra := if rb < rb then 1 else 0 |
| LTI  | 0100100 ra rb ... imm | ra := if rb < imm then 1 else 0 |
| LTU  | 0100101 ra rb rc ... | ra := if unsigned(rb) < unsigned(rb) then 1 else 0 |
| LTUI | 0100110 ra rb ... imm | ra := if unsigned(rb) < unsigned(imm) then 1 else 0 |
| GT   | 0100111 ra rb rc ... | ra := if rb > rb then 1 else 0 |
| GTI  | 0101000 ra rb ... imm | ra := if rb > imm then 1 else 0 |
| GTU  | 0101001 ra rb rc ... | ra := if unsigned(rb) > unsigned(rb) then 1 else 0 |
| GTUI | 0101010 ra rb ... imm | ra := if unsigned(rb) > unsigned(imm) then 1 else 0 |
| LE   | 0101011 ra rb rc ... | ra := if rb <= rb then 1 else 0 |
| LEI  | 0101100 ra rb ... imm | ra := if rb <= imm then 1 else 0 |
| LEU  | 0101101 ra rb rc ... | ra := if unsigned(rb) <= unsigned(rb) then 1 else 0 |
| LEUI | 0101110 ra rb ... imm | ra := if unsigned(rb) <= unsigned(imm) then 1 else 0 |
| GE   | 0101111 ra rb rc ... | ra := if rb >= rb then 1 else 0 |
| GEI  | 0110000 ra rb ... imm | ra := if rb >= imm then 1 else 0 |
| GEU  | 0110001 ra rb rc ... | ra := if unsigned(rb) >= unsigned(rb) then 1 else 0 |
| GEUI | 0110010 ra rb ... imm | ra := if unsigned(rb) >= unsigned(imm) then 1 else 0 |

### Floating-point

| Name | Machine Code | Meaning |
| :----- | :----------- | :-------- |
| ADDF | 0110011 fa fb fc ... | fa := fb .+ fc |
| SUBF | 0110100 fa fb fc ... | fa := fb .- fc |
| MULF | 0110101 fa fb fc ... | fa := fb .* fc |
| DIVF | 0110110 fa fb fc ... | fa := fb ./ fc |
| EQF  | 0110111 fa fb fc ... | fa := if fb .= fc then 1 else 0 |
| NEF  | 0111000 fa fb fc ... | fa := if fb .!= fc then 1 else 0 |
| LTF  | 0111001 fa fb fc ... | fa := fb .< fc |
| GTF  | 0111010 fa fb fc ... | fa := fb .> fc |
| LEF  | 0111011 fa fb fc ... | fa := fb .<= fc |
| GTF  | 0111100 fa fb fc ... | fa := fb .>= fc |
| POW  | 0111101 fa fb fc ... | fa := pow(fb, fc) |
| FABS | 0111110 fa fb ... ... | fa := fabs(fb) |
| LOG  | 0111111 fa fb ... ... | fa := log(fb) |
| LOGT | 1000000 fa fb ... ... | fa := log10(fb) |
| EXP  | 1000001 fa fb ... ... | fa := exp(fb) |
| FLOR | 1000010 fa fb ... ... | fa := floor(fb) |
| CEIL | 1000011 fa fb ... ... | fa := ceil(fb) |
| SIN  | 1000100 fa fb ... ... | fa := sin(fb) |
| COS  | 1000101 fa fb ... ... | fa := cos(fb) |
| TAN  | 1000110 fa fb ... ... | fa := tan(fb) |
| ASIN | 1000111 fa fb ... ... | fa := asin(fb) |
| ACOS | 1001000 fa fb ... ... | fa := acos(fb) |
| ATAN | 1001001 fa fb ... ... | fa := arctan(fb) |
| SQRT | 1001010 fa fb ... ... | fa := sqrt(fb) |
| FMOD | 1001011 fa fb fc ... | fa := fb .% fc |

### Conversion

| Name | Machine Code | Meaning |
| :----- | :----------- | :-------- |
| CIF | 1001100 fa ra ... ... | fa := float(ra) |
| CUF | 1001101 fa ra ... ... | fa := float(unsigned(ra)) |
| CFI | 1001110 ra fa ... ... | ra := signed(fa) |
| CFU | 1001111 ra fa ... ... | ra := signed(fabs(fa)) |

### Branch

| Name | Machine Code | Meaning |
| :----- | :----------- | :-------- |
| BZ   | 1010000 ra ... ... imm | if ra = 0 then PC := PC + offset(imm) |
| BZF  | 1010001 fa ... ... imm | if fa = 0 then PC := PC + offset(imm) |
| BNZ  | 1010010 ra ... ... imm | if ra != 0 then PC := PC + offset(imm) |
| BNZF | 1010011 fa ... ... imm | if fa != 0 then PC := PC + offset(imm) |
| BE   | 1010100 ra rb ... imm | if ra = rb then PC := PC + offset(imm) |
| BEF  | 1010101 fa fb ... imm | if fa .= fb then PC := PC + offset(imm) |
| BNE  | 1010110 ra rb ... imm | if ra != rb then PC := PC + offset(imm) |
| BNEF | 1010111 fa fb ... imm | if fa != fb then PC := PC + offset(imm) |
| BLT  | 1011000 ra rb ... imm | if ra .< rb then PC := PC + offset(imm) |
| BLTU | 1011001 ra rb ... imm | if unsigned(ra) < unsigned(rb) then PC := PC + offset(imm) |
| BLTF | 1011010 fa fb ... imm | if fa .< fb then PC := PC + offset(imm) |
| BGT  | 1011011 ra rb ... imm | if ra > rb then PC := PC + offset(imm) |
| BGTU | 1011100 ra rb ... imm | if unsigned(ra) > unsigned(rb) then PC := PC + offset(imm) |
| BGTF | 1011101 fa fb ... imm | if fa .> fb then PC := PC + offset(imm) |

### Jump

| Name | Machine Code | Meaning |
| :----- | :----------- | :-------- |
| J    | 1011110 ... ... ... imm | PC := PC + offset(imm) |
| JR   | 1011111 ra ... ... ... | PC := PC + ra |

### Stack

| Name | Machine Code | Meaning |
| :----- | :----------- | :-------- |
| POPW | 1100000 000 ra ... ... | ra := word(SP), SP := SP - 8 |
| POPL | 1100000 001 fa ... ... | fa := long(SP - 4), SP := SP - 8 |
| PSHW | 1100001 000 ra ... ... | word(SP + 4) := ra, SP := SP + 8 |
| PSHL | 1100001 001 fa ... ... | long(SP) := fa, SP := SP + 8 |
| CALL | 1100010 000 ... ... imm | ? PC := PC + offset(imm) |
| CALL | 1100010 010 ra ... imm | ? PC := PC + ra |
| RET  | 1100010 001 ... ... ... | ? |

### Load

| Name | Machine Code | Meaning |
| :----- | :----------- | :-------- |
| LW  | 1100011 ra rb ... imm | ra := word(rb + imm) |
| LH  | 1100011 ra rb ... imm | ra := half(rb + imm) |
| LB  | 1100100 ra rb ... imm | ra := byte(rb + imm) |
| LF  | 1100101 fa ra ... imm | fa := word(ra + imm) |
| LWS | 1100110 ra ... ... imm | ra := word(SP + imm) |
| LHS | 1100111 ra ... ... imm | ra := half(SP + imm) |
| LBS | 1101000 ra ... ... imm | ra := byte(SP + imm) |
| LFS | 1101001 fa ... ... imm | fa := long(SP + imm) |
| LWI | 1101010 ra ... ... imm | ra := word(PC + imm) |
| LI  | 1101011 ra ... ... imm | ra := imm |
| LIU | 1101100 ra ... ... imm | ra := unsigned(imm) |
| LIH | 1101101 ra ... ... imm | ra := imm << 16 |
| LFI | 1101110 fa ... ... imm | fa := imm |
| LFIC| 1101111 fa ... ... imm | fa := float(imm) |

### Store

| Name | Machine Code | Meaning |
| :----- | :----------- | :-------- |
| SW  | 1110000 ra rb ... imm | word(rb + imm) := ra |
| SH  | 1110001 ra rb ... imm | half(rb + imm) := ra(15..0) |
| SB  | 1110010 ra rb ... imm | byte(rb + imm) := ra(7..0) |
| SF  | 1110011 ra fa ... imm | long(ra + imm) := fa |
| SWS | 1110100 ra ... ... imm | word(SP + imm) := ra |
| SHS | 1110101 ra ... ... imm | half(SP + imm) := ra(15..0) |
| SBS | 1110110 ra ... ... imm | byte(SP + imm) := ra(7..0) |
| SFS | 1110111 fa ... ... imm | word(SP + imm) := fa |
| SWI | 1111000 ra ... ... imm | word(PC + imm) := ra |

### System

| Name | Machine Code | Meaning |
| :----- | :----------- | :-------- |
| IDLE | 1111111 000 ... ... ... | response hardware interrupt (include timer) |
| CLI  | 1111111 001 001 rc ... | clear interrupt (rc := IENA, IENA := 0) |
| STI  | 1111111 001 010 ... ... | set interrupt (IENA := 1) |
| LUSP | 1111111 010 001 rc ... | rc := USP |
| SUSP | 1111111 010 010 rc ... | USP := rc |
| IRET | 1111111 011 ... ... ... | return from interrupt |
| BIN  | 1111111 100 001 rc ... | rc := getchar() |
| BOUT | 1111111 100 010 rc ... | putchar(rc) |
| TRAP | 1111111 110 ... ... ... | trap |
| STIV | 1111111 111 000 rc ... | IVEC := rc |
| STPD | 1111111 111 001 rc ... | PDIR := rc |
| STPG | 1111111 111 010 rc ... | PG := rc |
| STTO | 1111111 111 011 rc ... | TO := rc |
| LVAD | 1111111 111 100 rc ... | rc := VADR |
| HALT | 1111111 111 111 111 1111111111111111 | halt system |

### Memory

Notice that here we change the instruction format as

```
+--------------+--------+--------+--------+--------+------+
|    31..22    | 21..19 | 18..16 | 15..13 | 12..10 | 9..0 |
+--------------+--------+--------+--------+--------+------+
|  1111111 101 | op_code|   ra   |   rb   |   rc   |      |
+--------------+--------+--------+--------+--------+------+
```

| Name | Machine Code | Meaning |
| :----- | :----------- | :-------- |
| LMSZ | 1111111 101 001 ra ... | ra := MEMSZ |
| MCPY | 1111111 101 010 ra rb rc ... | memcpy(ra, rb, rc) |
| MCMP | 1111111 101 011 ra rb rc ... | memcmp(ra, rb, rc) |
| MCHR | 1111111 101 100 ra rb rc ... | memchr(ra, rb, rc) |
| MSET | 1111111 101 101 ra rb rc ... | memset(ra, rb, rc) |
