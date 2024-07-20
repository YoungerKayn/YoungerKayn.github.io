---
layout: post

title: "MicroComputer reviewing"

subtitle: "From chapter 1 to chapter 4."

author: "Younger"

header-style: text

tags:
  - 笔记
  - 微机
  - MicroComputer
  - linux
  - 嵌入式
---

> A microcomputer is a complete computer on a small scale, designed for use by one person at a time. An antiquated term, a microcomputer is now primarily called a personal computer (PC), or a device based on a single-chip microprocessor. Common microcomputers include laptops and desktops. (techtarget.com)

## Chapter 1

## Chapter 2

## Chapter 3
1. ### 数据寻址方式  
    
    * #### 立即寻址
        例：
        ``` x86ASM
        MOV AX,2
        MOV AL,10010010B AND 0FEH
        MOV AL,PORT1
        MOV AX,DATA1
        ```

    * #### 寄存器寻址
        例：
        ``` x86ASM
        MOV AL,BL
        MOV AX,BX
        ```

    * #### 存储器寻址
        1. 直接寻址
        2. 寄存器间接寻址
        3. 寄存器相对寻址
        4. 基址变址寻址
        5. 相对基址变址寻址

2. ### 指令寻址方式

## Chapter 4