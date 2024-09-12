# Verilog ICAPE Components Readme

## Introduction
    
Xilinx FPGA 中ICAP(Internal Configuration Access Port) 指的是内部配置访问端口，其主要作用是通过内部配置访问端口（ICAP），
用户可以在FPGA逻辑代码中直接读写FPGA内部配置寄存器（类似SelectMAP），从而实现特定的配置功能，例如Multiboot。
FPGA实现IPROG通常有两种方式，一种是通过ICAP配置，一种是把相关指令嵌入bit文件中。与通过bit文件实现IPROG相比，通过ICAP更灵活。

ICAP目前为止有三个版本，包括ICAP，ICAPE2以及ICAPE3。 UltraScale系列对应ICAPE3，7系列对应ICAPE2，7系列之前的对应ICAP。每个版本有少许区别。

以下以ICAPE3 为例，ICAPE3 的接口如下：

![image](https://github.com/user-attachments/assets/bcc24823-6402-48fd-a8c8-8a73305b57fa)

##    通过ICAP发送IPROG指令实现Multiboot的步骤如下

![image](https://github.com/user-attachments/assets/754aab97-9ecb-43ac-8c02-ae9107f285bf)

首先写入同步头 32’hAA995566, 然后将需要跳转到的bit文件的起始地址写入WBSTAR寄存器，最后写入IPROG（internal PROGRAM_B）指令。

这里需要注意一点，ICAP以及SelectMAP都存在位反转（Bit Swapping），也就是说，上表中所有的数据需要进行位反转之后才能接到ICAP的输入接口，同理，ICAP输出的值需要进行位反转后才能与实际的值对应起来，位反转的示例如下图。

![image](https://github.com/user-attachments/assets/42e806cd-73a1-4b9f-b49f-a068e77149c8)

