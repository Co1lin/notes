# Instruction

## Format

![Screen Shot 2021-09-21 at 6.46.16 PM](4_Instruction.assets/Screen%20Shot%202021-09-21%20at%206.46.16%20PM.png)

指令操作码拓展技术

- OP || A1 || A2 || A3

- 指令长度一定，基本操作码和地址码长度总和不变

- 假设总长 16 bits ，每个 OP 长度为之后的长度保留的位数不同时，一、二、三地址码的指令的条数比例可以变化

- 例 1 ：

    ![Screen Shot 2021-12-22 at 9.25.00 PM](4_Instruction.assets/Screen%20Shot%202021-12-22%20at%209.25.00%20PM.png)

- 例 2 ：

    ![Screen Shot 2021-12-22 at 9.25.16 PM](4_Instruction.assets/Screen%20Shot%202021-12-22%20at%209.25.16%20PM.png)

## Addressing Mode

寻址方式：如何获取操作数

1. 立即数寻址 [RISC-V]：指令字中直接给出数值
2. 直接寻址：指令字中给出数值在 Mem 中的地址
3. 间接寻址：指令字中给出数值在 Mem 中的地址的地址
4. 寄存器寻址 [RISC-V]：指令字中给出数值在哪个寄存器
5. 寄存器间接寻址：指令字中给出数值在 Mem 中的地址存在哪个寄存器
6. 变址寻址 [RISC-V]：操作数在 Mem 中的地址由「变址寄存器」和固定的「变址偏移量」相加得到
7. 相对寻址 [RISC-V]：操作数在 Mem 中的地址由 PC 的值和相对偏移量相加得到（如转移指令）
8. 基址寻址：操作数在 Mem 中的地址由基址寄存器和指令中的（相对？）地址相加得到
9. 堆栈寻址

形式地址：指令中给出的操作数（或指令）的地址

实际地址：使用形式地址信息并按一定规则计算出来或读操作得到的一个数值（数据（或指令）的实际地址）

## System Mode

系统运行模式

- 实模式：直接访问物理地址
- 保护模式：通过段页式转换访问物理地址
- 虚拟机模式：两层转换得到物理地址

## RISC-V

工作模式/特权模式 Privileged Mode

- Machine Mode 机器模式：必须实现
- Supervisor Mode 监督模式
- User Mode 用户模式

RISC-V 忽略溢出问题

little endian: 低权字节存在低地址

寄存器惯例

- Caller: 调用者
- Callee: 被调用者
- Caller saved register: 调用者自己负责保存， callee 不用管，自由使用
- Callee saved register: 被调用者需要保存这些 registers 的值，使其被调用前后对于 caller 来说这些值不变

伪指令 `li rd, imm` 翻译注意事项：

- 第一步， `lui rd, imm_high20` （`lui` 把立即数左移 12 位）
- 第二步，若 `imm_low12` 第一位是 1 ， `imm_high20` 减一； `addi rd, rd, imm_low12` （`addi` 使立即数符号拓展至 32 位）

