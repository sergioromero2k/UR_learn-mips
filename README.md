#  MIPSfromZero

> Learn MIPS assembly language from scratch  step by step, with examples, exercises, and a full cheat sheet.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![MIPS](https://img.shields.io/badge/language-MIPS%20Assembly-blue.svg)]()

---

##  About

**MIPSfromZero** is a hands-on learning repository for anyone wanting to understand MIPS assembly from the ground up. Whether you're a student tackling computer architecture for the first time, a self-learner curious about low-level programming, or an enthusiast exploring how processors really work  this repo has you covered.

It covers:
- Basic concepts & register usage
- Control structures (if/else, loops)
- Memory manipulation & arrays
- System calls (I/O)
- Functions & stack management
- And more!

---

##  Repository Structure

```
MIPSfromZero/
 01_basics/          # Registers, data types, simple instructions
 02_arithmetic/      # ALU operations
 03_control_flow/    # Branches and jumps
 04_memory/          # Load/store, arrays
 05_syscalls/        # I/O with the OS
 06_functions/       # Procedures and the stack
 exercises/          # Practice problems
 solutions/          # Exercise solutions
```

---

##  Getting Started

1. Install [MARS MIPS Simulator](http://courses.missouristate.edu/KenVollmar/MARS/) or [SPIM](http://spimsimulator.sourceforge.net/)
2. Clone this repository:
   ```bash
   git clone https://github.com/your-username/MIPSfromZero.git
   ```
3. Open any `.asm` file in MARS or SPIM and hit **Run**!

---

##  MIPS Cheat Sheet

###  Registers

| Register | Name | Use |
|----------|------|-----|
| `$zero` | `$0` | Always 0 (constant) |
| `$at` | `$1` | Assembler temporary |
| `$v0$v1` | `$2$3` | Return values / syscall codes |
| `$a0$a3` | `$4$7` | Function arguments |
| `$t0$t9` | `$8$15, $24$25` | Temporary (not preserved) |
| `$s0$s7` | `$16$23` | Saved temporaries (preserved) |
| `$sp` | `$29` | Stack pointer |
| `$fp` | `$30` | Frame pointer |
| `$ra` | `$31` | Return address |

---

###  Program Structure

```masm
.data
    msg: .asciiz "Hello, MIPS!\n"
    num: .word 42

.text
.globl main
main:
    # your code here
    li $v0, 10      # syscall 10 = exit
    syscall
```

---

###  Arithmetic Instructions

```masm
add  $t0, $t1, $t2    # $t0 = $t1 + $t2  (with overflow check)
addu $t0, $t1, $t2    # $t0 = $t1 + $t2  (no overflow check)
addi $t0, $t1, 10     # $t0 = $t1 + 10   (immediate)
sub  $t0, $t1, $t2    # $t0 = $t1 - $t2
mul  $t0, $t1, $t2    # $t0 = $t1 * $t2  (MARS pseudo-instruction)
div  $t1, $t2         # HI = $t1 % $t2 / LO = $t1 / $t2
mflo $t0              # $t0 = LO (quotient)
mfhi $t0              # $t0 = HI (remainder)
```

---

###  Logical Instructions

```masm
and  $t0, $t1, $t2    # bitwise AND
or   $t0, $t1, $t2    # bitwise OR
nor  $t0, $t1, $t2    # bitwise NOR
xor  $t0, $t1, $t2    # bitwise XOR
andi $t0, $t1, 0xFF   # AND with immediate
ori  $t0, $t1, 0x01   # OR  with immediate
sll  $t0, $t1, 2      # shift left  logical (x4)
srl  $t0, $t1, 2      # shift right logical (÷4)
sra  $t0, $t1, 2      # shift right arithmetic (preserves sign)
```

---

###  Memory (Load / Store)

```masm
lw   $t0, 0($s0)      # load  word  (4 bytes)
lh   $t0, 0($s0)      # load  half  (2 bytes, sign-extended)
lb   $t0, 0($s0)      # load  byte  (1 byte,  sign-extended)
lhu  $t0, 0($s0)      # load  half  (unsigned)
lbu  $t0, 0($s0)      # load  byte  (unsigned)
sw   $t0, 0($s0)      # store word
sh   $t0, 0($s0)      # store half
sb   $t0, 0($s0)      # store byte
la   $t0, label       # load address of label
li   $t0, 42          # load immediate value
```

---

###  Control Flow

```masm
# Branches
beq  $t0, $t1, label  # branch if $t0 == $t1
bne  $t0, $t1, label  # branch if $t0 != $t1
blt  $t0, $t1, label  # branch if $t0 <  $t1  (pseudo)
ble  $t0, $t1, label  # branch if $t0 <= $t1  (pseudo)
bgt  $t0, $t1, label  # branch if $t0 >  $t1  (pseudo)
bge  $t0, $t1, label  # branch if $t0 >= $t1  (pseudo)
bgtz $t0, label       # branch if $t0 > 0
beqz $t0, label       # branch if $t0 == 0

# Jumps
j    label            # unconditional jump
jr   $ra              # jump to address in register (return)
jal  label            # jump and link (call function)

# Set on less than
slt  $t0, $t1, $t2   # $t0 = 1 if $t1 < $t2, else 0
slti $t0, $t1, 10    # $t0 = 1 if $t1 < 10,  else 0
```

---

###  Common Syscalls

| `$v0` | Service | Arguments | Result |
|-------|---------|-----------|--------|
| `1` | Print integer | `$a0` = integer |  |
| `2` | Print float | `$f12` = float |  |
| `4` | Print string | `$a0` = address of string |  |
| `5` | Read integer |  | `$v0` = integer |
| `6` | Read float |  | `$f0` = float |
| `8` | Read string | `$a0` = buffer, `$a1` = length |  |
| `9` | Allocate heap | `$a0` = bytes | `$v0` = address |
| `10` | Exit |  |  |
| `11` | Print char | `$a0` = char (ASCII) |  |
| `12` | Read char |  | `$v0` = char (ASCII) |

```masm
# Example: print "Hello!"
.data
    hello: .asciiz "Hello!\n"
.text
    la  $a0, hello
    li  $v0, 4
    syscall
```

---

###  Functions & Stack

```masm
# Calling a function
jal  my_function       # call: saves PC+4 in $ra
# ... (execution returns here after jr $ra)

# Function template
my_function:
    # prologue: save registers
    addi $sp, $sp, -8
    sw   $ra, 4($sp)
    sw   $s0, 0($sp)

    # function body
    # ...

    # epilogue: restore registers
    lw   $ra, 4($sp)
    lw   $s0, 0($sp)
    addi $sp, $sp, 8
    jr   $ra            # return
```

---

###  Data Directives

```masm
.data
    myWord:   .word   100         # 32-bit integer
    myByte:   .byte   65          # 8-bit value (65 = 'A')
    myHalf:   .half   512         # 16-bit integer
    myFloat:  .float  3.14        # single-precision float
    myDouble: .double 2.718281    # double-precision float
    myStr:    .asciiz "hello"     # null-terminated string
    myArr:    .word   1, 2, 3, 4  # array of 4 words
    mySpace:  .space  20          # 20 bytes of zeroed space
```

---

###  Common Patterns

**If / Else:**
```masm
    bne  $t0, $t1, else
    # if body
    j    end_if
else:
    # else body
end_if:
```

**While loop:**
```masm
loop:
    beq  $t0, $zero, end_loop
    # loop body
    j    loop
end_loop:
```

**For loop (i = 0 to 9):**
```masm
    li   $t0, 0         # i = 0
    li   $t1, 10        # limit
for:
    bge  $t0, $t1, end_for
    # loop body
    addi $t0, $t0, 1   # i++
    j    for
end_for:
```

**Traverse an array:**
```masm
.data
    arr: .word 10, 20, 30, 40, 50
.text
    la   $t0, arr       # base address
    li   $t1, 5         # array length
    li   $t2, 0         # index i = 0
traverse:
    bge  $t2, $t1, end_traverse
    sll  $t3, $t2, 2    # offset = i * 4
    add  $t3, $t0, $t3  # address = base + offset
    lw   $t4, 0($t3)    # load arr[i]
    addi $t2, $t2, 1
    j    traverse
end_traverse:
```

---

##  Tools & Simulators

| Tool | Platform | Notes |
|------|----------|-------|
| [MARS](http://courses.missouristate.edu/KenVollmar/MARS/) | Windows/Mac/Linux | Recommended for beginners |
| [SPIM](http://spimsimulator.sourceforge.net/) | Windows/Mac/Linux | Lightweight, CLI support |
| [QtSPIM](http://spimsimulator.sourceforge.net/) | Windows/Mac/Linux | GUI version of SPIM |
| [WebMIPS](https://www.webmips.com/) | Browser | No install needed |

---

##  Contributing

Contributions, issues and feature requests are welcome!

1. Fork the project
2. Create your branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m 'Add some feature'`
4. Push to the branch: `git push origin feature/your-feature`
5. Open a Pull Request

---

##  License

Distributed under the MIT License. See `LICENSE` for more information.

---

<p align="center">Made with  for anyone daring enough to go low-level</p>
