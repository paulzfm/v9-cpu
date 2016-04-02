# Instruction Set for v9

## Registers

- R0~R7 for general purpose (32-bit)
- F0~F1 for floating-point numeric computation (64-bit)
- RA
- SP for stack pointer (32-bit)
- PC for program counter (32-bit)
- TSP (32-bit)
- FLAGS (32-bit)

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
| NOP | 0000000 000 000 000 000000000000000 | |

### Arithmetic

| Syntax | Machine Code | Semantics |
| :----- | :----------- | :-------- |
| ADD  | 00000001 ra rb rc ... | ra := rb + rc |
| ADDI | 00000010 ra rb ... imm | ra := rb + imm |
| ADDU | 00000011 ra rb rc ... | ra := unsigned(rb) + unsigned(rc) |
| ADDIU| 00000100 ra rb ... imm | ra := unsigned(rb) + unsigned(imm) |
| SUB  | 00000101 ra rb rc ... | ra := rb - rc |
| SUBI | 00000110 ra rb ... imm | ra := rb - imm |
| MUL  | 00000111 ra rb rc ... | ra := rb * rc |
| MULI | 00001000 ra rb ... imm | ra := rb * imm |
| MULU | 00001001 ra rb rc ... | ra := unsigned(rb) * unsigned(rc) |
| MULIU| 00001010 ra rb ... imm | ra := unsigned(rb) * unsigned(imm) |
| DIV  | 00001011 ra rb rc ... | ra := rb / rc |
| DIVI | 00001100 ra rb ... imm | ra := rb / imm |
| DIVU | 00001101 ra rb rc ... | ra := unsigned(rb) / unsigned(rc) |
| DIVIU| 00001110 ra rb ... imm | ra := unsigned(rb) / unsigned(imm) |
| MOD  | 00001111 ra rb rc ... | ra := rb % rc |
| MODI | 00010000 ra rb ... imm | ra := rb % imm |
| MODU | 00010001 ra rb rc ... | ra := unsigned(rb) % unsigned(rc) |
| MODIU| 00010010 ra rb ... imm | ra := unsigned(rb) % unsigned(imm) |

### Logic

| Syntax | Machine Code | Semantics |
| :----- | :----------- | :-------- |
| AND  | 00010011 ra rb rc ... | ra := rb & rc |
| ANDI | 00010100 ra rb ... imm | ra := rb & imm |
| OR   | 00010101 ra rb rc ... | ra := rb | rc |
| ORI  | 00010110 ra rb ... imm | ra := rb | imm |
| XOR  | 00010111 ra rb rc ... | ra := rb ^ rc |
| XORI | 00011000 ra rb ... imm | ra := rb ^ imm |
| SHL  | 00011001 ra rb rc ... | ra := unsigned(rb) << unsigned(rc) |
| SHLI | 00011010 ra rb ... imm | ra := unsigned(rb) << unsigned(imm) |
| SLR  | 00011011 ra rb rc ... | ra := unsigned(rb) >> unsigned(rc) |
| SLRI | 00011100 ra rb ... imm | ra := unsigned(rb) >> unsigned(imm) |
| SAR  | 00011101 ra rb rc ... | ra := rb >> rc |
| SARI | 00011110 ra rb ... imm | ra := rb >> imm |
| EQ   | 00011111 ra rb rc ... | ra := if rb = rc then 1 else 0 |
| EQI  | 00100000 ra rb ... imm | ra := if rb = imm then 1 else 0 |
| NE   | 00100001 ra rb rc ... | ra := if rb = rc then 0 else 1 |
| NEI  | 00100010 ra rb ... imm | ra := if rb = imm then 0 else 1 |
| LT   | 00100011 ra rb rc ... | ra := if rb < rb then 1 else 0 |
| LTI  | 00100100 ra rb ... imm | ra := if rb < imm then 1 else 0 |
| LTU  | 00100101 ra rb rc ... | ra := if unsigned(rb) < unsigned(rb) then 1 else 0 |
| LTUI | 00100110 ra rb ... imm | ra := if unsigned(rb) < unsigned(imm) then 1 else 0 |
| GT   | 00100111 ra rb rc ... | ra := if rb > rb then 1 else 0 |
| GTI  | 00101000 ra rb ... imm | ra := if rb > imm then 1 else 0 |
| GTU  | 00101001 ra rb rc ... | ra := if unsigned(rb) > unsigned(rb) then 1 else 0 |
| GTUI | 00101010 ra rb ... imm | ra := if unsigned(rb) > unsigned(imm) then 1 else 0 |
| LE   | 00101011 ra rb rc ... | ra := if rb <= rb then 1 else 0 |
| LEI  | 00101100 ra rb ... imm | ra := if rb <= imm then 1 else 0 |
| LEU  | 00101101 ra rb rc ... | ra := if unsigned(rb) <= unsigned(rb) then 1 else 0 |
| LEUI | 00101110 ra rb ... imm | ra := if unsigned(rb) <= unsigned(imm) then 1 else 0 |
| GE   | 00101111 ra rb rc ... | ra := if rb >= rb then 1 else 0 |
| GEI  | 00110000 ra rb ... imm | ra := if rb >= imm then 1 else 0 |
| GEU  | 00110001 ra rb rc ... | ra := if unsigned(rb) >= unsigned(rb) then 1 else 0 |
| GEUI | 00110010 ra rb ... imm | ra := if unsigned(rb) >= unsigned(imm) then 1 else 0 |

### Floating-point

| Syntax | Machine Code | Semantics |
| :----- | :----------- | :-------- |
| ADDF | 00110011 ... ... ... ... | F0 := F0 .+ F1 |
| SUBF | 00110100 ... ... ... ... | F0 := F0 .- F1 |
| MULF | 00110101 ... ... ... ... | F0 := F0 .* F1 |
| DIVF | 00110110 ... ... ... ... | F0 := F0 ./ F1 |
| EQF  | 00110111 ... ... ... ... | F0 := if F0 .= F1 then 1 else 0 |
| NEF  | 00111000 ... ... ... ... | F0 := if F0 .= F1 then 0 else 1 |
| LTF  | 00111001 ... ... ... ... | F0 := F0 .< F1 |
| GTF  | 00111010 ... ... ... ... | F0 := F0 .> F1 |
| LEF  | 00111011 ... ... ... ... | F0 := F0 .<= F1 |
| GTF  | 00111100 ... ... ... ... | F0 := F0 .>= F1 |
| POW  | 00111101 ... ... ... ... | F0 := pow(F0, F1) |
| FABS | 00111110 ... ... ... ... | F0 := fabs(F1) |
| LOG  | 00111111 ... ... ... ... | F0 := log(F1) |
| LOGT | 01000000 ... ... ... ... | F0 := log10(F1) |
| EXP  | 01000001 ... ... ... ... | F0 := exp(F1) |
| FLOR | 01000010 ... ... ... ... | F0 := floor(F1) |
| CEIL | 01000011 ... ... ... ... | F0 := ceil(F1) |
| SIN  | 01000100 ... ... ... ... | F0 := sin(F1) |
| COS  | 01000101 ... ... ... ... | F0 := cos(F1) |
| TAN  | 01000110 ... ... ... ... | F0 := tan(F1) |
| ASIN | 01000111 ... ... ... ... | F0 := asin(F1) |
| ACOS | 01001000 ... ... ... ... | F0 := acos(F1) |
| ATAN | 01001001 ... ... ... ... | F0 := arctan(F1) |
| SQRT | 01001010 ... ... ... ... | F0 := sqrt(F1) |
| FMOD | 01001011 ... ... ... ... | F0 := fmod(F0, F1) |

### Conversion

| Syntax | Machine Code | Semantics |
| :----- | :----------- | :-------- |
| CIF | 01001100 ra ... ... ... | F0 := float(ra) |
| CUF | 01001101 ra ... ... ... | F0 := float(unsigned(ra)) |
| CFI | 01001110 ra ... ... ... | ra := signed(F0) |
| CFU | 01001111 ra ... ... ... | ra := unsigned(F0) |

### Branch

| Syntax | Machine Code | Semantics |
| :----- | :----------- | :-------- |
| BZ   | 01010000 ra ... ... imm | if ra = 0 then PC := PC + imm |
| BZF  | 01010001 ... ... ... imm | if F0 = 0 then PC := PC + imm |
| BNZ  | 01010010 ra ... ... imm | if ra != 0 then PC := PC + imm |
| BNZF | 01010011 ... ... ... imm | if F0 != 0 then PC := PC + imm |
| BE   | 01010100 ra rb ... imm | if ra = rb then PC := PC + imm |
| BEF  | 01010101 ... ... ... imm | if F0 = F1 then PC := PC + imm |
| BNE  | 01010110 ra rb ... imm | if ra != rb then PC := PC + imm |
| BNEF | 01010111 ... ... ... imm | if F0 != F1 then PC := PC + imm |
| BLT  | 01011000 ra rb ... imm | if ra < rb then PC := PC + imm |
| BLTU | 01011001 ra rb ... imm | if unsigned(ra) < unsigned(rb) then PC := PC + imm |
| BLTF | 01011010 ... ... ... imm | if F0 < F1 then PC := PC + imm |
| BGT  | 01011011 ra rb ... imm | if ra > rb then PC := PC + imm |
| BGTU | 01011100 ra rb ... imm | if unsigned(ra) > unsigned(rb) then PC := PC + imm |
| BGTF | 01011101 ... ... ... imm | if F0 > F1 then PC := PC + imm |

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
| PUSH | 1111001
| POP  | 1111010
| MTSP | 1111011
| MFPC | 1111100
| MFRA | 1111101
| MTRA | 1111110

### System

| Syntax | Machine Code | Semantics |
| :----- | :----------- | :-------- |
| IDLE | 1111111 000 ... ... ... | response hardware interrupt (include timer) |
| CLI  | 1111111 001 rb ... ... | clear interrupt (rb := IENA, IENA := 0) |
| STI  | 1111111 010 ... ... ... | set interrupt (IENA := 1) |
| IRET | 1111111 011 ... ... ... | return from interrupt |
| BIN  | 1111111 100
| BOUT | 1111111 101
| TRAP | 1111111 110 ... ... imm |
| STIV | 1111111 111 000 rc ... | IVEC := rc |
| STPD | 1111111 111 001 rc ... | PDIR := rc |
| STPG | 1111111 111 010 rc ... | PG := rc |
| STTO | 1111111 111 011 rc ... | TO := rc |
| LVAD | 1111111 111 100 rc ... | rc := VADR |
| HALT | 1111111 111 111 111 1111111111111111 | halt system |

### Memory

| Syntax | Machine Code | Semantics |
| :----- | :----------- | :-------- |
| LMSZ | 1111111 111 101 000 ... | R0 := MEMSZ |
| MCPY | 1111111 111 101 001 ... | memcpy(R0, R1, R2) |
| MCMP | 1111111 111 101 010 ... | memcmp(R0, R1, R2) |
| MCHR | 1111111 111 101 011 ... | memchr(R0, R1, R2) |
| MSET | 1111111 111 101 100 ... | memset(R0, R1, R2) |
