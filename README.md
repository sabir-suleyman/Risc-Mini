## Read this File in other languages:
<a href="README.md"><img src="https://img.shields.io/badge/-ENGLISH-red?style=for-the-badge"></a>
<a href="README.tr.md"><img src="https://img.shields.io/badge/-T%C3%9CRK%C3%87E-red?style=for-the-badge"></a>

# 🧩 Project brief
Inspired by the 32I Extension of the RISC-V ISA, a RISC (Reduced Instruction Set Computer) Instruction Set Architecture (ISA) was designed and built on a microprocessor with 32 registers and 5 flags. The designed instruction set architecture (hereinafter abbreviated as ICC) is based on the x86 and RISCV dialects of the assembly language and a transpiler and parser for the custom designed ICC and a simulation environment to execute the instructions. The new dialect, which we call "RISC-Mini", basically consists of 21 instructions. The simulation environment is available in two different forms, both graphical and console. Figma was used for the graphical interface design and Python's Tkinter library was used for the coding.



# 🏆 Team Members

| Name                  | GitHub Username                                     |
|-----------------------|-----------------------------------------------------|
| Sabir SÜLEYMANLI      | [sabir-suleyman](https://github.com/sabir-suleyman) |
| Damla SOYDAN          | [damlas21](https://github.com/damlas21)             |
| Ferit Yiğit BALABAN   | [fybx](https://github.com/fybx)                     |
| İrem İÇÖZ             | [irem0](https://github.com/irem0)                   |
| Zeynep KILINÇER       | [zkilincer](https://github.com/zkilincer)           |

<br>

# 1. INTRODUCTION 💡

## 1.1. Importance of Project Proposal
Nowadays, the best way to start learning microprocessor design is to study how historical processors and chipsets were invented. At this point, the simulation tools we have written provide equal opportunity by making it easier for those who do not physically own the chip to use the necessary training and experimentation environments. Learning the inner workings of a microprocessor by experiencing a real chip that has changed the world will provide anecdotal information for students using this tool for their future studies and research.

## 1.2. Goals

Providing a real-time microprocessor simulation, displaying microprocessor-specific assembly language instruction inputs as machine code on the fly, and modifying a virtual memory to make the hardware interface of a real computer easily accessible and usable in experimental environments.

# 2. Details of the Software 👨‍💻

## 2.1. Internal Structure of the Microprocessor

### 2.1.1. Registers
Processor 32 32-bit wide signed integers ordered from `x0` to `x31` has a general purpose font that holds.

### 2.1.2. Flags

| Flag Abbreviation | Description                 |
|-------------------|-----------------------------|
| C                 | carry                       |
| Z                 | zero                        |
| S                 | sign                        |
| V                 | overflow                    |
| XLEN              | Holds 32 in integer form    |

Next to the registers are flag registers addressable by C, Z, S and V. The flags are also 32-bit wide but should only be used to store 0h and 1h values.

## 2.2 RISC-Mini Assembly Dialect Syntax and KKM

![image](https://github.com/fybx/bmb2014/assets/41127439/efd836d4-0d1f-4da5-a5e2-bc6cb9651ad7)

### 2.2.1. Arithmetic Instructions

| Mnemonic | Syntax             | Description                            |
|----------|--------------------|----------------------------------------|
| add      | `add rd, rs1, rs2` | `rd` ← (`rs1` + `rs2`)                 |
| inv      | `inv rd`           | `rd` ← (-1 * `rd`)                     |
| sub      | `sub rd, rs1, rs2` | `rd` ← (`rs1` - `rs2`)                 |
| slt      | `slt rd, rs1, rs2` | `rd` ← (`rs1` < `rs2` ? `rs1` : `rs2`) |
| nop      | `nop`              | `x0` ← `x0`                            |

### 2.2.2. Logical Instructions

| Mnemonic | Syntax             | Description             |
|----------|--------------------|-------------------------|
| `and`    | `and rd, rs1, rs2` | `rd` ← (`rs1` & `rs2`)  |
| `or`     | `or rd, rs1, rs2`  | `rd` ← (`rs1` \| `rs2`) |
| `xor`    | `xor rd, rs1, rs2` | `rd` ← (`rs1` ^ `rs2`)  |
| `shl`    | `shl rd, rs1, rs2` | `rd` ← (`rs1` << `rs2`) |
| `shr`    | `shr rd, rs1, rs2` | `rd` ← (`rs1` >> `rs2`) |

### 2.2.3. Branch Instructions

| Mnemonic | Syntax                  | Description                                                         |
|----------|-------------------------|---------------------------------------------------------------------|
| `jmp`    | `jmp section`           | Jumps to `section` by storing the program counter in `x30` register |  
| `beq`    | `beq rs1, rs2, section` | `rs1` == `rs2` ? `jmp section` : `nop`                              |
| `bne`    | `bne rs1, rs2, section` | `rs1` != `rs2` ? `jmp section` : `nop`                              |
| `bge`    | `bge rs1, rs2, section` | `rs1` >= `rs2` ? `jmp section` : `nop`                              |
| `ble`    | `ble rs1, rs2, section` | `rs1` <= `rs2` ? `jmp section` : `nop`                              |


### 2.2.4. Memory Instructions

| Mnemonic | Syntax                 | Description                                                            |
|----------|------------------------|------------------------------------------------------------------------|
| `lfm`    | `lfm rd, [hex_value]h` | Loads the value of memory at `hex_value` into the `rd` register        |
| `stm`    | `stm rd, [hex_value]h` | Stores the value in the `rd` register at `hex_value` in memory         |
| `mov`    | `mov rd, rs1`          | `rd` ← `rs1`                                                           |
| `mvi`    | `mvi rd, [hex_value]h` | `rd` ← `hex_value`                                                     |

### 2.2.5. Additional Instructions

| Mnemonic | Syntax                       | Description                                                                                  |
|----------|------------------------------|----------------------------------------------------------------------------------------------|
| `cll`    | `cll`                        | Makes a system call.                                                                         |
| `dbs`    | `dbs name \"quoted_string\"` | Stores a null-terminated character array in memory that can be referenced by `name`.         |


## 2.3. Microprocessor Calls (Syscalls / Interrupts, System Calls / Interrupts

### 2.3.1. Stop System (HALT)

| Register | Expected Value                 | 
|----------|--------------------------------|
| `x1`     | 0                              |
| `X2`     | Status code returned by the operation |

### 2.3.2. Print Register Value on Screen

| Register | Expected Value                                         | 
|----------|--------------------------------------------------------|
| `x1`     | 1                                                      |
| `x2`     | The value in this register will be written to the screen.   |
| `x3`     | Number format (0: binary; 1: decimal; 2: hexadecimal; 3: utf8) |


### 2.3.4. Print String to Screen

| Register | Expected Value                 | 
|----------|--------------------------------|
| `x1`     | 2                              |
| `X2`     | Address of the string in memory     |

### 2.3.5. Read Character from Keyboard

| Register | Expected Value                             | 
|----------|--------------------------------------------|
| `x1`     | 3                                          |
| `X2`     | Memory address to save the character     |

### 2.3.6. Read String Input from Keyboard

It saves the numeric value of the read character in the UTF-8 encoding system either to the `x4` register or to the memory address given in the `x2` register according to the value in the `x3` register. Note that in case of writing to memory, `0` will be written to the address after the destination register address.

| Register | Expected Value                                                             | 
|----------|----------------------------------------------------------------------------|
| `x1`     | 4                                                                          |
| `x2`     | Starting address of the memory block where the character will be stored                  |
| `x3`     | Selects the register location (0: register `x4`; 1: memory address in register `x2`) |

### 2.3.7. Read Number from Keyboard

Converts the read character string to a number according to the given number acceptance format and stores it in the `x3` register stores the number. If the number cannot be read in the given format, the value `1` is stored in the `x4` register.

| Register | Expected Value                                           | 
|----------|----------------------------------------------------------|
| `x1`     | 5                                                        |
| `X2`     | Number acceptance format (0: binary; 1: decimal; 2: hexadecimal)     |


# 3. METHOD 📚

There are two main criteria we consider when choosing a microprocessor to simulate the instruction set: Simplicity of the instruction set and similarity to current processor designs. Keeping these criteria in mind, we analyzed the Zilog Z80, Konrad Zuse's Z2, Intel C4004, 8086 and 8088 in detail. Finally, we analyzed RISCV and decided to create our own microprocessor "RISC-Mini" inspired by it. We determined the basic components of the graphical interface environment to be developed as follows:

1. Virtual processor runtime engine
2. A virtual memory from which the processor reads its instructions, accessible to the user through the runtime engine
3. Two types of registers, general purpose and flag, in which the processor stores data registers during execution
4. A parser that parses and checks the input from the source code editor and places it in virtual memory for the processor to access
5. A source editor that accepts Assembly language code from the user and then displays changes in memory and registers

For the design of the interface, it was decided to use the Figma tool, which is widely accepted by the community, and for GUI development with Python, the Tkinter module, which is the most basic and widespread, was chosen for use.


# 4. THEORETICAL BASIS and SOURCE RESEARCH 🔎

Before starting our work, we started by investigating the requirements of the microprocessor and why it should be used. 

Microprocessors are used as a fundamental component in today's computer and electronic systems. These systems are controlled and managed by instructions executed by the microprocessor.

Memory must store blocks of memory in a list to perform operations. The data type ensures that the data to be stored in memory is stored in a word-length size of memory space.

The processor executes the main processing tasks in the microprocessor. It accesses memory and performs operations on memory. The microprocessor has methods for executing its instructions. Processor instructions can include a number of operations, for example, loading memory addresses, writing to memory addresses, performing numeric operations, performing logical operations, making comparisons, and making assignments. The processor must determine which operation to execute by decoding the code. It should be used to execute different operations for different codes. The processor should contain an array representing registers to temporarily store the data of specific operations that have taken place. 

Flags should be used to identify states and errors. Flags are set after the execution of certain operations. 

The user interface is used to enable interactive control of the microprocessor simulation by the user. The user interface contains a text box where the user can enter the code. There is an area where we can monitor the changes in memory, registers and flags after the code is processed. Being able to follow these areas step by step provides debugging functionality.


# 5. CONCLUSION (DISCUSSION and CONCLUSION) 💬

Virtualized microprocessor interpretation environments offer many advantages. These include the ease of working on different microprocessor architectures, the ability to work at various scaling levels, and the possibility of efficient code testing and development. Virtualized microprocessor interpretation environments allow a larger number of systems and devices to be emulated with fewer hardware requirements.

However, virtualized microprocessor interpretation environments also have some disadvantages. The biggest disadvantage is that performance issues that may occur in the real system cannot be emulated as desired. In addition, virtualized microprocessor interpretation environments are built by combining simulation technologies, which places a higher burden on system resources. In conclusion, virtualized microprocessor interpretation environments are an important tool for testing and development of microprocessor-based systems. With their different features and advantages, they should be evaluated according to users' needs. Virtualized microprocessor interpretation environments can be used in the design and testing of new devices and systems that need to be developed. However, the disadvantage of not being able to exactly mimic real system performance should be taken into account.

# 6. Examples Running on KKM 📝

## 6.1. Program that adds 2 numbers received from the user

```
; toplama.asm

.global
  mvi x9, Ah ; x9 yazmacinda yeni satir karakterini tut
  
  mvi x1, 5h ; syscall no.5 hazirligi
  mvi x2, 1h ; sayiyi onlu sistemde oku
  cll ; cagriyi gerceklestir
  mov x20, x3 ; okunan sayiyi x20'de sakla
  
  mvi x1, 2h ; syscall no.2 hazirligi
  mvi x2, [str0] ; ekrana yazilacak stringin adresini sakla
  cll ; cagriyi gerceklestir
  
  mvi x1, 5h ; syscall no.5 hazirligi
  mvi x2, 1h ; sayiyi onlu sistemde oku
  cll ; cagriyi gerceklestir
  mov x21, x3 ; okunan sayiyi x21'de sakla
  
  mvi x1, 2h ; syscall no.2 hazirligi  
  mvi x2, [str1] ; ekrana yazilacak stringin adresini sakla
  cll ; cagriyi gerceklestir

  mvi x1, 1h ; syscall no.1 hazirligi
  add x2, x20, x21 ; ekrana yazilacak sayiyi (toplami) x2'de sakla
  mvi x3, 1h ; sayiyi onlu sistemde yaz
  cll ; cagriyi gerceklestir

  mvi x1, 2h ; syscall no.2 hazirligi
  mov x2, x9 ; yeni satir karakterini sakla
  cll ; cagriyi gerceklestir

  mov x1, x0 ; islemciyi durdur
  mov x2, x0 ; durum kodu 0
  cll

.store
  dbs str0, " + "
  dbs str1, " = "
  
  ```

## 6.2. A program that takes the user's name and writes it to the screen

```
.global
  mvi x9, 10h ; yeni satır karakterini x9'da sakla
  
  mvi x1, 2h
  mvi x2, [str0]
  cll
  
  mvi x1, 4h
  mvi x2, [FFh]
  cll
  mvi x1, 2h
  mvi x2, [str1]
  cll
  mvi x2, [FFh]
  cll
  mvi x2, [str2]
  cll
  mvi x1, 1h
  mov x2, x9
  mvi x3, 4h
  cll
  
  mov x1, x0
  mov x2, x0
  cll
 
 .store
  dbs str0, "Adınızı girin: "
  dbs str1, "Merhaba, "
  dbs str2, "!"

```

# 7. Images from the Program 📸

## 7.1. GUI (Graphical Interface) Design

![image](https://github.com/sabir-suleyman/Risc-Mini/blob/main/download%20(1).png)

## 7.2. RISC-Mini Instance Running on TUI (Terminal Interface)

![image](https://github.com/sabir-suleyman/Risc-Mini/blob/main/download%20(2).png)

# 8. SOURCES ⚙

1. https://www.eng.auburn.edu/~sylee/ee2220/8086_instruction_set.html
2. https://docs.python.org/3/library/tkinter.html
3. https://dev.to/yash_makan/4-ways-to-create-modern-gui-in-python-in-easiest-way-possible5e0e
4. https://www.activestate.com/blog/top-10-python-gui-frameworks-compared/
5. https://en.wikipedia.org/wiki/Intel_8086
6. http://www.math.uaa.alaska.edu/~afkjm/cs221/handouts/irvine2.pdf
7. https://www.youtube.com/watch?v=Ps0JFsyX2fU
8. https://en.wikipedia.org/wiki/RISC-V
9. https://www.youtube.com/watch?v=Qd-jJjduWeQ
10. https://www.youtube.com/watch?v=66jIYW5kbj4
11. https://www.youtube.com/watch?v=7A_csP9drJw
12. https://www.cs.cornell.edu/courses/cs3410/2019sp/riscv/interpreter/
13. https://riscv.org/wp-content/uploads/2017/05/riscv-spec-v2.2.pdf
14. https://marz.utk.edu/my-courses/cosc230/book/example-risc-v-assembly-programs/
15. https://msyksphinz-self.github.io/riscv-isadoc/html/regs.html
16. https://msyksphinz-self.github.io/riscv-isadoc/html/rvi.html
17. https://itnext.io/risc-v-instruction-set-cheatsheet-70961b4bbe8?gi=a48779e4d7eb
18. https://www.pcpolytechnic.com/it/ppt/8086_instruction_set.pdf
19. https://web.karabuk.edu.tr/emelkocak/indir/MTM305/KOMUT%20SET%C4%B0.pdf
20. https://aakgul.sakarya.edu.tr/sites/aakgul.sakarya.edu.tr/file/_8086KomutlarOrnekler.PDF
21. https://en.wikipedia.org/wiki/MOS_Technology_6508
22. https://en.wikipedia.org/wiki/Intel_4004
23. https://en.wikipedia.org/wiki/Zilog_Z80
24. https://en.wikipedia.org/wiki/Z1_(computer)
25. https://www.youtube.com/watch?v=cNN_tTXABUA
26. https://www.youtube.com/watch?v=Z5JC9Ve1sfI
27. https://www.youtube.com/watch?v=sK-49uz3lGg
28. https://www.youtube.com/watch?v=QXjU9qTsYCc
29. https://github.com/fybx/processor
30. https://github.com/fybx/interpreter

