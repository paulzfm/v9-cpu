# Instruction Set for v9

## Registers

- R0~R7 for general purpose
- F0~F1 for floating-point numeric computation
- RA for return address
- SP for stack pointer
- PC for program counter
- FLAGS

All registers have 32 bits.

## Instruction Format

```
+----------+--------+--------+--------+-------+
|  31..25  | 24..22 | 21..19 | 18..16 | 15..0 |
+----------+--------+--------+--------+-------+
|  op_code |   ra   |   rb   |   rc   |  imm  |
+----------+--------+--------+--------+-------+
```

## Instructions

### Instruction Control

| Syntax | Machine Code | Semantics |
| :----- | :----------- | :-------- |
| NOP | 0000000 000 000 000 000000000000000 | do nothing |

### Arithmetic

| Syntax | Machine Code | Semantics |
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

| Syntax | Machine Code | Semantics |
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
| NE   | 0100001 ra rb rc ... | ra := if rb = rc then 0 else 1 |
| NEI  | 0100010 ra rb ... imm | ra := if rb = imm then 0 else 1 |
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

| Syntax | Machine Code | Semantics |
| :----- | :----------- | :-------- |
| ADDF | 0110011 ... ... ... ... | F0 := F0 .+ F1 |
| SUBF | 0110100 ... ... ... ... | F0 := F0 .- F1 |
| MULF | 0110101 ... ... ... ... | F0 := F0 .* F1 |
| DIVF | 0110110 ... ... ... ... | F0 := F0 ./ F1 |
| EQF  | 0110111 ... ... ... ... | F0 := if F0 .= F1 then 1 else 0 |
| NEF  | 0111000 ... ... ... ... | F0 := if F0 .= F1 then 0 else 1 |
| LTF  | 0111001 ... ... ... ... | F0 := F0 .< F1 |
| GTF  | 0111010 ... ... ... ... | F0 := F0 .> F1 |
| LEF  | 0111011 ... ... ... ... | F0 := F0 .<= F1 |
| GTF  | 0111100 ... ... ... ... | F0 := F0 .>= F1 |
| POW  | 0111101 ... ... ... ... | F0 := pow(F0, F1) |
| FABS | 0111110 ... ... ... ... | F0 := fabs(F1) |
| LOG  | 0111111 ... ... ... ... | F0 := log(F1) |
| LOGT | 1000000 ... ... ... ... | F0 := log10(F1) |
| EXP  | 1000001 ... ... ... ... | F0 := exp(F1) |
| FLOR | 1000010 ... ... ... ... | F0 := floor(F1) |
| CEIL | 1000011 ... ... ... ... | F0 := ceil(F1) |
| SIN  | 1000100 ... ... ... ... | F0 := sin(F1) |
| COS  | 1000101 ... ... ... ... | F0 := cos(F1) |
| TAN  | 1000110 ... ... ... ... | F0 := tan(F1) |
| ASIN | 1000111 ... ... ... ... | F0 := asin(F1) |
| ACOS | 1001000 ... ... ... ... | F0 := acos(F1) |
| ATAN | 1001001 ... ... ... ... | F0 := arctan(F1) |
| SQRT | 1001010 ... ... ... ... | F0 := sqrt(F1) |
| FMOD | 1001011 ... ... ... ... | F0 := fmod(F0, F1) |

### Conversion

| Syntax | Machine Code | Semantics |
| :----- | :----------- | :-------- |
| CIF | 1001100 ra ... ... ... | F0 := float(ra) |
| CUF | 1001101 ra ... ... ... | F0 := float(unsigned(ra)) |
| CFI | 1001110 ra ... ... ... | ra := signed(F0) |
| CFU | 1001111 ra ... ... ... | ra := unsigned(F0) |

### Branch

| Syntax | Machine Code | Semantics |
| :----- | :----------- | :-------- |
| BZ   | 1010000 ra ... ... imm | if ra = 0 then PC := PC + imm |
| BZF  | 1010001 ... ... ... imm | if F0 = 0 then PC := PC + imm |
| BNZ  | 1010010 ra ... ... imm | if ra != 0 then PC := PC + imm |
| BNZF | 1010011 ... ... ... imm | if F0 != 0 then PC := PC + imm |
| BE   | 1010100 ra rb ... imm | if ra = rb then PC := PC + imm |
| BEF  | 1010101 ... ... ... imm | if F0 = F1 then PC := PC + imm |
| BNE  | 1010110 ra rb ... imm | if ra != rb then PC := PC + imm |
| BNEF | 1010111 ... ... ... imm | if F0 != F1 then PC := PC + imm |
| BLT  | 1011000 ra rb ... imm | if ra < rb then PC := PC + imm |
| BLTU | 1011001 ra rb ... imm | if unsigned(ra) < unsigned(rb) then PC := PC + imm |
| BLTF | 1011010 ... ... ... imm | if F0 < F1 then PC := PC + imm |
| BGT  | 1011011 ra rb ... imm | if ra > rb then PC := PC + imm |
| BGTU | 1011100 ra rb ... imm | if unsigned(ra) > unsigned(rb) then PC := PC + imm |
| BGTF | 1011101 ... ... ... imm | if F0 > F1 then PC := PC + imm |

### Jump

| Syntax | Machine Code | Semantics |
| :----- | :----------- | :-------- |
| J    | 1011110 ... ... ... imm | PC := PC + imm |
| JR   | 1011111 ra ... ... ... | PC := PC + ra |
| JAL  | 1100000 ... ... ... imm | RA := PC + 4, PC := PC + imm |
| JALR | 1100001 ra ... ... ... | RA := PC + 4, PC := PC + ra |
| JRRA | 1100010 ... ... ... ... | PC := ra |

### Load

| Syntax | Machine Code | Semantics |
| :----- | :----------- | :-------- |
| LW  | 1100011 ra rb ... imm | ra := word(rb + imm) |
| LH  | 1100011 ra rb ... imm | ra := half(rb + imm) |
| LB  | 1100100 ra rb ... imm | ra := byte(rb + imm) |
| LF  | 1100101 ra ... ... imm | F0 := word(ra + imm) |
| LWS | 1100110 ra ... ... imm | ra := word(SP + imm) |
| LHS | 1100111 ra ... ... imm | ra := half(SP + imm) |
| LBS | 1101000 ra ... ... imm | ra := byte(SP + imm) |
| LFS | 1101001 ... ... ... imm | F0 := word(SP + imm) |
| LWI | 1101010 ra ... ... imm | ra := word(PC + imm) |
| LI  | 1101011 ra ... ... imm | ra := imm |
| LIU | 1101100 ra ... ... imm | ra := unsigned(imm) |
| LIH | 1101101 ra ... ... imm | ra := imm << 16 |
| LFI | 1101110 ... ... ... imm | F0 := imm |
| LFIC| 1101111 ... ... ... imm | F0 := float(imm) |

### Store

| Syntax | Machine Code | Semantics |
| :----- | :----------- | :-------- |
| SW  | 1110000 ra rb ... imm | word(rb + imm) := ra |
| SH  | 1110001 ra rb ... imm | half(rb + imm) := ra(15..0) |
| SB  | 1110010 ra rb ... imm | byte(rb + imm) := ra(7..0) |
| SF  | 1110011 ra ... ... imm | word(ra + imm) := F0 |
| SWS | 1110100 ra ... ... imm | word(SP + imm) := ra |
| SHS | 1110101 ra ... ... imm | half(SP + imm) := ra(15..0) |
| SBS | 1110110 ra ... ... imm | byte(SP + imm) := ra(7..0) |
| SFS | 1110111 ... ... ... imm | word(SP + imm) := F0 |
| SWI | 1111000 ra ... ... imm | word(PC + imm) := ra |

### Register

| Syntax | Machine Code | Semantics |
| :----- | :----------- | :-------- |
| PUSH | 1111001 ra ... ... ... | SP := SP + 4, word(SP + 4) := ra |
| POP  | 1111010 ra ... ... ... | ra := word(SP), SP := SP - 4 |
| MTSP | 1111011 ra ... ... imm | SP := ra + imm |
| MFPC | 1111100 ra ... ... ... | ra := PC |
| MFRA | 1111101 ra ... ... ... | ra := RA |
| MTRA | 1111110 ra ... ... imm | RA := ra + imm |

### System

| Syntax | Machine Code | Semantics |
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

| Syntax | Machine Code | Semantics |
| :----- | :----------- | :-------- |
| LMSZ | 1111111 101 001 ra ... | ra := MEMSZ |
| MCPY | 1111111 101 010 ra rb rc ... | memcpy(ra, rb, rc) |
| MCMP | 1111111 101 011 ra rb rc ... | memcmp(ra, rb, rc) |
| MCHR | 1111111 101 100 ra rb rc ... | memchr(ra, rb, rc) |
| MSET | 1111111 101 101 ra rb rc ... | memset(ra, rb, rc) |
